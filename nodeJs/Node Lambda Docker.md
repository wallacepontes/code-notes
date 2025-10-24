# Node Lambda Docker

## Table of Contents
- [Node Lambda Docker](#node-lambda-docker)
  - [Table of Contents](#table-of-contents)
  - [Using Docker](#using-docker)
  - [Let’s fix this 👇](#lets-fix-this-)
    - [✅ **Option 1: Use the full curl.exe (the real one)**](#-option-1-use-the-full-curlexe-the-real-one)
    - [✅ **Option 2: Use PowerShell’s native syntax**](#-option-2-use-powershells-native-syntax)
    - [✅ **Option 3: Run inside Linux shell (WSL or Git Bash)**](#-option-3-run-inside-linux-shell-wsl-or-git-bash)
    - [🧠 Why this happens](#-why-this-happens)
  - [References](#references)

## Using Docker
1. **`docker pull node:22-alpine`**
   → Downloads the lightweight Alpine-based Node.js 22 image from Docker Hub.
   (This is ideal for minimal, production-friendly images.)

2. **`docker run -it --rm --entrypoint sh node:22-alpine`**
   → Starts a temporary container, launches an interactive shell (`sh`), and removes the container automatically after you exit.
   You’re basically “inside” the Node environment now.

3. **`node -v`**
   → Confirms Node.js was installed and prints the version (v22.21.0).

4. **`npm -v`**
   → Confirms the bundled `npm` package manager is available (v10.9.4 in your case).

If you want to **run your own Node project inside Docker**, the next step would typically be to:

1. Create a `Dockerfile` like this:

   ```Dockerfile
   FROM node:22-alpine

   WORKDIR /app
   COPY package*.json ./
   RUN npm install --production
   COPY . .

   CMD ["node", "index.js"]
   ```

2. Then build and run it:

   ```bash
   docker build -t my-node-app .
   docker run -p 3000:3000 my-node-app
   ```

---

## Let’s fix this 👇

Ah yes — that error happens because you’re running the `curl` command **inside PowerShell on Windows**, not inside a Linux shell (like bash).

PowerShell has its **own built-in** `curl` alias that maps to `Invoke-WebRequest`, and its syntax is **different** from the real `curl` command-line tool.

---

### ✅ **Option 1: Use the full curl.exe (the real one)**

If you have Docker Desktop or Git Bash installed, you already have the real `curl.exe`.

Just type:

```powershell
curl.exe -X POST "http://localhost:9000/2015-03-31/functions/function/invocations" -d "{}"
```

> The key detail: add `.exe` — this bypasses PowerShell’s alias and calls the real curl binary.

---

### ✅ **Option 2: Use PowerShell’s native syntax**

If you prefer staying inside PowerShell, run this equivalent:

```powershell
Invoke-WebRequest -Uri "http://localhost:9000/2015-03-31/functions/function/invocations" `
  -Method POST `
  -Body '{}' `
  -ContentType "application/json"
```

> The backticks ( ` ) allow multiline commands; you can also put it on one line.

---

### ✅ **Option 3: Run inside Linux shell (WSL or Git Bash)**

If you have **WSL** (Windows Subsystem for Linux) or **Git Bash**, open that shell and use the original command exactly as written:

```bash
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'
```

---

### 🧠 Why this happens

* PowerShell defines `curl` and `wget` as aliases for `Invoke-WebRequest`.
* These expect PowerShell-style parameters like `-Method`, not Unix flags like `-XPOST`.

---

Would you like me to show how to **inspect the Lambda logs** from that container (to verify the handler ran correctly)?


## References
1. https://nodejs.org/en/download/