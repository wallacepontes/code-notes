# Dependency Management

## Dependency Management

As someone coming from a Java/Spring background where **Maven** and `pom.xml` provide a clear, centralized way to manage dependencies, you're likely finding Node.js's **npm** (Node Package Manager) dependency management a bit different. Since you're working on a serverless Node.js project for a telecom company in Brazil, I'll explain how to manage dependencies effectively in Node.js, clarify the difference between `-D` (`--save-dev`) and `-g` (`--global`), and provide best practices to gain control over your project's dependencies, drawing parallels to Maven where possible.

---

### 1. Understanding npm Dependency Management
In Java, `pom.xml` defines all dependencies (compile, runtime, test) in a single file, and Maven handles their download and versioning. In Node.js, **npm** uses `package.json` (and optionally `package-lock.json`) as the equivalent, central configuration file for managing project dependencies. Here's how it compares:

- **Maven (`pom.xml`) vs. npm (`package.json`)**:
  - `pom.xml`: Lists dependencies, their versions, and scopes (e.g., `compile`, `test`, `provided`).
  - `package.json`: Lists dependencies, their versions, and types (`dependencies` for runtime, `devDependencies` for development/test).
  - `package-lock.json`: Locks exact versions of dependencies and their sub-dependencies, similar to Maven’s dependency resolution but more explicit.

- **Dependency Installation**:
  - Maven: `mvn install` downloads dependencies to `~/.m2`.
  - npm: `npm install` downloads dependencies to `node_modules/` in your project directory (or globally for `-g`).

- **Key npm Commands**:
  - `npm install <package>`: Installs a package and saves it to `package.json` (if `--save` is implied or specified).
  - `npm install -D <package>` (or `--save-dev`): Installs a package as a **devDependency**, used for development/test tools (e.g., Jest, `serverless-offline`), like Maven’s `test` scope.
  - `npm install -g <package>` (or `--global`): Installs a package globally, not tied to a project, for CLI tools (e.g., `serverless`, `npm`), like installing Maven globally.

---

### 2. Clarifying `-D` vs. `-g`
Your use of `npm install -D` and `npm install -g` suggests some confusion about when to use each. Let’s break it down:

- **`npm install -D <package>` (or `--save-dev`)**:
  - Adds the package to `devDependencies` in `package.json`.
  - Used for tools needed during development or testing (e.g., `jest`, `serverless-offline`, `eslint`), not required in production.
  - Installed locally in `node_modules/` and included in your project.
  - Maven equivalent: `<scope>test</scope>` or tools like Surefire for testing.
  - Example:
    ```bash
    npm install -D jest
    ```
    Updates `package.json`:
    ```json
    "devDependencies": {
      "jest": "^29.7.0"
    }
    ```

- **`npm install -g <package>` (or `--global`)**:
  - Installs the package globally on your machine, typically in `~/.npm-global` (or system-specific paths).
  - Used for CLI tools you want to run from anywhere (e.g., `serverless`, `typescript`, `create-react-app`), not tied to a specific project.
  - Does **not** add to `package.json`, so it’s not tracked in your project.
  - Maven equivalent: Installing Maven or Gradle globally to run `mvn` or `gradle` commands.
  - Example:
    ```bash
    npm install -g serverless
    ```
    You can run `serverless` from any directory, but it’s not part of your project’s dependencies.

- **Key Difference**:
  - `-D`: Project-specific, tracked in `package.json`, included in builds (e.g., for CI/CD pipelines).
  - `-g`: System-wide, not tracked in your project, meant for tools you use across projects.

---

### 3. Best Practices to Control Dependencies in Node.js
To gain control over your dependencies in a way that feels as structured as Maven, follow these steps:

#### 3.1. Use `package.json` as Your Single Source of Truth
- Always initialize a project with `npm init` to create `package.json`.
- Add dependencies explicitly:
  - Runtime dependencies (like Spring’s `compile` scope):
    ```bash
    npm install express swagger-ui-express
    ```
    This adds to `dependencies` in `package.json`:
    ```json
    "dependencies": {
      "express": "^4.18.2",
      "swagger-ui-express": "^5.0.0"
    }
    ```
  - Development dependencies (like Spring’s `test` scope):
    ```bash
    npm install -D jest serverless-offline
    ```
    This adds to `devDependencies`:
    ```json
    "devDependencies": {
      "jest": "^29.7.0",
      "serverless-offline": "^12.0.4"
    }
    ```

