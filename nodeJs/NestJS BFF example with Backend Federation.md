# NestJS BFF example with Backend Federation

---

## Table of Contents

- [NestJS BFF example with Backend Federation](#nestjs-bff-example-with-backend-federation)
  - [Table of Contents](#table-of-contents)
  - [Real-world NestJS example](#real-world-nestjs-example)
  - [🏗️ **Architecture Overview**](#️-architecture-overview)
    - [🛠️ **1. Create the BFF Project**](#️-1-create-the-bff-project)
    - [⚙️ **2. Configure a Federated API Layer**](#️-2-configure-a-federated-api-layer)
    - [🧱 **3. BFF “Dashboard” Aggregation Endpoint**](#-3-bff-dashboard-aggregation-endpoint)
    - [🔐 **4. Add BFF-Specific Authentication**](#-4-add-bff-specific-authentication)
    - [🌐 **5. Environment Setup**](#-5-environment-setup)
    - [🏁 **6. Final BFF Module Setup**](#-6-final-bff-module-setup)
    - [🖥️ **7. Example BFF Output**](#️-7-example-bff-output)
  - [🎯 **BFF Responsibilities Shown Here**](#-bff-responsibilities-shown-here)
  - [🧩 **Backend Federation in This Example**](#-backend-federation-in-this-example)
  - [Want More?](#want-more)

---

## Real-world NestJS example

Below is a **clean, real-world NestJS example** that combines:

✅ **BFF (Backend for Frontend)**
with
✅ **Backend Federation (multiple domain services unified behind the BFF)**

This architecture is ideal when:

* You have several backend services (User, Orders, Billing, etc.)
* You want a **single optimized API** for the frontend
* But you **don’t want a monolithic API layer**

---

## 🏗️ **Architecture Overview**

```
                  ┌─────────────────────┐
   React Web  ───▶│  Web BFF (NestJS)   │──┐
                  └─────────────────────┘  │
                         ▲                 │ federated calls
                         │                 ▼
           ┌────────────────────┐   ┌────────────────────┐
           │ User Service       │   │ Orders Service     │
           └────────────────────┘   └────────────────────┘
                     ▲                     ▲
           ┌────────────────────┐   ┌────────────────────┐
           │ Billing Service    │   │ Inventory Service   │
           └────────────────────┘   └────────────────────┘
```

👉 The **BFF** exposes one frontend-friendly API.
👉 Each backend remains independent (**federation**).
👉 The BFF performs **aggregation + translation**, not business logic.

---

### 🛠️ **1. Create the BFF Project**

```bash
nest new web-bff
cd web-bff
npm install axios
```

---

### ⚙️ **2. Configure a Federated API Layer**

Your BFF will call multiple backend services in a **federated way**.

Create `src/federation/federated-api.service.ts`:

```ts
import { Injectable } from '@nestjs/common';
import axios from 'axios';

@Injectable()
export class FederatedApiService {
  private services = {
    user: process.env.USER_SERVICE_URL,
    orders: process.env.ORDER_SERVICE_URL,
    billing: process.env.BILLING_SERVICE_URL,
  };

  async getUser(id: string) {
    return axios.get(`${this.services.user}/users/${id}`).then(r => r.data);
  }

  async getOrdersByUser(id: string) {
    return axios.get(`${this.services.orders}/orders/by-user/${id}`).then(r => r.data);
  }

  async getBillingSummary(id: string) {
    return axios.get(`${this.services.billing}/billing/summary/${id}`).then(r => r.data);
  }
}
```

This is **federation**:
Each backend is independent, but the BFF exposes one contract.

---

### 🧱 **3. BFF “Dashboard” Aggregation Endpoint**

This is UI-specific and classic BFF logic:

`src/dashboard/dashboard.controller.ts`:

```ts
import { Controller, Get, Req } from '@nestjs/common';
import { DashboardService } from './dashboard.service';

@Controller('dashboard')
export class DashboardController {
  constructor(private readonly dashboard: DashboardService) {}

  @Get('me')
  async getMyDashboard(@Req() req) {
    return this.dashboard.getDashboardForUser(req.user.id);
  }
}
```

And the **service**:

`src/dashboard/dashboard.service.ts`:

```ts
import { Injectable } from '@nestjs/common';
import { FederatedApiService } from '../federation/federated-api.service';

@Injectable()
export class DashboardService {
  constructor(private readonly api: FederatedApiService) {}

  async getDashboardForUser(userId: string) {
    const [user, orders, billing] = await Promise.all([
      this.api.getUser(userId),
      this.api.getOrdersByUser(userId),
      this.api.getBillingSummary(userId),
    ]);

    return {
      user,
      recentOrders: orders.slice(0, 5),
      billingSummary: billing,
    };
  }
}
```

🔥 **This is BFF logic**:

* Aggregates 3 backend services
* Formats data for frontend
* Adds UX optimization (slice(0,5))
* No business rules

---

### 🔐 **4. Add BFF-Specific Authentication**

Intercept requests and forward tokens to backend services.

`src/auth/session.middleware.ts`:

```ts
import { Injectable, NestMiddleware } from '@nestjs/common';

@Injectable()
export class SessionMiddleware implements NestMiddleware {
  use(req: any, res: any, next: () => void) {
    const token = req.headers['x-session-token'];
    if (!token) return res.status(401).json({ error: 'Session token missing' });

    req.user = { id: '123', token }; // In real-life: decode the session/JWT
    next();
  }
}
```

Then apply to the app:

```ts
import { MiddlewareConsumer, Module } from '@nestjs/common';
import { SessionMiddleware } from './auth/session.middleware';

@Module({
  // ...
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(SessionMiddleware).forRoutes('*');
  }
}
```

---

### 🌐 **5. Environment Setup**

`.env`:

```
USER_SERVICE_URL=http://localhost:3001
ORDER_SERVICE_URL=http://localhost:3002
BILLING_SERVICE_URL=http://localhost:3003
```

These are your **federated backend services**.

---

### 🏁 **6. Final BFF Module Setup**

`src/app.module.ts`:

```ts
import { Module } from '@nestjs/common';
import { FederatedApiService } from './federation/federated-api.service';
import { DashboardService } from './dashboard/dashboard.service';
import { DashboardController } from './dashboard/dashboard.controller';

@Module({
  imports: [],
  controllers: [DashboardController],
  providers: [FederatedApiService, DashboardService],
})
export class AppModule {}
```

---

### 🖥️ **7. Example BFF Output**

Frontend hits:

```
GET /dashboard/me
```

Response:

```json
{
  "user": { "id": "123", "name": "Wallace Pontes" },
  "recentOrders": [
    { "id": "A1", "total": 200 },
    { "id": "A2", "total": 350 }
  ],
  "billingSummary": {
    "pending": 0,
    "nextInvoice": "2025-12-10"
  }
}
```

This is **clean, frontend-optimized, aggregated**.

---

## 🎯 **BFF Responsibilities Shown Here**

✔ One endpoint for UI
✔ Aggregates several backend services
✔ Hides internal backend topology
✔ Applies UI-specific transformations
✔ Caches and fuses data
✔ Simplifies frontend effort
✔ Centralizes frontend-specific auth logic

---

## 🧩 **Backend Federation in This Example**

* The BFF **does not own any domain**.
* Each backend service keeps its own API.
* The BFF composes them into a unified experience.
* No monolithic aggregation backend — just UI-specific integration.

Federation = **distributed services, unified API surface**.

---

## Want More?

I can provide:

📌 A **GraphQL Federation version** of the same BFF (Apollo Federation + NestJS)
📌 A version using **RxJS** for smarter parallel federation
📌 A version using **Circuit Breakers (Resilience4JS)**
📌 A version using **Redis caching**
📌 A real-world **telecom OSS/BSS federated BFF** example

Just tell me!
