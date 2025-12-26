# NestJS for Beginners

---

## Table of Contents

- [NestJS for Beginners](#nestjs-for-beginners)
  - [Table of Contents](#table-of-contents)
  - [**Course Presentation: NestJS for Beginners**](#course-presentation-nestjs-for-beginners)
    - [**(0:00:00) Course Introduction**](#00000-course-introduction)
    - [**(0:01:15) What is NestJS**](#00115-what-is-nestjs)
    - [**(0:02:35) Why use NestJS**](#00235-why-use-nestjs)
    - [**(0:03:35) What we are building**](#00335-what-we-are-building)
    - [**(0:03:55) NestJS project setup**](#00355-nestjs-project-setup)
    - [**(0:05:55) Modules**](#00555-modules)
    - [**(0:15:30) Auth module**](#01530-auth-module)
    - [**(0:19:50) Dependency injection**](#01950-dependency-injection)
    - [**(0:20:30) Auth controller**](#02030-auth-controller)
    - [**(0:26:50) Setting up Postgres in Docker**](#02650-setting-up-postgres-in-docker)
    - [**(0:29:10) Setting up Prisma**](#02910-setting-up-prisma)
    - [**(0:32:10) User \& bookmark models**](#03210-user--bookmark-models)
    - [**(0:35:50) Running Prisma migrations**](#03550-running-prisma-migrations)
    - [**(0:40:10) Prisma module**](#04010-prisma-module)
    - [**(0:52:10) Using Auth DTOs**](#05210-using-auth-dtos)
    - [**(0:57:45) NestJS pipes**](#05745-nestjs-pipes)
    - [**(0:59:10) NestJS global pipe**](#05910-nestjs-global-pipe)
    - [**(1:02:50) Hashing user password with Argon**](#10250-hashing-user-password-with-argon)
    - [**(1:03:20) Sign up logic**](#10320-sign-up-logic)
    - [**(1:13:13) Sign in logic**](#11313-sign-in-logic)
    - [**(1:16:50) Automate Postgres restart \& Prisma migrations**](#11650-automate-postgres-restart--prisma-migrations)
    - [**(1:27:40) NestJS config module**](#12740-nestjs-config-module)
    - [**(1:34:40) Using Passport.js \& JWT module with NestJS**](#13440-using-passportjs--jwt-module-with-nestjs)
    - [**(1:55:10) Get current user with access token**](#15510-get-current-user-with-access-token)
    - [**(1:57:00) NestJS Guards**](#15700-nestjs-guards)
    - [**(2:12:10) NestJS custom param decorator**](#21210-nestjs-custom-param-decorator)
    - [**(2:27:10) e2e tests with PactumJS**](#22710-e2e-tests-with-pactumjs)
    - [**(2:35:10) Setting up test database**](#23510-setting-up-test-database)
    - [**(2:36:10) Automate test database restart \& migrations**](#23610-automate-test-database-restart--migrations)
    - [**(2:37:10) Using Dotenv CLI with Prisma**](#23710-using-dotenv-cli-with-prisma)
    - [**(2:44:30) Prisma database teardown logic**](#24430-prisma-database-teardown-logic)
    - [**(2:53:10) Auth e2e tests**](#25310-auth-e2e-tests)
    - [**(3:02:01) User e2e tests**](#30201-user-e2e-tests)
    - [**(3:13:20) Bookmarks e2e test**](#31320-bookmarks-e2e-test)
  - [Video](#video)
  - [References](#references)

---

## **Course Presentation: NestJS for Beginners**

This presentation follows the provided course timeline, drawing directly from the provided transcript to explain the core concepts and implementation steps for building a REST API with NestJS.

```bash
$> npm install -g yarn
$> Get-ExecutionPolicy -List
$> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
$> yarn --version // It works now in vscode, see References
```

### **(0:00:00) Course Introduction**

This course, taught by Vladimir, focuses on building a **production-ready CRUD REST API** using NestJS. The project incorporates essential real-world features, including **authentication, relational databases, and end-to-end (e2e) testing**. To follow along, a basic understanding of JavaScript (ES6) and TypeScript is required.

- CRUD REST API with NESTJS
- PRODUCTION ORIENTED (production-ready)
- E2E TESTS
- DEV DATABASE SETUP
- WITH JWT AUTHENTICATION
- BASIC JAVASCRIPT REQUIRED

### **(0:01:15) What is NestJS**

NestJS is a **Node.js backend framework** built with TypeScript. While it uses **Express** under the hood as an abstraction, it is significantly different because it provides a clear **architectural direction**. This solves the common "messy" structure issues found in large-scale Express projects.

- NODEJS BACKEND
- SCALABLE & MAINTAINABLE
- TYPESCRIPT FRIENDLY
- MODULAR STRUCTURE
- SOLVES ARCHITECTURE
- USES DEPENDENCY INJECTION
- USES EXPRESS JS
- "ANGULAR FOR BACKEND"

### **(0:02:35) Why use NestJS**

NestJS is often called the "**Angular for the backend**" because of its focus on modularity and dependency injection. It offers:

- **Native TypeScript support** and out-of-the-box functionality.
- **Scalability and maintainability** for growing projects.
- **Ease of hiring**, as developers don't need to learn a custom architecture for every new project.

- STRUCTURE
- MICROSERVICES
- MODULARITY
- REST API
- TYPESCRIPT
- DOCUMENTATION
- GRAPHQL
- POPULARITY / COMMUNITY
- [npmtrends nestjs](https://npmtrends.com/@adonisjs/core-vs-@feathersjs/feathers-vs-@hapi/hapi-vs-@nestjs/core-vs-koa-vs-meteor-vs-sails)

### **(0:03:35) What we are building**
The project is a **CRUD API for bookmarks**. Users will be able to create, read, update, and delete bookmark resources.

### **(0:03:55) NestJS project setup**
1.  **Install the CLI:** Use `npm install -g @nestjs/cli`.
2.  **Create Project:** Run `nest new project-name`.
3.  **Cleanup:** Remove default files like `app.controller.ts` and `app.service.ts` to start from a clean slate.
4.  **Dev Mode:** Run `yarn start:dev` to enable file watching and automatic recompilation.

### **(0:05:55) Modules**
Modules are classes annotated with the **`@Module` decorator**, which adds metadata describing the module's structure. They are the building blocks of NestJS, used to organize the application into **feature-based modules** (e.g., Auth, User, Bookmarks).

### **(0:15:30) Auth module**
The Auth module handles user creation and login. While the CLI can generate modules (`nest g module auth`), creating them manually at first helps build **muscle memory** regarding the decorator syntax and file naming conventions (e.g., `auth.module.ts`).

### **(0:19:50) Dependency injection**
Dependency Injection (DI) is a core NestJS concept where the framework handles the **instantiation and management** of classes. Instead of manually creating a `new AuthService()`, you simply define it in a controller's constructor. This makes dependency management significantly easier.

### **(0:20:30) Auth controller**
Controllers handle **incoming requests** and return responses. In the `AuthController`, decorators like **`@Post('signup')`** and **`@Post('signin')`** define the API endpoints.

### **(0:26:50) Setting up Postgres in Docker**
To avoid manual database installation, **Docker Compose** is used to spawn a **Postgres 13** container. The configuration includes environment variables for the user, password, and database name (e.g., "nest").

### **(0:29:10) Setting up Prisma**
Prisma is a modern **ORM/Query Builder**. 
*   **Installation:** Install `prisma` as a dev dependency and `@prisma/client` as a regular dependency.
*   **Initialization:** Run `npx prisma init` to generate a schema file and an environment variable file.

### **(0:32:10) User & bookmark models**
Models are defined in the `schema.prisma` file.
*   **User:** Contains fields like `email` (unique), `hash` (password), and optional names.
*   **Bookmark:** Includes a `title`, `link`, and a **many-to-one relationship** to a User.

### **(0:35:50) Running Prisma migrations**
Use `npx prisma migrate dev` to transform the schema into **SQL migrations**. This command also generates **TypeScript types** based on your models, allowing you to use `User` or `Bookmark` types directly in your code.

### **(0:40:10) Prisma module**
A dedicated `PrismaModule` is created to encapsulate the database connection. By adding the **`@Global()` decorator**, the `PrismaService` becomes available to all other modules in the app without needing to import it repeatedly.

### **(0:52:10) Using Auth DTOs**
**Data Transfer Objects (DTOs)** define the shape of data for requests. Using a class instead of an interface allows for **runtime validation**.

### **(0:57:45) NestJS pipes**
Pipes are functions used for **data transformation** or **validation**. They can convert strings to numbers or validate that an incoming object meets the DTO's requirements.

### **(0:59:10) NestJS global pipe**
By setting `app.useGlobalPipes(new ValidationPipe())` in `main.ts`, NestJS automatically validates all incoming requests against their DTOs. Setting **`whitelist: true`** in the pipe configuration strips out any fields not defined in the DTO, enhancing security.

### **(1:02:50) Hashing user password with Argon**
**Argon2** is used for hashing because it is considered superior to BCrypt, which is limited to the first 72 bytes of a password.

### **(1:03:20) Sign up logic**
The service hashes the password and creates a user record. If a duplicate email is provided, the code catches the **Prisma unique constraint error** and throws a `ForbiddenException`.

### **(1:13:13) Sign in logic**
1.  Find the user by unique email.
2.  If not found, throw an exception.
3.  Compare the provided password with the stored hash using `argon.verify`.
4.  Return the user (or token) if successful.

### **(1:16:50) Automate Postgres restart & Prisma migrations**
Custom scripts are added to `package.json` to:
*   **Remove and restart** the Docker container.
*   **Deploy migrations** automatically after the database is ready.

### **(1:27:40) NestJS config module**
The **`@nestjs/config`** module is used to load variables from `.env` files. This prevents hard-coding sensitive data like database URLs and JWT secrets.

### **(1:34:40) Using Passport.js & JWT module with NestJS**
NestJS integrates with **Passport.js** for authentication strategies.
*   **JWT Module:** Used to sign and decode tokens.
*   **Strategy:** A `JwtStrategy` class is created to extract the token from headers and verify its signature.

### **(1:55:10) Get current user with access token**
Once the strategy validates the token, the **user's identity** (from the payload) is appended to the request object. This allows the API to return the current user's profile.

### **(1:57:00) NestJS Guards**
Guards (using the **`@UseGuards` decorator**) protect routes by checking for a valid JWT before allowing access to an endpoint.

### **(2:12:10) NestJS custom param decorator**
Instead of manually accessing `req.user`, a custom **`@GetUser()`** decorator is created to cleanly extract the user object from the execution context.

### **(2:27:10) e2e tests with PactumJS**
**PactumJS** is a clean, descriptive library for testing REST APIs. Tests simulate user journeys, such as signing up and then signing in to get a token.

### **(2:35:10) Setting up test database**
A separate **test database container** is defined in Docker Compose on a different port (e.g., 5435) to ensure testing doesn't interfere with development data.

### **(2:36:10) Automate test database restart & migrations**
A `pretest` hook is used to restart and migrate the test database before running the e2e test suite.

### **(2:37:10) Using Dotenv CLI with Prisma**
Because Prisma doesn't natively manage multiple `.env` files well, **`dotenv-cli`** is used to inject the correct environment file (e.g., `.env.test`) into Prisma commands.

### **(2:44:30) Prisma database teardown logic**
A `cleanDb` function is implemented in the `PrismaService` using a **transaction** to ensure tables (Bookmarks, then Users) are cleared in the correct order before each test.

### **(2:53:10) Auth e2e tests**
Tests verify that:
*   Users can sign up and sign in.
*   Validation errors are thrown if fields are missing.
*   JWT tokens are correctly returned and stored for future requests.

### **(3:02:01) User e2e tests**
These tests ensure a user can retrieve their profile (`/users/me`) and **patch** their information using the `@GetUser` decorator and a valid JWT.

### **(3:13:20) Bookmarks e2e test**
The final tests cover the full **CRUD cycle** for bookmarks. It verifies that bookmarks are only accessible to the users who created them and that the **delete** and **update** functions work as intended.

---

## Video

* [NestJs Course for Beginners - Create a REST API](https://www.youtube.com/watch?v=GHTA143_b-s)
	> [<img src="https://img.youtube.com/vi/GHTA143_b-s/0.jpg" width="200">](https://www.youtube.com/watch?v=GHTA143_b-s "Learn NestJs by building a CRUD REST API with end-to-end tests using modern web development techniques. NestJs is a rapidly growing node js framework that helps build scalable and maintainable backend applications. by freeCodeCamp.org 1,960,119 views 3 hour, 42 minutes")

## References

1. https://docs.nestjs.com/
2. https://github.com/vladwulf/nestjs-api-tutorial
3. https://dev.to/devaugusto/como-solucionar-o-erro-execucao-de-scripts-desabilitada-neste-sistema-execution-scripts-is-disabled-on-this-system-error-25ch
4. https://koajs.com/
5. https://npmtrends.com/@hapi/hapi-vs-@nestjs/core-vs-koa-vs-next
