# 02 — Python for Data Engineering
#python #etl #oop #async #pydantic #testing #cli

**Roadmap Days:** 3–4 | [[DE_Vault/00_Master_Index]] | Prev → [[01_SQL_Advanced]] | Next → [[03_Linux_Shell]]

---

## 🗺️ Topic Map

```
Python for Data Engineering
├── OOP Patterns (ETLBase, Abstract Classes)
├── Decorators (retry, logging, timing)
├── Custom Exceptions
├── Structured Logging (loguru)
├── Type Hints & Pydantic Validation
├── File I/O (CSV, JSON, Parquet, Avro)
├── Async Programming (asyncio, httpx)
├── Concurrency (ThreadPool, ProcessPool)
├── Testing (pytest, mocks, fixtures)
├── CLI (argparse)
└── Project Structure (poetry, dotenv)
```

---

## 1. OOP for ETL — Abstract Base Classes

```python
from abc import ABC, abstractmethod
import pandas as pd
from typing import Optional

class ETLBase(ABC):
    """Base class for all ETL pipelines."""

    def __init__(self, source: str, target: str):
        self.source = source
        self.target = target
        self.logger = get_logger(self.__class__.__name__)

    @abstractmethod
    def extract(self) -> pd.DataFrame:
        """Pull raw data from source."""
        ...

    @abstractmethod
    def transform(self, df: pd.DataFrame) -> pd.DataFrame:
        """Apply business logic transformations."""
        ...

    @abstractmethod
    def load(self, df: pd.DataFrame) -> None:
        """Write to target system."""
        ...

    def run(self, dry_run: bool = False) -> None:
        """Orchestrate the full ETL."""
        self.logger.info(f"Starting ETL: {self.__class__.__name__}")
        df = self.extract()
        df = self.transform(df)
        if not dry_run:
            self.load(df)
        self.logger.info("ETL complete.")


class WeatherETL(ETLBase):
    def extract(self) -> pd.DataFrame: ...
    def transform(self, df): ...
    def load(self, df): ...
```

> 💡 **Why ABC?** Enforces contract — if a subclass doesn't implement `extract/transform/load`, Python raises `TypeError` at instantiation, not at runtime. Catches missing implementations early.

---

## 2. Decorators

### Retry Decorator
```python
import time
import functools
from typing import Callable, Type

def retry(max_attempts: int = 3, delay: float = 1.0, 
          exceptions: tuple = (Exception,)):
    def decorator(func: Callable) -> Callable:
        @functools.wraps(func)  # preserves __name__, __doc__
        def wrapper(*args, **kwargs):
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    if attempt == max_attempts:
                        raise
                    time.sleep(delay * attempt)  # exponential backoff
            
        return wrapper
    return decorator

@retry(max_attempts=3, delay=2.0, exceptions=(ConnectionError, TimeoutError))
def fetch_api_data(url: str) -> dict:
    ...
```

### Timing + Logging Decorator
```python
import time
from loguru import logger

def timed(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        logger.info(f"{func.__name__} completed in {elapsed:.2f}s")
        return result
    return wrapper
```

> 💡 **`functools.wraps`:** Always use this — without it, `func.__name__` returns `wrapper`, breaking logging, pytest, and introspection.

> 💡 **Decorator Stacking Order:** Decorators apply bottom-up. `@retry` on top of `@timed` means retry wraps timed. Usually put `@retry` outermost so timing includes retry attempts.

---

## 3. Custom Exceptions

```python
class ETLError(Exception):
    """Base exception for all ETL errors."""
    pass

class ExtractionError(ETLError):
    """Raised when data cannot be extracted from source."""
    def __init__(self, source: str, reason: str):
        self.source = source
        self.reason = reason
        super().__init__(f"Extraction failed for '{source}': {reason}")

class DataValidationError(ETLError):
    """Raised when data fails schema/quality validation."""
    def __init__(self, field: str, issue: str, value=None):
        self.field = field
        self.issue = issue
        super().__init__(f"Validation failed on '{field}': {issue} (got: {value})")

class LoadError(ETLError):
    """Raised when data cannot be written to target."""
    pass
```

---

## 4. Structured Logging with Loguru

```python
from loguru import logger
import sys

# Configure logger
logger.remove()  # Remove default handler
logger.add(sys.stderr, level="INFO", 
           format="{time:YYYY-MM-DD HH:mm:ss} | {level} | {name}:{function}:{line} | {message}")
logger.add("logs/etl_{time}.log", rotation="100 MB", retention="7 days", 
           level="DEBUG", serialize=True)  # JSON logs to file

# Usage
logger.info("Starting extraction", source="api", date="2024-01-01")
logger.warning("Retry attempt {attempt}", attempt=2)
logger.exception("Unhandled error")  # Includes full stack trace
```

> 💡 **`serialize=True`:** Writes JSON logs to file — parseable by Splunk, Datadog, ELK. Always use structured logging in production.

