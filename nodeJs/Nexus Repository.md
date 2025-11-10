# Nexus Repository

## Table of Contents

- [Nexus Repository](#nexus-repository)
  - [Table of Contents](#table-of-contents)
  - [🚀 1. What is Nexus Repository?](#-1-what-is-nexus-repository)
  - [🧩 2. Installing Nexus Repository OSS](#-2-installing-nexus-repository-oss)
    - [Option A — Using Docker (Recommended for quick setup)](#option-a--using-docker-recommended-for-quick-setup)
    - [Option B — Manual Installation](#option-b--manual-installation)
  - [🔐 3. First Login](#-3-first-login)
  - [🏗️ 4. Repository Types Overview](#️-4-repository-types-overview)
    - [Example:](#example)
  - [⚙️ 5. Configuring a Maven Repository](#️-5-configuring-a-maven-repository)
    - [Create Repositories](#create-repositories)
    - [Configure Maven to use Nexus](#configure-maven-to-use-nexus)
  - [📦 6. Uploading Artifacts](#-6-uploading-artifacts)
    - [Option 1 — via Maven deploy](#option-1--via-maven-deploy)
    - [Option 2 — via UI](#option-2--via-ui)
  - [🐳 7. Using Nexus as Docker Registry](#-7-using-nexus-as-docker-registry)
  - [🔍 8. Common Use Cases](#-8-common-use-cases)
  - [🧰 9. Maintenance Tips](#-9-maintenance-tips)
  - [📚 10. Next Steps](#-10-next-steps)
- [NPM](#npm)
- [🧩 Getting Started with **Nexus Repository for npm**](#-getting-started-with-nexus-repository-for-npm)
  - [📘 1. Prerequisites](#-1-prerequisites)
  - [⚙️ 2. Create npm Repositories in Nexus](#️-2-create-npm-repositories-in-nexus)
    - [Step 1 — Create a **Hosted Repository** (for private packages)](#step-1--create-a-hosted-repository-for-private-packages)
    - [Step 2 — Create a **Proxy Repository** (for npmjs.org)](#step-2--create-a-proxy-repository-for-npmjsorg)
    - [Step 3 — Create a **Group Repository**](#step-3--create-a-group-repository)
  - [🧭 3. Configure npm to Use Nexus](#-3-configure-npm-to-use-nexus)
    - [Option A — Global configuration](#option-a--global-configuration)
    - [Option B — Project configuration](#option-b--project-configuration)
  - [🔐 4. Authenticate with Nexus (Optional)](#-4-authenticate-with-nexus-optional)
  - [📦 5. Publish Your Private npm Package](#-5-publish-your-private-npm-package)
  - [🔄 6. Install Packages via Nexus](#-6-install-packages-via-nexus)
  - [🧰 7. Verifying Cache Behavior](#-7-verifying-cache-behavior)
  - [🧹 8. Maintenance \& Best Practices](#-8-maintenance--best-practices)
  - [🧪 9. Quick Example: Private Package + Project](#-9-quick-example-private-package--project)
    - [Step 1 — Create `hello-nexus` package](#step-1--create-hello-nexus-package)
    - [Step 2 — Publish it](#step-2--publish-it)
    - [Step 3 — In another project](#step-3--in-another-project)
  - [📚 10. Next Steps](#-10-next-steps-1)
- [References](#references)

Perfect 👌 — let’s go through a **complete “Getting Started with Nexus Repository” tutorial**, from installation to first usage.

This will focus on **Sonatype Nexus Repository OSS**, which is free and widely used for hosting and managing dependencies (like Maven, npm, Docker, PyPI, etc.).

---

## 🚀 1. What is Nexus Repository?

**Nexus Repository** is a **binary artifact repository manager** that helps store, manage, and distribute software components like:

* Maven packages (Java)
* npm packages (Node.js)
* Docker images
* NuGet, PyPI, RubyGems, Yum, apt, Helm charts, etc.

It can act as:

* **Proxy repository** (cache remote repositories like Maven Central or npmjs)
* **Hosted repository** (your internal/private packages)
* **Group repository** (aggregate multiple repositories behind one URL)

---

## 🧩 2. Installing Nexus Repository OSS

### Option A — Using Docker (Recommended for quick setup)

```bash
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

Then open your browser at:

```
http://localhost:8081
```

> Default data directory: `/nexus-data`
> You can map it to your host machine for persistence:

```bash
docker run -d -p 8081:8081 --name nexus \
  -v /your/local/nexus-data:/nexus-data \
  sonatype/nexus3
```

---

### Option B — Manual Installation

1. Download from:
   👉 [https://help.sonatype.com/repomanager3/download](https://help.sonatype.com/repomanager3/download)

2. Extract and start:

   ```bash
   tar -zxvf nexus-3.*-unix.tar.gz
   cd nexus-3*/
   ./bin/nexus start
   ```

3. Access via browser:

   ```
   http://localhost:8081
   ```

---

## 🔐 3. First Login

* Go to: `http://localhost:8081`
* Default admin username:

  ```
  admin
  ```
* Find the password at:

  ```
  /nexus-data/admin.password
  ```
* Change password and enable anonymous access (optional)

---

## 🏗️ 4. Repository Types Overview

Nexus supports multiple repository types:

| Type       | Example Usage                            |
| ---------- | ---------------------------------------- |
| **Hosted** | Internal/private packages                |
| **Proxy**  | Cache public repos (e.g., Maven Central) |
| **Group**  | Combine multiple repositories into one   |

### Example:

For Maven, you can have:

* `maven-central` (proxy)
* `maven-releases` (hosted)
* `maven-snapshots` (hosted)
* `maven-public` (group that includes all above)

---

## ⚙️ 5. Configuring a Maven Repository

### Create Repositories

1. Log into Nexus as admin.
2. Go to **"Server → Repositories → Create repository"**.
3. Choose “maven2 (hosted)” for internal artifacts.
4. Optionally, create a “maven2 (proxy)” for Maven Central.
5. Then create a “maven2 (group)” and include both hosted + proxy repos.

---

### Configure Maven to use Nexus

In your `~/.m2/settings.xml`:

```xml
<settings>
  <mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
  </mirrors>

  <servers>
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>yourpassword</password>
    </server>
  </servers>
</settings>
```

Now when you build with Maven:

```bash
mvn clean install
```

All dependencies will be fetched (and cached) via Nexus.

---

## 📦 6. Uploading Artifacts

### Option 1 — via Maven deploy

In your `pom.xml`:

```xml
<distributionManagement>
  <repository>
    <id>nexus</id>
    <url>http://localhost:8081/repository/maven-releases/</url>
  </repository>
</distributionManagement>
```

Then:

```bash
mvn deploy
```

### Option 2 — via UI

You can manually upload `.jar`, `.zip`, `.npm`, `.tgz`, or `.docker` images directly through the **Upload** button in the Nexus interface.

---

## 🐳 7. Using Nexus as Docker Registry

1. Create a new repository → choose **docker (hosted)**.
2. Set HTTP port (e.g. `5000`).
3. Login with Docker:

   ```bash
   docker login -u admin -p yourpassword localhost:5000
   ```
4. Push an image:

   ```bash
   docker tag myapp:1.0 localhost:5000/myapp:1.0
   docker push localhost:5000/myapp:1.0
   ```

---

## 🔍 8. Common Use Cases

| Use Case                      | Repository Type | Example                   |
| ----------------------------- | --------------- | ------------------------- |
| Cache Maven Central           | Proxy           | `maven2 (proxy)`          |
| Private npm packages          | Hosted          | `npm (hosted)`            |
| Single endpoint for all repos | Group           | `maven-public`, `npm-all` |
| Internal Docker images        | Hosted          | `docker (hosted)`         |

---

## 🧰 9. Maintenance Tips

* Backup `/nexus-data`
* Regularly cleanup old snapshots (using scheduled tasks)
* Configure LDAP or SSO if needed
* Use HTTPS in production (via reverse proxy like Nginx)

---

## 📚 10. Next Steps

* [Official Documentation](https://help.sonatype.com/repomanager3)
* [Nexus REST API reference](https://help.sonatype.com/repomanager3/integrations/rest-and-integration-api)
* Integrate with CI/CD (e.g. Jenkins, GitLab, GitHub Actions)

---

# NPM 

Perfect 👍 — let’s extend your **“Getting Started with Nexus Repository”** tutorial to cover **npm repositories** (for Node.js developers).

This will show you how to:

1. Create npm repositories in Nexus,
2. Configure your npm client to use Nexus, and
3. Publish & install private packages.

---

# 🧩 Getting Started with **Nexus Repository for npm**

## 📘 1. Prerequisites

Before we start:

* You already have Nexus Repository 3 running (e.g., on `http://localhost:8081`).
* Node.js and npm are installed.

  ```bash
  node -v
  npm -v
  ```

---

## ⚙️ 2. Create npm Repositories in Nexus

In the Nexus admin UI:

### Step 1 — Create a **Hosted Repository** (for private packages)

1. Go to **“Repositories” → “Create repository”**
2. Choose **`npm (hosted)`**
3. Name it something like: `npm-internal`
4. Keep defaults and click **Create repository**

### Step 2 — Create a **Proxy Repository** (for npmjs.org)

1. Create another one with type **`npm (proxy)`**
2. Name it `npmjs`
3. Remote storage URL:

   ```
   https://registry.npmjs.org/
   ```

### Step 3 — Create a **Group Repository**

1. Create one more with type **`npm (group)`**
2. Name it `npm-all`
3. Under **“Member Repositories”**, include:

   * `npm-internal`
   * `npmjs`
4. Save.

✅ Your npm group repository is now available at:

```
http://localhost:8081/repository/npm-all/
```

---

## 🧭 3. Configure npm to Use Nexus

You can configure npm globally or per project.

### Option A — Global configuration

Run:

```bash
npm set registry http://localhost:8081/repository/npm-all/
```

Then verify:

```bash
npm get registry
# should output http://localhost:8081/repository/npm-all/
```

### Option B — Project configuration

Create a `.npmrc` file in your project root:

```bash
registry=http://localhost:8081/repository/npm-all/
```

> 💡 If your Nexus requires authentication, also add:

```bash
//localhost:8081/repository/npm-all/:_authToken=<your-base64-token>
always-auth=true
```

---

## 🔐 4. Authenticate with Nexus (Optional)

If authentication is enabled:

1. Log into Nexus.
2. Go to your profile → **Security → User Token** → Generate a new token.
3. Then set it in npm:

   ```bash
   npm login --registry=http://localhost:8081/repository/npm-internal/
   ```

   Enter:

   ```
   Username: admin
   Password: <your password or token>
   Email: (your email)
   ```

---

## 📦 5. Publish Your Private npm Package

In your package’s `package.json`:

```json
{
  "name": "@myorg/hello-nexus",
  "version": "1.0.0",
  "main": "index.js",
  "publishConfig": {
    "registry": "http://localhost:8081/repository/npm-internal/"
  }
}
```

Then publish:

```bash
npm publish
```

✅ This uploads your package to the hosted repository `npm-internal`.

---

## 🔄 6. Install Packages via Nexus

When installing packages:

```bash
npm install express
```

Nexus will:

* Check `npm-internal` first (your private packages),
* Then `npmjs` proxy (cached from npmjs.org).

---

## 🧰 7. Verifying Cache Behavior

You can see caching in action:

1. In the Nexus UI → `Repositories → npmjs`
2. Check the “Browse” tab.
3. You’ll see folders being created for each downloaded package.

This speeds up builds and reduces external dependency failures.

---

## 🧹 8. Maintenance & Best Practices

| Task               | How                                               |
| ------------------ | ------------------------------------------------- |
| Clean old packages | Scheduled Tasks → “Cleanup npm components”        |
| Restrict access    | Create roles → assign permissions to repositories |
| Use HTTPS          | Reverse proxy (e.g., Nginx + SSL)                 |
| Backup data        | Backup `/nexus-data` folder regularly             |

---

## 🧪 9. Quick Example: Private Package + Project

### Step 1 — Create `hello-nexus` package

```bash
mkdir hello-nexus && cd hello-nexus
npm init -y
echo "module.exports = () => console.log('Hello from Nexus!')" > index.js
```

### Step 2 — Publish it

```bash
npm publish --registry=http://localhost:8081/repository/npm-internal/
```

### Step 3 — In another project

```bash
npm install @myorg/hello-nexus --registry=http://localhost:8081/repository/npm-all/
```

---

## 📚 10. Next Steps

* [Official npm repository setup guide (Sonatype)](https://help.sonatype.com/repomanager3/node-packaged-modules-and-npm-registries)
* Integrate with CI/CD (e.g. GitHub Actions, Jenkins)
* Create automation scripts using Nexus **REST API**

---

# References
1. https://siddhivinayak-sk.medium.com/manage-libraries-artifacts-and-deliverables-with-nexus-repository-manager-oss-2252ec3a35ff