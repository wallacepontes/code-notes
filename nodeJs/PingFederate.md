# Ping Federate

## Table of Contents

- [Ping Federate](#ping-federate)
  - [Table of Contents](#table-of-contents)
  - [🧭 1. What Is PingFederate?](#-1-what-is-pingfederate)
  - [⚙️ 2. Installation and Setup](#️-2-installation-and-setup)
    - [🧩 Prerequisites](#-prerequisites)
    - [🐳 Option 1: Run via Docker (Recommended for Beginners)](#-option-1-run-via-docker-recommended-for-beginners)
    - [🖥️ Option 2: Manual Installation (Zip/Tar)](#️-option-2-manual-installation-ziptar)
  - [🧩 3. Navigating the Admin Console](#-3-navigating-the-admin-console)
  - [🔐 4. Creating a Simple SSO Federation (SAML)](#-4-creating-a-simple-sso-federation-saml)
    - [Step 1. Create an IdP Connection](#step-1-create-an-idp-connection)
    - [Step 2. Map User Attributes](#step-2-map-user-attributes)
    - [Step 3. Configure an Authentication Source](#step-3-configure-an-authentication-source)
    - [Step 4. Test](#step-4-test)
  - [🔑 5. Getting Started with OAuth2 / OIDC](#-5-getting-started-with-oauth2--oidc)
    - [Step 1. Enable OAuth Server](#step-1-enable-oauth-server)
    - [Step 2. Create a Client](#step-2-create-a-client)
    - [Step 3. Test Authorization Code Flow](#step-3-test-authorization-code-flow)
  - [🧱 6. Extending PingFederate](#-6-extending-pingfederate)
  - [🧩 7. Common Use Cases](#-7-common-use-cases)
  - [🧰 8. Troubleshooting](#-8-troubleshooting)
  - [📚 9. Learning Resources](#-9-learning-resources)
  - [✅ Summary](#-summary)
  - [References](#references)

Perfect — let’s go step-by-step through **Getting Started with PingFederate**, the **enterprise-grade Identity Provider (IdP) and federation server** used for implementing **Single Sign-On (SSO)**, **OAuth2**, and **OpenID Connect (OIDC)** integrations.

---

## 🧭 1. What Is PingFederate?

**PingFederate** is a **federation server** that enables **SSO, identity federation, and secure API access** between identity providers (IdPs) and service providers (SPs).
It supports industry standards like:

* **SAML 2.0 / 1.1**
* **OAuth 2.0**
* **OpenID Connect (OIDC)**
* **WS-Federation**
* **SCIM (User Provisioning)**

You can use PingFederate as:

* An **Identity Provider (IdP)**: authenticating users and issuing tokens/assertions.
* A **Service Provider (SP)**: relying on tokens/assertions from another IdP.
* An **Authorization Server (AS)**: issuing OAuth2 / OIDC tokens to protect APIs.

---

## ⚙️ 2. Installation and Setup

### 🧩 Prerequisites

* **Java 11 or 17** (JDK required)
* **4GB+ RAM** recommended
* **Admin rights** on your system
* Optional: **Docker** if you prefer containerized setup

---

### 🐳 Option 1: Run via Docker (Recommended for Beginners)

```bash
docker run -d \
  -p 9999:9999 -p 9031:9031 \
  --name pingfederate \
  pingidentity/pingfederate:latest
```

* `9031`: Admin console port
* `9999`: Runtime port (used by SP/IdP endpoints)

After container starts, open:

👉 [http://localhost:9031/pingfederate/app](http://localhost:9031/pingfederate/app)
(Default admin credentials are shown in container logs on first run.)

---

### 🖥️ Option 2: Manual Installation (Zip/Tar)

1. Download from [Ping Identity Downloads](https://www.pingidentity.com/en/resources/downloads.html).
2. Unzip:

   ```bash
   unzip pingfederate-<version>.zip -d /opt/pingfederate
   ```
3. Start the server:

   ```bash
   cd /opt/pingfederate
   ./bin/run.sh
   ```
4. Open: [http://localhost:9999/pingfederate/app](http://localhost:9999/pingfederate/app)

---

## 🧩 3. Navigating the Admin Console

After login, you’ll see the **PingFederate Administration Console**, where you can configure:

* **Identity Provider (IdP) connections**
* **Service Provider (SP) connections**
* **OAuth Clients**
* **Token Issuers / Mappers**
* **Authentication Sources (LDAP, JDBC, etc.)**

---

## 🔐 4. Creating a Simple SSO Federation (SAML)

Let’s simulate a **SAML 2.0 connection** between an IdP (PingFederate) and an SP.

### Step 1. Create an IdP Connection

* Go to **IdP Configuration → SP Connections → Create New**
* Choose **Browser SSO** profile
* Enter **SP metadata** (either upload XML or manually fill endpoints)
* Configure:

  * **Assertion Consumer Service (ACS)** URL of SP
  * **Entity ID** of SP
  * **Certificate** for signing

### Step 2. Map User Attributes

* Under **Attribute Contract**, map internal attributes (e.g., username, email) to outgoing SAML attributes.

### Step 3. Configure an Authentication Source

* Go to **Authentication → Policies**
* Add a new policy using:

  * **Username/Password**
  * or connect to **LDAP**, **AD**, or **custom IdP adapter**

### Step 4. Test

* Go to **SP Initiated SSO URL**
* Authenticate as a user
* Verify SAML assertion in browser dev tools or Ping logs

---

## 🔑 5. Getting Started with OAuth2 / OIDC

PingFederate can act as an **Authorization Server (AS)**.

### Step 1. Enable OAuth Server

* Navigate to **OAuth Settings → Authorization Server Settings**
* Enable and configure:

  * **Issuer Identifier**
  * **Token Endpoint**
  * **Scopes**

### Step 2. Create a Client

* Go to **OAuth Settings → Clients → Create New**
* Define:

  * **Client ID / Secret**
  * **Grant Type** (Authorization Code, Client Credentials, etc.)
  * **Redirect URIs**

### Step 3. Test Authorization Code Flow

```bash
https://localhost:9031/as/authorization.oauth2?client_id=my-client&response_type=code&redirect_uri=https://example.com/callback&scope=openid
```

Then exchange the `code` for a token:

```bash
curl -u my-client:secret \
  -d "grant_type=authorization_code&code=<CODE>&redirect_uri=https://example.com/callback" \
  https://localhost:9031/as/token.oauth2
```

---

## 🧱 6. Extending PingFederate

PingFederate is highly **extensible** via **plugins and SDKs**:

* **Java SDK** for custom IdP/SP adapters
* **Authentication APIs**
* **Provisioning connectors**
* **REST APIs** for automation (`/pf-admin-api/v1`)

Example: call the Admin REST API

```bash
curl -u admin:password \
  https://localhost:9999/pf-admin-api/v1/version
```

---

## 🧩 7. Common Use Cases

| Use Case               | PingFederate Role    | Protocol       |
| ---------------------- | -------------------- | -------------- |
| SSO between companies  | IdP/SP               | SAML 2.0       |
| Login via corporate AD | IdP                  | LDAP + SAML    |
| API protection         | Authorization Server | OAuth2         |
| Mobile login           | OIDC Provider        | OpenID Connect |
| User provisioning      | SCIM                 | REST/SCIM      |

---

## 🧰 8. Troubleshooting

| Issue                | Cause                     | Fix                               |
| -------------------- | ------------------------- | --------------------------------- |
| Admin UI not loading | Wrong port                | Use 9031                          |
| Invalid certificate  | Expired or missing        | Import valid cert                 |
| Token not issued     | Wrong client config       | Verify redirect URIs & grant type |
| SAML error           | Wrong ACS URL or mismatch | Check entity IDs and metadata     |

Logs are under:

```
<PF_HOME>/pingfederate/log/
```

---

## 📚 9. Learning Resources

* 🔗 [Official PingFederate Documentation](https://docs.pingidentity.com/pingfederate)
* 🧠 [Ping Identity YouTube Channel](https://www.youtube.com/user/pingidentity)
* 📘 [PingFederate Administrator Guide (PDF)](https://docs.pingidentity.com/r/en-us/pingfederate)
* 💬 [Ping Community Forum](https://community.pingidentity.com/)

---

## ✅ Summary

| Goal           | Step                                                                             |
| -------------- | -------------------------------------------------------------------------------- |
| Install        | Docker or Zip                                                                    |
| Access UI      | [http://localhost:9031/pingfederate/app](http://localhost:9031/pingfederate/app) |
| Configure SSO  | IdP/SP Connections (SAML)                                                        |
| Configure APIs | OAuth2/OIDC Clients                                                              |
| Extend         | Use SDKs & REST APIs                                                             |

---

Would you like me to extend this tutorial with a **hands-on SAML or OAuth2 example** (e.g., connecting PingFederate to a Node.js app or Spring Boot API)?


## References

1. https://www.pingidentity.com/en/product/pingfederate.html
2. 