> 💡 **`logger.contextualize()`:** Add context to all log messages in a block:
> ```python
> with logger.contextualize(pipeline_run_id="abc123", date="2024-01"):
>     logger.info("Starting")  # every log in this block has run_id attached
> ```

---

## 5. Type Hints & Pydantic

### Type Hints
```python
from typing import Optional, Union, List, Dict, Tuple, Generator
from pathlib import Path

def process_files(
    paths: List[Path],
    output: Optional[Path] = None,
    schema: Dict[str, str] | None = None,  # Python 3.10+ union syntax
) -> Generator[pd.DataFrame, None, None]:
    ...
```

### Pydantic Models (v2)
```python
from pydantic import BaseModel, Field, field_validator, model_validator
from datetime import datetime
from enum import Enum

class SourceType(str, Enum):
    API = "api"
    FILE = "file"
    DATABASE = "db"

class WeatherRecord(BaseModel):
    station_id: str = Field(..., min_length=3, max_length=20)
    temperature: float = Field(..., ge=-90.0, le=60.0)
    humidity: float = Field(ge=0.0, le=100.0)
    recorded_at: datetime
    source: SourceType = SourceType.API

    @field_validator("station_id")
    @classmethod
    def station_must_be_uppercase(cls, v: str) -> str:
        return v.upper()

    @model_validator(mode="after")
    def check_heat_index(self) -> "WeatherRecord":
        if self.temperature > 35 and self.humidity > 80:
            # Could raise or just log a warning
            pass
        return self

# Parse and validate
record = WeatherRecord.model_validate(raw_dict)
records = [WeatherRecord.model_validate(r) for r in raw_list]

# To dict/JSON
record.model_dump()
record.model_dump_json()
```

> ⚠️ **Pydantic v1 vs v2:** `@validator` → `@field_validator`, `.dict()` → `.model_dump()`, `parse_obj` → `model_validate`. Many tutorials still show v1 syntax.

---

## 6. File I/O at Scale

```python
import pandas as pd
import pyarrow as pa
import pyarrow.parquet as pq
import json

# Parquet — best for analytics (columnar, compressed)
df.to_parquet("output.parquet", engine="pyarrow", compression="snappy")
pq.write_to_dataset(table, root_path="s3://bucket/data/", 
                    partition_cols=["year", "month"])  # partitioned write

# Read partitioned Parquet
df = pd.read_parquet("s3://bucket/data/", filters=[("year", "=", 2024)])

# Avro — best for schema evolution, streaming (Kafka)
import fastavro
schema = {"type": "record", "name": "User", "fields": [...]}
with open("data.avro", "wb") as f:
    fastavro.writer(f, schema, records)

# Large CSV — chunked processing
for chunk in pd.read_csv("huge.csv", chunksize=100_000):
    process(chunk)
```

> 💡 **File Format Decision Tree:**
> - Speed (analytics) → **Parquet**
> - Schema evolution + streaming → **Avro**
> - Interoperability with Hadoop ecosystem → **ORC**
> - Human readable + debug → **JSON/CSV** (never for production scale)

---

## 7. Async Programming

```python
import asyncio
import httpx
from typing import List

async def fetch_one(client: httpx.AsyncClient, url: str) -> dict:
    response = await client.get(url, timeout=30.0)
    response.raise_for_status()
    return response.json()

async def fetch_all(urls: List[str]) -> List[dict]:
    async with httpx.AsyncClient() as client:
        tasks = [fetch_one(client, url) for url in urls]
        results = await asyncio.gather(*tasks, return_exceptions=True)
    
    # Filter errors
    successes = [r for r in results if not isinstance(r, Exception)]
    errors = [r for r in results if isinstance(r, Exception)]
    return successes

# Rate limiting with semaphore
async def fetch_with_rate_limit(urls: List[str], max_concurrent: int = 10):
    semaphore = asyncio.Semaphore(max_concurrent)
    async with httpx.AsyncClient() as client:
        async def bounded_fetch(url):
            async with semaphore:
                return await fetch_one(client, url)
        return await asyncio.gather(*[bounded_fetch(u) for u in urls])

# Run
results = asyncio.run(fetch_all(urls))
```

> 💡 **asyncio vs threading:** asyncio for I/O-bound (HTTP, DB queries). threading for I/O-bound with blocking libraries. multiprocessing for CPU-bound (data transformations).

> ⚠️ **`asyncio.gather` with `return_exceptions=True`:** Without this, one failure cancels all tasks. Always use when you want partial results.

---

## 8. Concurrency — ThreadPool & ProcessPool

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor, as_completed
import os

# Threading — I/O bound (file reads, DB writes)
def process_file(path: str) -> dict:
    ...  # Read and transform one file

files = list(Path("data/").glob("*.parquet"))
with ThreadPoolExecutor(max_workers=8) as executor:
    futures = {executor.submit(process_file, f): f for f in files}
    for future in as_completed(futures):
        file = futures[future]
        try:
            result = future.result()
        except Exception as e:
            logger.error(f"Failed: {file} — {e}")

