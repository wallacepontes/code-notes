# Migration - Switching to NodeJS Event-Driven Architecture

## Table of Contents
- [Migration - Switching to NodeJS Event-Driven Architecture](#migration---switching-to-nodejs-event-driven-architecture)
  - [Table of Contents](#table-of-contents)
  - [Node.js Event-Driven Architecture](#nodejs-event-driven-architecture)
    - [1. Node.js Event-Driven Architecture vs. Traditional Architecture](#1-nodejs-event-driven-architecture-vs-traditional-architecture)
      - [1.1. Understanding Event-Driven Architecture in Node.js](#11-understanding-event-driven-architecture-in-nodejs)
      - [1.2. Considerations for Your Node.js Project](#12-considerations-for-your-nodejs-project)
    - [2. Handling Apigee Token Management](#2-handling-apigee-token-management)
      - [2.1. Apigee Token Workflow](#21-apigee-token-workflow)
      - [2.2. Implementation in Node.js/TypeScript](#22-implementation-in-nodejstypescript)
    - [3. Project Structure with DDD, TypeScript, and Apigee](#3-project-structure-with-ddd-typescript-and-apigee)
    - [4. Applying Event-Driven Architecture in Your Project](#4-applying-event-driven-architecture-in-your-project)
    - [5. Handling the 322 URIs](#5-handling-the-322-uris)
    - [6. Best Practices](#6-best-practices)
    - [7. Example: Full Implementation for `SelfService`](#7-example-full-implementation-for-selfservice)
    - [8. Next Steps](#8-next-steps)

## Node.js Event-Driven Architecture

Given your project involves migrating a Java 8 WOA (Web-Oriented Architecture) application with 322 URIs to a serverless Node.js architecture for a large telecom company in Brazil, and you're incorporating **Domain-Driven Design (DDD)**, **TypeScript**, and **Apigee** as an API gateway to mediate access to SOA endpoints, I'll address two key aspects:

1. How Node.js's **event-driven architecture** differs from traditional architectures (like Java/Spring's thread-based model) and how to leverage it in your serverless project.
2. How to handle **Apigee token management** (including expired tokens) in your Node.js application.

I'll keep the explanation beginner-friendly, draw parallels to your Java/Spring experience, and align with your serverless setup (using Serverless Framework or AWS SAM), DDD structure, and global exception handling. I'll also provide practical examples tailored to your project’s needs, including TypeScript and Apigee integration.

---

### 1. Node.js Event-Driven Architecture vs. Traditional Architecture

#### 1.1. Understanding Event-Driven Architecture in Node.js
Node.js uses an **event-driven, non-blocking I/O model**, which is fundamentally different from the **thread-based, synchronous model** common in Java/Spring applications. Here’s a breakdown:

- **Node.js Event-Driven Model**:
  - **Event Loop**: Node.js runs on a single-threaded event loop, handling I/O operations (e.g., HTTP requests, database calls) asynchronously using callbacks, Promises, or `async/await`. Events (e.g., incoming HTTP requests) are queued and processed one at a time.
  - **Non-Blocking I/O**: Operations like calling an SOA endpoint don’t block the thread. Instead, Node.js delegates them to the operating system’s I/O handlers and continues processing other events, invoking callbacks when results are ready.
  - **Key Components**:
    - **EventEmitter**: A core Node.js module for emitting and listening to events (e.g., `http.createServer` emits a `request` event).
    - **Callbacks/Promises**: Handle asynchronous results (e.g., `axios.get().then()`).
    - **Libuv**: The underlying C library managing the event loop and thread pool for I/O tasks.
  - **Example**: An Express route handling an HTTP request:
    ```javascript
    app.get('/hello', async (req, res) => {
      const result = await someAsyncOperation(); // Non-blocking
      res.json(result);
    });
    ```

- **Traditional Architecture (Java/Spring)**:
  - **Thread-Based**: Spring MVC (e.g., running on Tomcat) uses a thread-per-request model. Each HTTP request gets a dedicated thread from a thread pool, which handles the request synchronously (or with explicit async mechanisms like `@Async` or `CompletableFuture`).
  - **Blocking I/O**: Operations like REST calls or database queries block the thread until complete, unless explicitly made asynchronous.
  - **Example**:
    ```java
    @RestController
    public class MyController {
        @GetMapping("/hello")
        public ResponseEntity<String> hello() {
            String result = restTemplate.getForObject("http://soa-endpoint", String.class); // Blocks thread
            return ResponseEntity.ok(result);
        }
    }
    ```

- **Key Differences**:
  | **Aspect**                | **Node.js (Event-Driven)**                     | **Java/Spring (Traditional)**                  |
  |---------------------------|-----------------------------------------------|-----------------------------------------------|
  | **Execution Model**       | Single-threaded, event loop                   | Multi-threaded, thread-per-request             |
  | **I/O Handling**          | Non-blocking, async (callbacks/Promises)      | Blocking (unless using `@Async` or WebFlux)    |
  | **Scalability**           | Handles many concurrent requests with low memory | Thread pool limits concurrency; higher memory  |
  | **Serverless Fit**        | Ideal for short-lived Lambda functions        | Less suited; heavier for serverless           |

- **Serverless Context**: In AWS Lambda, Node.js’s event-driven model aligns perfectly:
  - Lambda functions are triggered by events (e.g., API Gateway HTTP requests).
  - The event loop processes each request efficiently, minimizing cold start latency.
  - No need to manage threads, as Lambda scales automatically.

#### 1.2. Considerations for Your Node.js Project
To leverage Node.js’s event-driven architecture in your serverless, DDD-based project:

1. **Embrace Asynchronous Programming**:
   - Use `async/await` for SOA calls via Apigee, as most I/O operations (e.g., HTTP requests) are async in Node.js.
   - Example (TypeScript, in `self-service/adapters.ts`):
     ```typescript
     import axios from 'axios';
     import { Customer } from './models';

     export async function getCustomerFromApigee(accountId: string): Promise<Customer> {
       const response = await axios.get(`https://apigee-endpoint/self-service/account/${accountId}`, {
         headers: { Authorization: `Bearer ${await getApigeeToken()}` },
       });
       return new Customer(response.data.id, response.data.name);
     }
     ```
     - **Spring Parallel**: Replace `RestTemplate`’s blocking calls with `WebClient` in Spring WebFlux for async behavior.

2. **Organize by Events in DDD**:
   - In DDD, model **domain events** (e.g., `CustomerAccountRetrieved`, `OrderCreated`) to represent state changes, emitted within Bounded Contexts.
   - Use Node.js’s `EventEmitter` or libraries like `eventemitter3` for intra-context communication.
   - Example in `self-service/services.ts`:
     ```typescript
     import { EventEmitter } from 'events';
     import { Customer } from './models';
     import { getCustomerFromApigee } from './adapters';
     import { CustomerNotFoundError } from './errors';

     const emitter = new EventEmitter();

     export async function getCustomer(accountId: string): Promise<Customer> {
       const customer = await getCustomerFromApigee(accountId);
       if (!customer) throw new CustomerNotFoundError(accountId);
       emitter.emit('CustomerAccountRetrieved', { accountId, timestamp: new Date() });
       return customer;
     }

     emitter.on('CustomerAccountRetrieved', (event) => {
       console.log(`Event: Customer account ${event.accountId} retrieved at ${event.timestamp}`);
       // Publish to AWS SNS/SQS for other services
     });
     ```
     - **Why?** Aligns with DDD’s event-driven nature and Node.js’s strengths, enabling decoupled communication (e.g., via AWS SNS/SQS).
     - **Spring Parallel**: Like Spring’s `@EventListener` or publishing to a message broker (e.g., RabbitMQ).

3. **Optimize for Serverless**:
   - Keep Lambda functions lightweight, as the event loop processes one event at a time per invocation.
   - Use AWS SDK’s async methods (e.g., `aws-sdk` for S3, DynamoDB) or libraries like `axios` for Apigee calls.
   - Example: Structure each Bounded Context (e.g., `self-service`) as a separate Lambda function, triggered by API Gateway events.

4. **Handle Concurrency**:
   - Node.js’s event loop handles concurrency well, but beware of callback hell or unhandled Promise rejections.
   - Use `try/catch` with `async/await` and global error handling (as discussed previously) to catch errors.
   - Example: Reuse your `ApiError` class and middleware:
     ```typescript
     // shared/errors.ts
     export class ApiError extends Error {
       constructor(public statusCode: number, message: string, public details?: any) {
         super(message);
       }
     }
     ```

5. **Avoid Blocking Operations**:
   - Avoid synchronous operations (e.g., `fs.readFileSync`) that block the event loop, as they increase Lambda execution time.
   - Use async equivalents (e.g., `fs.promises.readFile`).

---

### 2. Handling Apigee Token Management
Since you’re using **Apigee** as an API gateway to avoid direct SOA endpoint access, you’ll need to handle OAuth 2.0 tokens, including refreshing expired tokens. This is a common pattern in enterprise APIs, similar to securing REST calls in Spring with OAuth.

#### 2.1. Apigee Token Workflow
- **Process**:
  1. Request a token from Apigee’s token endpoint (e.g., `/oauth/token`) using client credentials.
  2. Cache the token (e.g., in memory, Redis, or DynamoDB) to avoid frequent requests.
  3. Use the token in API calls to Apigee.
  4. Handle token expiration (HTTP 401/403 errors) by refreshing the token and retrying the request.
- **Spring Parallel**: Using Spring Security’s OAuth2 `RestTemplate` interceptor to manage tokens.

#### 2.2. Implementation in Node.js/TypeScript
Here’s how to implement token management in your DDD-based, serverless Node.js project.

1. **Create a Token Service** (in `shared/apigee.ts`):
   ```typescript
   import axios from 'axios';
   import { ApiError } from './errors';

   interface Token {
     access_token: string;
     expires_in: number;
     expires_at: number;
   }

   let cachedToken: Token | null = null;

   export async function getApigeeToken(): Promise<string> {
     if (cachedToken && cachedToken.expires_at > Date.now() + 60000) {
       return cachedToken.access_token;
     }

     try {
       const response = await axios.post('https://apigee-endpoint/oauth/token', {
         grant_type: 'client_credentials',
         client_id: process.env.APIGEE_CLIENT_ID,
         client_secret: process.env.APIGEE_CLIENT_SECRET,
       });
       cachedToken = {
         access_token: response.data.access_token,
         expires_in: response.data.expires_in,
         expires_at: Date.now() + response.data.expires_in * 1000 - 60000, // Buffer
       };
       return cachedToken.access_token;
     } catch (err) {
       throw new ApiError(500, 'Failed to obtain Apigee token', err.message);
     }
   }
   ```

   - **Why?**:
     - Caches the token in memory to avoid repeated requests, suitable for short-lived Lambda executions.
     - Checks expiration with a 60-second buffer to prevent edge-case failures.
     - Uses environment variables for credentials, like Spring’s `@Value` or `application.properties`.

2. **Integrate with Adapters** (e.g., `self-service/adapters.ts`):
   ```typescript
   import axios from 'axios';
   import { getApigeeToken } from '../shared/apigee';
   import { Customer } from './models';
   import { CustomerNotFoundError } from './errors';

   export async function getCustomerFromApigee(accountId: string): Promise<Customer> {
     try {
       const token = await getApigeeToken();
       const response = await axios.get(`https://apigee-endpoint/self-service/account/${accountId}`, {
         headers: { Authorization: `Bearer ${token}` },
       });
       return new Customer(response.data.id, response.data.name);
     } catch (err) {
       if (err.response?.status === 401 || err.response?.status === 403) {
         // Token expired or invalid; clear cache and retry
         cachedToken = null;
         const token = await getApigeeToken();
         const retryResponse = await axios.get(`https://apigee-endpoint/self-service/account/${accountId}`, {
           headers: { Authorization: `Bearer ${token}` },
         });
         return new Customer(retryResponse.data.id, retryResponse.data.name);
       }
       if (err.response?.status === 404) {
         throw new CustomerNotFoundError(accountId);
       }
       throw new ApiError(500, 'Apigee request failed', err.message);
     }
   }
   ```

   - **Why?**:
     - Automatically retries on 401/403 errors by refreshing the token, similar to Spring’s `OAuth2RestTemplate` retry logic.
     - Maps Apigee errors to domain-specific errors, integrating with your global exception handling.

3. **Environment Variables**:
   Store Apigee credentials in `.env` for local testing and AWS Parameter Store or Secrets Manager for production:
   ```env
   APIGEE_CLIENT_ID=your-client-id
   APIGEE_CLIENT_SECRET=your-client-secret
   STAGE=local
   ```
   In `serverless.yml`:
   ```yaml
   provider:
     name: aws
     runtime: nodejs20.x
     stage: ${opt:stage, 'dev'}
     environment:
       APIGEE_CLIENT_ID: ${ssm:/${opt:stage, 'dev'}/apigee/client-id}
       APIGEE_CLIENT_SECRET: ${ssm:/${opt:stage, 'dev'}/apigee/client-secret}
   ```

4. **Global Exception Handling**:
   Reuse your existing middleware (from `index.ts`):
   ```typescript
   import { Request, Response, NextFunction } from 'express';
   import { ApiError } from './shared/errors';

   app.use((err: ApiError, req: Request, res: Response, next: NextFunction) => {
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

---

### 3. Project Structure with DDD, TypeScript, and Apigee
Update your DDD structure to incorporate TypeScript and Apigee:

```
serverless-openapi/
├── src/
│   ├── self-service/
│   │   ├── routes.ts          // Express routes
│   │   ├── services.ts        // Business logic
│   │   ├── adapters.ts        // Apigee/SOA calls
│   │   ├── models.ts          // Domain models
│   │   ├── errors.ts          // Domain-specific errors
│   │   └── handler.ts         // Lambda handler
│   ├── sales/
│   │   ├── routes.ts
│   │   ├── services.ts
│   │   ├── adapters.ts
│   │   ├── models.ts
│   │   ├── errors.ts
│   │   └── handler.ts
│   ├── shared/
│   │   ├── apigee.ts          // Token management
│   │   ├── errors.ts          // ApiError class
│   │   ├── logger.ts          // CloudWatch logging
│   │   └── swagger.ts         // OpenAPI spec
├── index.ts                   // Main entry point
├── package.json
├── tsconfig.json
├── serverless.yml
├── .env
└── tests/
```

Example `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./src"
  }
}
```

Example `package.json`:
```json
{
  "name": "serverless-openapi",
  "version": "1.0.0",
  "scripts": {
    "start": "ts-node src/index.ts",
    "test": "jest",
    "deploy": "serverless deploy --stage ${STAGE:-dev}",
    "offline": "serverless offline --stage local"
  },
  "dependencies": {
    "express": "^4.18.2",
    "swagger-ui-express": "^5.0.0",
    "swagger-jsdoc": "^6.2.8",
    "serverless-http": "^3.2.0",
    "axios": "^1.7.2"
  },
  "devDependencies": {
    "typescript": "^5.2.2",
    "ts-node": "^10.9.1",
    "@types/node": "^20.8.0",
    "@types/express": "^4.17.17",
    "jest": "^29.7.0",
    "serverless": "^3.38.0",
    "serverless-offline": "^12.0.4"
  }
}
```

---

### 4. Applying Event-Driven Architecture in Your Project
To incorporate Node.js’s event-driven model into your DDD-based project:

1. **Bounded Contexts as Event Handlers**:
   - Treat each Bounded Context (e.g., `self-service`) as an event processor, handling HTTP events (via API Gateway) and domain events (via SNS/SQS).
   - Example: Publish a `CustomerAccountRetrieved` event to AWS SNS:
     ```typescript
     import { SNSClient, PublishCommand } from '@aws-sdk/client-sns';

     const snsClient = new SNSClient({ region: process.env.AWS_REGION });

     async function publishEvent(eventName: string, data: any) {
       await snsClient.send(
         new PublishCommand({
           TopicArn: process.env.SNS_TOPIC_ARN,
           Message: JSON.stringify({ eventName, data }),
         })
       );
     }
     ```

2. **Apigee Calls as Async Operations**:
   - All Apigee calls are async, fitting Node.js’s non-blocking model.
   - Use retry logic for token expiration, as shown above.

3. **Serverless Event Triggers**:
   - Configure API Gateway to trigger Lambda functions for each Bounded Context.
   - Use AWS EventBridge or SQS for cross-context communication (e.g., `Sales` notifying `Billing` of a new order).

4. **Performance Optimization**:
   - Cache Apigee tokens in memory for Lambda executions (reset per invocation) or use DynamoDB for cross-invocation caching.
   - Batch SOA calls where possible to reduce Apigee requests, similar to Spring’s batch REST calls.

---

### 5. Handling the 322 URIs
- **Group by Bounded Context**: Map the 322 URIs to 4-6 Bounded Contexts (e.g., `SelfService`, `Sales`, `Credit`). Example:
  - `/woa/rest/Billing/v1/account/*` → `self-service`
  - `/woa/rest/Sales/v1/order/*` → `sales`
- **Migrate Incrementally**: Start with one context (e.g., `SelfService`, ~50 URIs), test locally with `serverless offline`, and deploy to `dev`.
- **OpenAPI Documentation**: Update your Swagger spec per context, grouping paths (e.g., `/self-service/*`).

---

### 6. Best Practices
- **TypeScript**: Use interfaces and types for models (e.g., `interface Customer { id: string; name: string; }`) to mimic Java’s type safety.
- **Event-Driven DDD**: Define domain events for key actions (e.g., `CustomerUpdated`) and handle them within or across contexts.
- **Apigee Token Management**:
  - Cache tokens to minimize requests.
  - Handle 401/403 errors with automatic retries.
  - Secure credentials in AWS Secrets Manager.
- **Testing**: Write tests per Bounded Context (e.g., `self-service/tests/customer.test.ts`) using Jest, mocking Apigee calls.
- **Serverless**: Deploy each Bounded Context as a separate Lambda function for isolation and scalability.

---

### 7. Example: Full Implementation for `SelfService`
```typescript
// self-service/models.ts
export class Customer {
  constructor(public id: string, public name: string) {}
}

// self-service/errors.ts
import { ApiError } from '../shared/errors';

export class CustomerNotFoundError extends ApiError {
  constructor(accountId: string) {
    super(404, `Customer with account ${accountId} not found`);
  }
}

// self-service/adapters.ts
import axios from 'axios';
import { Customer } from './models';
import { CustomerNotFoundError } from './errors';
import { getApigeeToken } from '../shared/apigee';

export async function getCustomerFromApigee(accountId: string): Promise<Customer> {
  try {
    const token = await getApigeeToken();
    const response = await axios.get(`https://apigee-endpoint/self-service/account/${accountId}`, {
      headers: { Authorization: `Bearer ${token}` },
    });
    return new Customer(response.data.id, response.data.name);
  } catch (err) {
    if (err.response?.status === 401 || err.response?.status === 403) {
      cachedToken = null;
      const token = await getApigeeToken();
      const retryResponse = await axios.get(`https://apigee-endpoint/self-service/account/${accountId}`, {
        headers: { Authorization: `Bearer ${token}` },
      });
      return new Customer(retryResponse.data.id, retryResponse.data.name);
    }
    if (err.response?.status === 404) throw new CustomerNotFoundError(accountId);
    throw new ApiError(500, 'Apigee request failed', err.message);
  }
}

// self-service/services.ts
import { getCustomerFromApigee } from './adapters';
import { CustomerNotFoundError } from './errors';
import { publishEvent } from '../shared/events';

export async function getCustomer(accountId: string): Promise<Customer> {
  const customer = await getCustomerFromApigee(accountId);
  if (!customer) throw new CustomerNotFoundError(accountId);
  await publishEvent('CustomerAccountRetrieved', { accountId, timestamp: new Date() });
  return customer;
}

// self-service/routes.ts
import { Router } from 'express';
import { getCustomer } from './services';

const router = Router();

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

export default router;

// self-service/handler.ts
import serverless from 'serverless-http';
import express from 'express';
import router from './routes';

const app = express();
app.use('/self-service', router);
export const handler = serverless(app);
```

---

### 8. Next Steps
- **Start with TypeScript**: Convert your existing JavaScript files to TypeScript (e.g., rename `.js` to `.ts`, add types).
- **Implement Apigee**: Use the `shared/apigee.ts` example, securing credentials in AWS Secrets Manager.
- **Test Locally**: Run `npm run offline` or `sam local start-api` to test Apigee calls and event handling.
- **Migrate Incrementally**: Focus on one Bounded Context (e.g., `SelfService`) and migrate its URIs.
- **CI/CD**: Update your pipeline to deploy each context separately, using environment-specific Apigee credentials.