- **Why?** Like `pom.xml`, `package.json` ensures all developers and CI/CD pipelines use the same dependencies. Avoid manual `node_modules/` edits.

#### 3.2. Minimize Global Installs (`-g`)
- Only use `-g` for CLI tools you need system-wide (e.g., `serverless`, `npm`, `typescript`).
- For project-specific tools, install locally as `devDependencies` and use **npm scripts** to run them.
  - Example: Instead of `npm install -g serverless`, install locally:
    ```bash
    npm install -D serverless
    ```
    Add a script to `package.json`:
    ```json
    "scripts": {
      "deploy": "serverless deploy",
      "offline": "serverless offline"
    }
    ```
    Run with:
    ```bash
    npm run deploy
    npm run offline
    ```
  - Maven equivalent: Running `mvn test` via a `pom.xml` plugin instead of a global Maven command.

- **Why?** Local installs ensure your project is self-contained, reproducible, and portable, just like a Maven project with a `pom.xml`.

#### 3.3. Use `package-lock.json` for Reproducible Builds
- When you run `npm install`, npm generates `package-lock.json`, which locks the exact versions of dependencies and their sub-dependencies.
- Commit `package-lock.json` to your Git repository.
- Run `npm ci` (instead of `npm install`) in CI/CD pipelines for deterministic installs, similar to Maven’s `mvn install` with a locked dependency tree.
- **Why?** Ensures consistent builds across environments (local, dev, qa, uat, prod), like Maven’s dependency resolution.

#### 3.4. Audit and Clean Up Dependencies
- **Check for outdated dependencies**:
  ```bash
  npm outdated
  ```
  Similar to `mvn versions:display-dependency-updates`.
- **Update dependencies**:
  ```bash
  npm update
  ```
  Or specify versions in `package.json` (e.g., `"express": "^4.18.2"` for minor updates, `"express": "4.18.2"` for exact).
- **Remove unused dependencies**:
  Use `npm uninstall <package>` to remove unneeded packages.
  Tool: `npm-check` or `depcheck` to find unused dependencies:
  ```bash
  npm install -g npm-check
  npm-check
  ```
- **Audit for security**:
  ```bash
  npm audit
  ```
  Fix vulnerabilities:
  ```bash
  npm audit fix
  ```
  Maven equivalent: `mvn dependency:check` or tools like OWASP Dependency-Check.

#### 3.5. Organize Dependencies for Serverless
- **Minimize `node_modules` size**: Serverless platforms like AWS Lambda have size limits (e.g., 50MB zipped for Lambda). Only include necessary `dependencies` in production.
  - Use `serverless.yml` to exclude `devDependencies`:
    ```yaml
    package:
      patterns:
        - '!node_modules/**'
        - 'node_modules/express/**'
        - 'node_modules/swagger-ui-express/**'
    ```
  - AWS SAM equivalent: Use `sam build` to package only required dependencies.
- **Layer dependencies** (AWS Lambda-specific): Move common dependencies to a Lambda Layer to reduce deployment package size, similar to shared libraries in a Java WAR.

#### 3.6. Local Development and Testing
- **Install dependencies locally**:
  ```bash
  npm install
  ```
  This installs all `dependencies` and `devDependencies` from `package.json` into `node_modules/`.
- **Test locally**:
  Use `serverless offline` (as shown earlier) or `sam local start-api` for AWS SAM to test your API.
- **Avoid global dependencies for project tools**:
  Instead of `npm install -g serverless`, install locally and use `npx`:
  ```bash
  npx serverless offline
  ```
  This runs the local version of Serverless Framework, ensuring consistency.

---

### 4. Example: Structured `package.json`
Here’s how your `package.json` might look for your serverless OpenAPI project, mirroring the earlier example:

```json
{
  "name": "serverless-openapi",
  "version": "1.0.0",
  "description": "Serverless API with OpenAPI for telecom project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest",
    "deploy": "serverless deploy --stage ${STAGE:-dev}",
    "offline": "serverless offline --stage local"
  },
  "dependencies": {
    "express": "^4.18.2",
    "swagger-ui-express": "^5.0.0",
    "swagger-jsdoc": "^6.2.8",
    "serverless-http": "^3.2.0"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "serverless": "^3.38.0",
    "serverless-offline": "^12.0.4",
    "express-validator": "^7.0.1"
  }
}
```

