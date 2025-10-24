# NPM

## Table of Contents
- [NPM](#npm)
  - [Table of Contents](#table-of-contents)
  - [npm install -g](#npm-install--g)
    - [What `npm install -g` Does](#what-npm-install--g-does)
      - [Key Effects](#key-effects)
    - [When NOT to Use `-g`](#when-not-to-use--g)
    - [Summary](#summary)
  - [Path Issues with npm install -g](#path-issues-with-npm-install--g)
    - [Why This Happens (The PATH Problem)](#why-this-happens-the-path-problem)
    - [Solutions for Windows](#solutions-for-windows)
      - [1. Restart the Terminal](#1-restart-the-terminal)
      - [2. Verify and Add the Directory to Your PATH](#2-verify-and-add-the-directory-to-your-path)
        - [Step 2a: Find the Directory](#step-2a-find-the-directory)
        - [Step 2b: Add the Path to Environment Variables](#step-2b-add-the-path-to-environment-variables)
      - [3. Test the Command](#3-test-the-command)
    - [Alternative: Using `npx` (No Global Install Required)](#alternative-using-npx-no-global-install-required)
  - [npm install -D](#npm-install--d)
    - [What `npm install -D` Does](#what-npm-install--d-does)
      - [Key Characteristics:](#key-characteristics)
    - [Use Cases for Development Dependencies (`devDependencies`)](#use-cases-for-development-dependencies-devdependencies)
      - [Example](#example)

## npm install -g 

Using the `-g` flag with `npm install` installs a package **globally** on your system.

---

### What `npm install -g` Does

When you run `npm install <package-name> -g`, the package is installed in a central location on your computer rather than in the `node_modules` folder of your current project. This makes the package and any executables it provides available to your **entire system**, regardless of which directory you are in.

#### Key Effects

* **System-Wide Availability:** The primary purpose is to make a tool available from your **command line** (terminal) so you can run it as a standalone command from any folder.
* **Location:** Packages are usually installed in a specific user or system-wide directory, which is configured to be in your system's `PATH`. You can find out the exact location by running `npm root -g`.
* **Use Case:** This flag is almost exclusively used for installing **command-line tools** (CLIs) like:
    * **Webpack** (`webpack`)
    * **Create React App** (`create-react-app`)
    * **Gulp** (`gulp-cli`)
    * **Nodemon** (`nodemon`)
    * **TypeScript** (`tsc`)

---

### When NOT to Use `-g`

**Avoid** using the `-g` flag for **project dependencies** (like React, Lodash, or Express).

* Project dependencies should always be installed **locally** (without `-g`) so they are saved in your project's `package.json` file. This ensures:
    1.  Your project is **portable** and everyone working on it uses the correct versions.
    2.  You don't have version conflicts between different projects on your machine.

---

### Summary

| Command | Flag | Installation Location | Use Case |
| :--- | :--- | :--- | :--- |
| `npm install <package>` | **(None)** | `node_modules` in the current project | **Project Dependencies** (e.g., React, Express, Lodash) |
| `npm install <package> -g` | **`-g`** (Global) | System-wide location (in your PATH) | **Command-Line Tools/CLIs** (e.g., `create-react-app`, `nodemon`) |

## Path Issues with npm install -g

Ah, that clarifies things\! When you're using **Windows** and running `bash` (like Git Bash, Cygwin, or WSL), the common reason for the "command not found" error after a global `npm install -g` is a conflict or mismatch in your system's **PATH environment variable**.

-----

### Why This Happens (The PATH Problem)

In Windows, globally installed npm packages are typically placed in a directory like:

```
C:\Users\<Your_User>\AppData\Roaming\npm
```

The issue is that your **Bash shell environment** (where you typed `serverless`) isn't looking in this specific Windows directory for executable commands, even though the installation finished successfully.

-----

### Solutions for Windows

Here are the steps to resolve this, from simplest to most likely to succeed:

#### 1\. Restart the Terminal

Simply **close and reopen the terminal/shell window** where you ran the command. This is the quickest way for the shell to reload the `PATH`.

#### 2\. Verify and Add the Directory to Your PATH

If a restart doesn't work, you need to manually ensure the npm global executable folder is in your Windows `PATH`.

##### Step 2a: Find the Directory

In any terminal (or standard Windows Command Prompt), run this command to find the exact location npm is using:

```bash
npm prefix -g
```

This will likely output the path you need, which is often: `C:\Users\<Your_User>\AppData\Roaming\npm`.

##### Step 2b: Add the Path to Environment Variables

1.  Press the **Windows Key** and search for **"Environment Variables."**
2.  Click **"Edit the system environment variables"** or **"Edit environment variables for your account."**
3.  In the "Environment Variables" window, under **User variables for `<Your_User>`**, find the **`Path`** variable and select **Edit**.
4.  Click **"New"** and paste the directory you found in Step 2a (e.g., `C:\Users\Your_User\AppData\Roaming\npm`).
5.  Click **OK** on all windows to save the changes.

#### 3\. Test the Command

After making any changes to the environment variables, you **must close all existing terminal windows and open a new one**.

Try the command again in the new terminal:

```bash
serverless
```

It should now be recognized\!

-----

### Alternative: Using `npx` (No Global Install Required)

As a modern best practice, many developers avoid global installation entirely by using **`npx`** (which comes bundled with `npm` versions 5.2+).

`npx` runs an executable from a package, and if the package isn't found locally, it temporarily downloads and runs the latest version, avoiding the need to worry about global paths or installation conflicts.

To run the `serverless` command using `npx`, you can use it like this:

```bash
npx serverless
```

## npm install -D

The flag **`-D`** or **`--save-dev`** with `npm install` installs a package as a **development dependency**.

-----

### What `npm install -D` Does

When you run `npm install <package-name> -D`, the package is installed locally to your project and its name and version are automatically added to the `devDependencies` section of your project's `package.json` file.

#### Key Characteristics:

  * **Local Installation:** The package files are placed inside the project's **`node_modules`** folder.
  * **Purpose:** It signifies that the package is **only required during the development, testing, and building stages** of your project.
  * **Production Omission:** When your application is deployed to a production environment (like a live web server), these packages are typically **not installed**. You can instruct NPM to skip them by running:
    ```bash
    npm install --production
    ```

-----

### Use Cases for Development Dependencies (`devDependencies`)

You should use `-D` for any tool that helps you write, test, or bundle your code, but is not needed for the code to run in its final environment.

| Tool Category | Example Packages |
| :--- | :--- |
| **Testing Frameworks** | Jest, Mocha, Cypress |
| **Linters and Formatters** | ESLint, Prettier |
| **Bundlers and Transpilers** | Webpack, Rollup, Babel (and its presets) |
| **Task Runners** | Gulp, Grunt |
| **Utility CLIs** | `cross-env`, `nodemon` |

#### Example

If you want to install ESLint to check your code quality:

```bash
npm install eslint -D 
# OR
npm install eslint --save-dev
```

This ensures that only the code your application *needs* to run (found in `dependencies`) is deployed, keeping your production environment lean and fast.