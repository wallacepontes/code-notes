# Migration - Switching to REST Best Practices

## Table of Contents
- [Migration - Switching to REST Best Practices](#migration---switching-to-rest-best-practices)
  - [Table of Contents](#table-of-contents)
  - [Steps to Migration](#steps-to-migration)
    - [1. Understanding the Context](#1-understanding-the-context)
    - [2. Why DDD for This Migration?](#2-why-ddd-for-this-migration)
    - [3. Steps to Apply DDD in the Node.js Migration](#3-steps-to-apply-ddd-in-the-nodejs-migration)
      - [Step 3.1: Identify Bounded Contexts](#step-31-identify-bounded-contexts)
      - [Step 3.2: Organize Project Structure](#step-32-organize-project-structure)
      - [Step 3.3: Define Domain Models and Aggregates](#step-33-define-domain-models-and-aggregates)
      - [Step 3.4: Implement Routes and Services](#step-34-implement-routes-and-services)
      - [Step 3.5: Global Exception Handling](#step-35-global-exception-handling)
      - [Step 3.6: Update OpenAPI Specification](#step-36-update-openapi-specification)
      - [Step 3.7: Serverless Deployment](#step-37-serverless-deployment)
      - [Step 3.8: CI/CD Pipeline](#step-38-cicd-pipeline)
    - [4. Handling SOA Integration](#4-handling-soa-integration)
    - [5. Addressing the Flat Structure](#5-addressing-the-flat-structure)
    - [6. Best Practices for DDD in Node.js](#6-best-practices-for-ddd-in-nodejs)
    - [7. Example: Migrating a Feature](#7-example-migrating-a-feature)
    - [8. Next Steps](#8-next-steps)

## Steps to Migration

Migrating a Java 8 project with a Web-Oriented Architecture (WOA) and 322 URIs from a flat, layered structure (Controller, Business, Adapters) to a Node.js-based serverless architecture while incorporating **Domain-Driven Design (DDD)** is a complex but rewarding task. Since you're working for a large telecom company in Brazil, deploying to AWS across multiple stages (dev, qa, uat, prod) with a serverless setup (likely using Serverless Framework or AWS SAM), and given that 90% of the APIs interact with a Service-Oriented Architecture (SOA) endpoint, I'll provide a structured approach to apply DDD principles during this migration. I'll draw on your familiarity with Java/Spring, address the flat structure's limitations, and tailor the solution to your Node.js and serverless context, incorporating global exception handling as discussed previously.

---

### 1. Understanding the Context
- **Current Java Project**:
  - **Architecture**: WOA with three layers (Controller, Business, Adapters), where each feature (e.g., `ListDebt`) has a corresponding class in each layer.
  - **Flat Structure**: Features are grouped by layer (e.g., all controllers together), not by business domain, despite spanning domains like self-service, sales, service, and credit.
  - **APIs**: 322 URIs, 90% of which proxy to an SOA endpoint, suggesting a facade or gateway pattern.
  - **Technology**: Java 8, likely using Spring MVC or a similar framework (e.g., Backbase or custom Servlet-based REST).

- **Target Node.js Project**:
  - **Serverless**: Deployed to AWS Lambda, using Express and OpenAPI (as per prior discussions).
  - **Goal**: Restructure into a DDD-based architecture to improve modularity, maintainability, and alignment with business domains.
  - **Challenges**:
    - Mapping flat, layer-based features to domain-driven modules.
    - Handling SOA interactions in a serverless context.
    - Maintaining global exception handling (akin to Spring’s `@ControllerAdvice`).
    - Ensuring compatibility with CI/CD pipelines and multi-stage deployments.

- **DDD Overview**:
  - DDD organizes code around **business domains** (e.g., self-service, sales) rather than technical layers, using concepts like **Bounded Contexts**, **Aggregates**, and **Entities**.
  - Spring parallel: Moving from a generic `@RestController` + `@Service` + `@Repository` structure to domain-specific modules (e.g., `sales` package with its own controllers, services, and models).
  - Benefits: Better alignment with business needs, easier maintenance, and scalability in serverless environments.

---

### 2. Why DDD for This Migration?
The flat structure (all controllers, services, or adapters grouped together) can lead to:
- **Tight Coupling**: Features across domains (e.g., self-service, credit) are intermingled, making changes risky.
- **Poor Maintainability**: With 322 URIs, a flat structure is hard to navigate and extend, especially for a telecom with complex domains.
- **SOA Dependency**: Since 90% of APIs call SOA endpoints, the flat structure may obscure domain-specific logic or error handling.

DDD addresses these by:
- Grouping code by **Bounded Contexts** (e.g., `Sales`, `SelfService`), isolating domain logic.
- Defining clear **Aggregates** for data and behavior (e.g., a `Customer` aggregate in `SelfService`).
- Simplifying serverless deployments by organizing Lambda functions by domain.

---

### 3. Steps to Apply DDD in the Node.js Migration
Here’s a step-by-step approach to restructure your Node.js project using DDD, tailored for serverless and SOA integration.

#### Step 3.1: Identify Bounded Contexts
- **Definition**: A Bounded Context is a specific business domain with its own models, language, and rules. For your telecom project, domains might include `SelfService`, `Sales`, `Service`, and `Credit`.
- **Process**:
  1. Analyze the 322 URIs to group them by business domain. For example:
     - `/woa/rest/Billing/v1/account/get` → `SelfService` (customer account management).
     - `/woa/rest/Sales/v1/order/create` → `Sales` (order processing).
     - `/woa/rest/Credit/v1/check` → `Credit` (credit evaluation).
  2. Map URIs to domains based on their purpose or SOA endpoint. Since 90% call SOA, check the SOA service contracts to confirm domain boundaries.
  3. Define 4-6 Bounded Contexts to avoid over-fragmentation (e.g., `SelfService`, `Sales`, `Service`, `Credit`, `Billing`, `CustomerSupport`).
- **Spring Parallel**: Refactoring a monolithic Spring app into domain-specific packages (e.g., `com.telecom.sales`, `com.telecom.selfservice`).

#### Step 3.2: Organize Project Structure
Restructure your Node.js project to group code by Bounded Context, not layers. Example directory structure:

```
serverless-openapi/
├── src/
│   ├── self-service/
│   │   ├── routes.js          // Express routes (like Controllers)
│   │   ├── services.js        // Business logic (like Services)
│   │   ├── adapters.js        // SOA calls (like Adapters)
│   │   ├── models.js          // Domain models (e.g., Customer, Account)
│   │   └── errors.js          // Domain-specific errors
│   ├── sales/
│   │   ├── routes.js
│   │   ├── services.js
│   │   ├── adapters.js
│   │   ├── models.js
│   │   └── errors.js
│   ├── credit/
│   │   ├── routes.js
│   │   ├── services.js
│   │   ├── adapters.js
│   │   ├── models.js
│   │   └── errors.js
│   ├── shared/
│   │   ├── errors.js          // Shared error classes (e.g., ApiError)
│   │   ├── logger.js          // Shared utilities (e.g., CloudWatch logging)
│   │   └── swagger.js         // OpenAPI spec generation
├── index.js                   // Main entry point
├── package.json
├── serverless.yml
├── .env
└── tests/
```

- **Why?**:
  - Each Bounded Context (`self-service`, `sales`, `credit`) is a module with its own routes, services, adapters, and models, like a Spring sub-package.
  - `shared/` contains cross-cutting concerns (e.g., global error handling, logging), like Spring’s `@ControllerAdvice` or utility classes.
  - Reduces the flat structure’s chaos by grouping related URIs (e.g., all `/woa/rest/Billing/*` in `self-service`).

#### Step 3.3: Define Domain Models and Aggregates
- **Models**: Define JavaScript classes or objects for domain entities (e.g., `Customer`, `Order`). Example in `self-service/models.js`:
  ```javascript
  class Customer {
    constructor(id, name, accountId) {
      this.id = id;
      this.name = name;
      this.accountId = accountId;
    }
  }
  module.exports = { Customer };
  ```
- **Aggregates**: Group related entities and logic. For example, a `Customer` aggregate in `SelfService` might include account details and billing operations.
- **SOA Integration**: Since 90% of APIs call SOA endpoints, treat the SOA service as an external system. Use adapters to map SOA responses to domain models.
  Example in `self-service/adapters.js`:
  ```javascript
  const axios = require('axios');
  const { Customer } = require('./models');

  async function getCustomerFromSOA(accountId) {
    const response = await axios.get(`http://soa-endpoint/Billing/v1/account/${accountId}`);
    return new Customer(response.data.id, response.data.name, accountId);
  }
  module.exports = { getCustomerFromSOA };
  ```

- **Spring Parallel**: Models are like `@Entity` classes, and adapters are like `@Repository` or REST clients (e.g., `RestTemplate`).

#### Step 3.4: Implement Routes and Services
- **Routes** (`self-service/routes.js`): Define Express routes for the Bounded Context, like Spring `@RestController`.
  ```javascript
  const express = require('express');
  const { getCustomer } = require('./services');
  const router = express.Router();

  /**
   * @openapi
   * /self-service/customer/{id}:
   *   get:
   *     summary: Get customer by ID
   *     parameters:
   *       - in: path
   *         name: id
   *         required: true
   *         schema:
   *           type: string
   *     responses:
   *       200:
   *         description: Customer details
   *         content:
   *           application/json:
   *             schema:
   *               type: object
   *               properties:
   *                 id: { type: string }
   *                 name: { type: string }
   *       404:
   *         description: Customer not found
   */
  router.get('/customer/:id', async (req, res, next) => {
    try {
      const customer = await getCustomer(req.params.id);
      res.json(customer);
    } catch (err) {
      next(err);
    }
  });

  module.exports = router;
  ```
- **Services** (`self-service/services.js`): Contain business logic, like Spring `@Service`.
  ```javascript
  const { getCustomerFromSOA } = require('./adapters');
  const { ApiError } = require('../shared/errors');

  async function getCustomer(accountId) {
    const customer = await getCustomerFromSOA(accountId);
    if (!customer) {
      throw new ApiError(404, 'Customer not found');
    }
    return customer;
  }
  module.exports = { getCustomer };
  ```

- **Why?**:
  - Routes handle HTTP requests, services encapsulate business logic, and adapters manage SOA calls, mirroring the Java project’s layers but scoped to a Bounded Context.
  - Errors are thrown using the `ApiError` class from your global exception handling setup.

#### Step 3.5: Global Exception Handling
Reuse the global error-handling middleware from the previous discussion, placed in `shared/errors.js` and included in `index.js`:
```javascript
const { ApiError } = require('./shared/errors');

app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';
  res.status(statusCode).json({
    error: {
      status: statusCode,
      message,
      details: err.details || null,
    },
  });
});
```

- **Domain-Specific Errors**: Define custom errors per Bounded Context in `errors.js` (e.g., `self-service/errors.js`):
  ```javascript
  class CustomerNotFoundError extends ApiError {
    constructor(accountId) {
      super(404, `Customer with account ${accountId} not found`);
    }
  }
  module.exports = { CustomerNotFoundError };
  ```

- **Spring Parallel**: Like `@ExceptionHandler` for specific exceptions in a `@ControllerAdvice`.

#### Step 3.6: Update OpenAPI Specification
Group OpenAPI specs by Bounded Context, either in separate YAML files or JSDoc comments. Example in `shared/swagger.js`:
```javascript
const swaggerJsdoc = require('swagger-jsdoc');

const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: { title: 'Telecom API', version: '1.0.0' },
    servers: [{ url: `/${process.env.STAGE || 'dev'}` }],
  },
  apis: ['./src/self-service/routes.js', './src/sales/routes.js', './src/credit/routes.js'],
};
module.exports = swaggerJsdoc(swaggerOptions);
```

- **Why?**: Organizes the 322 URIs into domain-specific sections in Swagger UI, like Springdoc grouping endpoints by tags.

#### Step 3.7: Serverless Deployment
- **Lambda Functions**: Deploy each Bounded Context as a separate Lambda function or group related routes into one function for simplicity.
  Example `serverless.yml`:
  ```yaml
  service: telecom-api
  provider:
    name: aws
    runtime: nodejs20.x
    stage: ${opt:stage, 'dev'}
  functions:
    selfService:
      handler: src/self-service/handler.handler
      events:
        - http:
            path: /self-service/{proxy+}
            method: ANY
    sales:
      handler: src/sales/handler.handler
      events:
        - http:
            path: /sales/{proxy+}
            method: ANY
  plugins:
    - serverless-offline
  ```
  Create `handler.js` per Bounded Context (e.g., `self-service/handler.js`):
  ```javascript
  const serverless = require('serverless-http');
  const express = require('express');
  const router = require('./routes');
  const app = express();
  app.use('/self-service', router);
  module.exports.handler = serverless(app);
  ```

- **AWS SAM Alternative**: If Serverless Framework’s licensing is an issue, use SAM:
  ```yaml
  AWSTemplateFormatVersion: '2010-09-09'
  Transform: AWS::Serverless-2016-10-31
  Resources:
    SelfServiceFunction:
      Type: AWS::Serverless::Function
      Properties:
        CodeUri: ./src/self-service
        Handler: handler.handler
        Runtime: nodejs20.x
        Events:
          Api:
            Type: Api
            Properties:
              Path: /self-service/{proxy+}
              Method: ANY
  ```

- **Why?**: Each Bounded Context can scale independently, and serverless functions align with DDD’s isolation.

#### Step 3.8: CI/CD Pipeline
- Use AWS CodePipeline or GitHub Actions to deploy to `dev`, `qa`, `uat`, and `prod` stages.
- Example GitHub Actions workflow:
  ```yaml
  name: Deploy to AWS
  on:
    push:
      branches:
        - main
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - uses: actions/setup-node@v3
          with:
            node-version: '20'
        - run: npm ci
        - run: npm test
        - run: npm run deploy
          env:
            AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
            AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            STAGE: dev
  ```

---

### 4. Handling SOA Integration
Since 90% of APIs call SOA endpoints:
- **Adapters**: Encapsulate SOA calls in `adapters.js` per Bounded Context, using `axios` or `node-fetch`.
- **Error Handling**: Map SOA errors to domain-specific errors (e.g., `CustomerNotFoundError`).
- **Caching**: Use AWS ElastiCache or DynamoDB to cache SOA responses, reducing latency in serverless.
- **Spring Parallel**: Like using `RestTemplate` or `WebClient` in Spring to call external services, with `@ExceptionHandler` for SOA errors.

Example:
```javascript
// self-service/adapters.js
const axios = require('axios');
const { CustomerNotFoundError } = require('./errors');

async function getCustomerFromSOA(accountId) {
  try {
    const response = await axios.get(`http://soa-endpoint/Billing/v1/account/${accountId}`);
    return response.data;
  } catch (err) {
    if (err.response?.status === 404) {
      throw new CustomerNotFoundError(accountId);
    }
    throw new ApiError(500, 'SOA service error', err.message);
  }
}
```

---

### 5. Addressing the Flat Structure
The Java project’s flat structure (all controllers together, etc.) is a technical layering antipattern for DDD. By:
- Grouping URIs into Bounded Contexts (e.g., `/self-service/*`, `/sales/*`).
- Defining domain-specific models, services, and errors.
- Deploying as modular Lambda functions.
You avoid replicating the flat structure’s issues (e.g., tangled dependencies, hard-to-maintain code).

---

### 6. Best Practices for DDD in Node.js
- **Keep Bounded Contexts Small**: Limit each context to a cohesive set of URIs (e.g., 50-100 per context for 322 URIs).
- **Use TypeScript (Optional)**: For stronger typing, like Java’s type safety:
  ```bash
  npm install -D typescript ts-node @types/node @types/express
  ```
  Rename files to `.ts` and define interfaces (e.g., `interface Customer { id: string; name: string; }`).
- **Test per Context**: Write unit tests for each Bounded Context (e.g., `self-service/tests/`), like Spring’s `@Test` classes.
- **Monitor SOA Calls**: Log SOA response times in CloudWatch to optimize performance.
- **Iterate**: Migrate one Bounded Context at a time (e.g., start with `SelfService`), testing locally with `serverless offline` or `sam local start-api`.

---

### 7. Example: Migrating a Feature
For a URI like `/woa/rest/Billing/v1/account/get`:
1. **Identify Context**: `SelfService`.
2. **Define Route**:
   ```javascript
   // self-service/routes.js
   router.get('/account/:id', async (req, res, next) => {
     try {
       const account = await getAccount(req.params.id);
       res.json(account);
     } catch (err) {
       next(err);
     }
   });
   ```
3. **Define Service**:
   ```javascript
   // self-service/services.js
   async function getAccount(id) {
     const account = await getAccountFromSOA(id);
     if (!account) throw new CustomerNotFoundError(id);
     return account;
   }
   ```
4. **Define Adapter**:
   ```javascript
   // self-service/adapters.js
   async function getAccountFromSOA(id) {
     const response = await axios.get(`http://soa-endpoint/Billing/v1/account/${id}`);
     return response.data;
   }
   ```
5. **Document in OpenAPI**:
   ```javascript
   /**
    * @openapi
    * /self-service/account/{id}:
    *   get:
    *     summary: Get account details
    *     parameters:
    *       - in: path
    *         name: id
    *         required: true
    *         schema:
    *           type: string
    *     responses:
    *       200:
    *         description: Account details
    *       404:
    *         description: Account not found
    */
   ```

---

### 8. Next Steps
- **Start Small**: Pick one domain (e.g., `SelfService`) and migrate its URIs (e.g., 50-60 billing-related URIs).
- **Validate with Team**: Ensure SOA endpoints are accessible and documented for mapping.
- **Use SAM if Needed**: If Serverless Framework’s licensing is an issue, switch to AWS SAM (I can provide a `template.yaml`).
- **Test Locally**: Use `serverless offline` or `sam local start-api` for each Bounded Context.
- **CI/CD**: Ensure your pipeline deploys each context independently (e.g., separate Lambda functions per context).

