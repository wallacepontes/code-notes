# Nodemon

## Table of Contents

- [Nodemon](#nodemon)
  - [Table of Contents](#table-of-contents)
  - [Nodemon for automatically restarts](#nodemon-for-automatically-restarts)
    - [Key Functions](#key-functions)
    - [How it Works](#how-it-works)
    - [Use Case](#use-case)
  - [Install Nodemon](#install-nodemon)
    - [Local Installation (Recommended)](#local-installation-recommended)
    - [Global Installation](#global-installation)

## Nodemon for automatically restarts

Nodemon is a command-line utility for Node.js that automatically restarts your application whenever it detects changes in your source code, streamlining the development process by eliminating the need to manually stop and restart the server after every code modification. It acts as a wrapper, watching specified files and directories, and when changes are saved, it restarts the Node.js process, making development faster and more efficient, especially for large projects.

### Key Functions

- Automatic Restarts: Watches for file changes (JS, JSON, etc.) and reboots the server. 
- Developer Productivity: Saves time by handling restarts, letting developers focus on coding. 
- No Code Changes: Works with your existing Node.js code without requiring modifications. 
- Customizable: Can ignore specific directories (like node_modules) and watch for certain file types, configured via nodemon.json. 

### How it Works

1. You run your application using npx nodemon app.js (instead of node app.js). 
2. Nodemon starts your Node.js app and watches for file changes. 
3. When you save a file, Nodemon detects it and automatically restarts the server to reflect the changes. 

### Use Case

Nodemon is primarily a development tool, ideal for building and testing applications locally, not typically for production environments where tools like PM2 are used for uptime and clustering, notes DigitalOcean.

## Install Nodemon

To install Nodemon using npm, you have two primary methods: installing it locally as a development dependency (recommended for projects) or globally on your system. 

### Local Installation (Recommended)

This approach adds Nodemon as a development dependency specific to your project, which is best practice to ensure consistent versioning across different development environments.

1. Navigate to your project's root directory in your terminal.
2. Initialize npm in your project if you haven't already:

```bash
npm init -y
```

3. Install Nodemon as a development dependency using one of these commands:

```bash
npm install nodemon --save-dev

# or the shorthand version
npm i nodemon -D
```

4. Configure a script in your package.json file to run Nodemon, as you cannot run locally installed packages directly from the command line.In your package.json file, add a "start" or "dev" script:

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "nodemon your-app-file.js"
}
```

(Replace your-app-file.js with your main application file name, e.g., index.js or server.js).

5. Run your application using the defined script:

```bash
npm start
# or if you named the script "dev"
npm run dev
```
 
### Global Installation

A global installation makes the nodemon command available from any directory in your terminal. 

- Run the following command:

```bash
npm install -g nodemon
```

- After installing globally, you can run your application from anywhere using the nodemon command directly:

```bash
nodemon your-app-file.js
```

If you encounter a "nodemon: command not found" error after a global install, you may need to ensure your system's PATH environment variable includes the global npm packages directory. 