# Yarn


## Table of Contents
- [Yarn](#yarn)
  - [Table of Contents](#table-of-contents)
  - [🧠 What Is Yarn?](#-what-is-yarn)
  - [Yarn is an open-source JavaScript package manager that automates and speeds up the process of installing, updating, and removing dependencies for JavaScript projects. To install it, the most common method is to use npm, Node.js's own package manager, by running npm install -g yarn in your terminal.](#yarn-is-an-open-source-javascript-package-manager-that-automates-and-speeds-up-the-process-of-installing-updating-and-removing-dependencies-for-javascript-projects-to-install-it-the-most-common-method-is-to-use-npm-nodejss-own-package-manager-by-running-npm-install--g-yarn-in-your-terminal)
  - [🧩 Step 1: Check Node.js Installation](#-step-1-check-nodejs-installation)
  - [⚙️ Step 2: Install Yarn](#️-step-2-install-yarn)
    - [🪟 Windows](#-windows)
    - [🍎 macOS](#-macos)
    - [🐧 Linux](#-linux)
    - [Using a shell script (macOS and generic Unix-like systems)](#using-a-shell-script-macos-and-generic-unix-like-systems)
  - [🏗️ Step 3: Initialize a New Project](#️-step-3-initialize-a-new-project)
  - [📦 Step 4: Add Dependencies](#-step-4-add-dependencies)
    - [Add as a dev dependency:](#add-as-a-dev-dependency)
    - [Add a specific version:](#add-a-specific-version)
  - [🔄 Step 5: Remove or Upgrade Dependencies](#-step-5-remove-or-upgrade-dependencies)
  - [🚀 Step 6: Run Scripts](#-step-6-run-scripts)
  - [🔍 Step 7: Check Installed Packages](#-step-7-check-installed-packages)
  - [🧩 Step 8: Use Global Packages](#-step-8-use-global-packages)
  - [🧱 Step 9: Workspaces (Monorepo Feature)](#-step-9-workspaces-monorepo-feature)
  - [🧹 Step 10: Clean Cache](#-step-10-clean-cache)
  - [🔧 Bonus: Yarn vs npm Commands](#-bonus-yarn-vs-npm-commands)
  - [✅ Summary](#-summary)
  - [Video](#video)
  - [References](#references)

Here’s a **complete beginner-friendly tutorial** to get you started with **Yarn**, the fast and reliable JavaScript package manager.

---

## 🧠 What Is Yarn?

**Yarn** is a **package manager** for JavaScript — like **npm**, but faster and more consistent.
It helps you:

* Install and manage project dependencies
* Lock dependency versions for consistency
* Run project scripts
* Manage workspaces (monorepos)

Yarn is an open-source JavaScript package manager that automates and speeds up the process of installing, updating, and removing dependencies for JavaScript projects. To install it, the most common method is to use npm, Node.js's own package manager, by running npm install -g yarn in your terminal. 
---

## 🧩 Step 1: Check Node.js Installation

Yarn depends on **Node.js**, so first ensure it’s installed.

```bash
node -v
```

If you don’t have Node.js, install it from:
👉 [https://nodejs.org](https://nodejs.org)

---

## ⚙️ Step 2: Install Yarn

There are several ways to install Yarn depending on your system.

### 🪟 Windows

Use **Chocolatey** or **Scoop**:

```bash
choco install yarn
```

or

```bash
scoop install yarn
```

### 🍎 macOS

Use **Homebrew**:

```bash
brew install yarn
```

### 🐧 Linux

Use **npm** (simplest way):

```bash
npm install --global yarn
```

Check if Yarn is installed:

```bash
yarn -v
```

### Using a shell script (macOS and generic Unix-like systems)

1. Run the installation script: Open your terminal and execute the following command:

    ```bash
    curl -o- -L https://yarnpkg.com/install.sh | bash
    ```

2. Verify the installation: After the script finishes, check the version with yarn --version. 

---

## 🏗️ Step 3: Initialize a New Project

Create a new folder and initialize your project:

```bash
mkdir my-app
cd my-app
yarn init
```

It will ask a few questions (name, version, etc.).
To skip all prompts:

```bash
yarn init -y
```

This creates a `package.json` file — the heart of your Node.js project.

---

## 📦 Step 4: Add Dependencies

Install a package, for example **Express**:

```bash
yarn add express
```

This will:

* Install `express`
* Add it to your `package.json`
* Create a `yarn.lock` file (to lock dependency versions)

### Add as a dev dependency:

```bash
yarn add --dev jest
```

### Add a specific version:

```bash
yarn add react@18.2.0
```

---

## 🔄 Step 5: Remove or Upgrade Dependencies

Remove a package:

```bash
yarn remove express
```

Upgrade:

```bash
yarn up express
```

Upgrade all:

```bash
yarn up
```

---

## 🚀 Step 6: Run Scripts

Add custom scripts in your `package.json`:

```json
"scripts": {
  "start": "node app.js",
  "test": "jest"
}
```

Then run them:

```bash
yarn start
yarn test
```

---

## 🔍 Step 7: Check Installed Packages

List installed dependencies:

```bash
yarn list
```

Show outdated packages:

```bash
yarn outdated
```

---

## 🧩 Step 8: Use Global Packages

Install a package globally:

```bash
yarn global add nodemon
```

Check global packages:

```bash
yarn global list
```

---

## 🧱 Step 9: Workspaces (Monorepo Feature)

Yarn supports **workspaces** — multiple related packages in one repo.

Example `package.json` (root):

```json
{
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}
```

Now each folder under `packages/` can be a mini-project sharing dependencies efficiently.

---

## 🧹 Step 10: Clean Cache

If things go wrong:

```bash
yarn cache clean
```

---

## 🔧 Bonus: Yarn vs npm Commands

| Task               | npm                     | Yarn                  |
| ------------------ | ----------------------- | --------------------- |
| Initialize project | `npm init`              | `yarn init`           |
| Install all deps   | `npm install`           | `yarn install`        |
| Add dependency     | `npm install package`   | `yarn add package`    |
| Remove dependency  | `npm uninstall package` | `yarn remove package` |
| Run script         | `npm run start`         | `yarn start`          |

---

## ✅ Summary

| Step | Command                     | Description          |
| ---- | --------------------------- | -------------------- |
| 1    | `npm install --global yarn` | Install Yarn         |
| 2    | `yarn init -y`              | Create new project   |
| 3    | `yarn add <pkg>`            | Install dependencies |
| 4    | `yarn remove <pkg>`         | Remove dependencies  |
| 5    | `yarn start`                | Run scripts          |
| 6    | `yarn workspaces`           | Manage monorepos     |

---

## Video
 * [Getting Started with Yarn Package Manager](https://www.youtube.com/watch?v=223uxFCu74s)
	> [<img src="https://img.youtube.com/vi/223uxFCu74s/0.jpg" width="200">](https://www.youtube.com/watch?v=223uxFCu74s "This tutorial explains how Yarn, the alternative to NPM, works as a package manager. Yarn is considered to be the default package manager tool for use with React. by Steve Griffith - Prof3ssorSt3v3 10k views 12 minutes, 20 seconds")

## References
1. https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable
2. https://yarnpkg.com/