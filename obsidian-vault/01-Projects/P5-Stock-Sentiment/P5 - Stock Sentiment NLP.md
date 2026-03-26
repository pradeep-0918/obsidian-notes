# 📁 Project 5 — Stock & Earnings Sentiment NLP Engine
**Target Companies:** JPMorgan Chase (primary)
**Duration:** 2 weeks | **Difficulty:** ⭐⭐⭐ Intermediate

---

## 🎯 What You're Building

An **NLP pipeline** that:
1. Scrapes financial news headlines or earnings call transcripts
2. Analyses sentiment (Positive / Negative / Neutral)
3. Correlates sentiment signals with stock price movements
4. Outputs a daily sentiment score for any stock ticker

> JPMorgan's trading and research divisions use exactly this type of NLP system to analyse news, Fed statements, and earnings calls for market signals.

---

## 📚 Concepts to Study First (Week 1, Days 1–3)

### What is NLP?
NLP = Natural Language Processing. Teaching computers to understand human text.

```
Input:  "JPMorgan reports record profits, beats analyst expectations"
Output: POSITIVE (sentiment), confidence: 0.92

Input:  "Bank faces $2B fine in regulatory crackdown"
Output: NEGATIVE (sentiment), confidence: 0.88
```

### Sentiment Analysis Approaches

#### Approach 1: Rule-Based (VADER) — Easy, no ML needed
```python
# VADER = Valence Aware Dictionary and sEntiment Reasoner
# Pre-built dictionary of financial words and their sentiment scores
# Works well for short texts like headlines

from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer

sia = SentimentIntensityAnalyzer()
text = "JPMorgan beats earnings expectations by wide margin"
scores = sia.polarity_scores(text)
print(scores)
# {'neg': 0.0, 'neu': 0.508, 'pos': 0.492, 'compound': 0.6369}
# compound score: -1 (most negative) to +1 (most positive)
```

#### Approach 2: FinBERT — Pretrained Finance Model (Better)
```python
# FinBERT is a BERT model fine-tuned on financial text
# Much more accurate for financial news than general VADER

from transformers import pipeline

sentiment_pipeline = pipeline(
    "sentiment-analysis",
    model="ProsusAI/finbert"
)

result = sentiment_pipeline("The Federal Reserve raises rates amid inflation concerns")
# [{'label': 'negative', 'score': 0.87}]
```

### Getting Stock Data with yfinance
```python
import yfinance as yf

# Download stock price history
jpm = yf.download("JPM", start="2023-01-01", end="2024-12-31")
print(jpm.head())
#               Open    High     Low   Close    Volume
# 2023-01-03  133.02  134.25  132.47  133.12  10234567

# Daily returns
jpm["daily_return"] = jpm["Close"].pct_change()

# Next day return (to correlate with today's news sentiment)
jpm["next_day_return"] = jpm["daily_return"].shift(-1)
```

### Text Preprocessing
```python
import re
import string

def clean_text(text):
    # Lowercase
    text = text.lower()
    # Remove URLs
    text = re.sub(r"http\S+|www\S+", "", text)
    # Remove punctuation
    text = text.translate(str.maketrans("", "", string.punctuation))
    # Remove extra whitespace
    text = " ".join(text.split())
    return text

# Example
raw = "JPMorgan CEO says 'economy looks strong' — stock up 2%!"
clean = clean_text(raw)
# "jpmorgan ceo says economy looks strong  stock up 2"
```

---

## 🔨 Step-by-Step Build Guide

### Step 1: Get News Data

**Option A: Pre-collected Dataset (Recommended for Beginners)**
- Kaggle: search "Financial News Sentiment" or "Stock Market News"
- Good dataset: "Sentiment Analysis for Financial News" on Kaggle
- Contains thousands of pre-labelled headlines

**Option B: Use News API (Free Tier)**
```python
# newsapi.org — free tier: 100 requests/day
import requests

API_KEY = "your_key_here"
url = f"https://newsapi.org/v2/everything?q=JPMorgan&apiKey={API_KEY}&language=en"
response = requests.get(url).json()
articles = response["articles"]
headlines = [a["title"] for a in articles]
```

**Option C: yfinance News (Free, No API Key)**
```python
import yfinance as yf

ticker = yf.Ticker("JPM")
news = ticker.news  # returns list of recent news dicts
headlines = [item["title"] for item in news]
```

### Step 2: Build Sentiment Scorer (`sentiment.py`)
```python
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import pandas as pd

sia = SentimentIntensityAnalyzer()

def score_headline(headline):
    scores = sia.polarity_scores(headline)
    compound = scores["compound"]

    if compound >= 0.05:
        label = "POSITIVE"
    elif compound <= -0.05:
        label = "NEGATIVE"
    else:
        label = "NEUTRAL"

    return {"headline": headline, "compound": compound, "sentiment": label}

def score_dataframe(df, text_col="headline"):
    results = df[text_col].apply(lambda x: pd.Series(score_headline(x)))
    return pd.concat([df, results[["compound", "sentiment"]]], axis=1)
```

