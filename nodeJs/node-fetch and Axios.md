# Node-fetch and Axios

## Table of Contents
- [Node-fetch and Axios](#node-fetch-and-axios)
  - [Table of Contents](#table-of-contents)
- [Node-fetch](#node-fetch)
  - [🧠 What Is `node-fetch`?](#-what-is-node-fetch)
  - [⚙️ Step 1: Project Setup](#️-step-1-project-setup)
  - [📦 Step 2: Importing `fetch`](#-step-2-importing-fetch)
    - [🧩 Option A — ES Modules (`.mjs` or `"type": "module"` in package.json\`)](#-option-a--es-modules-mjs-or-type-module-in-packagejson)
    - [🧩 Option B — CommonJS (default in older Node)](#-option-b--commonjs-default-in-older-node)
  - [🌐 Step 3: Making a Basic GET Request](#-step-3-making-a-basic-get-request)
  - [📤 Step 4: Sending a POST Request](#-step-4-sending-a-post-request)
  - [🔄 Step 5: Handling Errors Gracefully](#-step-5-handling-errors-gracefully)
  - [⚡ Step 6: Using Query Parameters](#-step-6-using-query-parameters)
  - [🔧 Step 7: Using Timeouts or AbortController](#-step-7-using-timeouts-or-abortcontroller)
  - [🧪 Step 8: Example — Combine GET and POST](#-step-8-example--combine-get-and-post)
  - [🧰 Bonus: Node 18+ Native Fetch](#-bonus-node-18-native-fetch)
  - [🧾 Summary](#-summary)
- [Axios](#axios)
  - [🧠 What Is Axios?](#-what-is-axios)
  - [⚙️ Step 1: Project Setup](#️-step-1-project-setup-1)
  - [📦 Step 2: Importing Axios](#-step-2-importing-axios)
    - [If you use **ES Modules** (`"type": "module"` in `package.json` or `.mjs` files):](#if-you-use-es-modules-type-module-in-packagejson-or-mjs-files)
    - [If you use **CommonJS** (`.js` default in Node):](#if-you-use-commonjs-js-default-in-node)
  - [🌐 Step 3: Basic GET Request](#-step-3-basic-get-request)
  - [📤 Step 4: Sending a POST Request](#-step-4-sending-a-post-request-1)
  - [🔧 Step 5: Using Query Parameters](#-step-5-using-query-parameters)
  - [⚠️ Step 6: Handling Errors](#️-step-6-handling-errors)
  - [⏱️ Step 7: Timeout and Headers](#️-step-7-timeout-and-headers)
  - [♻️ Step 8: Axios Instance (Reusable Config)](#️-step-8-axios-instance-reusable-config)
  - [🧩 Step 9: Using Interceptors](#-step-9-using-interceptors)
  - [🚫 Step 10: Cancelling Requests](#-step-10-cancelling-requests)
  - [🧪 Step 11: Small Example Project](#-step-11-small-example-project)
  - [🧾 Summary](#-summary-1)
- [Node Fetch and Axios](#node-fetch-and-axios-1)
    - [Node Fetch:](#node-fetch-1)
    - [Axios:](#axios-1)
    - [When to Choose Which:](#when-to-choose-which)
  - [References](#references)

---

# Node-fetch

Here’s a **complete tutorial to get started with `node-fetch`**, the popular HTTP client library for Node.js that brings the **browser `fetch()` API** to the server side.

## 🧠 What Is `node-fetch`?

`node-fetch` is a lightweight module that lets you make HTTP requests in Node.js using the same **`fetch()`** API you use in the browser.

This makes your code more consistent across frontend and backend, especially if you’re working on full-stack JavaScript apps.

---

## ⚙️ Step 1: Project Setup

Create a new project folder and initialize it:

```bash
mkdir node-fetch-tutorial
cd node-fetch-tutorial
npm init -y
```

Install `node-fetch`:

```bash
npm install node-fetch
```

> ✅ **Note:** Starting from Node.js **v18**, `fetch` is already built-in!
> So if you’re using Node 18+, you can skip installation — but you may still use `node-fetch` for backward compatibility or consistent behavior across versions.

---

## 📦 Step 2: Importing `fetch`

There are two ways depending on your module system.

### 🧩 Option A — ES Modules (`.mjs` or `"type": "module"` in package.json`)

```js
import fetch from "node-fetch";
```

### 🧩 Option B — CommonJS (default in older Node)

```js
const fetch = require("node-fetch");
```

---

## 🌐 Step 3: Making a Basic GET Request

```js
import fetch from "node-fetch";

async function getData() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");

  // Always check response.ok
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

getData();
```

✅ Output:

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit..."
}
```

---

## 📤 Step 4: Sending a POST Request

```js
import fetch from "node-fetch";

async function createPost() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      title: "Hello from Node",
      body: "This is a test post!",
      userId: 1,
    }),
  });

  const data = await response.json();
  console.log(data);
}

createPost();
```

✅ Output:

```json
{
  "title": "Hello from Node",
  "body": "This is a test post!",
  "userId": 1,
  "id": 101
}
```

---

## 🔄 Step 5: Handling Errors Gracefully

Always handle both **network errors** and **response errors**:

```js
import fetch from "node-fetch";

async function safeFetch(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    console.log("Data:", data);
  } catch (error) {
    console.error("Fetch failed:", error.message);
  }
}

