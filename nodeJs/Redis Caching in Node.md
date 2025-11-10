# Redis Caching in Node

## Table of Contents

- [Redis Caching in Node](#redis-caching-in-node)
  - [Table of Contents](#table-of-contents)
  - [🚀 1. What Is Redis and Why Use It?](#-1-what-is-redis-and-why-use-it)
  - [🧩 2. Prerequisites](#-2-prerequisites)
  - [⚙️ 3. Setting Up Redis Client in Node.js](#️-3-setting-up-redis-client-in-nodejs)
  - [⚡ 4. Basic Caching Example (with API Data)](#-4-basic-caching-example-with-api-data)
  - [🧠 5. Useful Redis Operations](#-5-useful-redis-operations)
  - [🧱 6. Caching Strategy Best Practices](#-6-caching-strategy-best-practices)
  - [🧰 7. Optional: Using Middleware for Caching](#-7-optional-using-middleware-for-caching)
  - [🧩 8. Example Directory Structure](#-8-example-directory-structure)
  - [✅ 9. Summary](#-9-summary)
  - [References](#references)

Here’s a **step-by-step tutorial** to help you **get started with Redis caching in Node.js** — from setup to common usage patterns and best practices.

---

## 🚀 1. What Is Redis and Why Use It?

**Redis (Remote Dictionary Server)** is an **in-memory data store** often used for:

* **Caching** (to reduce database load and speed up response time)
* **Session management**
* **Pub/Sub messaging**
* **Rate limiting**

For this tutorial, we’ll focus on **using Redis as a cache layer** in a Node.js app.

---

## 🧩 2. Prerequisites

* Node.js ≥ 14 installed

* Redis installed locally or via Docker

  ```bash
  docker run --name redis -p 6379:6379 -d redis
  ```

* A sample Node.js project:

  ```bash
  mkdir node-redis-demo && cd node-redis-demo
  npm init -y
  npm install express redis
  ```

---

## ⚙️ 3. Setting Up Redis Client in Node.js

Create a file named **`server.js`**:

```js
import express from "express";
import { createClient } from "redis";

const app = express();
const port = 3000;

// Connect to Redis
const redisClient = createClient({ url: "redis://localhost:6379" });

redisClient.on("error", (err) => console.error("Redis Client Error", err));

await redisClient.connect();
console.log("✅ Connected to Redis");

// Example endpoint
app.get("/", (req, res) => {
  res.send("Redis Caching Example");
});

app.listen(port, () => console.log(`Server running at http://localhost:${port}`));
```

Then run:

```bash
node server.js
```

---

## ⚡ 4. Basic Caching Example (with API Data)

Imagine you’re fetching data from an external API that takes time to respond.
We’ll cache the result in Redis for faster subsequent requests.

Add this code to **`server.js`**:

```js
import fetch from "node-fetch";

// Simulate slow data fetch
async function fetchData() {
  console.log("⏳ Fetching data from API...");
  const response = await fetch("https://jsonplaceholder.typicode.com/todos/1");
  return response.json();
}

app.get("/todo", async (req, res) => {
  const cacheKey = "todo:1";

  // 1️⃣ Check cache
  const cachedData = await redisClient.get(cacheKey);
  if (cachedData) {
    console.log("✅ Cache hit");
    return res.json(JSON.parse(cachedData));
  }

  // 2️⃣ Fetch from source
  const data = await fetchData();

  // 3️⃣ Store in Redis with TTL (e.g. 60 seconds)
  await redisClient.setEx(cacheKey, 60, JSON.stringify(data));
  console.log("💾 Cached new data");

  res.json(data);
});
```

Now test:

```bash
curl http://localhost:3000/todo
```

* The first request will fetch from the API.
* Subsequent requests (within 60 seconds) will return instantly from Redis.

---

## 🧠 5. Useful Redis Operations

Here are common methods you can use with the Redis client:

| Operation       | Method                   | Example                                          |
| --------------- | ------------------------ | ------------------------------------------------ |
| Set key         | `set(key, value)`        | `await redisClient.set("name", "Wallace")`       |
| Get key         | `get(key)`               | `await redisClient.get("name")`                  |
| Delete key      | `del(key)`               | `await redisClient.del("name")`                  |
| Set with expiry | `setEx(key, ttl, value)` | `await redisClient.setEx("token", 60, "abc123")` |
| Check existence | `exists(key)`            | `await redisClient.exists("cacheKey")`           |
| Flush all keys  | `flushAll()`             | `await redisClient.flushAll()`                   |

---

## 🧱 6. Caching Strategy Best Practices

✅ **Use structured cache keys:**
`user:${userId}:profile` or `product:${id}:details`

✅ **Set TTL (time-to-live):**
Prevents stale data and avoids unbounded memory usage.

✅ **Invalidate cache after updates:**
When data changes in DB, delete or refresh the corresponding Redis entry.

✅ **Use JSON serialization:**
Always `JSON.stringify` and `JSON.parse` to handle objects safely.

✅ **Monitor Redis:**
Use Redis CLI or GUI tools like **RedisInsight** to track cache hits/misses.

---

## 🧰 7. Optional: Using Middleware for Caching

You can create a simple **cache middleware** for repeated patterns:

```js
function cacheMiddleware(keyBuilder, ttl = 60) {
  return async (req, res, next) => {
    const key = keyBuilder(req);

    const cached = await redisClient.get(key);
    if (cached) {
      console.log("✅ Cache hit for", key);
      return res.json(JSON.parse(cached));
    }

    res.sendResponse = res.json;
    res.json = async (body) => {
      await redisClient.setEx(key, ttl, JSON.stringify(body));
      res.sendResponse(body);
    };

    next();
  };
}

// Usage
app.get("/todo2", cacheMiddleware(() => "todo:2"), async (req, res) => {
  const data = await fetchData();
  res.json(data);
});
```

---

## 🧩 8. Example Directory Structure

```
node-redis-demo/
│
├── package.json
├── server.js
└── .env        # (optional if using Redis credentials)
```

---

## ✅ 9. Summary

| Step | Description                                      |
| ---- | ------------------------------------------------ |
| 1    | Install Redis and connect using `createClient()` |
| 2    | Wrap expensive calls with a cache check          |
| 3    | Use `setEx()` for TTL caching                    |
| 4    | Implement cache invalidation on updates          |
| 5    | Add middleware to simplify caching logic         |

---

Would you like me to extend this tutorial with an example of **Redis caching integrated into a database-backed API** (like PostgreSQL or MongoDB)? That’s usually the next real-world step.


## References

1. https://www.youtube.com/watch?v=QqTB97aMa4c
2. 