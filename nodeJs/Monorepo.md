# Monorepo

## Table of Contents
- [Monorepo](#monorepo)
  - [Table of Contents](#table-of-contents)
  - [🚀 What You’ll Learn](#-what-youll-learn)
  - [🧩 1. What Is a Monorepo?](#-1-what-is-a-monorepo)
  - [🏗️ 2. Setting Up the Project Structure](#️-2-setting-up-the-project-structure)
    - [Step 1 — Create the root folder](#step-1--create-the-root-folder)
    - [Step 2 — Initialize with npm workspaces](#step-2--initialize-with-npm-workspaces)
  - [🧱 3. Create the App and Packages](#-3-create-the-app-and-packages)
    - [Create folders](#create-folders)
    - [Initialize each project](#initialize-each-project)
  - [🧠 4. Build a Shared Utils Package](#-4-build-a-shared-utils-package)
  - [⚙️ 5. Create an API Using Express](#️-5-create-an-api-using-express)
    - [Update `apps/api/package.json`](#update-appsapipackagejson)
  - [🧪 6. Run the App](#-6-run-the-app)
  - [🧰 7. Add a Build Tool (Turborepo or Nx)](#-7-add-a-build-tool-turborepo-or-nx)
    - [Option 1: Turborepo](#option-1-turborepo)
  - [🧩 8. Share TypeScript Types (optional)](#-8-share-typescript-types-optional)
  - [🧾 9. Common Alternatives](#-9-common-alternatives)
  - [✅ 10. Summary](#-10-summary)
- [🧠 Part 2 — TypeScript + Jest in a Node.js Monorepo](#-part-2--typescript--jest-in-a-nodejs-monorepo)
  - [🗂 1. Final Structure](#-1-final-structure)
  - [⚙️ 2. Install Dependencies](#️-2-install-dependencies)
  - [🧩 3. Base TypeScript Configuration](#-3-base-typescript-configuration)
  - [🧱 4. TypeScript Setup for Each Package](#-4-typescript-setup-for-each-package)
    - [`packages/utils/tsconfig.json`](#packagesutilstsconfigjson)
    - [`apps/api/tsconfig.json`](#appsapitsconfigjson)
  - [✍️ 5. Add Some TypeScript Code](#️-5-add-some-typescript-code)
    - [`packages/utils/src/math.ts`](#packagesutilssrcmathts)
    - [`packages/utils/src/index.ts`](#packagesutilssrcindexts)
  - [🚀 6. Update the API App](#-6-update-the-api-app)
    - [`apps/api/src/index.ts`](#appsapisrcindexts)
    - [`apps/api/package.json`](#appsapipackagejson)
  - [🧠 7. Add Jest Testing Support](#-7-add-jest-testing-support)
    - [Add a test in `packages/utils/tests/math.test.ts`:](#add-a-test-in-packagesutilstestsmathtestts)
  - [🧪 8. Add Build \& Test Scripts](#-8-add-build--test-scripts)
  - [🧰 9. Optional — Turborepo Optimization](#-9-optional--turborepo-optimization)
  - [✅ 10. Summary](#-10-summary-1)
- [Resumo](#resumo)
  - [Structure:](#structure)
  - [Key Concepts and Benefits:](#key-concepts-and-benefits)
  - [Example Configuration (using npm workspaces):](#example-configuration-using-npm-workspaces)
- [Monorepo](#monorepo-1)
  - [🧩 1. What Is a Monorepo?](#-1-what-is-a-monorepo-1)
    - [✅ Benefits:](#-benefits)
    - [⚠️ Challenges:](#️-challenges)
  - [⚙️ 2. Choosing a Tool](#️-2-choosing-a-tool)
  - [🏗️ 3. Setting Up a Monorepo Using Yarn Workspaces](#️-3-setting-up-a-monorepo-using-yarn-workspaces)
    - [Step 1: Create the root folder](#step-1-create-the-root-folder)
    - [Step 2: Enable Workspaces](#step-2-enable-workspaces)
    - [Step 3: Create your folders](#step-3-create-your-folders)
  - [🧱 4. Add Example Projects](#-4-add-example-projects)
    - [Example: React app (`apps/web`)](#example-react-app-appsweb)
    - [Example: Utility library (`packages/utils`)](#example-utility-library-packagesutils)
  - [🔗 5. Link Dependencies Between Projects](#-5-link-dependencies-between-projects)
  - [🚀 6. Add Turborepo (optional, for speed and tasks)](#-6-add-turborepo-optional-for-speed-and-tasks)
  - [🧪 7. Example Project Structure (Final)](#-7-example-project-structure-final)
  - [🧰 8. Advanced Add-ons](#-8-advanced-add-ons)
  - [📘 9. Example Commands](#-9-example-commands)
  - [🎯 10. Summary](#-10-summary-2)
  - [References](#references)


Here’s a **complete step-by-step tutorial** to get you started with a **Node.js monorepo** — a single repository that holds multiple packages or services (e.g., microservices, shared libraries, utilities).

---

## 🚀 What You’ll Learn

* What a monorepo is and why use it
* How to structure one for Node.js
* How to manage dependencies and builds
* How to share code between packages
* How to automate scripts using tools like **Turborepo**, **Nx**, or **Lerna**

---

## 🧩 1. What Is a Monorepo?

A **monorepo** (monolithic repository) is a single codebase that holds multiple projects, such as:

```
apps/
  api/
  web/
packages/
  shared/
  utils/
```

Benefits:

* Shared code (utilities, configs, types)
* Easier refactors
* Centralized version control
* Unified build/test pipeline

Drawbacks:

* Larger repository
* Build time can grow (needs caching)
* Requires tooling to manage dependencies efficiently

---

## 🏗️ 2. Setting Up the Project Structure

Let’s build a simple Node.js monorepo with:

* `api` (Express server)
* `utils` (shared functions)

### Step 1 — Create the root folder

```bash
mkdir node-monorepo
cd node-monorepo
```

### Step 2 — Initialize with npm workspaces

```bash
npm init -y
```

Edit the **root `package.json`** to enable workspaces:

```json
{
  "name": "node-monorepo",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}
```

---

## 🧱 3. Create the App and Packages

### Create folders

```bash
mkdir -p apps/api packages/utils
```

### Initialize each project

```bash
cd apps/api && npm init -y
cd ../../packages/utils && npm init -y
```

---

## 🧠 4. Build a Shared Utils Package

In `packages/utils/index.js`:

```js
function greet(name) {
  return `Hello, ${name}!`;
}

module.exports = { greet };
```

---

## ⚙️ 5. Create an API Using Express

In `apps/api/index.js`:

```js
const express = require('express');
const { greet } = require('@node-monorepo/utils'); // name from package.json

const app = express();

app.get('/', (req, res) => {
  res.send(greet('Node Monorepo'));
});

app.listen(3000, () => console.log('API running on port 3000'));
```

### Update `apps/api/package.json`

Make sure dependencies include both Express and your local package:

```json
{
  "name": "@node-monorepo/api",
  "version": "1.0.0",
  "main": "index.js",
  "dependencies": {
    "express": "^4.18.2",
    "@node-monorepo/utils": "*"
  }
}
```

Now run:

```bash
npm install
```

> 🧠 Note: npm will automatically link `@node-monorepo/utils` into `apps/api/node_modules`.

---

## 🧪 6. Run the App

From the project root:

```bash
node apps/api/index.js
```

Visit 👉 [http://localhost:3000](http://localhost:3000)
You should see:

```
Hello, Node Monorepo!
```

---

## 🧰 7. Add a Build Tool (Turborepo or Nx)

For better organization, caching, and parallel tasks.

### Option 1: Turborepo

```bash
npm install turbo --save-dev
```

Add `turbo.json`:

```json
{
  "pipeline": {
    "build": { "dependsOn": ["^build"], "outputs": ["dist/**"] },
    "dev": { "cache": false }
  }
}
```

Add scripts to the root `package.json`:

```json
{
  "scripts": {
    "dev": "turbo run dev --parallel",
    "build": "turbo run build"
  }
}
```

Then in `apps/api/package.json`:

```json
"scripts": {
  "dev": "node index.js"
}
```

Now run:

```bash
npm run dev
```

---

## 🧩 8. Share TypeScript Types (optional)

If you’re using TypeScript, you can add a `tsconfig.base.json` at the root and extend it in all projects.

Example `tsconfig.base.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@node-monorepo/utils": ["packages/utils/src"]
    }
  }
}
```

Then in each project’s `tsconfig.json`:

```json
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "outDir": "dist"
  }
}
```

---

## 🧾 9. Common Alternatives

| Tool                | Description                                             |
| ------------------- | ------------------------------------------------------- |
| **Turborepo**       | Fast task runner with caching & parallel builds         |
| **Nx**              | More opinionated, powerful project graph and generators |
| **Lerna**           | Classic monorepo manager for versioning and publishing  |
| **pnpm workspaces** | Lightweight, good for large dependency trees            |

---

## ✅ 10. Summary

You now have:

* A monorepo structure for Node.js
* Shared code via npm workspaces
* A working Express API
* Optional build orchestration via Turborepo

Your structure should look like this:

```
node-monorepo/
├── apps/
│   └── api/
│       ├── index.js
│       └── package.json
├── packages/
│   └── utils/
│       ├── index.js
│       └── package.json
├── package.json
└── turbo.json
```

---


Perfect 👍 Let’s now extend your **Node.js monorepo** into a **production-ready setup** using **TypeScript** and **Jest** — while keeping everything clean and reusable.

---

# 🧠 Part 2 — TypeScript + Jest in a Node.js Monorepo

We’ll evolve from the previous structure using **npm workspaces** (or Turborepo) and add:

* ✅ TypeScript compilation per package
* ✅ Shared type definitions
* ✅ Unit testing with Jest

---

## 🗂 1. Final Structure

Here’s the structure we’ll reach by the end:

```
node-monorepo/
├── apps/
│   └── api/
│       ├── src/
│       │   └── index.ts
│       ├── package.json
│       └── tsconfig.json
├── packages/
│   └── utils/
│       ├── src/
│       │   ├── index.ts
│       │   └── math.ts
│       ├── tests/
│       │   └── math.test.ts
│       ├── package.json
│       └── tsconfig.json
├── tsconfig.base.json
├── package.json
├── turbo.json
└── jest.config.js
```

---

## ⚙️ 2. Install Dependencies

From the **project root**:

```bash
npm install typescript ts-node @types/node --save-dev
npm install jest ts-jest @types/jest --save-dev
npm install express @types/express
```

---

## 🧩 3. Base TypeScript Configuration

At the root, create **`tsconfig.base.json`**:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "dist",
    "baseUrl": ".",
    "paths": {
      "@node-monorepo/utils": ["packages/utils/src"]
    }
  }
}
```

This sets shared defaults and path aliases for imports.

---

## 🧱 4. TypeScript Setup for Each Package

### `packages/utils/tsconfig.json`

```json
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist"
  },
  "include": ["src"]
}
```

### `apps/api/tsconfig.json`

```json
{
  "extends": "../../tsconfig.base.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "dist"
  },
  "include": ["src"]
}
```

---

## ✍️ 5. Add Some TypeScript Code

### `packages/utils/src/math.ts`

```ts
export function sum(a: number, b: number): number {
  return a + b;
}
```

### `packages/utils/src/index.ts`

```ts
export * from './math';

export function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

---

## 🚀 6. Update the API App

### `apps/api/src/index.ts`

```ts
import express from 'express';
import { greet, sum } from '@node-monorepo/utils';

const app = express();

app.get('/', (_req, res) => {
  res.send(greet('Node Monorepo with TypeScript!'));
});

app.get('/sum', (_req, res) => {
  res.json({ result: sum(10, 5) });
});

app.listen(3000, () => {
  console.log('🚀 API running on http://localhost:3000');
});
```

### `apps/api/package.json`

```json
{
  "name": "@node-monorepo/api",
  "version": "1.0.0",
  "main": "dist/index.js",
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc -p tsconfig.json"
  },
  "dependencies": {
    "express": "^4.18.2",
    "@node-monorepo/utils": "*"
  }
}
```

---

## 🧠 7. Add Jest Testing Support

At the **root**, create **`jest.config.js`**:

```js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  moduleNameMapper: {
    '^@node-monorepo/utils(.*)$': '<rootDir>/packages/utils/src$1'
  }
};
```

### Add a test in `packages/utils/tests/math.test.ts`:

```ts
import { sum } from '../src/math';

describe('math utils', () => {
  it('should sum two numbers', () => {
    expect(sum(2, 3)).toBe(5);
  });
});
```

---

## 🧪 8. Add Build & Test Scripts

In the **root `package.json`**:

```json
{
  "scripts": {
    "dev": "turbo run dev --parallel",
    "build": "turbo run build",
    "test": "jest"
  }
}
```

And in **`packages/utils/package.json`**:

```json
{
  "name": "@node-monorepo/utils",
  "version": "1.0.0",
  "main": "dist/index.js",
  "scripts": {
    "build": "tsc -p tsconfig.json",
    "test": "jest"
  }
}
```

Now you can run:

```bash
npm run test
```

Output ✅

```
 PASS  packages/utils/tests/math.test.ts
  math utils
    ✓ should sum two numbers (3 ms)
```

---

## 🧰 9. Optional — Turborepo Optimization

Your **turbo.json** can look like:

```json
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**"]
    },
    "test": {
      "dependsOn": ["build"],
      "outputs": []
    },
    "dev": {
      "cache": false
    }
  }
}
```

Then:

```bash
npm run build
npm run dev
npm run test
```

---

## ✅ 10. Summary

You now have a **fully working Node.js monorepo** with:

* 🧱 Modular project structure
* ⚙️ TypeScript compilation
* 🧪 Jest testing
* 🚀 Shared utils and TypeScript paths
* ⚡ Turborepo task caching (optional)

**Final structure:**

```
node-monorepo/
├── apps/
│   └── api/
│       ├── src/
│       │   └── index.ts
│       ├── package.json
│       └── tsconfig.json
├── packages/
│   └── utils/
│       ├── src/
│       │   ├── index.ts
│       │   └── math.ts
│       ├── tests/
│       │   └── math.test.ts
│       ├── package.json
│       └── tsconfig.json
├── tsconfig.base.json
├── turbo.json
├── jest.config.js
└── package.json
```
---

# Resumo

In a Node.js monorepo, the packages folder serves as a central location to house individual projects or modules that are part of the larger monorepo structure. This setup allows for efficient code sharing, dependency management, and streamlined development workflows across related projects.
Here's a breakdown of how the packages folder typically functions within a Node.js monorepo:

## Structure:

- Root Directory: The monorepo has a root directory containing a package.json file for the entire workspace, and potentially configuration files for monorepo tools like Lerna or Turborepo.
- packages Folder: This folder resides within the root directory and contains subdirectories, each representing a distinct package or application.
  - Individual Package Directories: Each subdirectory within packages holds a separate Node.js project (e.g., a library, a web application, an API service). Each of these individual package directories will have its own package.json file defining its specific dependencies and scripts.

## Key Concepts and Benefits:

- Workspaces: Monorepo tools like npm workspaces, Yarn workspaces, or pnpm workspaces leverage the packages folder to define the scope of the workspace. This allows a single node_modules directory at the root level to manage dependencies for all packages, reducing duplication and installation time.
- Code Sharing: Packages within the packages folder can easily depend on and import code from other internal packages, promoting code reuse and consistency across the monorepo.
- Centralized Dependency Management: Dependencies are often hoisted to the root node_modules by the package manager, simplifying dependency management and ensuring consistent versions across the monorepo.
- Simplified Development: Developers can work on multiple related projects within a single repository, making it easier to manage changes, test interactions, and deploy integrated solutions.
- Tools: Monorepo-specific tools like Lerna, Turborepo, Nx, or Rush.js are commonly used to manage tasks, builds, and releases across the packages within the packages folder.

## Example Configuration (using npm workspaces):

In your root package.json, you would define the workspaces field to point to your packages folder:
Code

```json
{
  "name": "my-monorepo",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    // ... root-level scripts
  }
}
```

This configuration tells npm to treat all subdirectories within the packages folder as individual workspaces, allowing for streamlined dependency management and script execution across your monorepo.

# Monorepo

Absolutely! Let’s go step-by-step through a **“Getting Started with Monorepo”** tutorial — covering what it is, why use it, and how to set it up using modern tools like **Nx**, **Turborepo**, or **Yarn Workspaces**.

---

## 🧩 1. What Is a Monorepo?

A **Monorepo** (monolithic repository) is a single Git repository that holds the code for **multiple projects** — which might include apps, services, or shared libraries.

For example:

```
/monorepo
 ├── apps/
 │   ├── web/
 │   └── api/
 └── packages/
     ├── ui/
     └── utils/
```

### ✅ Benefits:

* Shared dependencies and configurations.
* Easier code sharing (common libraries).
* Simplified refactoring across projects.
* Single source of truth for CI/CD.

### ⚠️ Challenges:

* Build and test performance (requires smart tooling).
* Dependency management can get complex.
* Requires tooling discipline.

---

## ⚙️ 2. Choosing a Tool

Several tools can help manage monorepos efficiently:

| Tool                | Language Focus                     | Features                                               |
| ------------------- | ---------------------------------- | ------------------------------------------------------ |
| **Nx**              | Full-stack (JS/TS, Java, Go, etc.) | Smart caching, dependency graph, generators            |
| **Turborepo**       | JS/TS                              | Fast builds, pipelines, remote caching                 |
| **Yarn Workspaces** | JS/TS                              | Simple dependency linking                              |
| **Lerna**           | JS/TS                              | Versioning, publishing, works with Yarn/NPM workspaces |

For simplicity, let’s start with **Yarn Workspaces** + optional **Turborepo**.

---

## 🏗️ 3. Setting Up a Monorepo Using Yarn Workspaces

### Step 1: Create the root folder

```bash
mkdir my-monorepo && cd my-monorepo
yarn init -y
```

### Step 2: Enable Workspaces

Edit your `package.json`:

```json
{
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}
```

### Step 3: Create your folders

```bash
mkdir -p apps/web apps/api packages/ui packages/utils
```

---

## 🧱 4. Add Example Projects

### Example: React app (`apps/web`)

```bash
cd apps/web
yarn init -y
yarn add react react-dom
```

### Example: Utility library (`packages/utils`)

```bash
cd ../../packages/utils
yarn init -y
echo "module.exports = { sum: (a,b) => a+b };" > index.js
```

---

## 🔗 5. Link Dependencies Between Projects

If `web` depends on `utils`, just add this to `apps/web/package.json`:

```json
"dependencies": {
  "utils": "1.0.0"
}
```

Then run:

```bash
yarn install
```

Yarn automatically links `packages/utils` into `apps/web/node_modules/utils`.

---

## 🚀 6. Add Turborepo (optional, for speed and tasks)

If you want to manage build/test pipelines efficiently:

```bash
yarn add -D turbo
```

Create `turbo.json`:

```json
{
  "$schema": "https://turbo.build/schema.json",
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**"]
    },
    "lint": {},
    "test": {}
  }
}
```

Now you can run:

```bash
yarn turbo run build
```

It will:

* Build dependencies first.
* Cache results (locally or remotely).
* Rebuild only changed projects.

---

## 🧪 7. Example Project Structure (Final)

```
my-monorepo/
 ├── package.json
 ├── turbo.json
 ├── apps/
 │   ├── web/
 │   │   ├── package.json
 │   │   └── src/
 │   └── api/
 └── packages/
     ├── ui/
     └── utils/
```

---

## 🧰 8. Advanced Add-ons

| Feature                 | Tool                        | Description                             |
| ----------------------- | --------------------------- | --------------------------------------- |
| Linting & Formatting    | ESLint + Prettier           | Apply shared config across all packages |
| TypeScript project refs | TS `paths` + `references`   | Improves type safety between packages   |
| Testing                 | Jest                        | Each package can have its own config    |
| CI/CD                   | GitHub Actions or GitLab CI | Run per-project pipelines using cache   |
| Versioning              | Lerna or Changesets         | For publishing packages to npm          |

---

## 📘 9. Example Commands

| Command                             | Description                  |
| ----------------------------------- | ---------------------------- |
| `yarn workspaces list`              | List all workspaces          |
| `yarn workspace web add lodash`     | Add dependency only to `web` |
| `yarn turbo run build --filter=web` | Build only `web` project     |
| `yarn lint`                         | Run shared lint command      |

---

## 🎯 10. Summary

A **monorepo** is a powerful structure for multi-project teams.
Start simple with **Yarn Workspaces**, then add **Turborepo** or **Nx** for scalable pipelines and dependency graphs.

---

Would you like me to extend this tutorial with a **TypeScript + Turborepo** example (including `tsconfig.json` setup and shared interfaces)?
That’s the most common real-world monorepo setup for Node and frontend teams.


## References
1. https://www.youtube.com/watch?v=gpWDZir8dAA
2. https://www.youtube.com/watch?v=TeOSuGRHq7k
3. https://www.youtube.com/watch?v=yFmE-NFYjIc
4. https://medium.com/@svyatogor/managing-and-deploying-nodejs-monorepo-project-b836b2fa61de
5. 