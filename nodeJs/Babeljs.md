# BabelJs

## Table of Contents
- [BabelJs](#babeljs)
  - [Table of Contents](#table-of-contents)
  - [🧠 What is Babel.js?](#-what-is-babeljs)
  - [🚀 Step 1: Initialize your Node.js project](#-step-1-initialize-your-nodejs-project)
  - [🧩 Step 2: Install Babel](#-step-2-install-babel)
    - [Optional (for runtime transpilation)](#optional-for-runtime-transpilation)
  - [⚙️ Step 3: Configure Babel](#️-step-3-configure-babel)
  - [🧾 Step 4: Create some ES6+ code](#-step-4-create-some-es6-code)
  - [🏗️ Step 5: Add a build script](#️-step-5-add-a-build-script)
  - [▶️ Step 6: Run the compiled code](#️-step-6-run-the-compiled-code)
  - [⚡ Bonus: Run Babel directly (without building)](#-bonus-run-babel-directly-without-building)
  - [🧪 Optional Add-ons](#-optional-add-ons)
    - [1. Babel + TypeScript](#1-babel--typescript)
    - [2. Babel + JSX (React)](#2-babel--jsx-react)
  - [✅ Summary](#-summary)
  - [References](#references)


Sure! Here’s a **step-by-step tutorial to get started with Babel.js in Node.js**, from setup to running your first transpiled code.

---

## 🧠 What is Babel.js?

**Babel** is a JavaScript compiler that lets you use **modern JavaScript (ES6+)** features while maintaining compatibility with older environments (like Node.js versions or browsers that don’t support new syntax yet).

In a Node.js project, Babel is mainly used to:

* Use `import/export` instead of `require/module.exports`.
* Write ESNext syntax (optional chaining, nullish coalescing, etc.).
* Transpile TypeScript (optional).

---

## 🚀 Step 1: Initialize your Node.js project

Create a new folder and initialize it:

```bash
mkdir babel-tutorial
cd babel-tutorial
npm init -y
```

This creates a `package.json` file.

---

## 🧩 Step 2: Install Babel

Install Babel core and CLI tools:

```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

### Optional (for runtime transpilation)

If you want Babel to compile code **on the fly** when running Node, also install:

```bash
npm install --save-dev @babel/register
```

---

## ⚙️ Step 3: Configure Babel

Create a configuration file:
`.babelrc` or `babel.config.json`

Example `.babelrc`:

```json
{
  "presets": ["@babel/preset-env"]
}
```

This tells Babel to use the `@babel/preset-env` preset, which compiles modern JS down to a version compatible with your target environment.

---

## 🧾 Step 4: Create some ES6+ code

Example file: `src/index.js`

```js
// src/index.js
import { greet } from './utils.js';

console.log(greet('Wallace'));
```

And the module file: `src/utils.js`

```js
export const greet = (name) => `Hello, ${name}! 👋`;
```

---

## 🏗️ Step 5: Add a build script

In your `package.json`, add this inside `"scripts"`:

```json
"scripts": {
  "build": "babel src --out-dir dist"
}
```

Now run:

```bash
npm run build
```

Babel will transpile all files in `src` into the `dist` directory.

---

## ▶️ Step 6: Run the compiled code

After building, you can run your transpiled JS with Node:

```bash
node dist/index.js
```

Output:

```
Hello, Wallace! 👋
```

---

## ⚡ Bonus: Run Babel directly (without building)

You can use **@babel/register** to transpile on-the-fly:

Create a `start.js`:

```js
require('@babel/register')({
  presets: ['@babel/preset-env']
});
require('./src/index.js');
```

Then run:

```bash
node start.js
```

Babel will transpile your source files in memory (no `dist` folder needed).

---

## 🧪 Optional Add-ons

### 1. Babel + TypeScript

```bash
npm install --save-dev @babel/preset-typescript
```

Update `.babelrc`:

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-typescript"]
}
```

### 2. Babel + JSX (React)

```bash
npm install --save-dev @babel/preset-react
```

Then update `.babelrc`:

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

---

## ✅ Summary

| Step | Command / File                                      | Purpose                         |
| ---- | --------------------------------------------------- | ------------------------------- |
| 1    | `npm init -y`                                       | Create Node project             |
| 2    | `npm i -D @babel/core @babel/cli @babel/preset-env` | Install Babel                   |
| 3    | `.babelrc`                                          | Configure presets               |
| 4    | `src/index.js`                                      | Write ES6+ code                 |
| 5    | `"build": "babel src --out-dir dist"`               | Transpile to dist               |
| 6    | `node dist/index.js`                                | Run compiled code               |
| 7    | (Optional) `@babel/register`                        | Run directly without build step |

---

## References

1. https://babeljs.io/
