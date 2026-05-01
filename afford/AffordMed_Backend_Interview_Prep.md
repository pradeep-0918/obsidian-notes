# 🧠 AffordMed Backend Interview Prep — Express.js

> **Tags:** #interview #backend #express #nodejs #mongodb #fullstack
> **Round:** Web Development — Round 1
> **Time Limit:** 3 hours
> **Stack:** Node.js + Express + MongoDB (Mongoose)

---

## 📌 Your Current Stack (Quick Reference)

```
express, mongoose, dotenv, nodemon
Route: /subscribers → GET, POST, GET/:id, PATCH/:id, PUT/:id, DELETE/:id
DB: MongoDB via Mongoose
```

---

## 🗂️ Table of Contents

1. [[#1. URL Shortener System]]
2. [[#2. Login System with JWT Middleware]]
3. [[#3. Request Logging Middleware]]
4. [[#4. CRUD API for Todos]]
5. [[#5. API Rate Limiter]]
6. [[#6. Click Analytics System]]
7. [[#7. Search + Filter API]]
8. [[#8. Data Aggregation Service]]
9. [[#9. Session Management System]]
10. [[#10. Simple Microservice API]]

---

## 1. URL Shortener System

> **Concept:** Hash long URLs → short codes. Redirect on visit. Track hits.

### Schema
```javascript
// models/Url.js
const mongoose = require('mongoose')

const urlSchema = new mongoose.Schema({
  originalUrl: { type: String, required: true },
  shortCode:   { type: String, required: true, unique: true },
  clicks:      { type: Number, default: 0 },
  createdAt:   { type: Date, default: Date.now }
})

module.exports = mongoose.model('Url', urlSchema)
```

### Route
```javascript
// routes/url.js
const express = require('express')
const router = express.Router()
const Url = require('../models/Url')
const crypto = require('crypto')

// POST /shorten → create short URL
router.post('/shorten', async (req, res) => {
  const { originalUrl } = req.body
  if (!originalUrl) return res.status(400).json({ message: 'URL required' })

  const shortCode = crypto.randomBytes(4).toString('hex') // e.g. "a3f9c1b2"

  try {
    const url = new Url({ originalUrl, shortCode })
    await url.save()
    res.status(201).json({ shortUrl: `http://localhost:3000/${shortCode}` })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

// GET /:code → redirect to original
router.get('/:code', async (req, res) => {
  try {
    const url = await Url.findOne({ shortCode: req.params.code })
    if (!url) return res.status(404).json({ message: 'URL not found' })

    url.clicks++
    await url.save()
    res.redirect(url.originalUrl)
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

// GET /stats/:code → view click count
router.get('/stats/:code', async (req, res) => {
  try {
    const url = await Url.findOne({ shortCode: req.params.code })
    if (!url) return res.status(404).json({ message: 'Not found' })
    res.json({ originalUrl: url.originalUrl, clicks: url.clicks })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- `crypto.randomBytes(4).toString('hex')` → 8-char unique code
- Increment clicks on each redirect
- `res.redirect()` sends HTTP 302

---

## 2. Login System with JWT Middleware

> **Concept:** Register/Login users → issue JWT → protect routes with middleware

### Dependencies
```bash
npm i bcryptjs jsonwebtoken
```

### Schema
```javascript
// models/User.js
const mongoose = require('mongoose')
const bcrypt = require('bcryptjs')

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  password: { type: String, required: true }
})

// Hash password before save
userSchema.pre('save', async function (next) {
  if (!this.isModified('password')) return next()
  this.password = await bcrypt.hash(this.password, 10)
  next()
})

module.exports = mongoose.model('User', userSchema)
```

### Auth Routes
```javascript
// routes/auth.js
const express = require('express')
const router = express.Router()
const jwt = require('jsonwebtoken')
const bcrypt = require('bcryptjs')
const User = require('../models/User')

// POST /auth/register
router.post('/register', async (req, res) => {
  const { username, password } = req.body
  try {
    const user = new User({ username, password })
    await user.save()
    res.status(201).json({ message: 'User created' })
  } catch (err) {
    res.status(400).json({ message: err.message })
  }
})

// POST /auth/login
router.post('/login', async (req, res) => {
  const { username, password } = req.body
  try {
    const user = await User.findOne({ username })
    if (!user) return res.status(404).json({ message: 'User not found' })

    const isMatch = await bcrypt.compare(password, user.password)
    if (!isMatch) return res.status(401).json({ message: 'Invalid credentials' })

    const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, { expiresIn: '1h' })
    res.json({ token })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Auth Middleware
```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken')

function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization']
  const token = authHeader && authHeader.split(' ')[1] // "Bearer <token>"

  if (!token) return res.status(401).json({ message: 'Access denied. No token.' })

  try {
    const verified = jwt.verify(token, process.env.JWT_SECRET)
    req.user = verified  // { id: "..." }
    next()
  } catch (err) {
    res.status(403).json({ message: 'Invalid or expired token' })
  }
}

module.exports = authenticateToken
```

### Protected Route Usage
```javascript
const auth = require('../middleware/auth')

// Only logged-in users can access this
router.get('/profile', auth, async (req, res) => {
  const user = await User.findById(req.user.id).select('-password')
  res.json(user)
})
```

### .env Addition
```dotenv
JWT_SECRET=your_super_secret_key_here
```

### Key Concepts
- `bcrypt.hash()` one-way hashing; never store plain passwords
- `jwt.sign(payload, secret, options)` → token
- `jwt.verify(token, secret)` → decoded payload or throws
- Middleware sits between route and handler via `next()`

---

## 3. Request Logging Middleware

> **Concept:** Log every request's method, URL, status, and response time

### Simple Logger (Manual)
```javascript
// middleware/logger.js
function requestLogger(req, res, next) {
  const start = Date.now()

  // Override res.send to capture status after response
  const originalSend = res.send.bind(res)
  res.send = function (body) {
    const duration = Date.now() - start
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url} → ${res.statusCode} (${duration}ms)`)
    return originalSend(body)
  }

  next()
}

module.exports = requestLogger
```

### Usage in server.js
```javascript
const requestLogger = require('./middleware/logger')
app.use(requestLogger)  // before routes
```

### File Logger (writes to disk)
```javascript
const fs = require('fs')
const path = require('path')

function fileLogger(req, res, next) {
  const log = `${new Date().toISOString()} | ${req.method} | ${req.url} | IP: ${req.ip}\n`
  fs.appendFile(path.join(__dirname, '../logs/requests.log'), log, (err) => {
    if (err) console.error('Log write failed:', err)
  })
  next()
}
```

### Key Concepts
- Middleware runs via `next()` chain
- `Date.now()` for timing before/after
- `app.use()` applies globally; `router.use()` applies to a specific router

---

## 4. CRUD API for Todos

> **Concept:** Classic CRUD with validation — similar to your Subscriber API but with more fields

### Schema
```javascript
// models/Todo.js
const mongoose = require('mongoose')

const todoSchema = new mongoose.Schema({
  title:     { type: String, required: true, trim: true },
  completed: { type: Boolean, default: false },
  priority:  { type: String, enum: ['low', 'medium', 'high'], default: 'medium' },
  dueDate:   { type: Date },
  createdAt: { type: Date, default: Date.now }
})

module.exports = mongoose.model('Todo', todoSchema)
```

### Routes
```javascript
// routes/todos.js
const express = require('express')
const router = express.Router()
const Todo = require('../models/Todo')

// GET all → with optional ?completed=true filter
router.get('/', async (req, res) => {
  try {
    const filter = {}
    if (req.query.completed !== undefined) {
      filter.completed = req.query.completed === 'true'
    }
    const todos = await Todo.find(filter).sort({ createdAt: -1 })
    res.json(todos)
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

// POST → create
router.post('/', async (req, res) => {
  const todo = new Todo({
    title:    req.body.title,
    priority: req.body.priority,
    dueDate:  req.body.dueDate
  })
  try {
    const newTodo = await todo.save()
    res.status(201).json(newTodo)
  } catch (err) {
    res.status(400).json({ message: err.message })
  }
})

// PATCH /:id → partial update (toggle complete, change title, etc.)
router.patch('/:id', async (req, res) => {
  try {
    const todo = await Todo.findByIdAndUpdate(
      req.params.id,
      { $set: req.body },
      { new: true, runValidators: true }
    )
    if (!todo) return res.status(404).json({ message: 'Todo not found' })
    res.json(todo)
  } catch (err) {
    res.status(400).json({ message: err.message })
  }
})

// DELETE /:id
router.delete('/:id', async (req, res) => {
  try {
    const todo = await Todo.findByIdAndDelete(req.params.id)
    if (!todo) return res.status(404).json({ message: 'Todo not found' })
    res.json({ message: 'Todo deleted' })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- `$set: req.body` safely updates only provided fields
- `runValidators: true` applies schema validation on update
- Query params: `req.query.completed` for filtering

---

## 5. API Rate Limiter

> **Concept:** Limit each IP to N requests per time window (e.g., 100 req/15 min)

### In-Memory Rate Limiter (no dependencies)
```javascript
// middleware/rateLimiter.js

const requestCounts = {}  // { "ip": { count, resetTime } }

function rateLimiter(limit = 100, windowMs = 15 * 60 * 1000) {
  return (req, res, next) => {
    const ip = req.ip
    const now = Date.now()

    if (!requestCounts[ip] || now > requestCounts[ip].resetTime) {
      requestCounts[ip] = { count: 1, resetTime: now + windowMs }
      return next()
    }

    requestCounts[ip].count++

    if (requestCounts[ip].count > limit) {
      return res.status(429).json({
        message: 'Too many requests. Please try again later.',
        retryAfter: Math.ceil((requestCounts[ip].resetTime - now) / 1000) + 's'
      })
    }

    next()
  }
}

module.exports = rateLimiter
```

### Usage
```javascript
const rateLimiter = require('./middleware/rateLimiter')

// Apply globally: 100 req per 15 min
app.use(rateLimiter(100, 15 * 60 * 1000))

// Apply strictly to auth routes: 5 req per minute
app.use('/auth', rateLimiter(5, 60 * 1000), authRouter)
```

### Using express-rate-limit package (production)
```bash
npm i express-rate-limit
```
```javascript
const rateLimit = require('express-rate-limit')

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: { message: 'Too many requests' }
})

app.use(limiter)
```

### Key Concepts
- HTTP 429 = "Too Many Requests"
- In-memory works for single server; use Redis for distributed
- Each IP tracked independently

---

## 6. Click Analytics System

> **Concept:** Track which links/buttons are clicked, when, by whom (IP/user)

### Schema
```javascript
// models/Click.js
const mongoose = require('mongoose')

const clickSchema = new mongoose.Schema({
  targetId:  { type: String, required: true },  // e.g., button ID or link ID
  userIp:    { type: String },
  userAgent: { type: String },
  timestamp: { type: Date, default: Date.now }
})

module.exports = mongoose.model('Click', clickSchema)
```

### Routes
```javascript
// routes/analytics.js
const express = require('express')
const router = express.Router()
const Click = require('../models/Click')

// POST /analytics/click → record a click event
router.post('/click', async (req, res) => {
  try {
    const click = new Click({
      targetId:  req.body.targetId,
      userIp:    req.ip,
      userAgent: req.headers['user-agent']
    })
    await click.save()
    res.status(201).json({ message: 'Click recorded' })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

// GET /analytics/:targetId → get click count + breakdown
router.get('/:targetId', async (req, res) => {
  try {
    const { targetId } = req.params
    const total = await Click.countDocuments({ targetId })

    // Clicks per day (aggregation pipeline)
    const perDay = await Click.aggregate([
      { $match: { targetId } },
      {
        $group: {
          _id: { $dateToString: { format: '%Y-%m-%d', date: '$timestamp' } },
          count: { $sum: 1 }
        }
      },
      { $sort: { _id: 1 } }
    ])

    res.json({ targetId, total, perDay })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- `req.ip` → client IP address
- `req.headers['user-agent']` → browser info
- MongoDB aggregation: `$group`, `$dateToString`, `$sum`

---

## 7. Search + Filter API

> **Concept:** Search by keyword, filter by field, sort, paginate results

### Route
```javascript
// routes/products.js (or any collection)
const express = require('express')
const router = express.Router()
const Product = require('../models/Product')

// GET /products?search=phone&category=electronics&minPrice=100&maxPrice=500&sort=price&page=1&limit=10
router.get('/', async (req, res) => {
  try {
    const {
      search, category,
      minPrice, maxPrice,
      sort = 'createdAt',
      page = 1, limit = 10
    } = req.query

    // Build filter dynamically
    const filter = {}

    if (search) {
      filter.$or = [
        { name:        { $regex: search, $options: 'i' } },
        { description: { $regex: search, $options: 'i' } }
      ]
    }
    if (category) filter.category = category
    if (minPrice || maxPrice) {
      filter.price = {}
      if (minPrice) filter.price.$gte = Number(minPrice)
      if (maxPrice) filter.price.$lte = Number(maxPrice)
    }

    const skip = (Number(page) - 1) * Number(limit)

    const [products, total] = await Promise.all([
      Product.find(filter)
        .sort({ [sort]: 1 })
        .skip(skip)
        .limit(Number(limit)),
      Product.countDocuments(filter)
    ])

    res.json({
      data: products,
      pagination: {
        total,
        page: Number(page),
        pages: Math.ceil(total / Number(limit))
      }
    })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- `$regex` → case-insensitive text search (`$options: 'i'`)
- `$gte` / `$lte` → range filters
- `.skip()` + `.limit()` → pagination
- `Promise.all()` → run queries in parallel

---

## 8. Data Aggregation Service

> **Concept:** Merge data from multiple sources (internal DB + external API) into one response

### Example: Merge user data with GitHub profile
```javascript
// routes/userProfile.js
const express = require('express')
const router = express.Router()
const User = require('../models/User')
const https = require('https')

function fetchGitHub(username) {
  return new Promise((resolve, reject) => {
    https.get(`https://api.github.com/users/${username}`, {
      headers: { 'User-Agent': 'MyApp' }
    }, (res) => {
      let data = ''
      res.on('data', chunk => data += chunk)
      res.on('end', () => resolve(JSON.parse(data)))
    }).on('error', reject)
  })
}

// GET /profile/:userId → merged local DB + GitHub data
router.get('/:userId', async (req, res) => {
  try {
    const user = await User.findById(req.params.userId).select('-password')
    if (!user) return res.status(404).json({ message: 'User not found' })

    let githubData = null
    if (user.githubUsername) {
      githubData = await fetchGitHub(user.githubUsername)
    }

    res.json({
      user: {
        id:       user._id,
        username: user.username,
        email:    user.email
      },
      github: githubData ? {
        repos:     githubData.public_repos,
        followers: githubData.followers,
        avatar:    githubData.avatar_url
      } : null
    })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- Aggregate different data sources into one clean response
- `Promise.all([dbQuery, apiCall])` → fetch in parallel
- Always handle partial failure gracefully (null if external fails)

---

## 9. Session Management System

> **Concept:** Server-side sessions using express-session (alternative to JWT)

### Dependencies
```bash
npm i express-session connect-mongo
```

### Setup in server.js
```javascript
const session = require('express-session')
const MongoStore = require('connect-mongo')

app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  store: MongoStore.create({ mongoUrl: process.env.DATABASE_URL }),
  cookie: {
    maxAge: 1000 * 60 * 60 * 24,  // 1 day
    httpOnly: true,                 // blocks JS access
    secure: false                   // true in production (HTTPS only)
  }
}))
```

### Auth Routes with Sessions
```javascript
// POST /login → store session
router.post('/login', async (req, res) => {
  const { username, password } = req.body
  const user = await User.findOne({ username })
  if (!user) return res.status(404).json({ message: 'Not found' })

  const isMatch = await bcrypt.compare(password, user.password)
  if (!isMatch) return res.status(401).json({ message: 'Wrong password' })

  req.session.userId = user._id   // Store user ID in session
  req.session.username = user.username
  res.json({ message: 'Logged in', username: user.username })
})

// POST /logout → destroy session
router.post('/logout', (req, res) => {
  req.session.destroy(err => {
    if (err) return res.status(500).json({ message: 'Logout failed' })
    res.clearCookie('connect.sid')
    res.json({ message: 'Logged out' })
  })
})
```

### Session Middleware (protect routes)
```javascript
function requireSession(req, res, next) {
  if (!req.session.userId) {
    return res.status(401).json({ message: 'Not authenticated' })
  }
  next()
}

// Protected route
router.get('/dashboard', requireSession, (req, res) => {
  res.json({ message: `Welcome, ${req.session.username}` })
})
```

### JWT vs Session — When to Use What

| Feature | JWT | Session |
|---|---|---|
| Storage | Client (localStorage/cookie) | Server (DB/memory) |
| Stateless | ✅ Yes | ❌ No |
| Revoke easily | ❌ Hard | ✅ Easy (delete from DB) |
| Scalability | ✅ Great (microservices) | ⚠️ Needs shared store |
| Use case | APIs, mobile | Traditional web apps |

---

## 10. Simple Microservice API

> **Concept:** Single-responsibility service with its own route, model, and clear interface

### Notification Microservice Example
```javascript
// services/notification.service.js
const Notification = require('../models/Notification')

const notificationService = {
  async create(userId, message, type = 'info') {
    return await Notification.create({ userId, message, type })
  },

  async getUnread(userId) {
    return await Notification.find({ userId, read: false }).sort({ createdAt: -1 })
  },

  async markRead(notificationId) {
    return await Notification.findByIdAndUpdate(
      notificationId,
      { read: true },
      { new: true }
    )
  }
}

module.exports = notificationService
```

### Schema
```javascript
// models/Notification.js
const mongoose = require('mongoose')

const notificationSchema = new mongoose.Schema({
  userId:    { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  message:   { type: String, required: true },
  type:      { type: String, enum: ['info', 'warning', 'error'], default: 'info' },
  read:      { type: Boolean, default: false },
  createdAt: { type: Date, default: Date.now }
})

module.exports = mongoose.model('Notification', notificationSchema)
```

### API Routes
```javascript
// routes/notifications.js
const express = require('express')
const router = express.Router()
const auth = require('../middleware/auth')
const notificationService = require('../services/notification.service')

router.get('/', auth, async (req, res) => {
  try {
    const notifications = await notificationService.getUnread(req.user.id)
    res.json(notifications)
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

router.patch('/:id/read', auth, async (req, res) => {
  try {
    const updated = await notificationService.markRead(req.params.id)
    res.json(updated)
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

module.exports = router
```

### Key Concepts
- Single Responsibility: each service does ONE thing
- Service layer separates business logic from routes
- `ref: 'User'` → Mongoose population (`.populate('userId')`)

---

## ⚡ Quick Patterns Cheatsheet

```javascript
// 1. Async error wrapper (avoid try/catch repetition)
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next)

// Usage:
router.get('/', asyncHandler(async (req, res) => {
  const data = await SomeModel.find()
  res.json(data)
}))

// 2. Global error handler (in server.js, AFTER all routes)
app.use((err, req, res, next) => {
  console.error(err.stack)
  res.status(err.status || 500).json({ message: err.message || 'Internal Server Error' })
})

// 3. 404 handler
app.use((req, res) => {
  res.status(404).json({ message: 'Route not found' })
})

// 4. Validate ObjectId before DB query
const mongoose = require('mongoose')
function validateId(req, res, next) {
  if (!mongoose.Types.ObjectId.isValid(req.params.id)) {
    return res.status(400).json({ message: 'Invalid ID format' })
  }
  next()
}
```

---

## 📁 Recommended Project Structure

```
project/
├── server.js
├── .env
├── models/
│   ├── User.js
│   ├── Todo.js
│   └── Notification.js
├── routes/
│   ├── auth.js
│   ├── todos.js
│   └── notifications.js
├── middleware/
│   ├── auth.js          ← JWT verify
│   ├── logger.js        ← request logger
│   └── rateLimiter.js
├── services/
│   └── notification.service.js
└── logs/
    └── requests.log
```

---

## 🎯 Interview Day Tips

> [!tip] Before You Start
> - Read the problem fully before coding
> - Ask: "Should I add authentication to this?"
> - Plan: Schema → Routes → Middleware → Test

> [!note] Common HTTP Status Codes
> | Code | Meaning |
> |------|---------|
> | 200 | OK |
> | 201 | Created |
> | 400 | Bad Request |
> | 401 | Unauthorized (not logged in) |
> | 403 | Forbidden (logged in, no permission) |
> | 404 | Not Found |
> | 429 | Too Many Requests |
> | 500 | Internal Server Error |

> [!warning] Common Mistakes to Avoid
> - Forgetting `async/await` on DB calls
> - Not handling the case where `findById` returns `null`
> - Storing plain text passwords (always hash with bcrypt)
> - Sending response twice (res.json() after res.redirect())

---

*Last updated: Pre-AffordMed Interview | Good luck! 🚀*
