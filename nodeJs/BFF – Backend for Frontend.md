# BFF – Backend for Frontend

---

## Table of Contents

- [BFF – Backend for Frontend](#bff--backend-for-frontend)
  - [Table of Contents](#table-of-contents)
  - [🚀 **Getting Started with BFF (Backend for Frontend)**](#-getting-started-with-bff-backend-for-frontend)
    - [1. **What is a BFF? – The Short Version**](#1-what-is-a-bff--the-short-version)
    - [2. **When Should You Use a BFF?**](#2-when-should-you-use-a-bff)
      - [✔️ You have multiple frontend types](#️-you-have-multiple-frontend-types)
      - [✔️ The frontend needs **specific data formatting**](#️-the-frontend-needs-specific-data-formatting)
      - [✔️ The frontend is overloaded with business logic](#️-the-frontend-is-overloaded-with-business-logic)
      - [✔️ Different frontends authenticate differently](#️-different-frontends-authenticate-differently)
      - [✔️ You want a safer architecture](#️-you-want-a-safer-architecture)
    - [3. **Typical BFF Architecture (Simple Diagram)**](#3-typical-bff-architecture-simple-diagram)
    - [4. **Responsibilities of a BFF**](#4-responsibilities-of-a-bff)
      - [🎯 4.1 What the BFF **should** handle](#-41-what-the-bff-should-handle)
      - [❌ 4.2 What the BFF **should NOT** handle](#-42-what-the-bff-should-not-handle)
    - [5. **Choosing a Tech Stack for BFF**](#5-choosing-a-tech-stack-for-bff)
      - [💻 Common Stacks](#-common-stacks)
    - [6. **How to Build Your First BFF (Hands-On)**](#6-how-to-build-your-first-bff-hands-on)
      - [Step 1 — Create project](#step-1--create-project)
      - [Step 2 — Basic server](#step-2--basic-server)
      - [Step 3 — Create a simple aggregated endpoint](#step-3--create-a-simple-aggregated-endpoint)
      - [Step 4 — Add client-specific authentication](#step-4--add-client-specific-authentication)
      - [Step 5 — Add simple caching (optional)](#step-5--add-simple-caching-optional)
    - [7. Best Practices for BFF](#7-best-practices-for-bff)
      - [✔️ One BFF per frontend type](#️-one-bff-per-frontend-type)
      - [✔️ Keep it thin](#️-keep-it-thin)
      - [✔️ Version your BFF](#️-version-your-bff)
      - [✔️ Add observability](#️-add-observability)
      - [✔️ Feature flags](#️-feature-flags)
      - [✔️ Include security logic](#️-include-security-logic)
    - [8. Anti-Patterns (Avoid These)](#8-anti-patterns-avoid-these)
    - [9. BFF vs API Gateway vs Backend](#9-bff-vs-api-gateway-vs-backend)
    - [10. Example Architecture With All Layers](#10-example-architecture-with-all-layers)
    - [11. Summary](#11-summary)

---

## 🚀 **Getting Started with BFF (Backend for Frontend)**

Here’s a **clear, practical, real-world tutorial** to get you started with **BFF – Backend for Frontend**. This is designed for architects and developers who want to understand *why*, *when*, and *how* to implement a BFF architecture.


A practical, architecture-first guide

---

### 1. **What is a BFF? – The Short Version**

A **Backend for Frontend (BFF)** is a backend layer **dedicated to a specific client experience** (web, mobile, smartwatch, kiosk, partner portal, etc.).

👉 Instead of one backend serving all clients, each client gets **its own optimized backend**.

This solves the problems of:

* different UI needs
* different data shaping
* different performance requirements
* different authentication mechanisms
* reducing complexity in frontend
* reducing backend coupling

---

### 2. **When Should You Use a BFF?**

You need a BFF when:

#### ✔️ You have multiple frontend types

* Web app
* Mobile app
* IoT device
* Internal portal / external customer portal

#### ✔️ The frontend needs **specific data formatting**

Example: Mobile needs lighter responses and fewer requests.

#### ✔️ The frontend is overloaded with business logic

➡️ Push that logic into the BFF.

#### ✔️ Different frontends authenticate differently

* Web might use cookies/sessions
* Mobile might use OAuth2 + JWT
* Kiosk might use client certificates

#### ✔️ You want a safer architecture

BFF acts as a **security layer** shielding your internal microservices.

---

### 3. **Typical BFF Architecture (Simple Diagram)**

```
        Web Frontend ——┐
                        ├——> Web BFF ——> API Gateway / Microservices
Mobile Frontend ——┐     │
                  └——> Mobile BFF ——> API Gateway / Microservices
```

Points to note:

* Each BFF **consumes** your domain microservices.
* The BFF **aggregates**, **transforms**, **secure**, and **simplifies** data.
* BFF is not a database owner — it is an orchestration layer.

---

### 4. **Responsibilities of a BFF**

#### 🎯 4.1 What the BFF **should** handle

* Authentication/Authorization specific to that client
* Token refresh mechanism
* Calling multiple microservices and merging data
* UI/UX-specific transformations
* Caching (client-aware)
* Throttling, rate-limit per client
* Feature toggles per frontend version
* A/B testing integration
* Input/output validation

#### ❌ 4.2 What the BFF **should NOT** handle

* Deep domain logic
* Database persistence
* Cross-channel business rules
* Shared business logic between frontends
* Heavy batch processing

---

### 5. **Choosing a Tech Stack for BFF**

#### 💻 Common Stacks

| Frontend Type         | Recommended BFF Tech           |
| --------------------- | ------------------------------ |
| Web React/Vue/Angular | Node.js + Express/NestJS       |
| Mobile (iOS/Android)  | Kotlin/Swift server or Node.js |
| Enterprise Systems    | Java (Spring Boot)             |
| High-performance      | Go / Rust                      |

Most popular and productive: **Node.js + NestJS** and **Java + Spring Boot**.

---

### 6. **How to Build Your First BFF (Hands-On)**

We'll use **Node.js + Express** for simplicity, but the same principles apply to Java/Spring.

---

#### Step 1 — Create project

```bash
mkdir web-bff
cd web-bff
npm init -y
npm install express axios cors
```

---

#### Step 2 — Basic server

```js
const express = require("express");
const axios = require("axios");
const app = express();

app.use(express.json());
app.use(require("cors")());
```

---

#### Step 3 — Create a simple aggregated endpoint

Imagine the frontend needs user info + orders summary on one page.

Without BFF:

* Frontend makes 2–5 API calls.
* Needs orchestration logic.
* Handling errors is complex.

With BFF:

```js
app.get("/me/dashboard", async (req, res) => {
    try {
        const user = await axios.get("https://api.company.com/users/me");
        const orders = await axios.get("https://api.company.com/orders?limit=5");

        res.json({
            user: user.data,
            recentOrders: orders.data,
            totalOrders: orders.data.length
        });
    } catch (err) {
        res.status(500).json({ error: "Failed to load dashboard data" });
    }
});
```

➡️ The frontend now calls **only this one endpoint**.

---

#### Step 4 — Add client-specific authentication

Example: Use a session token from web frontend.

```js
app.use((req, res, next) => {
    const token = req.headers["x-session-token"];
    if (!token) return res.status(401).send("No session token");
    
    req.userToken = token;
    next();
});
```

Then forward token to microservices:

```js
axios.get(url, {
    headers: { Authorization: `Bearer ${req.userToken}` }
});
```

---

#### Step 5 — Add simple caching (optional)

```js
const cache = new Map();

app.get("/products", async (req, res) => {
    if (cache.has("products")) {
        return res.json(cache.get("products"));
    }

    const products = await axios.get("https://api.company.com/products");
    cache.set("products", products.data);
    res.json(products.data);
});
```

---

### 7. Best Practices for BFF

#### ✔️ One BFF per frontend type

Web BFF != Mobile BFF.

#### ✔️ Keep it thin

No heavy business rules.

#### ✔️ Version your BFF

Never break older app versions.

#### ✔️ Add observability

Logs + tracing + metrics.

#### ✔️ Feature flags

NB: Helps with soft releases.

#### ✔️ Include security logic

* Input validation
* Output sanitization
* Rate limit
* Access control

---

### 8. Anti-Patterns (Avoid These)

❌ One BFF shared by all apps
→ This becomes a monolith.

❌ Putting business logic inside the BFF
→ Leads to duplication between web/mobile.

❌ Treating BFF like an API Gateway
→ BFF is **per client**, gateway is **for all clients**.

❌ Making BFF call the database directly
→ You break microservice boundaries.

---

### 9. BFF vs API Gateway vs Backend

| Layer             | Purpose                            | Scope         |
| ----------------- | ---------------------------------- | ------------- |
| **Frontend**      | UI/UX                              | Client device |
| **BFF**           | Tailored backend for that frontend | Per frontend  |
| **API Gateway**   | Security, throttling, routing      | All clients   |
| **Microservices** | Business capabilities              | Domain-based  |

Think of BFF as a **UX-specific backend**, not infrastructure.

---

### 10. Example Architecture With All Layers

```
[Web App] ——> [Web BFF] ——> [API Gateway] ——> [User Service]
                                             ——> [Order Service]
                                             ——> [Billing Service]

[Mobile App] ——> [Mobile BFF] ——> [API Gateway] ——> Same services
```

---

### 11. Summary

A BFF is the best approach when:

* You have multiple frontends with different needs
* You want to reduce frontend complexity
* You want to isolate UI from backend structure
* You want better performance on specific frontends
* You want safer APIs with client-localized logic

BFFs make your architecture cleaner, more scalable, and easier to evolve.

---

Next:

- ✅ A BFF architecture diagram in PlantUML
- ✅ A Spring Boot BFF example
- ✅ A NestJS BFF example
- ✅ A list of what to configure in Apigee/AWS API Gateway when using BFFs
- ✅ A real-world design for telecom or banking BFFs

