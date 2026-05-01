# 🔗 URL Shortener — Full Code Explanation

> **Tags:** #fullstack #code #explanation #nodejs #react
> **Purpose:** Line-by-line understanding of every file in the project

---

## 📁 Project Structure

```
url-shortener/
├── backend/
│   ├── package.json
│   ├── server.js          ← Entry point
│   ├── models/
│   │   └── Url.js         ← MongoDB schema
│   └── routes/
│       └── url.js         ← All API routes
│
└── frontend/
    ├── package.json
    ├── src/
    │   ├── main.jsx        ← React entry
    │   ├── App.jsx         ← Root component
    │   └── components/
    │       └── UrlShortener.jsx  ← Main UI component
    └── index.html
```

---

# 🖥️ BACKEND — Complete Code + Explanation

---

## 1. `backend/package.json`

```json
{
  "name": "url-shortener-backend",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.0",
    "express": "^4.18.2",
    "mongoose": "^7.0.0",
    "nanoid": "^5.0.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  }
}
```

**Why each package:**
- `express` — Web server framework
- `mongoose` — MongoDB ODM (Object Document Mapper)
- `nanoid` — Generates unique short codes (URL-safe, compact)
- `cors` — Allows React (port 5173) to talk to Express (port 5000)
- `dotenv` — Loads `.env` variables like `MONGO_URI`
- `nodemon` — Auto-restarts server on file changes (dev only)

> `"type": "module"` → Enables ES Module syntax (`import/export`) instead of CommonJS (`require`)

---

## 2. `backend/.env`

```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/urlshortener
BASE_URL=http://localhost:5000
```

- `PORT` — Which port Express listens on
- `MONGO_URI` — MongoDB connection string (local or Atlas cloud)
- `BASE_URL` — Prefix for generating the short URL (e.g., `http://localhost:5000/abc123`)

---

## 3. `backend/models/Url.js` — MongoDB Schema

```js
import mongoose from "mongoose";
// Define the shape of a URL document in MongoDB
const urlSchema = new mongoose.Schema(
  {
    longUrl: {
      type: String,
      required: true,   // Must be provided — no empty documents
    },
    shortCode: {
      type: String,
      required: true,
      unique: true,     // No two documents can have the same shortCode
    },
  },
  {
    timestamps: true,   // Automatically adds createdAt + updatedAt fields
  }
);

// Create a MongoDB index on shortCode for fast lookups during redirect
// Without this, every redirect would do a full collection scan — very slow at scale
urlSchema.index({ shortCode: 1 });

// Export the model — 'Url' becomes the collection name 'urls' in MongoDB
export default mongoose.model("Url", urlSchema);
```

**Key concepts:**
- `mongoose.Schema` defines the structure (like a class or type)
- `unique: true` on `shortCode` prevents duplicates at the DB level (safety net beyond app-level checks)
- `timestamps: true` gives you free `createdAt` / `updatedAt` — useful for analytics or expiry
- The index on `shortCode` makes `findOne({ shortCode })` run in O(log n) instead of O(n)

---

## 4. `backend/routes/url.js` — API Routes

