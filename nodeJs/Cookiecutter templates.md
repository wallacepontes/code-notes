# Cookiecutter templates

## Table of Contents
- [Cookiecutter templates](#cookiecutter-templates)
  - [Table of Contents](#table-of-contents)
  - [🍪 What is Cookiecutter (and what’s the Node equivalent?)](#-what-is-cookiecutter-and-whats-the-node-equivalent)
  - [🧰 1. Setup Environment](#-1-setup-environment)
  - [📁 2. Choose or Create a Template](#-2-choose-or-create-a-template)
  - [🧑‍💻 3. Using Cookiecutter in Your Own Node Script](#-3-using-cookiecutter-in-your-own-node-script)
  - [🧩 4. Creating Your Own Cookiecutter Template](#-4-creating-your-own-cookiecutter-template)
    - [`cookiecutter.json`](#cookiecutterjson)
    - [`package.json` (inside the template)](#packagejson-inside-the-template)
  - [⚙️ 5. Advanced Tips](#️-5-advanced-tips)
  - [🧠 6. Alternative Tools (JS Native)](#-6-alternative-tools-js-native)
  - [✅ Example: Quick Start Command Summary](#-example-quick-start-command-summary)
  - [References](#references)

Perfect 👍 — let’s go step-by-step on how to **use Cookiecutter-style project templates** in **Node.js**.

---

## 🍪 What is Cookiecutter (and what’s the Node equivalent?)

* **Cookiecutter** (originally Python) lets you **generate new projects from templates** — it asks you questions, then fills files and directories dynamically.
* In **Node.js**, you can achieve the same goal using:

  * [`cookiecutter`](https://www.npmjs.com/package/cookiecutter) — a small Node wrapper for Cookiecutter templates, or
  * More popular JavaScript-native tools like **Hygen**, **Yeoman**, or **Plop**.

But if you specifically want **Cookiecutter in Node**, here’s the easiest path 👇

---

## 🧰 1. Setup Environment

Make sure you have **Node.js (>=16)** and **npm** or **yarn**.

Then install the cookiecutter package globally (or locally in a project):

```bash
npm install -g cookiecutter
```

or

```bash
npm install cookiecutter --save-dev
```

---

## 📁 2. Choose or Create a Template

You can use **any Cookiecutter template**, even Python ones — for example:

```bash
cookiecutter https://github.com/cookiecutter/nodejs-package
```

That one generates a clean Node.js library boilerplate.

Cookiecutter will ask questions like:

``` bash
project_name [my_node_project]:
version [0.1.0]:
author_name [Your Name]:
```

After that, you’ll see a new folder named after your project.

---

## 🧑‍💻 3. Using Cookiecutter in Your Own Node Script

If you want to **run Cookiecutter programmatically**, create a small script like:

```js
// generate.js
import { cookiecutter } from 'cookiecutter';

async function main() {
  await cookiecutter('https://github.com/cookiecutter/nodejs-package', {
    extraContext: {
      project_name: 'my-lib',
      author_name: 'Wallace Pontes'
    },
    outputDir: './generated',
  });
  console.log('✅ Project generated!');
}

main().catch(console.error);
```

Run it with:

```bash
node generate.js
```

This lets you integrate templates inside your own scaffolding tools.

---

## 🧩 4. Creating Your Own Cookiecutter Template

A Cookiecutter template is just a **folder** with placeholders inside double braces:

```
my-template/
├── cookiecutter.json
├── {{cookiecutter.project_name}}/
│   ├── package.json
│   ├── index.js
│   └── README.md
```

### `cookiecutter.json`

```json
{
  "project_name": "my-node-app",
  "description": "A Node.js app template",
  "author_name": "Your Name"
}
```

### `package.json` (inside the template)

```json
{
  "name": "{{cookiecutter.project_name}}",
  "description": "{{cookiecutter.description}}",
  "author": "{{cookiecutter.author_name}}"
}
```

When you run:

```bash
cookiecutter ./my-template
```

Cookiecutter will prompt for those fields and generate the project with values replaced.

---

## ⚙️ 5. Advanced Tips

| Goal                        | How to                                                          |
| --------------------------- | --------------------------------------------------------------- |
| Skip prompts                | Use `--no-input` and provide defaults in `cookiecutter.json`    |
| Generate into custom folder | `--output-dir ./apps`                                           |
| Automate inside a script    | Use the Node API as shown above                                 |
| Use in CI/CD                | Run it as part of scaffolding or boilerplate setup before build |

---

## 🧠 6. Alternative Tools (JS Native)

If you later want more Node-ecosystem-native solutions:

* **Hygen** → Great for code generators (`npx hygen init self` to start)
* **Plop.js** → Very simple file scaffolding using handlebars
* **Yeoman** → Classic generator framework

These tools behave similarly but are written in Node and integrate tightly with JS tooling.

---

## ✅ Example: Quick Start Command Summary

```bash
# install
npm install -g cookiecutter

# use a Node template
cookiecutter https://github.com/cookiecutter/nodejs-package

# or use your local one
cookiecutter ./my-template

# programmatically
node generate.js
```

---

## References

1. https://github.com/cookiecutter/cookiecutter
