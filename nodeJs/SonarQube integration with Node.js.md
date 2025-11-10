# SonarQube integration with Node.js

## Table of Contents
- [SonarQube integration with Node.js](#sonarqube-integration-with-nodejs)
  - [Table of Contents](#table-of-contents)
  - [🧩 What Is SonarQube?](#-what-is-sonarqube)
  - [⚙️ Step 1. Install and Run SonarQube Locally](#️-step-1-install-and-run-sonarqube-locally)
    - [Option 1: Run via Docker (recommended)](#option-1-run-via-docker-recommended)
  - [📁 Step 2. Prepare Your Node.js Project](#-step-2-prepare-your-nodejs-project)
  - [🧪 Step 3. Add a Jest Test and Coverage Report (optional but recommended)](#-step-3-add-a-jest-test-and-coverage-report-optional-but-recommended)
  - [🗂️ Step 4. Add a Sonar Project Configuration File](#️-step-4-add-a-sonar-project-configuration-file)
  - [🧰 Step 5. Install the Sonar Scanner](#-step-5-install-the-sonar-scanner)
    - [Option 1: Global Install](#option-1-global-install)
    - [Option 2: Local Install (recommended for CI)](#option-2-local-install-recommended-for-ci)
  - [🔐 Step 6. Get a Sonar Token](#-step-6-get-a-sonar-token)
  - [▶️ Step 7. Run the Analysis](#️-step-7-run-the-analysis)
  - [🧩 Step 8. View Results in SonarQube](#-step-8-view-results-in-sonarqube)
  - [🚀 Step 9. Integrate with CI/CD (optional)](#-step-9-integrate-with-cicd-optional)
  - [✅ Step 10. Best Practices](#-step-10-best-practices)

Here’s a **step-by-step tutorial** to help you get started with **SonarQube integration in a Node.js project**, including setup, configuration, and best practices for continuous inspection 👇

---

## 🧩 What Is SonarQube?

**SonarQube** is a code quality and security analysis tool.
It continuously inspects your code for:

* Bugs
* Code smells (bad practices)
* Security vulnerabilities
* Test coverage
* Duplications

It integrates easily with **Node.js**, **CI/CD pipelines**, and **GitHub**, providing dashboards and automated code reviews.

---

## ⚙️ Step 1. Install and Run SonarQube Locally

### Option 1: Run via Docker (recommended)

```bash
docker run -d --name sonarqube \
  -p 9000:9000 \
  sonarqube:latest
```

Access at → [http://localhost:9000](http://localhost:9000)
Default credentials:

```
Username: admin
Password: admin
```

After login, you’ll be asked to change the password.

---

## 📁 Step 2. Prepare Your Node.js Project

Inside your Node.js project directory:

```bash
mkdir my-node-app && cd my-node-app
npm init -y
```

Add some source code files (e.g. `src/app.js`, `src/utils.js`).

Then, install dependencies you need (optional):

```bash
npm install express
```

Also, add a test framework if you want coverage (SonarQube supports Jest, Mocha, etc.):

```bash
npm install --save-dev jest
```

---

## 🧪 Step 3. Add a Jest Test and Coverage Report (optional but recommended)

Add this in `package.json`:

```json
"scripts": {
  "test": "jest --coverage"
}
```

Then run:

```bash
npm test
```

It will create a `coverage/lcov.info` file — SonarQube uses this to measure test coverage.

---

## 🗂️ Step 4. Add a Sonar Project Configuration File

Create a file named **`sonar-project.properties`** in the root directory:

```properties
# Project identification
sonar.projectKey=my-node-app
sonar.projectName=My Node App
sonar.projectVersion=1.0

# Source and test paths
sonar.sources=src
sonar.tests=__tests__
sonar.language=js

# Exclude certain files
sonar.exclusions=node_modules/**,coverage/**

# Coverage report
sonar.javascript.lcov.reportPaths=coverage/lcov.info

# Encoding
sonar.sourceEncoding=UTF-8
```

---

## 🧰 Step 5. Install the Sonar Scanner

### Option 1: Global Install

```bash
npm install -g sonar-scanner
```

### Option 2: Local Install (recommended for CI)

```bash
npm install --save-dev sonar-scanner
```

Add to your `package.json` scripts:

```json
"scripts": {
  "sonar": "sonar-scanner"
}
```

---

## 🔐 Step 6. Get a Sonar Token

1. Go to **[http://localhost:9000](http://localhost:9000)** → Log in → Click your profile → **My Account → Security → Generate Token**
2. Copy the token.
3. In your environment, set:

   ```bash
   export SONAR_TOKEN=<your_generated_token>
   ```

---

## ▶️ Step 7. Run the Analysis

Run tests (if you want coverage first):

```bash
npm test
```

Then execute:

```bash
npm run sonar
```

If successful, you’ll see:

```
ANALYSIS SUCCESSFUL
You can browse http://localhost:9000/dashboard?id=my-node-app
```

---

## 🧩 Step 8. View Results in SonarQube

Open your browser → [http://localhost:9000](http://localhost:9000) → **Projects → My Node App**

You’ll see:

* Overall code quality metrics
* Coverage percentage
* Code smells, bugs, security hotspots
* Duplications

---

## 🚀 Step 9. Integrate with CI/CD (optional)

Example: **GitHub Actions**
Create `.github/workflows/sonarqube.yml`

```yaml
name: SonarQube Analysis

on:
  push:
    branches: [ main ]

jobs:
  sonar:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npm test
      - name: SonarQube Scan
        run: npx sonar-scanner \
          -Dsonar.projectKey=my-node-app \
          -Dsonar.sources=src \
          -Dsonar.host.url=${{ secrets.SONAR_HOST_URL }} \
          -Dsonar.login=${{ secrets.SONAR_TOKEN }}
```

---

## ✅ Step 10. Best Practices

| Practice                          | Description                                              |
| --------------------------------- | -------------------------------------------------------- |
| 💬 **Fail builds on low quality** | Set quality gates in SonarQube to reject bad code.       |
| 🧩 **Integrate early**            | Run SonarQube in your CI pipeline on every pull request. |
| 🧠 **Use branches**               | SonarQube can compare `main` vs. feature branches.       |
| 🧹 **Exclude generated files**    | Avoid noise from `dist`, `coverage`, etc.                |
| 🧪 **Keep tests updated**         | More coverage = better code quality insights.            |

---