safeFetch("https://jsonplaceholder.typicode.com/invalid-endpoint");
```

---

## ⚡ Step 6: Using Query Parameters

```js
import fetch from "node-fetch";

const params = new URLSearchParams({ userId: 1 });
const url = `https://jsonplaceholder.typicode.com/posts?${params}`;

const response = await fetch(url);
const posts = await response.json();
console.log(posts);
```

---

## 🔧 Step 7: Using Timeouts or AbortController

To cancel long requests:

```js
import fetch, { AbortController } from "node-fetch";

const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 3000); // 3s timeout

try {
  const response = await fetch("https://httpbin.org/delay/5", {
    signal: controller.signal,
  });
  const data = await response.json();
  console.log(data);
} catch (err) {
  console.error("Request aborted:", err.name);
} finally {
  clearTimeout(timeout);
}
```

---

## 🧪 Step 8: Example — Combine GET and POST

Let’s combine both methods in a small real-world example:

```js
import fetch from "node-fetch";

async function example() {
  const postsResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
  const posts = await postsResponse.json();

  const newPost = {
    title: "Post count",
    body: `Fetched ${posts.length} posts.`,
    userId: 1,
  };

  const postResponse = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(newPost),
  });

  const result = await postResponse.json();
  console.log("Result:", result);
}

example();
```

---

## 🧰 Bonus: Node 18+ Native Fetch

If you’re using Node 18 or later, you can just use:

```js
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
const data = await response.json();
console.log(data);
```

✅ No installation, no imports needed.

---

## 🧾 Summary

| Task          | Example                                          |
| ------------- | ------------------------------------------------ |
| Install       | `npm install node-fetch`                         |
| GET request   | `await fetch(url)`                               |
| POST request  | `{ method: 'POST', body: JSON.stringify(data) }` |
| Handle errors | `if (!response.ok) throw new Error(...)`         |
| Abort request | `AbortController`                                |
| Node 18+      | Built-in `fetch`                                 |

---

Would you like me to extend this tutorial into a **mini project example** (like fetching GitHub API data or calling a weather API)? It’s a great way to practice.
# Axios

Perfect — let’s go through a **complete, beginner-friendly tutorial to get started with Axios**, one of the most popular HTTP clients for Node.js and browsers.

You’ll learn **installation, basic GET/POST, error handling, interceptors, and configuration** — all step-by-step. 🚀

---

## 🧠 What Is Axios?

[Axios](https://axios-http.com) is a **promise-based HTTP client** that works both in:

* **Browser** (like `fetch`), and
* **Node.js** (using native HTTP module under the hood).

It makes HTTP calls **simpler and more powerful** than raw `fetch()`, with:
✅ automatic JSON parsing
✅ built-in timeouts
✅ request/response interceptors
✅ simpler error handling
✅ request cancellation

---

## ⚙️ Step 1: Project Setup

Create a new folder and initialize a Node project:

```bash
mkdir axios-tutorial
cd axios-tutorial
npm init -y
```

Install Axios:

```bash
npm install axios
```

---

## 📦 Step 2: Importing Axios

### If you use **ES Modules** (`"type": "module"` in `package.json` or `.mjs` files):

```js
import axios from "axios";
```

### If you use **CommonJS** (`.js` default in Node):

```js
const axios = require("axios");
```

---

## 🌐 Step 3: Basic GET Request

```js
import axios from "axios";

