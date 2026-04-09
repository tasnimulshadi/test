# Documentation 1
A brief, one or two-sentence description of what this project does and the problem it solves.

## 📖 Table of Contents
- [Global/Centralized Error Middleware](#-globalcentralized-error-middleware)
- [Async Wrapper](#-async-wrapper)
- [Standard Response Helpers](#-standard-response-helpers)

## 🌟 Global/Centralized Error Middleware
Acts as a single point to handle application-wide errors. By implementing this, a backend can consistently return actionable information to clients
```bash
// Use in index.js
const { errorHandler } = require("./middlewares/errorHandler");

// your routes here
app.use("/api", require("./routes"));

// 👇 ALWAYS last
app.use(errorHandler);
```

## 🌟 Async Wrapper
We create a helper that automatically catches errors and sends them to middleware. Instead of writing try/catch everywhere
Connected with Global Error Middleware
```bash
// Just import and use
const asyncHandler = require("../utils/asyncHandler");

exports.getUsers = asyncHandler(async (req, res) => {
  const [rows] = await db.query("SELECT * FROM users");
  res.json(rows);
});
```

## 🌟 Standard Response Helpers
Standardizes the structure of success, error, and validation responses across trhe application.
```bash
// ✅ Success
{
  "success": true,
  "message": "Orders fetched",
  "data": {
    "items": [],
    "page": 1,
    "limit": 10
  }
}

// ❌ Error
{
  "success": false,
  "message": "User not found",
  "error": {}
}
```

Import and Use
return success(...) // ✅ Success
throw fail(...) // ❌ Error

```bash
const { success, fail } = require("../utils/response");

exports.example = asyncHandler(async (req, res) => {
  if (!something) throw fail("Bad request", 400);

  return success(res, "Done", data);
});
```