- **Scripts**: Define common tasks like Maven’s `mvn` goals (e.g., `test`, `deploy`).
- **Dependencies**: Runtime packages (e.g., `express`), like Maven’s `compile` scope.
- **devDependencies**: Development tools (e.g., `jest`, `serverless-offline`), like Maven’s `test` scope.

Run locally:
```bash
npm run offline
```
Test:
```bash
npm run test
```
Deploy:
```bash
STAGE=dev npm run deploy
```

---

### 5. Parallels with Maven
- **Dependency Declaration**:
  - Maven: `<dependencies>` in `pom.xml`.
  - npm: `dependencies` and `devDependencies` in `package.json`.
- **Dependency Resolution**:
  - Maven: Resolves transitive dependencies automatically, stored in `~/.m2`.
  - npm: Resolves transitive dependencies, stored in `node_modules/` and locked in `package-lock.json`.
- **Build Commands**:
  - Maven: `mvn install`, `mvn test`.
  - npm: `npm install`, `npm test`.
- **CI/CD**:
  - Maven: Configured in CI tools (e.g., Jenkins, GitHub Actions) to run `mvn deploy`.
  - npm: Configured to run `npm ci` and `npm run deploy`.

---

### 6. Alternatives for Dependency Management
If npm feels too loose compared to Maven, consider these tools to add structure:

- **Yarn**: An alternative to npm with faster installs and stricter dependency locking.
  - Install: `npm install -g yarn`
  - Use: `yarn add <package>`, `yarn add -D <package>`
  - Generates `yarn.lock` (like `package-lock.json`).
- **pnpm**: A disk-efficient package manager with strict dependency isolation.
  - Install: `npm install -g pnpm`
  - Use: `pnpm add <package>`, `pnpm add -D <package>`
  - Generates `pnpm-lock.yaml`.
- **npm Workspaces**: For managing multiple Node.js projects in a monorepo, similar to Maven multi-module projects.

For your telecom project, **npm** is sufficient unless you’re managing a complex monorepo, in which case **pnpm** or **Yarn** might be worth exploring.

---

### 7. Handling Your Telecom Project’s Needs
Given your project’s context (AWS, multi-stage, CI/CD pipeline):
- **Avoid `-g` for project tools**: Install `serverless`, `aws-sam-cli`, or other CLIs locally as `devDependencies` to ensure reproducibility in CI/CD.
- **Centralize in `package.json`**: List all dependencies explicitly, and commit `package-lock.json` to Git.
- **Pipeline integration**:
  - Use `npm ci` in your pipeline to install dependencies exactly as specified.
  - Example GitHub Actions step:
    ```yaml
    - run: npm ci
    - run: npm run test
    - run: npm run deploy
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        STAGE: dev
    ```
- **Local isolation**: Use `.env` files for local testing (e.g., `STAGE=local`) to avoid touching `dev` environment.
- **AWS SAM as alternative**: If Serverless Framework’s licensing is an issue, switch to SAM (as shown in my previous response) to avoid costs while keeping a similar workflow.

---

### 8. Common Pitfalls and Solutions
- **Global dependency conflicts**: Avoid `-g` for tools used in CI/CD. Use `npx` or local installs.
- **Large `node_modules`**: Use `serverless.yml` or SAM’s `sam build` to exclude `devDependencies` from Lambda packages.
- **Version mismatches**: Always commit `package-lock.json` to avoid version drift, like Maven’s dependency lock files.
- **Audit regularly**:
  ```bash
  npm audit
  npm outdated
  ```

---

### 9. Next Steps
- **Review your `package.json`**: Ensure all dependencies are listed correctly, with runtime in `dependencies` and tools in `devDependencies`.
- **Switch to local CLIs**: If you’ve used `npm install -g serverless`, uninstall (`npm uninstall -g serverless`) and install locally (`npm install -D serverless`).
- **Try SAM**: If licensing is a concern, set up AWS SAM (as shown in my previous response) for a free alternative.
- **Pipeline setup**: Share details about your CI/CD setup (e.g., AWS CodePipeline, GitHub Actions), and I can provide a tailored workflow.
