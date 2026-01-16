# Migration - Global Exception Handling

## Table of Contents
- [Migration - Global Exception Handling](#migration---global-exception-handling)
  - [Table of Contents](#table-of-contents)
  - [Global Exception Handling](#global-exception-handling)
    - [1. Understanding Global Exception Handling](#1-understanding-global-exception-handling)
    - [2. Implementing Global Exception Handling in Node.js/Express](#2-implementing-global-exception-handling-in-nodejsexpress)
      - [Step 2.1: Define a Custom Error Class](#step-21-define-a-custom-error-class)
      - [Step 2.2: Create Global Error-Handling Middleware](#step-22-create-global-error-handling-middleware)
      - [Step 2.3: Integrate with Your Express App](#step-23-integrate-with-your-express-app)
      - [Step 2.4: Update OpenAPI Specification](#step-24-update-openapi-specification)
      - [Step 2.5: Local Testing](#step-25-local-testing)
      - [Step 2.6: Serverless Deployment](#step-26-serverless-deployment)
    - [3. Handling Specific Error Types](#3-handling-specific-error-types)
    - [4. Integration with AWS Lambda and CI/CD](#4-integration-with-aws-lambda-and-cicd)
    - [5. Parallels with Spring](#5-parallels-with-spring)
    - [6. Best Practices for Node.js](#6-best-practices-for-nodejs)
    - [7. Alternatives](#7-alternatives)
    - [8. Next Steps](#8-next-steps)

## Global Exception Handling

In Spring/Java projects, `@ControllerAdvice` and `@ExceptionHandler` provide a centralized way to handle exceptions across all controllers, allowing you to define consistent error responses for your APIs. Since you're working on a serverless Node.js project with an OpenAPI specification for a telecom company in Brazil, I'll explain how to achieve similar **global exception handling** in a Node.js environment, particularly using **Express** (as in your earlier example). I'll draw parallels to Spring, keep it beginner-friendly given your Node.js learning curve, and ensure it aligns with your serverless setup (e.g., AWS Lambda). I'll also provide a complete example and address deployment considerations.

---

### 1. Understanding Global Exception Handling
- **Spring Context**:
  - `@ControllerAdvice`: A class annotated with `@ControllerAdvice` intercepts exceptions thrown by any controller.
  - `@ExceptionHandler`: Methods in the advice class handle specific exceptions (e.g., `NotFoundException`) and return formatted responses (e.g., JSON with status codes).
  - Example:
    ```java
    @ControllerAdvice
    public class GlobalExceptionHandler {
        @ExceptionHandler(NotFoundException.class)
        public ResponseEntity<ErrorResponse> handleNotFound(NotFoundException ex) {
            ErrorResponse error = new ErrorResponse(404, ex.getMessage());
            return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
        }
    }
    ```

- **Node.js/Express Context**:
  - Express uses **middleware** to handle requests and errors. A special error-handling middleware (with four parameters: `err, req, res, next`) can catch errors thrown or passed from route handlers, similar to `@ControllerAdvice`.
  - You define a single middleware to handle all errors globally, or multiple middleware for specific error types.
  - In a serverless setup (e.g., AWS Lambda with `serverless-http`), this middleware works seamlessly as long as your Express app is properly wrapped.

---

### 2. Implementing Global Exception Handling in Node.js/Express
Here’s how to implement global exception handling in your Node.js project using Express, tailored for your serverless OpenAPI API.

#### Step 2.1: Define a Custom Error Class
To mimic Spring’s custom exceptions (e.g., `NotFoundException`), create a custom error class in Node.js.

```javascript
class ApiError extends Error {
  constructor(statusCode, message, details = null) {
    super(message);
    this.statusCode = statusCode;
    this.details = details;
  }
}
```

- **Purpose**: Like a custom `RuntimeException` in Spring, this allows you to throw errors with HTTP status codes and optional details.
- **Usage**: Throw in your routes, e.g., `throw new ApiError(404, 'User not found')`.

#### Step 2.2: Create Global Error-Handling Middleware
Express middleware with the signature `(err, req, res, next)` acts as the equivalent of `@ControllerAdvice`.

```javascript
const errorHandler = (err, req, res, next) => {
  // Default values
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';

  // Log error (optional, for debugging)
  console.error(`Error: ${statusCode} - ${message}`, err.stack);

  // Send JSON response
  res.status(statusCode).json({
    error: {
      status: statusCode,
      message,
      details: err.details || null,
    },
  });
};
```

- **How it works**: Catches any error thrown or passed to `next(err)` in your routes or middleware.
- **Spring parallel**: Like a `@ControllerAdvice` method with `@ExceptionHandler(Exception.class)` for generic errors.

#### Step 2.3: Integrate with Your Express App
Update your `index.js` from the earlier serverless OpenAPI example to include global exception handling.

```javascript
const express = require('express');
const swaggerUi = require('swagger-ui-express');
const swaggerJsdoc = require('swagger-jsdoc');
const serverless = require('serverless-http');
require('dotenv').config();

const app = express();
app.use(express.json());

// Custom Error Class
class ApiError extends Error {
  constructor(statusCode, message, details = null) {
    super(message);
    this.statusCode = statusCode;
    this.details = details;
  }
}

// Swagger definition
const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: { title: 'Serverless API', version: '1.0.0' },
    servers: [{ url: `/${process.env.STAGE || 'dev'}` }],
  },
  apis: ['./index.js'],
};
const swaggerDocs = swaggerJsdoc(swaggerOptions);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocs));

/**
 * @openapi
 * /hello:
 *   get:
 *     summary: Returns a greeting message
 *     responses:
 *       200:
 *         description: A successful response
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 message:
 *                   type: string
 *       404:
 *         description: Not found
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 error:
 *                   type: object
 *                   properties:
 *                     status:
 *                       type: integer
 *                     message:
 *                       type: string
 */
app.get('/hello', (req, res, next) => {
  try {
    // Simulate a not-found error
    if (!req.query.name) {
      throw new ApiError(404, 'Name not provided');
    }
    res.json({ message: `Hello, ${req.query.name}!` });
  } catch (err) {
    next(err); // Pass to error-handling middleware
  }
});

// Global error-handling middleware
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

// Export for serverless
module.exports.handler = serverless(app);
```

- **Key Points**:
  - Place the error-handling middleware **last** in the middleware chain (`app.use` at the bottom), like `@ControllerAdvice` applying globally.
  - Use `try/catch` in routes to throw `ApiError` or pass errors to `next(err)`, similar to throwing exceptions in Spring controllers.
  - The response format mimics Spring’s `ResponseEntity<ErrorResponse>`.

#### Step 2.4: Update OpenAPI Specification
Add error responses to your OpenAPI spec to document possible errors, like Springdoc annotations in Spring.

```javascript
/**
 * @openapi
 * /hello:
 *   get:
 *     summary: Returns a greeting message
 *     parameters:
 *       - in: query
 *         name: name
 *         schema:
 *           type: string
 *         required: true
 *     responses:
 *       200:
 *         description: A successful response
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 message:
 *                   type: string
 *       404:
 *         description: Not found
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 error:
 *                   type: object
 *                   properties:
 *                     status:
 *                       type: integer
 *                     message:
 *                       type: string
 *                     details:
 *                       type: object
 *                       nullable: true
 */
```

- **Why?** Documents error responses in Swagger UI, similar to Spring’s `@ApiResponse` annotations.

#### Step 2.5: Local Testing
- Use `serverless-offline` (as in your setup):
  ```bash
  npm install -D serverless-offline
  npm run offline
  ```
- Test with `curl` or Postman:
  ```bash
  curl http://localhost:3000/hello
  ```
  Response (404 error):
  ```json
  {
    "error": {
      "status": 404,
      "message": "Name not provided",
      "details": null
    }
  }
  ```

- **Spring parallel**: Testing a Spring endpoint with a missing parameter and seeing the `@ExceptionHandler` response.

#### Step 2.6: Serverless Deployment
- Deploy to AWS Lambda using Serverless Framework or AWS SAM (as discussed previously).
- The error-handling middleware works seamlessly in Lambda via `serverless-http`.
- Example `serverless.yml`:
  ```yaml
  service: serverless-openapi
  provider:
    name: aws
    runtime: nodejs20.x
    stage: ${opt:stage, 'dev'}
  functions:
    api:
      handler: index.handler
      events:
        - http:
            path: /
            method: ANY
        - http:
            path: /{proxy+}
            method: ANY
  plugins:
    - serverless-offline
  ```

---

### 3. Handling Specific Error Types
To mimic Spring’s ability to handle specific exceptions with different `@ExceptionHandler` methods, you can use multiple middleware or conditional logic.

Example: Handling specific errors differently:
```javascript
app.use((err, req, res, next) => {
  if (err instanceof ApiError) {
    // Handle ApiError specifically
    res.status(err.statusCode).json({
      error: {
        status: err.statusCode,
        message: err.message,
        details: err.details,
      },
    });
  } else if (err.name === 'ValidationError') {
    // Handle validation errors (e.g., from express-validator)
    res.status(400).json({
      error: {
        status: 400,
        message: 'Validation failed',
        details: err.errors,
      },
    });
  } else {
    // Generic error
    res.status(500).json({
      error: {
        status: 500,
        message: 'Internal Server Error',
        details: null,
      },
    });
  }
});
```

- **Spring parallel**: Multiple `@ExceptionHandler` methods for `NotFoundException`, `ValidationException`, etc.
- **Example**: Add `express-validator` for input validation:
  ```bash
  npm install express-validator
  ```
  Update route:
  ```javascript
  const { check, validationResult } = require('express-validator');

  app.post(
    '/user',
    [
      check('name').notEmpty().withMessage('Name is required'),
      check('age').isInt({ min: 0 }).withMessage('Age must be a positive integer'),
    ],
    (req, res, next) => {
      const errors = validationResult(req);
      if (!errors.isEmpty()) {
        throw new ApiError(400, 'Validation failed', errors.array());
      }
      res.status(201).json({ message: `User ${req.body.name} created` });
    }
  );
  ```

---

### 4. Integration with AWS Lambda and CI/CD
- **Serverless Compatibility**: The error-handling middleware works in AWS Lambda via `serverless-http`. No changes are needed for deployment.
- **Pipeline**: As discussed earlier, use `npm ci` and `serverless deploy` (or `sam deploy`) in your CI/CD pipeline (e.g., AWS CodePipeline, GitHub Actions).
- **Stage Isolation**: Use environment variables (`STAGE=dev`, `qa`, etc.) to configure stage-specific behavior, similar to Spring profiles.
- **Example GitHub Actions**:
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

### 5. Parallels with Spring
| **Feature**                     | **Spring (Java)**                              | **Node.js (Express)**                        |
|---------------------------------|-----------------------------------------------|---------------------------------------------|
| **Global Exception Handling**   | `@ControllerAdvice` + `@ExceptionHandler`      | Error-handling middleware (`err, req, res, next`) |
| **Custom Exceptions**           | `class NotFoundException extends RuntimeException` | `class ApiError extends Error`             |
| **Error Response**              | `ResponseEntity<ErrorResponse>`               | `res.status().json()`                       |
| **Validation**                  | `@Valid` + Bean Validation                    | `express-validator` or `joi`                 |
| **Documentation**               | Springdoc/Swagger annotations                 | `@openapi` JSDoc or YAML                    |

---

### 6. Best Practices for Node.js
- **Centralize error handling**: Place error middleware at the end of your app’s middleware chain.
- **Use custom errors**: Standardize errors with `ApiError` for consistent responses.
- **Log errors**: Integrate with a logging service (e.g., AWS CloudWatch) for production, like Spring’s `log.error`.
- **Validate inputs**: Use `express-validator` or `joi` to catch invalid inputs early, like Spring’s `@Valid`.
- **Document errors**: Update your OpenAPI spec to include error responses, visible in Swagger UI.

---

### 7. Alternatives
If you find Express middleware limiting:
- **NestJS**: A Node.js framework inspired by Angular/Spring, with built-in `@Catch` and `@ExceptionFilter` decorators that closely mimic `@ControllerAdvice`. Example:
  ```typescript
  import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';

  @Catch(HttpException)
  export class HttpExceptionFilter implements ExceptionFilter {
    catch(exception: HttpException, host: ArgumentsHost) {
      const ctx = host.switchToHttp();
      const response = ctx.getResponse();
      const status = exception.getStatus();
      response.status(status).json({
        error: { status, message: exception.message },
      });
    }
  }
  ```
  - **Pros**: More structured, Spring-like, supports TypeScript.
  - **Cons**: Steeper learning curve, heavier for serverless.

- **Fastify**: A faster alternative to Express with built-in error handling. Example:
  ```javascript
  fastify.setErrorHandler((error, request, reply) => {
    reply.status(error.statusCode || 500).send({
      error: { status: error.statusCode, message: error.message },
    });
  });
  ```

---

### 8. Next Steps
- **Implement the example**: Use the `index.js` above in your project and test locally with `serverless offline`.
- **Add validation**: Integrate `express-validator` for input validation, as shown.
- **Switch to SAM**: If Serverless Framework’s licensing is an issue, adapt to AWS SAM (I can provide a `template.yaml`).
- **Pipeline**: Ensure your CI/CD pipeline runs tests and deploys to the correct stage.


