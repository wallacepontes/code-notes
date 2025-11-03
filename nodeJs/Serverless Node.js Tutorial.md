# Serverless Node.js Tutorial

## Table of Contents

- [Serverless Node.js Tutorial](#serverless-nodejs-tutorial)
  - [Table of Contents](#table-of-contents)
  - [Tech Stack](#tech-stack)
  - [A Step-by-Step Guide](#a-step-by-step-guide)
    - [Set Up Your Project](#set-up-your-project)
    - [Swagger + Node.js (Express)](#swagger--nodejs-express)
  - [🧩 The two module systems in Node.js](#-the-two-module-systems-in-nodejs)
    - [⚙️ Why you’re using `import / export` (ESM)](#️-why-youre-using-import--export-esm)
    - [✅ What you should use (since you’re in ESM mode)](#-what-you-should-use-since-youre-in-esm-mode)
    - [🚀 Alternative: go back to CommonJS (if you prefer `require`)](#-alternative-go-back-to-commonjs-if-you-prefer-require)
    - [TL;DR](#tldr)
  - [Install NeonDatabase Serverless](#install-neondatabase-serverless)
  - [Dotenv Issues](#dotenv-issues)
    - [🛠️ The Correct Fix: Use `--legacy-peer-deps`](#️-the-correct-fix-use---legacy-peer-deps)
      - [1. Run the Command with the Flag](#1-run-the-command-with-the-flag)
      - [2. Manual Configuration (Crucial Step)](#2-manual-configuration-crucial-step)
  - [Next](#next)
  - [Video](#video)
  - [References](#references)

## Tech Stack

- **Compute Service - AWS Lambda** : Run code without thinking about servers or clusters. Only pay for what you use.
- **Serverless** : Easy Serverless Apps on AWS Lambda.Deploy APIs, scheduled tasks, workflows and event-driven apps to AWS Lambda easily with the Serverless Framework.
  - Install (npm i serverless)
- **Express** : Fast, unopinionated, minimalist web framework for Node.js
- **Neon** :  Ship faster with Postgres. The database developers trust, on a serverless platform designed to help you build reliable and scalable applications faster.
- **Vercel** : Build and deploy on the AI Cloud. Vercel provides the developer tools and cloud infrastructure to build, scale, and secure a faster, more personalized web.
- **NodeJS** : Run JavaScript Everywhere. Node.js® is a free, open-source, cross-platform JavaScript runtime environment that lets developers create servers, web apps, command line tools and scripts.
  - Download (<https://nodejs.org/en/download>)
- **Github** : Let's build from here. The world's leading AI-powered developer platform.
- **AWS IAM** : AWS Identity and Access Management (IAM) is a web service for securely controlling access to AWS services. With IAM, you can centrally manage users, security credentials such as access keys, and permissions that control which AWS resources users and applications can access.
- **Visual Studio Code** : The open source AI code editor
- **PostgreSQL** : PostgreSQL: The World's Most Advanced Open Source Relational Database
- **@neondatabase/serverless** (Weekly Downloads 735.647): Neon's PostgreSQL driver for JavaScript and TypeScript. It's:
  - Low-latency, thanks to message pipelining and other optimizations
  - Ideal for serverless/edge deployment, using https and WebSockets in place of TCP
  - A drop-in replacement for node-postgres, aka pg (on which it's based)
- **neonctl** (Weekly Downloads 40.773) : The Neon CLI is a command-line interface that lets you manage Neon Serverless Postgres directly from the terminal. For the complete documentation, see Neon CLI.
- **drizzle-orm** (Weekly Downloads 2.400.624): Drizzle is a modern TypeScript ORM developers wanna use in their next project. It is lightweight at only ~7.4kb minified+gzipped, and it's tree shakeable with exactly 0 dependencies.
- **Drizzle Kit** (Weekly Downloads 1.820.319) : Drizzle Kit is a CLI migrator tool for Drizzle ORM. It is probably the one and only tool that lets you completely automatically generate SQL migrations and covers ~95% of the common cases like deletions and renames by prompting user input.
- **dotenv** (Weekly Downloads 57.037.835) : Dotenv is a zero-dependency module that loads environment variables from a .env file into process.env. Storing configuration in the environment separate from code is based on The Twelve-Factor App methodology.
- **Serverless Dotenv Plugin** : Preload environment variables from `.env` into serverless.

## A Step-by-Step Guide

### Set Up Your Project

- **(1) Check your environment, install nvm**

```bash
$ node --version
v20.12.2
$ npm --version
10.5.0
```

- **(2) Install the Serverless**

```bash
$ serverless -> aws lambda
bash: serverless: command not found
# Using the -g flag with npm install installs a package globally on your system.
$ npm install -g serverless
added 71 packages in 11s
20 packages are looking for funding
  run `npm fund` for details
# we're going to create a new serverless project
$ serverless
bash: serverless: command not found
# This is a common issue! 
# The reason is that the directory where npm installed the serverless executable is not included in your system's PATH environment variable.
$ npm root -g
C:\Users\"Your User"\AppData\Roaming\npm\node_modules
$ npm prefix -g
C:\Users\"Your User"\AppData\Roaming\npm
```

- **(3) First, create a new Node.js project with Serverless**

```bash
#For Windows (if using a standard command prompt/PowerShell):
#You'll need to manually edit your System Environment Variables and add the global npm directory (usually %AppData%\npm) to your user or system PATH.
$ serverless
Serverless ϟ Framework

Welcome to Serverless Framework V.4

Create a new project by selecting a Template to generate scaffolding for a specific use-case.

? Select A Template: ...
  AWS / Node.js / HTTP API
> AWS / Node.js / Express API
  AWS / Node.js / Express API with DynamoDB
  AWS / Node.js / Scheduled Task
  AWS / Node.js / Simple Function
  AWS / Python / HTTP API
  AWS / Python / Flask API
  AWS / Python / Flask API with DynamoDB
  AWS / Python / Scheduled Task
  AWS / Python / Simple Function
  AWS / Compose / Serverless + Cloudformation + SAM

$ cd <folder-created>
# Start the VSCode
$ code .
```

- **(4) In VSCode, save as workspace and start `git init`**

  - In VSCode, save as a workspace (File - Save as...)
  - git init

### Swagger + Node.js (Express)

- **(1) Install dependencies**

```bash
npm install swagger-ui-express swagger-jsdoc
```

- **(2) Modify the index file**

```js
const serverless = require("serverless-http");
const express = require("express");
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require("swagger-ui-express");

const app = express();

// Swagger definition
const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Serverless API',
      version: '1.0.0',
      description: 'A simple serverless API with OpenAPI',
    },
    servers: [
      { url: "http://localhost:3000", description: "Local Express" },
      { url: "http://localhost:3000/dev", description: "Serverless Offline" },
      { url: "http://localhost:3000/dev/api-docs", description: "Serverless Offline Swagger" },
    ],
  },
  apis: ['./src/sales/routes.js'], // Path to files with JSDoc comments
};

const swaggerDocs = swaggerJsdoc(swaggerOptions);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocs));
app.use("/dev/api-docs", swaggerUi.serve, swaggerUi.setup(swaggerDocs));

app.get("/", (req, res, next) => {
  return res.status(200).json({
    message: "Hello from root!",
  });
});

// Sample API endpoint
app.get("/hello", (req, res, next) => {
  return res.status(200).json({
    message: "Hello from path!",
  });
});

// 404 fallback
app.use((req, res, next) => {
  return res.status(404).json({
    error: "Not Found",
  });
});

app.listen(3000,()=>{
 console.log("running at http://localhost:3000")
})

exports.handler = serverless(app);
```

- **(3) Create Your Routes**

```js
module.exports = (app) => {
  /**
   * @swagger
   * /api/user:
   *   get:
   *     summary: Retrieve a list of users
   *     responses:
   *       200:
   *         description: A list of users
   *         content:
   *           application/json:
   *             schema:
   *               type: array
   *               items:
   *                 type: object
   *                 properties:
   *                   id:
   *                     type: integer
   *                     example: 1
   *                   name:
   *                     type: string
   *                     example: John Doe
   */
  app.get("/api/user", (req, res) => {
    console.log("[Express] Acessando /api/user");
    res.json([{ id: 1, name: "John Doe" }]);
  });
};
```

- **(5) Run your Server**

```bash
## Not work for serverless yet
$ node index.js
```

**server-full app** : Add this in the index.js to act as server-full

```js
app.listen(3000,()=>{
console.log("running at http://localhost:3000")
})
```

- **(6) Access Swagger UI**

Open your browser and navigate to <http://localhost:3000/api-docs>.

```bash
curl http://localhost:3000
curl http://localhost:3000/hello
```

- **(7) Update the package.json**

  - **Add between description and dependencies**

    ```json
    {"scripts": {
      "dev": "serverless offline --stage dev"
    }}
    ```

  - **Run in terminal**

  ```bash
  npm run dev
  ```

  - **Or run directly**

```bash
npm run serverless offline --stage dev
```

- **(8) Clean project dependencies**

```bash
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

## 🧩 The two module systems in Node.js

| Type                 | Syntax                       | Example                                                     | Typical file type                                            |
| -------------------- | ---------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| **CommonJS (CJS)**   | `require` / `module.exports` | `const express = require('express'); module.exports = app;` | `.js` (default before Node 14)                               |
| **ES Modules (ESM)** | `import` / `export`          | `import express from 'express'; export default app;`        | `.mjs` or `.js` **if** `"type": "module"` is in package.json |

---

### ⚙️ Why you’re using `import / export` (ESM)

Your project likely has this line in your **package.json**:

```json
"type": "module"
```

This tells Node:

> “Interpret all `.js` files as ES modules.”

That means Node expects **ESM syntax** everywhere.
So things like this will break:

```js
const express = require('express'); // ❌ Error in ESM
exports.handler = serverless(app);  // ❌ 'exports' is not defined
```

Because in ESM, there’s **no `require`, `module.exports`, or `exports`** — they belong to the old CommonJS system.

### ✅ What you should use (since you’re in ESM mode)

Use `import` and `export` consistently:

```js
import express from 'express';
import serverless from 'serverless-http';

const app = express();

// your routes...
app.get('/', (req, res) => res.send('Hello'));

const handler = serverless(app);
export { handler };
export default app;
```

### 🚀 Alternative: go back to CommonJS (if you prefer `require`)

If you’d rather use the old syntax:

1. Remove or change this line in your **package.json**:

   ```json
   "type": "module"
   ```

2. Then rewrite your code:

   ```js
   const express = require('express');
   const serverless = require('serverless-http');
   const app = express();

   exports.handler = serverless(app);
   module.exports = app;
   ```

### TL;DR

  - `"type": "module"` → forces Node to use **ESM** (`import/export`)
  - Without it → Node uses **CommonJS** (`require/module.exports`)
  - You can only use **one system consistently per file**

## Install NeonDatabase Serverless

```bash
$ npm i @neondatabase/serverless
$ npm install -g neonctl
$ neon auth
#INFO: Awaiting authentication in web browser.
$ npm i drizzle-orm
# The flag -D or --save-dev with npm install installs a package as a development dependency.
$ npm i -D drizzle-kit

$ node index.js
```

**Neon CLI** : Would like permission to access your Wallace de Pontes account and perform the following actions on your behalf:

- create projects
- read projects
- modify projects
- delete projects
- create organizations
- read organizations
- modify organizations
- delete organizations
- modify organization permissions

## Dotenv Issues

The direct `npm i -D serverless-dotenv-plugin` command **still failed** for the same reason: **incompatible Serverless Framework versions**.

The error clearly states:

- Your project uses **`serverless@4.22.0`**.
- The `serverless-dotenv-plugin@6.0.0` requires **`serverless@"1 || 2 || pre-3 || 3"`** (meaning versions 1, 2, or 3).

The key is that the plugin version you're trying to install **does not support Serverless v4**.

-----

### 🛠️ The Correct Fix: Use `--legacy-peer-deps`

Since a direct install of the default version failed, you should move to the next recommended solution, which is to **force NPM to ignore the peer dependency version check** using the `--legacy-peer-deps` flag.

This is the cleanest workaround for a known conflict between a project's new major version (Serverless v4) and an older plugin's compatibility manifest.

#### 1\. Run the Command with the Flag

Execute the following command in your terminal:

```bash
npm i -D serverless-dotenv-plugin --legacy-peer-deps
```

This will force the plugin to install as a development dependency, despite the version mismatch warning.

-----

#### 2\. Manual Configuration (Crucial Step)

Because you bypassed the standard `serverless plugin install` command, you **must manually register the plugin** in your configuration file.

Open your **`serverless.yml`** file and ensure the plugin is listed under the `plugins:` section:

```yaml
# serverless.yml

plugins:
  - serverless-dotenv-plugin # <--- Make sure this line is present
  - serverless-offline
```

After performing these steps, the plugin should be installed and recognized by your Serverless Framework. If this still results in errors, it means the plugin simply hasn't been updated to work with the major architectural changes in Serverless v4, and you'll need to **search for an alternative plugin** or method to handle environment variables.

## Next

## Video

- [Serverless Node.js Tutorial – Neon Serverless Postgres, AWS Lambda, Next.js, Vercel](https://www.youtube.com/watch?v=cxgAN7T3rq8)
 > [<img src="https://img.youtube.com/vi/cxgAN7T3rq8/0.jpg" width="200">](https://www.youtube.com/watch?v=cxgAN7T3rq8 "Learn how to code an Express.js and Node.js app running on AWS Lambda, leveraging Neon Serverless Postgres (https://neon.tech/education) and the Serverless Framework. This course will also teach you how to deploy to Vercel. by freeCodeCamp.org 100K views 4 hours")

## References

1. <https://aws.amazon.com/pt/lambda/>
2. <https://www.serverless.com/>
3. <https://expressjs.com/>
4. <https://neon.com/>
5. <https://vercel.com/>
6. <https://nodejs.org/en/>
7. <https://github.com/>
8. <https://docs.aws.amazon.com/iam/>
9. <https://code.visualstudio.com/>
10. <https://www.npmjs.com/package/serverless>
11. <https://www.postgresql.org/>
12. <https://github.com/brianc/node-postgres>
13. <https://www.npmjs.com/package/@neondatabase/serverless>
14. <https://www.npmjs.com/package/drizzle-orm>
15. <https://www.npmjs.com/package/drizzle-kit>
16. <https://www.npmjs.com/package/neonctl>
17. <https://github.com/drizzle-team/drizzle-orm>
18. <https://www.npmjs.com/package/dotenv>
19. <https://12factor.net/config>
20. <https://www.serverless.com/plugins/serverless-dotenv-plugin>
21. <https://dev.to/cuongnp/swagger-nodejs-express-a-step-by-step-guide-4ob>
22. <https://github.com/swagger-autogen/swagger-autogen#>
23.