### Step 3: Merge Sentiment with Stock Prices (`correlate.py`)
```python
import yfinance as yf
import pandas as pd

def get_stock_data(ticker, start, end):
    df = yf.download(ticker, start=start, end=end)
    df["daily_return"] = df["Close"].pct_change()
    df["next_day_return"] = df["daily_return"].shift(-1)
    df = df.reset_index()
    df["Date"] = pd.to_datetime(df["Date"]).dt.date
    return df[["Date", "Close", "daily_return", "next_day_return"]]

def merge_sentiment_prices(sentiment_df, price_df, date_col="date"):
    sentiment_df[date_col] = pd.to_datetime(sentiment_df[date_col]).dt.date

    # Average daily sentiment
    daily_sentiment = sentiment_df.groupby(date_col)["compound"].mean().reset_index()
    daily_sentiment.columns = ["Date", "avg_sentiment"]

    merged = price_df.merge(daily_sentiment, on="Date", how="inner")
    return merged
```

### Step 4: Visualise Correlation
```python
import matplotlib.pyplot as plt
import seaborn as sns

def plot_sentiment_vs_returns(df):
    fig, axes = plt.subplots(3, 1, figsize=(14, 12))

    # 1. Sentiment Score Over Time
    axes[0].plot(df["Date"], df["avg_sentiment"], color="teal", linewidth=1.5)
    axes[0].axhline(0, color="gray", linestyle="--", alpha=0.5)
    axes[0].fill_between(df["Date"], df["avg_sentiment"], 0,
                         where=df["avg_sentiment"] >= 0, alpha=0.3, color="green")
    axes[0].fill_between(df["Date"], df["avg_sentiment"], 0,
                         where=df["avg_sentiment"] < 0, alpha=0.3, color="red")
    axes[0].set_title("Daily News Sentiment Score")
    axes[0].set_ylabel("Compound Sentiment")

    # 2. Stock Price Over Time
    axes[1].plot(df["Date"], df["Close"], color="navy", linewidth=1.5)
    axes[1].set_title("Stock Price")
    axes[1].set_ylabel("Price ($)")

    # 3. Scatter: Sentiment vs Next Day Return
    axes[2].scatter(df["avg_sentiment"], df["next_day_return"], alpha=0.4, color="steelblue")
    axes[2].axhline(0, color="gray", linestyle="--")
    axes[2].axvline(0, color="gray", linestyle="--")
    axes[2].set_title("Sentiment vs Next Day Return")
    axes[2].set_xlabel("Avg Daily Sentiment Score")
    axes[2].set_ylabel("Next Day Return (%)")

    plt.tight_layout()
    plt.savefig("reports/sentiment_analysis.png")
    print("Chart saved!")

# Correlation coefficient
corr = df["avg_sentiment"].corr(df["next_day_return"])
print(f"Sentiment-Return Correlation: {corr:.4f}")
```

### Step 5: Bonus — Upgrade to FinBERT
```python
# pip install transformers torch
from transformers import pipeline

finbert = pipeline("sentiment-analysis", model="ProsusAI/finbert")

def score_with_finbert(headlines):
    results = finbert(headlines, truncation=True, max_length=512)
    output = []
    for headline, result in zip(headlines, results):
        score = result["score"]
        if result["label"] == "negative":
            score = -score  # make negative scores negative numbers
        elif result["label"] == "neutral":
            score = 0
        output.append({"headline": headline, "finbert_score": score, "label": result["label"]})
    return pd.DataFrame(output)
```

---

## 📊 What Good Results Look Like

| Metric | Target |
|---|---|
| Sentiment-Return Correlation | > 0.15 (even 0.1 is meaningful in finance) |
| Model Accuracy (if labelled) | > 70% |
| Headlines Processed | > 1,000 |

---

## ✅ Checklist

- [ ] News dataset collected (Kaggle or API)
- [ ] VADER sentiment scores computed
- [ ] Stock price data downloaded via yfinance
- [ ] Sentiment merged with prices
- [ ] Correlation calculated and visualised
- [ ] FinBERT upgrade attempted (bonus)
- [ ] GitHub pushed with charts in README

---

## 💼 How to Explain This in an Interview

> *"I built an NLP pipeline that analyses financial news headlines and correlates sentiment signals with stock price movements. Starting with VADER for rule-based sentiment scoring, I then upgraded to FinBERT — a BERT model pre-trained on financial text — for significantly better accuracy. I found a correlation of 0.18 between same-day news sentiment and next-day returns for JPMorgan's stock. This is directly aligned with the alternative data and NLP research workflows used in JPMorgan's Markets Intelligence team."*

---

## 🔗 Resources
- FinBERT: huggingface.co/ProsusAI/finbert
- yfinance: `pip install yfinance`
- VADER: `pip install vaderSentiment`
- YouTube: "NLP Sentiment Analysis Python" by Sentdex
- Kaggle: "Financial News Sentiment" datasets

---
**Previous →** [[P4 - Mortgage Market Analytics]]
**Back to Home →** [[Dashboard]]
