# Dependency Injection (DI) in Node.js

## Table of Contents

- [Dependency Injection (DI) in Node.js](#dependency-injection-di-in-nodejs)
  - [Table of Contents](#table-of-contents)
  - [Dependency Injection (DI) in Node.js](#dependency-injection-di-in-nodejs-1)
    - [Key Concepts of Dependency Injection in Node.js:](#key-concepts-of-dependency-injection-in-nodejs)
    - [How to Implement Dependency Injection in Node.js](#how-to-implement-dependency-injection-in-nodejs)
    - [Benefits of Dependency Injection in Node.js:](#benefits-of-dependency-injection-in-nodejs)
    - [Considerations:](#considerations)
    - [Achieving Dependency Injection in Node.js:](#achieving-dependency-injection-in-nodejs)
- [Step-by-step tutorial to get started with Dependency Injection (DI) in Node.js](#step-by-step-tutorial-to-get-started-with-dependency-injection-di-in-nodejs)
  - [🧠 1. What Is Dependency Injection?](#-1-what-is-dependency-injection)
  - [⚙️ 2. Why Use DI in Node.js?](#️-2-why-use-di-in-nodejs)
  - [🧩 3. Basic Manual DI (Vanilla JS Example)](#-3-basic-manual-di-vanilla-js-example)
  - [🧪 4. Unit Testing with DI](#-4-unit-testing-with-di)
  - [🧱 5. Using a DI Container](#-5-using-a-di-container)
  - [🚀 6. Dependency Injection with Awilix](#-6-dependency-injection-with-awilix)
  - [🌐 7. Integrating DI with Express.js](#-7-integrating-di-with-expressjs)
  - [🧭 8. DI Best Practices](#-8-di-best-practices)
  - [📘 9. Summary](#-9-summary)

## Dependency Injection (DI) in Node.js

Dependency Injection (DI) in Node.js is a software design pattern where components receive their dependencies from an external source, rather than creating them internally. This technique promotes loose coupling, enhances testability, and improves the overall maintainability of Node.js applications.

Dependency Injection (DI) in Node.js, like in other programming languages, is a design pattern that promotes loose coupling and improves the testability of your code. It involves providing the dependencies an object needs from an external source, rather than having the object create or manage them itself.

### Key Concepts of Dependency Injection in Node.js:

- Inversion of Control (IoC): DI is a specific form of IoC, where the control of creating and managing dependencies is inverted. Instead of a component creating its own dependencies, an external entity (like a DI container or the application's entry point) provides them.
- Decoupling: By injecting dependencies, components become less dependent on specific implementations. This means you can easily swap out one dependency (e.g., a database repository) for another (e.g., a mock repository for testing) without altering the core logic of the component.
- Testability: DI simplifies unit testing by allowing the injection of mock or stub implementations of dependencies. This isolates the component under test, making tests more focused and reliable.

### How to Implement Dependency Injection in Node.js 

Instead of a module directly requiring or instantiating its dependencies, those dependencies are passed into the module, typically through its constructor or setter methods. This means the module doesn't "know" how to create its dependencies; it just expects to receive them.

- Constructor Injection: This is the most common and recommended approach. Dependencies are passed as arguments to the constructor of a class.

``` JavaScript

    class UserRepository {
        constructor(dataSource) {
            this.dataSource = dataSource;
        }

        async findUserById(id) {
            return this.dataSource.query(`SELECT * FROM users WHERE id = ${id}`);
        }
    }

    class UserService {
        constructor(userRepository) {
            this.userRepository = userRepository;
        }

        async getUser(id) {
            return this.userRepository.findUserById(id);
        }
    }

    // In the application's entry point:
    const dataSource = new InMemoryDataSource(); // Or a real database connection
    const userRepository = new UserRepository(dataSource);
    const userService = new UserService(userRepository);

    // Now, userService is ready to be used.
```

- Setter Injection: Dependencies are provided through setter methods after the object has been instantiated.
- Property Injection: Dependencies are assigned directly to public properties of the object.

### Benefits of Dependency Injection in Node.js:

- Improved Testability: Easily substitute dependencies with mocks for unit testing. You can easily inject mock or stub implementations of dependencies during testing, allowing you to isolate and test individual components without relying on their actual implementations.
- Enhanced Maintainability: Decoupled components are easier to understand, modify, and extend.
- Increased Flexibility: Allows for easy swapping of different dependency implementations.
- Better Code Organization: Promotes a clear separation of concerns.
- Loose Coupling: Modules are less dependent on specific implementations of their dependencies, making it easier to swap out or change those dependencies without affecting the dependent module.
- Enhanced Reusability: Dependencies can be created and configured once, then reused across multiple modules.
- Better Maintainability: The separation of concerns makes the codebase easier to understand, modify, and maintain.

### Considerations:

- Initial Setup Complexity: Implementing DI might introduce some initial overhead, especially for smaller projects.
- DI Containers (Optional): For larger applications, DI containers like Awilix can help manage and resolve dependencies automatically, reducing boilerplate code.

### Achieving Dependency Injection in Node.js:

While Node.js doesn't have built-in DI frameworks like some other languages, you can implement DI through:

- Manual Injection: As shown in the example above, you can manually create and pass dependencies.
- Dependency Injection Libraries: Libraries like TypeDI, InversifyJS, and Awilix provide more advanced features for managing and injecting dependencies, including features like decorators, container management, and lifecycle management.
- Higher-Order Functions: In a more functional approach, you can pass functions as dependencies, which can be seen as a form of DI.

# Step-by-step tutorial to get started with Dependency Injection (DI) in Node.js

Here’s a **step-by-step tutorial to get started with Dependency Injection (DI) in Node.js**, including conceptual explanation, simple examples, and practical implementation patterns using both vanilla JS and frameworks.

---

## 🧠 1. What Is Dependency Injection?

**Dependency Injection (DI)** is a design pattern where objects receive their dependencies rather than creating them directly.

Instead of:

```js
const db = new Database();
const userService = new UserService(db);
```

You **inject** the dependency:

```js
const userService = new UserService(injectedDb);
```

👉 This decouples components and makes code **easier to test, maintain, and extend**.

---

## ⚙️ 2. Why Use DI in Node.js?

**Without DI:**

* Hard to test (you can’t mock dependencies easily)
* Tight coupling between modules
* Harder to maintain or extend code

**With DI:**

* Easier unit testing (mock dependencies)
* Loose coupling between components
* Reusable and configurable modules

---

## 🧩 3. Basic Manual DI (Vanilla JS Example)

Let’s start with **manual dependency injection**, which doesn’t need any library.

**database.js**

```js
class Database {
  constructor(connectionString) {
    this.connectionString = connectionString;
  }

  connect() {
    console.log(`Connected to ${this.connectionString}`);
  }
}

export default Database;
```

**userService.js**

```js
class UserService {
  constructor(database) {
    this.database = database;
  }

  getUser(id) {
    this.database.connect();
    console.log(`Fetching user ${id}`);
  }
}

export default UserService;
```

**app.js**

```js
import Database from './database.js';
import UserService from './userService.js';

const db = new Database('mongodb://localhost:27017/mydb');
const userService = new UserService(db);

userService.getUser(1);
```

✅ Here, `UserService` does not create its own database; it **receives it**. That’s dependency injection in action.

---

## 🧪 4. Unit Testing with DI

Since dependencies are injected, testing becomes simple:

**userService.test.js**

```js
import UserService from './userService.js';

const mockDb = {
  connect: jest.fn()
};

const userService = new UserService(mockDb);

userService.getUser(42);

expect(mockDb.connect).toHaveBeenCalled();
```

✅ You can now mock the database easily, without needing a real DB connection.

---

## 🧱 5. Using a DI Container

For larger Node.js apps, **manual DI** gets messy.
A **DI Container** can automatically resolve and inject dependencies.

Popular libraries:

* [`awilix`](https://github.com/jeffijoe/awilix)
* [`tsyringe`](https://github.com/microsoft/tsyringe)
* [`inversify`](https://github.com/inversify/InversifyJS)

We’ll use **Awilix**, since it’s framework-agnostic and works great in Node.js or Express.

---

## 🚀 6. Dependency Injection with Awilix

**Step 1 – Install**

```bash
npm install awilix
```

**Step 2 – Create Your Services**

`services/database.js`

```js
class Database {
  connect() {
    console.log('Connected to DB');
  }
}
export default Database;
```

`services/userService.js`

```js
class UserService {
  constructor({ database }) {
    this.database = database;
  }

  getUser(id) {
    this.database.connect();
    return { id, name: 'John Doe' };
  }
}

export default UserService;
```

**Step 3 – Create DI Container**

`container.js`

```js
import { createContainer, asClass } from 'awilix';
import Database from './services/database.js';
import UserService from './services/userService.js';

const container = createContainer();

container.register({
  database: asClass(Database).singleton(),
  userService: asClass(UserService).scoped(),
});

export default container;
```

**Step 4 – Use in Your App**

`app.js`

```js
import container from './container.js';

const userService = container.resolve('userService');
console.log(userService.getUser(123));
```

✅ Awilix automatically injects `database` into `userService`.

---

## 🌐 7. Integrating DI with Express.js

Here’s how to use Awilix in an Express app.

**Step 1 – Install middleware**

```bash
npm install awilix-express
```

**Step 2 – Setup container middleware**

`app.js`

```js
import express from 'express';
import { scopePerRequest } from 'awilix-express';
import container from './container.js';
import userRouter from './routes/user.js';

const app = express();

app.use(scopePerRequest(container));
app.use('/users', userRouter);

app.listen(3000, () => console.log('Server running'));
```

**Step 3 – Use DI in your route**

`routes/user.js`

```js
import { makeInvoker } from 'awilix-express';

const UserController = ({ userService }) => ({
  getUser: (req, res) => {
    const user = userService.getUser(req.params.id);
    res.json(user);
  }
});

const api = makeInvoker(UserController);

import express from 'express';
const router = express.Router();

router.get('/:id', api('getUser'));

export default router;
```

✅ Each request gets its own scoped container instance — perfect for Express apps.

---

## 🧭 8. DI Best Practices

1. **Inject interfaces (contracts), not implementations** when possible.
2. **Use singletons** for shared resources (e.g., DB connection).
3. **Use scoped instances** for request-based services.
4. **Keep constructors simple** — avoid mixing business logic with wiring.
5. **Test services with mocks** — DI makes this easier.

---

## 📘 9. Summary

| Concept                   | Example                                               |
| ------------------------- | ----------------------------------------------------- |
| **Manual DI**             | Pass dependencies explicitly in constructors          |
| **Container DI**          | Use a library like Awilix                             |
| **Testing**               | Mock injected dependencies easily                     |
| **Framework Integration** | Awilix + Express via `awilix-express`                 |
| **Result**                | More modular, maintainable, and testable Node.js apps |

---

Would you like me to continue this tutorial with **TypeScript integration for DI** (using Awilix or InversifyJS)? It adds typing, decorators, and better scalability for enterprise-level Node.js projects.
