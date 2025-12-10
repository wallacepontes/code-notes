# Design Patterns in Node.js

Excellent — let’s walk through a **practical and modern tutorial to get started with Design Patterns in Node.js**.
We’ll cover the *core patterns every Node.js developer should know*, with examples using **TypeScript**, since it adds structure and clarity to pattern implementation.

---

## 🧭 Table of Contents

- [Design Patterns in Node.js](#design-patterns-in-nodejs)
  - [🧭 Table of Contents](#-table-of-contents)
  - [1. What Are Design Patterns](#1-what-are-design-patterns)
  - [2. Why They Matter in Node.js](#2-why-they-matter-in-nodejs)
  - [3. Types of Design Patterns](#3-types-of-design-patterns)
  - [4. Core Patterns You Should Know](#4-core-patterns-you-should-know)
    - [🧩 Singleton Pattern](#-singleton-pattern)
    - [🏭 Factory Pattern](#-factory-pattern)
    - [⚙️ Strategy Pattern](#️-strategy-pattern)
    - [🔔 Observer Pattern (Node’s EventEmitter)](#-observer-pattern-nodes-eventemitter)
    - [🎨 Decorator Pattern](#-decorator-pattern)
    - [🧱 Repository Pattern](#-repository-pattern)
    - [🪤 Middleware Pattern (Common in Express)](#-middleware-pattern-common-in-express)
    - [🧩 Adapter Pattern](#-adapter-pattern)
    - [🧱 Builder Pattern](#-builder-pattern)
    - [🪞 Proxy Pattern](#-proxy-pattern)
    - [⚙️ Command Pattern](#️-command-pattern)
  - [5. 🧩 How They Fit Together](#5--how-they-fit-together)
  - [6. Project Setup](#6-project-setup)
  - [7. Best Practices](#7-best-practices)
  - [🚀 Next Step](#-next-step)
  - [References](#references)

---

## 1. What Are Design Patterns

**Design Patterns** are reusable solutions to common software design problems.
They help you:

* Write **clean, maintainable** code.
* Improve **scalability** and **testability**.
* Communicate architecture decisions clearly across teams.

---

## 2. Why They Matter in Node.js

Node.js applications often deal with:

* **Asynchronous flows**
* **Service layers**
* **Microservice communication**
* **Configuration management**

Using patterns can help structure these concerns consistently.

---

## 3. Types of Design Patterns

| Type           | Description                    | Example            |
| -------------- | ------------------------------ | ------------------ |
| **Creational** | Object creation logic          | Singleton, Factory |
| **Structural** | Composition of classes/objects | Decorator, Adapter |
| **Behavioral** | Communication between objects  | Strategy, Observer |

---

## 4. Core Patterns You Should Know

### 🧩 Singleton Pattern

Ensures only one instance of a class exists across the app.

📁 `singleton/Database.ts`

```ts
export class Database {
  private static instance: Database;

  private constructor() {
    console.log('Database connection created.');
  }

  public static getInstance(): Database {
    if (!Database.instance) {
      Database.instance = new Database();
    }
    return Database.instance;
  }

  query(sql: string) {
    console.log(`Running query: ${sql}`);
  }
}

// Usage
const db1 = Database.getInstance();
const db2 = Database.getInstance();
console.log(db1 === db2); // true
```

---

### 🏭 Factory Pattern

Creates objects without exposing the creation logic.

📁 `factory/LoggerFactory.ts`

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(`Console: ${message}`);
  }
}

class FileLogger implements Logger {
  log(message: string) {
    console.log(`File: ${message}`);
  }
}

class LoggerFactory {
  static createLogger(type: string): Logger {
    if (type === 'file') return new FileLogger();
    return new ConsoleLogger();
  }
}

// Usage
const logger = LoggerFactory.createLogger('console');
logger.log('Factory pattern example');
```

---

### ⚙️ Strategy Pattern

Encapsulates algorithms and makes them interchangeable.

📁 `strategy/PaymentStrategy.ts`

```ts
interface PaymentStrategy {
  pay(amount: number): void;
}

class PaypalPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid $${amount} using PayPal`);
  }
}

class CreditCardPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid $${amount} using Credit Card`);
  }
}

class PaymentContext {
  constructor(private strategy: PaymentStrategy) {}

  execute(amount: number) {
    this.strategy.pay(amount);
  }
}

// Usage
const context = new PaymentContext(new PaypalPayment());
context.execute(100);
```

---

### 🔔 Observer Pattern (Node’s EventEmitter)

Lets objects subscribe to events and react asynchronously.

📁 `observer/UserService.ts`

```ts
import { EventEmitter } from 'events';

class UserService extends EventEmitter {
  registerUser(name: string) {
    console.log(`User registered: ${name}`);
    this.emit('userRegistered', name);
  }
}

// Usage
const service = new UserService();

service.on('userRegistered', (name) => {
  console.log(`Sending welcome email to ${name}`);
});

service.registerUser('Alice');
```

---

### 🎨 Decorator Pattern

Adds new behavior to objects without modifying their structure.

📁 `decorator/LoggerDecorator.ts`

```ts
function logExecution(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result from ${propertyKey}:`, result);
    return result;
  };
}

class Calculator {
  @logExecution
  add(a: number, b: number) {
    return a + b;
  }
}

// Usage
const calc = new Calculator();
calc.add(2, 3);
```

---

### 🧱 Repository Pattern

Separates business logic from data access.

📁 `repository/UserRepository.ts`

```ts
interface User {
  id: number;
  name: string;
}

class UserRepository {
  private users: User[] = [];

  findAll(): User[] {
    return this.users;
  }

  save(user: User) {
    this.users.push(user);
  }
}

// Usage
const repo = new UserRepository();
repo.save({ id: 1, name: 'John' });
console.log(repo.findAll());
```

---

### 🪤 Middleware Pattern (Common in Express)

Allows chaining of functions that process a request/response.

📁 `middleware/app.ts`

```ts
import express from 'express';

const app = express();

app.use((req, res, next) => {
  console.log('Request received at:', new Date());
  next();
});

app.use((req, res, next) => {
  console.log('Authenticating...');
  next();
});

app.get('/', (req, res) => {
  res.send('Hello from Middleware Pattern');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

### 🧩 Adapter Pattern

**Purpose:**
Allows incompatible interfaces to work together.
Think of it as a *translator* between two systems or APIs.

**Use case example:**
You change your logging library (e.g., from Winston to Bunyan) but want to keep your existing `Logger` interface.

📁 `adapter/LoggerAdapter.ts`

```ts
// Old interface
interface Logger {
  info(message: string): void;
  error(message: string): void;
}

// New library with a different interface
class BunyanLogger {
  logInfo(msg: string) {
    console.log(`BUNYAN INFO: ${msg}`);
  }
  logError(msg: string) {
    console.error(`BUNYAN ERROR: ${msg}`);
  }
}

// Adapter converts BunyanLogger to Logger interface
class BunyanLoggerAdapter implements Logger {
  constructor(private bunyan: BunyanLogger) {}

  info(message: string) {
    this.bunyan.logInfo(message);
  }

  error(message: string) {
    this.bunyan.logError(message);
  }
}

// Usage
const adapter = new BunyanLoggerAdapter(new BunyanLogger());
adapter.info('This works with the old interface');
```

✅ **When to use:**

* Integrating a new API or SDK without changing your existing business logic.
* Migrating legacy code to new services.

---

### 🧱 Builder Pattern

**Purpose:**
Simplifies the creation of complex objects step-by-step, instead of using huge constructors.

**Use case example:**
Creating a complex user configuration object for an API request.

📁 `builder/UserBuilder.ts`

```ts
class User {
  constructor(
    public name: string,
    public email: string,
    public age?: number,
    public address?: string
  ) {}
}

class UserBuilder {
  private name!: string;
  private email!: string;
  private age?: number;
  private address?: string;

  setName(name: string): this {
    this.name = name;
    return this;
  }

  setEmail(email: string): this {
    this.email = email;
    return this;
  }

  setAge(age: number): this {
    this.age = age;
    return this;
  }

  setAddress(address: string): this {
    this.address = address;
    return this;
  }

  build(): User {
    return new User(this.name, this.email, this.age, this.address);
  }
}

// Usage
const user = new UserBuilder()
  .setName('Alice')
  .setEmail('alice@example.com')
  .setAge(30)
  .setAddress('New York')
  .build();

console.log(user);
```

✅ **When to use:**

* Objects with many optional or complex initialization parameters.
* Reducing constructor clutter.

---

### 🪞 Proxy Pattern

**Purpose:**
Acts as a *surrogate* for another object, controlling access to it.
Commonly used for:

* Lazy loading
* Caching
* Access control
* Logging

📁 `proxy/ImageProxy.ts`

```ts
interface Image {
  display(): void;
}

class RealImage implements Image {
  constructor(private filename: string) {
    this.loadFromDisk();
  }

  private loadFromDisk() {
    console.log(`Loading image from disk: ${this.filename}`);
  }

  display() {
    console.log(`Displaying ${this.filename}`);
  }
}

class ProxyImage implements Image {
  private realImage?: RealImage;

  constructor(private filename: string) {}

  display() {
    if (!this.realImage) {
      this.realImage = new RealImage(this.filename);
    }
    this.realImage.display();
  }
}

// Usage
const image = new ProxyImage('photo.png');
image.display(); // Loads and displays
image.display(); // Displays from cache
```

✅ **When to use:**

* You want to defer expensive operations until they’re needed.
* You need to add security, logging, or caching to an object without changing its core logic.

---

### ⚙️ Command Pattern

**Purpose:**
Encapsulates an operation (like “execute” or “undo”) into an object, allowing you to parameterize and queue operations.

**Use case example:**
You have multiple database or file operations and want to queue or undo them dynamically.

📁 `command/CommandPattern.ts`

```ts
interface Command {
  execute(): void;
}

class Light {
  turnOn() {
    console.log('Light turned ON');
  }
  turnOff() {
    console.log('Light turned OFF');
  }
}

class TurnOnCommand implements Command {
  constructor(private light: Light) {}
  execute() {
    this.light.turnOn();
  }
}

class TurnOffCommand implements Command {
  constructor(private light: Light) {}
  execute() {
    this.light.turnOff();
  }
}

class RemoteControl {
  private history: Command[] = [];

  press(command: Command) {
    this.history.push(command);
    command.execute();
  }
}

// Usage
const light = new Light();
const onCommand = new TurnOnCommand(light);
const offCommand = new TurnOffCommand(light);

const remote = new RemoteControl();
remote.press(onCommand);
remote.press(offCommand);
```

✅ **When to use:**

* Implementing undo/redo functionality.
* Queuing and batching tasks.
* Decoupling invokers (controllers, schedulers) from the actual business logic.

---

## 5. 🧩 How They Fit Together

Here’s how you might combine them in a Node.js backend:

| Pattern     | Common Use Case                                                     |
| ----------- | ------------------------------------------------------------------- |
| **Adapter** | Integrate a new payment or logging SDK into your existing service   |
| **Builder** | Construct API request payloads or configuration objects             |
| **Proxy**   | Add caching to database or API calls                                |
| **Command** | Manage queued tasks, message-driven actions, or undoable operations |

**Example combination:**
You might have a **Proxy** around a database repository, using an **Adapter** to communicate with an external SDK, and a **Command** layer to process user actions asynchronously.

---

## 6. Project Setup

```bash
mkdir node-design-patterns && cd node-design-patterns
npm init -y
npm install typescript ts-node @types/node express
npx tsc --init
```

Then you can run each example with:

```bash
npx ts-node singleton/Database.ts
```

---

## 7. Best Practices

✅ Start with patterns **only when needed** — avoid over-engineering.
✅ Use **composition over inheritance**.
✅ Combine patterns (e.g., Repository + Singleton).
✅ Keep them **consistent** across teams for maintainability.

---

Perfect 👏 — those four are **very useful structural and behavioral patterns** that take your Node.js architecture from “functional” to “elegant and scalable.”

Let’s walk through each one — **Adapter**, **Builder**, **Proxy**, and **Command** — with **TypeScript** examples and clear use cases.


✅ Prefer **composition over inheritance** when implementing patterns.
✅ Avoid using patterns *just for the sake of it* — each should solve a real complexity.
✅ Use **TypeScript interfaces** to clearly define roles (e.g., Command, Builder).
✅ Combine patterns naturally — e.g., Builder + Factory, Proxy + Singleton.

---

## 🚀 Next Step

Once you’re comfortable with these, explore:

* **CQRS Pattern** – command-query separation in scalable systems.
* **Event Sourcing** – advanced event-driven architectures.

---

## References
1. https://javascripttoday.com/blog/4-design-patterns-in-node
2. https://medium.com/zero-equals-false/builder-design-pattern-in-node-js-c942ac7354a9