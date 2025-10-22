# WOA

**Web-Oriented Architecture (WOA)** is an architectural style that extends the principles of **Service-Oriented Architecture (SOA)** to the **web environment**, focusing on simplicity, interoperability, and leveraging web standards like **HTTP, REST, and JSON**.

Here’s a breakdown of what WOA means and why it matters:

## Table of Contents
- [WOA](#woa)
  - [Table of Contents](#table-of-contents)
    - [🧩 **Definition**](#-definition)
    - [⚙️ **Core Principles**](#️-core-principles)
    - [🕸️ **WOA vs. SOA**](#️-woa-vs-soa)
    - [🧠 **Conceptual Stack**](#-conceptual-stack)
    - [🧰 **Common Technologies**](#-common-technologies)
    - [💡 **Benefits**](#-benefits)
    - [🚀 **Example**](#-example)
    - [🧭 **In summary**](#-in-summary)
  - [🧱 High-Level Overview](#-high-level-overview)
  - [📁 Example Project Structure](#-example-project-structure)
  - [⚙️ Layer by Layer Breakdown](#️-layer-by-layer-breakdown)
    - [1️⃣ **Model Layer (M)** — Domain Objects](#1️⃣-model-layer-m--domain-objects)
    - [2️⃣ **Controller Layer (C)** — Entry Point / API](#2️⃣-controller-layer-c--entry-point--api)
    - [3️⃣ **Business Layer (Service)** — Core Logic](#3️⃣-business-layer-service--core-logic)
    - [4️⃣ **Adapter Layer (Integration / Persistence)**](#4️⃣-adapter-layer-integration--persistence)
  - [🧠 Notes](#-notes)
  - [✅ Summary](#-summary)
  - [🧩 1. WOA Is *Stateless* by Principle](#-1-woa-is-stateless-by-principle)
  - [🔐 2. So What Is a “WOA Session ID”?](#-2-so-what-is-a-woa-session-id)
    - [✳️ Common Uses:](#️-common-uses)
  - [🧠 3. How It Typically Works](#-3-how-it-typically-works)
  - [⚙️ 4. Example in Practice (Telecom Case)](#️-4-example-in-practice-telecom-case)
  - [🧩 5. Implementation Detail (Java / Spring Example)](#-5-implementation-detail-java--spring-example)
  - [🚀 6. In Summary](#-6-in-summary)
    - [🧭 TL;DR](#-tldr)
  - [Videos](#videos)

---

### 🧩 **Definition**

**WOA (Web-Oriented Architecture)** is a lightweight, web-native subset of SOA designed to expose and consume services using standard web technologies.
It builds on the **open principles of the Web**—such as **URIs**, **HTTP verbs**, and **media types**—to enable distributed systems to interact easily across platforms.

---

### ⚙️ **Core Principles**

WOA embraces a few key ideas that make it distinct:

1. **Resource Orientation (RESTful design)**

   * Everything is treated as a **resource**, identified by a **URI**.
   * Operations are performed using standard **HTTP methods**:

     * `GET` (read), `POST` (create), `PUT/PATCH` (update), `DELETE` (remove).

2. **Use of Standard Web Protocols**

   * Communication happens via **HTTP/HTTPS**, with **JSON** or **XML** as data formats.
   * No custom transport or protocol layers (unlike SOAP or CORBA).

3. **Statelessness**

   * Each request from client to server contains all necessary information; no session state is stored on the server.

4. **Interoperability and Simplicity**

   * Any platform or language that can make HTTP requests can consume WOA services.

5. **Loose Coupling**

   * Services are independent and discoverable, promoting modularity and flexibility.

---

### 🕸️ **WOA vs. SOA**

| Aspect                | SOA (Service-Oriented Architecture) | WOA (Web-Oriented Architecture)    |
| --------------------- | ----------------------------------- | ---------------------------------- |
| **Protocol**          | Often SOAP, XML-RPC, JMS            | HTTP/HTTPS                         |
| **Data Format**       | XML-heavy (WSDL, SOAP)              | JSON, XML, YAML                    |
| **Complexity**        | Enterprise-grade, heavier           | Lightweight, web-friendly          |
| **Service Interface** | Described via WSDL                  | Discovered via URLs, OpenAPI, etc. |
| **Coupling**          | Tighter (more formal contracts)     | Looser (URI-based access)          |
| **Focus**             | Enterprise integration              | Web and mobile integration         |

---

### 🧠 **Conceptual Stack**

WOA can be visualized as part of a broader evolution:

> **EAI → SOA → WOA → Microservices → Serverless**

WOA paved the way for **microservices** and **API-first** architectures by encouraging small, reusable, web-native services.

---

### 🧰 **Common Technologies**

* **RESTful APIs**
* **JSON / XML**
* **OAuth 2.0 / JWT** for security
* **OpenAPI (Swagger)** for documentation
* **GraphQL** (as a modern complement)
* **HTTP/HTTPS**
* **AJAX / Fetch / WebSockets**

---

### 💡 **Benefits**

* Simpler integration with web and mobile clients
* Better scalability and performance (due to statelessness)
* Easy to deploy and evolve services independently
* Encourages API ecosystems (e.g., public developer APIs)

---

### 🚀 **Example**

A WOA service might expose:

```
GET  https://api.example.com/users/123
POST https://api.example.com/users
PUT  https://api.example.com/users/123
DELETE https://api.example.com/users/123
```

Each endpoint represents a **web-accessible resource**, using standard **HTTP verbs** and returning **JSON** responses.

---

### 🧭 **In summary**

**WOA** is essentially **SOA done the Web way** — a design philosophy that treats the web as the platform for integration.
It simplifies distributed systems by aligning with the **architecture of the Web itself** (HTTP, URIs, and resources).

---

## 🧱 High-Level Overview

We’ll use a **3-layer architecture**, common in Spring Boot or Java EE applications:

```
┌────────────────────────────┐
│        Presentation         │ ← Controller layer (REST, MVC)
├────────────────────────────┤
│          Business           │ ← Service layer (business rules)
├────────────────────────────┤
│         Data/Adapter        │ ← Repository, external adapters, integration
└────────────────────────────┘
```

---

## 📁 Example Project Structure

```
com.example.woaapp/
├── controller/
│   └── UserController.java
│
├── service/
│   ├── UserService.java
│   └── impl/
│       └── UserServiceImpl.java
│
├── adapter/
│   ├── UserRepository.java
│   ├── ExternalEmailAdapter.java
│   └── DatabaseConfig.java
│
├── model/
│   └── User.java
│
└── WoaAppApplication.java
```

---

## ⚙️ Layer by Layer Breakdown

### 1️⃣ **Model Layer (M)** — Domain Objects

```java
package com.example.woaapp.model;

public class User {
    private Long id;
    private String name;
    private String email;

    // Constructors
    public User() {}
    public User(Long id, String name, String email) {
        this.id = id; this.name = name; this.email = email;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

---

### 2️⃣ **Controller Layer (C)** — Entry Point / API

```java
package com.example.woaapp.controller;

import com.example.woaapp.model.User;
import com.example.woaapp.service.UserService;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping
    public List<User> getAll() {
        return service.getAllUsers();
    }

    @GetMapping("/{id}")
    public User getById(@PathVariable Long id) {
        return service.getUserById(id);
    }

    @PostMapping
    public User create(@RequestBody User user) {
        return service.createUser(user);
    }

    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        service.deleteUser(id);
    }
}
```

---

### 3️⃣ **Business Layer (Service)** — Core Logic

**Interface:**

```java
package com.example.woaapp.service;

import com.example.woaapp.model.User;
import java.util.List;

public interface UserService {
    List<User> getAllUsers();
    User getUserById(Long id);
    User createUser(User user);
    void deleteUser(Long id);
}
```

**Implementation:**

```java
package com.example.woaapp.service.impl;

import com.example.woaapp.model.User;
import com.example.woaapp.service.UserService;
import com.example.woaapp.adapter.UserRepository;
import com.example.woaapp.adapter.ExternalEmailAdapter;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserServiceImpl implements UserService {

    private final UserRepository repository;
    private final ExternalEmailAdapter emailAdapter;

    public UserServiceImpl(UserRepository repository, ExternalEmailAdapter emailAdapter) {
        this.repository = repository;
        this.emailAdapter = emailAdapter;
    }

    @Override
    public List<User> getAllUsers() {
        return repository.findAll();
    }

    @Override
    public User getUserById(Long id) {
        return repository.findById(id);
    }

    @Override
    public User createUser(User user) {
        User saved = repository.save(user);
        emailAdapter.sendWelcomeEmail(saved.getEmail());
        return saved;
    }

    @Override
    public void deleteUser(Long id) {
        repository.delete(id);
    }
}
```

---

### 4️⃣ **Adapter Layer (Integration / Persistence)**

**Repository (Adapter to DB):**

```java
package com.example.woaapp.adapter;

import com.example.woaapp.model.User;
import org.springframework.stereotype.Repository;
import java.util.*;

@Repository
public class UserRepository {
    private final Map<Long, User> db = new HashMap<>();
    private long counter = 1;

    public List<User> findAll() {
        return new ArrayList<>(db.values());
    }

    public User findById(Long id) {
        return db.get(id);
    }

    public User save(User user) {
        user.setId(counter++);
        db.put(user.getId(), user);
        return user;
    }

    public void delete(Long id) {
        db.remove(id);
    }
}
```

**External API Adapter (e.g., Email service):**

```java
package com.example.woaapp.adapter;

import org.springframework.stereotype.Component;

@Component
public class ExternalEmailAdapter {

    public void sendWelcomeEmail(String email) {
        // Simulate external API call
        System.out.println("Sending welcome email to " + email);
    }
}
```

---

## 🧠 Notes

* The **Controller** handles **HTTP requests** and delegates to the **Service layer**.
* The **Service** layer implements **business logic**, orchestrating data access and external calls.
* The **Adapter** layer isolates **external dependencies** (database, APIs, messaging).
* This makes the code **clean, modular, and testable**.

---

## ✅ Summary

| Layer          | Responsibility                  | Example Classes                          |
| -------------- | ------------------------------- | ---------------------------------------- |
| **Controller** | Handle web requests (REST, MVC) | `UserController`                         |
| **Service**    | Business logic, orchestration   | `UserService`, `UserServiceImpl`         |
| **Adapter**    | Integration with DBs, APIs      | `UserRepository`, `ExternalEmailAdapter` |
| **Model**      | Domain/data representation      | `User`                                   |

---

## 🧩 1. WOA Is *Stateless* by Principle

At its core, **WOA (Web-Oriented Architecture)** is built on **HTTP and REST principles**, which are **stateless**:

> Each request from client to server must contain all the information needed to understand and process it.
> The server does not store session context between requests.

So by default, **no session** (in the traditional “server session” sense) should be required.
Each call should be self-contained — authenticated (e.g. via token), and independent.

---

## 🔐 2. So What Is a “WOA Session ID”?

In some **WOA-based enterprise systems**, the term **“Session ID”** doesn’t mean a classic *HTTP server session* (like in old JSP/Servlet apps), but rather a **temporary transaction or context token** used for **correlation, audit, or multi-step interactions** between systems.

You can think of it as a **WOA correlation ID**, not a user session.

### ✳️ Common Uses:

| Purpose                              | Description                                                                                                          |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **Tracking**                         | Each request gets a unique ID so all logs and downstream calls can be correlated.                                    |
| **Security / Authorization Context** | Temporarily binds user credentials, scopes, or roles to a specific workflow.                                         |
| **Performance / Monitoring**         | Used by APM tools (like Dynatrace, AppDynamics) to trace distributed transactions.                                   |
| **Multi-step API orchestration**     | In complex workflows (e.g., telecom order management), a WOA Session ID groups multiple API calls under one process. |

---

## 🧠 3. How It Typically Works

Here’s what often happens in **WOA gateway or middleware layers** (like an API Gateway, ESB, or integration layer):

1. A client makes an initial request to the **WOA Gateway** (API façade).
2. The gateway authenticates the request (via OAuth2, SAML, etc.).
3. The gateway **generates a WOA Session ID** — often a UUID — and includes it in:

   * Response headers, e.g.

     ```
     X-WOA-Session-ID: 3e12f8a1-1a2b-46f2-8b83-ef812a25f741
     ```
   * Or passes it to backend systems in subsequent service calls.
4. Each downstream service logs and uses this ID for tracing and correlation.
5. The session is short-lived (e.g., only valid during that transaction chain).

So it’s **not stored in server memory** like an HTTP session — it’s more of a **context ID propagated along calls**.

---

## ⚙️ 4. Example in Practice (Telecom Case)

Let’s say a **WOA-based telecom API** handles a *portability request* (customer switching carriers):

```
POST /api/portability
Authorization: Bearer eyJhbGciOi...
X-Client-ID: retail-portal
```

* The **WOA Gateway** receives the call, authenticates it, and assigns a:

  ```
  X-WOA-Session-ID: 9f7c12e1-2d8a-47b9-bcc0-83021fae2d33
  ```
* That same session ID is used across:

  * Calls to CRM, billing, and regulatory systems
  * Log entries in multiple microservices
* Once the transaction completes, the ID expires — no persistent session.

---

## 🧩 5. Implementation Detail (Java / Spring Example)

You might see a filter or interceptor in the gateway layer like:

```java
@Component
public class WoaSessionFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String sessionId = UUID.randomUUID().toString();
        httpRequest.setAttribute("WOA_SESSION_ID", sessionId);
        
        // Add header for downstream services
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        httpResponse.setHeader("X-WOA-Session-ID", sessionId);

        chain.doFilter(httpRequest, httpResponse);
    }
}
```

This would let every request generate a **WOA Session ID** automatically — purely for tracking or context propagation.

---

## 🚀 6. In Summary

| Concept                | Classic Web Session                  | WOA Session ID                 |
| ---------------------- | ------------------------------------ | ------------------------------ |
| **Purpose**            | Maintain user state between requests | Correlation / context tracking |
| **Stored On**          | Server (in memory or cache)          | Request headers / logs only    |
| **Lifecycle**          | User login → logout                  | Request chain only             |
| **Stateful?**          | Yes                                  | No                             |
| **Aligned with REST?** | ❌ No                                 | ✅ Yes                          |

---

### 🧭 TL;DR

> **WOA Session ID** ≠ User session.
> It’s a **stateless tracking token** used across distributed service calls for **traceability, security, or auditing** within a WOA-based architecture.

---

## Videos
 * [WSO2Con Asia 2016 - Building Web Apps Using Web-Oriented Architecture](https://www.youtube.com/watch?v=pjTclCFuz0k)
	> [<img src="https://img.youtube.com/vi/pjTclCFuz0k/0.jpg" width="200">](https://www.youtube.com/watch?v=pjTclCFuz0k "WOA by WSO2 506 views 23 minutes")

 * [Day1 : Web oriented architecture](https://www.youtube.com/watch?v=vd0jBcvzNcw)
	> [<img src="https://img.youtube.com/vi/vd0jBcvzNcw/0.jpg" width="200">](https://www.youtube.com/watch?v=vd0jBcvzNcw "The first international workshop on The encounter between two worlds by Cloud Torus 145 views 45 minutes")