async function getData() {
  try {
    const response = await axios.get("https://jsonplaceholder.typicode.com/posts/1");
    console.log(response.data);
  } catch (error) {
    console.error("Error fetching data:", error.message);
  }
}

getData();
```

✅ Output:

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident...",
  "body": "quia et suscipit..."
}
```

---

## 📤 Step 4: Sending a POST Request

```js
import axios from "axios";

async function createPost() {
  try {
    const response = await axios.post("https://jsonplaceholder.typicode.com/posts", {
      title: "Hello from Axios",
      body: "This is a test post!",
      userId: 1,
    });

    console.log(response.data);
  } catch (error) {
    console.error("Error creating post:", error.message);
  }
}

createPost();
```

✅ Output:

```json
{
  "title": "Hello from Axios",
  "body": "This is a test post!",
  "userId": 1,
  "id": 101
}
```

---

## 🔧 Step 5: Using Query Parameters

Axios lets you pass query params easily via the `params` option:

```js
import axios from "axios";

async function getUserPosts() {
  const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
    params: { userId: 1 },
  });
  console.log(response.data);
}

getUserPosts();
```

✅ This sends:
`GET https://jsonplaceholder.typicode.com/posts?userId=1`

---

## ⚠️ Step 6: Handling Errors

Axios provides detailed error information, including:

* `error.response` (server returned non-2xx)
* `error.request` (no response received)
* `error.message` (network or config issues)

Example:

```js
import axios from "axios";

async function fetchData() {
  try {
    await axios.get("https://jsonplaceholder.typicode.com/invalid-endpoint");
  } catch (error) {
    if (error.response) {
      console.error("Server responded with:", error.response.status);
    } else if (error.request) {
      console.error("No response received:", error.request);
    } else {
      console.error("Error setting up request:", error.message);
    }
  }
}

fetchData();
```

---

## ⏱️ Step 7: Timeout and Headers

```js
import axios from "axios";

async function fetchWithTimeout() {
  try {
    const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
      timeout: 2000, // 2 seconds
      headers: {
        "Content-Type": "application/json",
        Authorization: "Bearer my-token",
      },
    });
    console.log(response.data);
  } catch (error) {
    console.error("Timeout or error:", error.message);
  }
}

fetchWithTimeout();
```

---

## ♻️ Step 8: Axios Instance (Reusable Config)

You can create a preconfigured Axios instance for consistent base URLs or headers.

```js
import axios from "axios";

const api = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
  timeout: 5000,
  headers: { "Content-Type": "application/json" },
});

async function getPost(id) {
  const response = await api.get(`/posts/${id}`);
  console.log(response.data);
}

getPost(1);
```

✅ Now `api` can be reused for all API calls in your app.

---

## 🧩 Step 9: Using Interceptors

Interceptors let you **log**, **modify**, or **authenticate** requests/responses globally.