# Multiprocessing — CPU bound (heavy transformations)
with ProcessPoolExecutor(max_workers=os.cpu_count()) as executor:
    results = list(executor.map(heavy_transform, chunks))
```

> 💡 **GIL (Global Interpreter Lock):** Python threads cannot run Python bytecode in parallel. Threading still helps for I/O (GIL released during I/O waits). For true parallelism, use `ProcessPoolExecutor`.

---

## 9. Testing with pytest

```python
# tests/test_transform.py
import pytest
import pandas as pd
from unittest.mock import patch, MagicMock
from src.transform import clean_weather_data
from src.extract import fetch_weather

# Fixtures
@pytest.fixture
def sample_df():
    return pd.DataFrame({
        "station_id": ["NYC001", "LA002"],
        "temperature": [22.5, 35.1],
        "humidity": [60.0, 45.0],
    })

@pytest.fixture
def mock_api_response():
    return {"results": [{"id": "NYC001", "temp": 22.5}]}

# Unit test
def test_clean_removes_nulls(sample_df):
    df = sample_df.copy()
    df.loc[0, "temperature"] = None
    result = clean_weather_data(df)
    assert result["temperature"].isna().sum() == 0

# Mock external API
def test_fetch_weather_calls_api(mock_api_response):
    with patch("src.extract.httpx.get") as mock_get:
        mock_get.return_value = MagicMock(
            status_code=200,
            json=lambda: mock_api_response
        )
        result = fetch_weather("NYC001", "2024-01-01")
        assert result["id"] == "NYC001"
        mock_get.assert_called_once()

# Parametrize
@pytest.mark.parametrize("temp,expected", [
    (100.0, True),   # invalid
    (22.5, False),   # valid
    (-100.0, True),  # invalid
])
def test_temperature_validation(temp, expected):
    assert is_invalid_temp(temp) == expected
```

> 💡 **`conftest.py`:** Put shared fixtures here — automatically discovered by pytest in the same dir and subdirs.

> 💡 **`pytest-mock` vs `unittest.mock`:** Both work. `pytest-mock` gives you `mocker` fixture instead of `patch` context manager — slightly cleaner.

---

## 10. CLI with argparse

```python
import argparse
from datetime import date

def build_parser() -> argparse.ArgumentParser:
    parser = argparse.ArgumentParser(
        description="DE ETL Pipeline",
        formatter_class=argparse.RawDescriptionHelpFormatter
    )
    parser.add_argument("--source", choices=["api", "file", "db"], required=True)
    parser.add_argument("--date", type=date.fromisoformat, 
                        default=date.today(), help="Processing date YYYY-MM-DD")
    parser.add_argument("--dry-run", action="store_true", 
                        help="Run without writing to target")
    parser.add_argument("--env", choices=["dev", "staging", "prod"], default="dev")
    parser.add_argument("--batch-size", type=int, default=1000)
    return parser

if __name__ == "__main__":
    args = build_parser().parse_args()
    pipeline = create_pipeline(args.source, args.env)
    pipeline.run(date=args.date, dry_run=args.dry_run)
```

---

## 11. Project Structure (Poetry)

```
my-etl/
├── pyproject.toml
├── .env
├── .env.example
├── README.md
├── docker-compose.yml
├── src/
│   └── my_etl/
│       ├── __init__.py
│       ├── extract.py
│       ├── transform.py
│       ├── load.py
│       ├── cli.py
│       ├── models.py      # Pydantic
│       └── utils/
│           ├── logging.py
│           └── decorators.py
└── tests/
    ├── conftest.py
    ├── test_extract.py
    ├── test_transform.py
    └── test_load.py
```

```toml
# pyproject.toml
[tool.poetry]
name = "my-etl"
version = "0.1.0"

[tool.poetry.dependencies]
python = "^3.11"
pandas = "^2.0"
pydantic = "^2.0"
loguru = "^0.7"
httpx = "^0.27"
sqlalchemy = "^2.0"
pyarrow = "^15.0"

[tool.poetry.group.dev.dependencies]
pytest = "^8.0"
pytest-mock = "^3.0"
pytest-asyncio = "^0.23"
```

---

## 🃏 Interview Cheatsheet
- **Why Pydantic over raw dicts?** Automatic validation, type coercion, clear error messages, IDE autocomplete, serialization.
- **asyncio vs threading vs multiprocessing?** I/O async → asyncio. I/O blocking libs → threading. CPU-bound → multiprocessing.
- **What is the GIL?** Global Interpreter Lock — one thread runs Python bytecode at a time. Released during I/O.
- **What is `functools.wraps`?** Preserves the wrapped function's metadata (`__name__`, `__doc__`). Always use in decorators.
- **Why `set -euo pipefail` in bash?** `-e`: exit on error, `-u`: exit on unset variable, `-o pipefail`: pipe fails if any command fails.