```js
import express from "express";
import { nanoid } from "nanoid";
import Url from "../models/Url.js";

const router = express.Router();

// ─────────────────────────────────────────────
// POST /api/shorten
// Purpose: Accept a long URL, return a short URL
// ─────────────────────────────────────────────
router.post("/shorten", async (req, res) => {
  const { longUrl } = req.body;

  // Step 1: Validate the URL
  // new URL() throws a TypeError if the string is not a valid URL
  try {
    new URL(longUrl);
  } catch {
    return res.status(400).json({ error: "Invalid URL. Please include https://" });
  }

  try {
    // Step 2: Check if this long URL was already shortened
    // This avoids creating duplicate entries for the same URL
    const existing = await Url.findOne({ longUrl });
    if (existing) {
      // Return the already-existing short URL
      return res.json({
        shortUrl: `${process.env.BASE_URL}/${existing.shortCode}`,
      });
	    }

    // Step 3: Generate a unique short code
    // nanoid(8) produces 8 random URL-safe characters like "V1StGXR8"
    let shortCode = nanoid(8);

    // Step 4: Collision check (extremely rare but good practice)
    // If the code already exists in DB, generate a new one
    let codeExists = await Url.findOne({ shortCode });
    while (codeExists) {
      shortCode = nanoid(8);
      codeExists = await Url.findOne({ shortCode });
    }

    // Step 5: Save the mapping to MongoDB
    const urlDoc = new Url({ longUrl, shortCode });
    await urlDoc.save();

    // Step 6: Return the complete short URL to the frontend
    return res.status(201).json({
      shortUrl: `${process.env.BASE_URL}/${shortCode}`,
    });

  } catch (err) {
    console.error(err);
    return res.status(500).json({ error: "Server error" });
  }
});


// ─────────────────────────────────────────────
// GET /:shortCode
// Purpose: Redirect user to the original long URL
// ─────────────────────────────────────────────
router.get("/:shortCode", async (req, res) => {
  const { shortCode } = req.params;

  try {
    // Look up the shortCode in MongoDB
    const urlDoc = await Url.findOne({ shortCode });

    if (!urlDoc) {
      // Short code doesn't exist in DB
      return res.status(404).json({ error: "Short URL not found" });
    }

    // 302 = Temporary Redirect
    // Browser goes to longUrl automatically
    // Use 301 only if the redirect is permanent (browsers cache 301 forever)
    return res.redirect(302, urlDoc.longUrl);

  } catch (err) {
    console.error(err);
    return res.status(500).json({ error: "Server error" });
  }
});

export default router;
```

**Deep dive — why `302` not `301`:**
- `301` = Permanent redirect. Browsers cache this. If you change the short URL's destination, users still go to the old one (from cache).
- `302` = Temporary redirect. Browser always asks the server. Safer for a URL shortener.

**Why check for existing `longUrl`?**
- Idempotency — shortening the same URL twice should give the same result
- Saves DB space

---

## 5. `backend/server.js` — Entry Point

```js
import express from "express";
import mongoose from "mongoose";
import cors from "cors";
import dotenv from "dotenv";
import urlRoutes from "./routes/url.js";

// Load environment variables from .env file
dotenv.config();

const app = express();

// ── Middleware ──────────────────────────────
// Parse incoming JSON request bodies
app.use(express.json());

// Allow cross-origin requests from React frontend
// In production, replace "*" with your actual frontend domain
app.use(cors({ origin: "*" }));


// ── Routes ──────────────────────────────────
// All /api/shorten routes
app.use("/api", urlRoutes);

// Redirect route (e.g., GET /abc12345)
// This must come AFTER /api routes to avoid conflicts
app.use("/", urlRoutes);


// ── Database Connection ──────────────────────
mongoose
  .connect(process.env.MONGO_URI)
  .then(() => {
    console.log("✅ MongoDB connected");

    // Only start listening AFTER DB is connected
    // Prevents requests from coming in before DB is ready
    app.listen(process.env.PORT, () => {
      console.log(`🚀 Server running on http://localhost:${process.env.PORT}`);
    });
  })
  .catch((err) => {
    console.error("❌ MongoDB connection failed:", err.message);
    process.exit(1); // Exit the process — no point running without a DB
  });
```

**Why start server inside `.then()`?**
- If server starts before DB connects, early requests will fail
- Putting `app.listen` inside the DB connection callback ensures the app is only live when ready

**Why `process.exit(1)` on DB failure?**
- Exit code `1` signals an error to the OS / deployment platform
- Orchestration tools (Docker, PM2, Kubernetes) can then auto-restart or alert

---

# ⚛️ FRONTEND — Complete Code + Explanation

---

## 6. `frontend/package.json`

```json
{
  "name": "url-shortener-frontend",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build"
  },
  "dependencies": {
    "axios": "^1.6.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.0",
    "vite": "^5.0.0"
  }
}
```

- `react` + `react-dom` — Core React library
- `axios` — HTTP client for API calls (cleaner than `fetch`)
- `vite` — Fast build tool and dev server

---

## 7. `frontend/src/main.jsx` — React Entry Point

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./index.css";

// Mount the React app into the <div id="root"> in index.html
ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**What is `StrictMode`?**
- Runs components twice in development to detect side effects
- Warns about deprecated APIs
- Has zero effect in production build

---

## 8. `frontend/src/App.jsx` — Root Component

```jsx
import UrlShortener from "./components/UrlShortener.jsx";