```js
api.interceptors.request.use(
  (config) => {
    console.log("➡️ Request:", config.method.toUpperCase(), config.url);
    return config;
  },
  (error) => Promise.reject(error)
);

api.interceptors.response.use(
  (response) => {
    console.log("✅ Response received");
    return response;
  },
  (error) => {
    console.error("❌ Response error:", error.message);
    return Promise.reject(error);
  }
);

async function demo() {
  await api.get("/posts/1");
}
demo();
```

---

## 🚫 Step 10: Cancelling Requests

```js
import axios from "axios";

const controller = new AbortController();

async function cancelRequest() {
  const request = axios.get("https://httpbin.org/delay/5", {
    signal: controller.signal,
  });

  setTimeout(() => controller.abort(), 2000); // cancel after 2 seconds

  try {
    const response = await request;
    console.log(response.data);
  } catch (error) {
    if (axios.isCancel(error)) {
      console.log("Request cancelled");
    } else {
      console.error("Error:", error.message);
    }
  }
}

cancelRequest();
```

---

## 🧪 Step 11: Small Example Project

Here’s a mini example combining multiple features:

```js
import axios from "axios";

const api = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
});

api.interceptors.request.use((config) => {
  console.log(`[${config.method.toUpperCase()}] ${config.url}`);
  return config;
});

async function example() {
  const { data: users } = await api.get("/users");
  console.log(`Fetched ${users.length} users.`);

  const newPost = {
    title: "Summary Post",
    body: `We have ${users.length} users in the system.`,
    userId: 1,
  };

  const { data: post } = await api.post("/posts", newPost);
  console.log("Created post:", post);
}

example();
```

---

## 🧾 Summary

| Task           | Code                                  |
| -------------- | ------------------------------------- |
| Install        | `npm install axios`                   |
| GET request    | `axios.get(url)`                      |
| POST request   | `axios.post(url, data)`               |
| Query params   | `{ params: { key: value } }`          |
| Timeout        | `{ timeout: 2000 }`                   |
| Base URL       | `axios.create({ baseURL })`           |
| Interceptors   | `axios.interceptors.request/response` |
| Cancel request | `AbortController`                     |

---

Would you like me to extend this into a **mini real API project** (for example, using Axios to consume GitHub or OpenWeather API with reusable structure)? It’s a great next step.


# Node Fetch and Axios

Node Fetch and Axios are both popular choices for making HTTP requests in Node.js environments, but they differ in their origin, features, and usage patterns.

### Node Fetch:
- Native API: Node Fetch is an implementation of the browser's Fetch API for Node.js, available natively in Node.js v18 and later. This means no external installation is required in newer Node.js versions.
- Simplicity: It offers a more low-level approach to HTTP requests, providing granular control over the request and response handling.
- Manual Handling: Requires manual parsing of JSON responses (e.g., response.json()) and manual error handling for HTTP status codes outside the 200-299 range.
Request Cancellation: Uses AbortController for request cancellation.
### Axios:
- Third-party Library: Axios is a popular, promise-based HTTP client that needs to be installed via npm.
- Feature-rich: It provides a more convenient and feature-rich experience, including automatic JSON parsing, built-in request/response interceptors, and more comprehensive error handling.
- Automatic Handling: Automatically handles JSON parsing and rejects promises for HTTP status codes outside the 200-299 range, simplifying error management.
- Built-in Cancellation: Offers a built-in method for request cancellation.
- Browser and Node.js Support: Works seamlessly in both browser and Node.js environments.
### When to Choose Which:
- Choose Node Fetch if you prioritize a lightweight, native solution, need fine-grained control over requests, and are comfortable with manual handling of JSON parsing and error conditions. It's suitable for simple scripts or microservices where minimal dependencies are desired.
- Choose Axios if you need a more robust and feature-rich solution, value convenience and automatic handling of common tasks like JSON parsing and error management, and require features like interceptors or built-in cancellation. It's often preferred for larger, more complex applications or when working in environments that require cross-platform compatibility (browser and Node.js).

## References
1. https://stackify.com/getting-started-with-node-fetch/
2. 