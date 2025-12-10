# Node Lambda Docker

## Table of Contents
- [Node Lambda Docker](#node-lambda-docker)
  - [Table of Contents](#table-of-contents)
  - [Node.js is working correctly inside a Docker](#nodejs-is-working-correctly-inside-a-docker)
  - [⚡ What “Serverless” and “Lambda” Mean](#-what-serverless-and-lambda-mean)
  - [💻 How to Develop Serverless (Lambda-style) Locally](#-how-to-develop-serverless-lambda-style-locally)
    - [🧰 Option 1: **Use AWS SAM CLI (recommended by AWS)**](#-option-1-use-aws-sam-cli-recommended-by-aws)
      - [🔧 Installation steps](#-installation-steps)
    - [🐳 Option 2: **Use Docker directly**](#-option-2-use-docker-directly)
    - [🚀 Option 3: Frameworks for Easier Development](#-option-3-frameworks-for-easier-development)
  - [🧠 Summary](#-summary)
  - [Let’s fix this 👇](#lets-fix-this-)
    - [✅ **Option 1: Use the full curl.exe (the real one)**](#-option-1-use-the-full-curlexe-the-real-one)
    - [✅ **Option 2: Use PowerShell’s native syntax**](#-option-2-use-powershells-native-syntax)
    - [✅ **Option 3: Run inside Linux shell (WSL or Git Bash)**](#-option-3-run-inside-linux-shell-wsl-or-git-bash)
    - [🧠 Why this happens](#-why-this-happens)
  - [References](#references)

## Node.js is working correctly inside a Docker

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

Excellent question — this touches the heart of **modern API development and cloud architecture**. Let’s unpack it clearly 👇

---

## ⚡ What “Serverless” and “Lambda” Mean

* **Serverless** ≠ no servers — it means **you don’t manage servers**.
  The cloud provider (like AWS, Google Cloud, or Azure) automatically provisions, scales, and runs your code.

* **AWS Lambda** is the most known **Function-as-a-Service (FaaS)** platform.
  You upload your code, and it runs only when invoked (e.g. via API Gateway, event, or schedule).

---

## 💻 How to Develop Serverless (Lambda-style) Locally

You can **absolutely** develop and test Lambda functions locally — and **Docker** plays a big role here.

There are **two main approaches**:

---

### 🧰 Option 1: **Use AWS SAM CLI (recommended by AWS)**

AWS SAM (Serverless Application Model) is a CLI tool that helps you **build, test, and debug** Lambda functions *locally*, with Docker simulating the Lambda runtime.

#### 🔧 Installation steps

1. Install:

   * [Docker](https://www.docker.com/)
   * [AWS CLI](https://aws.amazon.com/cli/)
   * [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)

2. Create a new project:

   ```bash
   sam init
   ```

   Then choose:

   * Runtime: `nodejs22.x`, `python3.12`, or `java21` etc.
   * Template: “Hello World Example”

3. Build the project:

   ```bash
   sam build
   ```

4. Run locally (this uses **Docker** to emulate Lambda):

   ```bash
   sam local start-api
   ```

   → It launches an API Gateway emulator at `http://127.0.0.1:3000`

5. Test it:

   ```bash
   curl http://127.0.0.1:3000/hello
   ```

---

### 🐳 Option 2: **Use Docker directly**

If you don’t want the SAM CLI, you can manually run your code in a Lambda container:

1. AWS provides **official Lambda base images**, e.g.:

   ```bash
   docker pull public.ecr.aws/lambda/nodejs:22
   ```

2. Create a simple handler (example: `app.js`):

   ```js
   exports.handler = async (event) => {
     console.log("Event received:", event);
     return { statusCode: 200, body: JSON.stringify({ message: "Hello from local Lambda!" }) };
   };
   ```

3. Create a `Dockerfile`:

   ```Dockerfile
   FROM public.ecr.aws/lambda/nodejs:22
   COPY app.js ./
   CMD ["app.handler"]
   ```

4. Build and run:

   ```bash
   docker build -t my-lambda .
   docker run -p 9000:8080 my-lambda
   ```

5. Invoke locally:

   ```bash
   curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" \
     -d '{}'
   ```

---

### 🚀 Option 3: Frameworks for Easier Development

| Framework                | Description                                                       | Local Simulation      |
| ------------------------ | ----------------------------------------------------------------- | --------------------- |
| **Serverless Framework** | Popular multi-cloud tool for deploying and testing Lambdas        | `serverless offline`  |
| **AWS SAM CLI**          | Official AWS tool for local Lambda emulation                      | `sam local start-api` |
| **LocalStack**           | Full AWS cloud emulator (S3, API Gateway, Lambda, DynamoDB, etc.) | `localstack start`    |
| **Docker + Base Image**  | Minimal setup, manual control                                     | Docker only           |

**LocalStack** is amazing if you want to simulate a **whole AWS environment locally** (API Gateway + Lambda + DynamoDB + S3, etc.) — but it’s heavier.

---

## 🧠 Summary

| Goal                                                  | Recommended Tool     | Uses Docker? |
| ----------------------------------------------------- | -------------------- | ------------ |
| Just test Lambda code locally                         | AWS SAM CLI          | ✅            |
| Simulate full AWS (API Gateway, DynamoDB, S3, Lambda) | LocalStack           | ✅            |
| Simple manual Lambda test                             | Docker base image    | ✅            |
| Multi-cloud serverless (AWS, GCP, Azure)              | Serverless Framework | Optional     |

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