function App() {
  return (
    <div className="app">
      <UrlShortener />
    </div>
  );
}

export default App;
```

Minimal root component — just renders the main feature component.
Keeps App.jsx clean for future routing (React Router) or layout wrappers.

---

## 9. `frontend/src/components/UrlShortener.jsx` — Main UI

```jsx
import { useState } from "react";
import axios from "axios";

// Base URL of your backend API
const API_BASE = "http://localhost:5000";

function UrlShortener() {
  // ── State ─────────────────────────────────
  const [longUrl, setLongUrl]   = useState("");       // What user types
  const [shortUrl, setShortUrl] = useState("");       // Result from API
  const [loading, setLoading]   = useState(false);   // Show spinner
  const [error, setError]       = useState("");       // Error message
  const [copied, setCopied]     = useState(false);   // Copy feedback

  // ── Submit Handler ────────────────────────
  const handleSubmit = async (e) => {
    e.preventDefault();  // Prevent page reload on form submit

    // Reset previous state
    setError("");
    setShortUrl("");

    // Basic client-side validation before sending to server
    if (!longUrl.trim()) {
      setError("Please enter a URL");
      return;
    }

    // Optional: basic URL format check on frontend too
    try {
      new URL(longUrl);
    } catch {
      setError("Please enter a valid URL (include https://)");
      return;
    }

    setLoading(true);

    try {
      // POST request to backend
      // axios automatically serializes the object to JSON
      // and sets Content-Type: application/json
      const response = await axios.post(`${API_BASE}/api/shorten`, { longUrl });

      // response.data is the parsed JSON from the server
      // { shortUrl: "http://localhost:5000/abc12345" }
      setShortUrl(response.data.shortUrl);

    } catch (err) {
      // err.response exists if server returned an error (4xx, 5xx)
      // err.message exists if network failed (no internet, CORS, etc.)
      if (err.response) {
        setError(err.response.data.error || "Something went wrong");
      } else {
        setError("Network error. Is the backend running?");
      }
    } finally {
      // Always runs — whether success or error
      // Hides the loading spinner
      setLoading(false);
    }
  };

  // ── Copy to Clipboard ─────────────────────
  const handleCopy = async () => {
    try {
      await navigator.clipboard.writeText(shortUrl);
      setCopied(true);
      // Reset "Copied!" back to "Copy" after 2 seconds
      setTimeout(() => setCopied(false), 2000);
    } catch {
      alert("Copy failed. Please copy manually.");
    }
  };

  // ── Render ─────────────────────────────────
  return (
    <div className="container">
      <h1>🔗 URL Shortener</h1>

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Enter long URL (https://...)"
          value={longUrl}
          onChange={(e) => setLongUrl(e.target.value)}
          disabled={loading}
        />
        <button type="submit" disabled={loading}>
          {loading ? "Shortening..." : "Shorten URL"}
        </button>
      </form>

      {/* Error display — only renders if error is non-empty */}
      {error && <p className="error">{error}</p>}

      {/* Result display — only renders if shortUrl is non-empty */}
      {shortUrl && (
        <div className="result">
          <a href={shortUrl} target="_blank" rel="noopener noreferrer">
            {shortUrl}
          </a>
          <button onClick={handleCopy}>
            {copied ? "✅ Copied!" : "Copy"}
          </button>
        </div>
      )}
    </div>
  );
}

export default UrlShortener;
```

**Line-by-line highlights:**

| Code | Why |
|------|-----|
| `e.preventDefault()` | Stops browser default form submit (page refresh) |
| `new URL(longUrl)` | Fast client-side validation before wasting a network call |
| `axios.post(url, data)` | Auto-sets JSON headers, cleaner than fetch |
| `err.response` | Axios puts server errors here; `err.message` is for network failures |
| `finally { setLoading(false) }` | Runs regardless of success/error — always hides spinner |
| `navigator.clipboard.writeText()` | Modern async clipboard API |
| `setTimeout(() => setCopied(false), 2000)` | Reset copy button after 2s |
| `target="_blank" rel="noopener noreferrer"` | Security: prevents tab-napping attacks |

---

## 10. `frontend/src/index.css` — Basic Styles

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  background: #f0f2f5;
  display: flex;
  justify-content: center;
  padding: 2rem;
}

.container {
  background: white;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  width: 100%;
  max-width: 500px;
}

h1 { margin-bottom: 1.5rem; color: #333; }

form { display: flex; flex-direction: column; gap: 0.75rem; }

input {
  padding: 0.75rem 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s;
}

input:focus { border-color: #4f46e5; }

button {
  padding: 0.75rem;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover { background: #4338ca; }
button:disabled { background: #a5b4fc; cursor: not-allowed; }

.error { color: #ef4444; margin-top: 0.75rem; font-size: 0.9rem; }

.result {
  margin-top: 1.25rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  background: #f0fdf4;
  border-radius: 8px;
  border: 1px solid #bbf7d0;
}

.result a { color: #16a34a; text-decoration: none; word-break: break-all; flex: 1; }
.result a:hover { text-decoration: underline; }
.result button { padding: 0.4rem 0.8rem; font-size: 0.85rem; background: #16a34a; }
```

---

# 🔄 Full Request Lifecycle — End to End

```
1. User types "https://very-long-url.com/article/page?query=123"
   └── React state: longUrl = "https://..."

2. User clicks "Shorten URL"
   └── handleSubmit fires
   └── e.preventDefault() — no page reload
   └── Validates URL client-side
   └── setLoading(true) — button shows "Shortening..."

3. axios.post("http://localhost:5000/api/shorten", { longUrl })
   └── HTTP Request:
       POST /api/shorten
       Content-Type: application/json
       Body: { "longUrl": "https://very-long-url.com/..." }

4. Express receives request
   └── express.json() middleware parses body
   └── req.body.longUrl = "https://very-long-url.com/..."
   └── Validates with new URL()
   └── Checks MongoDB for existing entry
   └── Generates shortCode = nanoid(8) → "xK9mPqRt"
   └── Saves { longUrl, shortCode } to MongoDB
   └── Returns { shortUrl: "http://localhost:5000/xK9mPqRt" }

5. React receives response
   └── setShortUrl("http://localhost:5000/xK9mPqRt")
   └── setLoading(false)
   └── UI shows the short URL with Copy button

6. User clicks the short URL
   └── Browser: GET http://localhost:5000/xK9mPqRt
   └── Express matches route GET /:shortCode
   └── shortCode = "xK9mPqRt"
   └── Finds document in MongoDB
   └── res.redirect(302, "https://very-long-url.com/...")
   └── Browser follows redirect → original site loads
```

---

# 🚀 How to Run the Project

## Backend
```bash
cd backend
npm install
# Create .env file with MONGO_URI, PORT, BASE_URL
npm run dev
# ✅ Server running on http://localhost:5000
```

## Frontend
```bash
cd frontend
npm install
npm run dev
# ✅ App running on http://localhost:5173
```

## Test the API (without frontend)
```bash
# Shorten a URL
curl -X POST http://localhost:5000/api/shorten \
  -H "Content-Type: application/json" \
  -d '{"longUrl": "https://www.google.com"}'

# Response:
# { "shortUrl": "http://localhost:5000/xK9mPqRt" }

# Test redirect
curl -I http://localhost:5000/xK9mPqRt
# Response: HTTP/1.1 302 Found
#           Location: https://www.google.com
```

---

*Last updated: 2026-05-02*
