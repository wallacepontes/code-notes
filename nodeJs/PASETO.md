# PASETO

## Table of Contents

- [PASETO](#paseto)
  - [Table of Contents](#table-of-contents)
  - [🧭 What Is PASETO?](#-what-is-paseto)
  - [🔑 Token Types in PASETO](#-token-types-in-paseto)
  - [⚙️ Step 1 — Install PASETO Library](#️-step-1--install-paseto-library)
  - [🧪 Step 2 — Generate Keys](#-step-2--generate-keys)
    - [For `v4.public` (signed tokens)](#for-v4public-signed-tokens)
    - [For `v4.local` (encrypted tokens)](#for-v4local-encrypted-tokens)
  - [🔏 Step 3 — Create a Token](#-step-3--create-a-token)
    - [✅ Example: Signed Token (`v4.public`)](#-example-signed-token-v4public)
    - [🔐 Example: Encrypted Token (`v4.local`)](#-example-encrypted-token-v4local)
  - [🧩 Step 4 — Verify or Decrypt Tokens](#-step-4--verify-or-decrypt-tokens)
    - [Verify a `public` (signed) token:](#verify-a-public-signed-token)
    - [Decrypt a `local` (encrypted) token:](#decrypt-a-local-encrypted-token)
  - [🧠 Step 5 — Best Practices](#-step-5--best-practices)
  - [🧰 Step 6 — Example: Using PASETO in Express.js](#-step-6--example-using-paseto-in-expressjs)
  - [🆚 PASETO vs JWT](#-paseto-vs-jwt)
  - [✅ Summary](#-summary)
  - [References](#references)

Here’s a **step-by-step tutorial to get started with PASETO (Platform-Agnostic Security Tokens)** — a modern, safer alternative to JWT (JSON Web Token).

---

## 🧭 What Is PASETO?

**PASETO (Platform-Agnostic Security Tokens)** is a security token format similar to JWT, but with **built-in cryptographic safety** — no insecure algorithms, no confusion between encryption and signing.

PASETO was designed to fix JWT’s most common pitfalls:

* No `"alg":"none"` issue.
* Strong default cryptography.
* Clear distinction between **local (encrypted)** and **public (signed)** tokens.

---

## 🔑 Token Types in PASETO

| Type       | Keyword     | Meaning                  | Use case                        |
| ---------- | ----------- | ------------------------ | ------------------------------- |
| **Local**  | `v2.local`  | Encrypted (confidential) | Sensitive data (like PII)       |
| **Public** | `v2.public` | Signed (integrity only)  | Identity / authorization tokens |

---

## ⚙️ Step 1 — Install PASETO Library

For Node.js (JavaScript/TypeScript):

```bash
npm install paseto
```

For Java:

```xml
<dependency>
  <groupId>dev.paseto</groupId>
  <artifactId>paseto</artifactId>
  <version>2.0.2</version>
</dependency>
```

For Python:

```bash
pip install paseto
```

We’ll continue the examples in **Node.js**.

---

## 🧪 Step 2 — Generate Keys

### For `v4.public` (signed tokens)

```js
import { V4 } from 'paseto';

const { secretKey, publicKey } = await V4.generateKey('public');
console.log('Secret Key:', secretKey.export());
console.log('Public Key:', publicKey.export());
```

### For `v4.local` (encrypted tokens)

```js
import { V4 } from 'paseto';

const key = await V4.generateKey('local');
console.log('Local Key:', key.export());
```

---

## 🔏 Step 3 — Create a Token

### ✅ Example: Signed Token (`v4.public`)

```js
import { V4 } from 'paseto';

const payload = {
  sub: 'user123',
  role: 'admin',
  exp: '2025-12-31T23:59:59Z',
};

const token = await V4.sign(payload, secretKey, {
  footer: { kid: 'key-id-123' },
});

console.log('Generated Token:', token);
```

Output looks like:

```
v4.public.eyJ...base64-encoded-data...
```

---

### 🔐 Example: Encrypted Token (`v4.local`)

```js
import { V4 } from 'paseto';

const payload = {
  email: 'user@example.com',
  sessionId: 'abc123',
  exp: '2025-12-31T23:59:59Z',
};

const token = await V4.encrypt(payload, key, {
  footer: { app: 'my-service' },
});

console.log('Encrypted Token:', token);
```

Output looks like:

```
v4.local.AC5v...encrypted-data...
```

---

## 🧩 Step 4 — Verify or Decrypt Tokens

### Verify a `public` (signed) token:

```js
import { V4 } from 'paseto';

try {
  const payload = await V4.verify(token, publicKey);
  console.log('Verified payload:', payload);
} catch (err) {
  console.error('Verification failed:', err.message);
}
```

---

### Decrypt a `local` (encrypted) token:

```js
import { V4 } from 'paseto';

try {
  const payload = await V4.decrypt(token, key);
  console.log('Decrypted payload:', payload);
} catch (err) {
  console.error('Decryption failed:', err.message);
}
```

---

## 🧠 Step 5 — Best Practices

✅ **Use `v4` version** (the latest and most secure).
✅ **Use `local` for confidentiality**, and `public` for integrity-only use cases.
✅ Always include an **`exp`** (expiration) claim.
✅ Rotate your keys periodically.
✅ Avoid encoding sensitive data in `public` tokens — use `local` instead.

---

## 🧰 Step 6 — Example: Using PASETO in Express.js

```js
import express from 'express';
import { V4 } from 'paseto';

const app = express();
app.use(express.json());

const { secretKey, publicKey } = await V4.generateKey('public');

app.post('/login', async (req, res) => {
  const user = { id: 'user123', role: 'admin' };
  const token = await V4.sign(user, secretKey, { expiresIn: '1h' });
  res.json({ token });
});

app.get('/protected', async (req, res) => {
  const token = req.headers.authorization?.replace('Bearer ', '');
  try {
    const payload = await V4.verify(token, publicKey);
    res.json({ message: 'Access granted', user: payload });
  } catch {
    res.status(401).json({ error: 'Invalid token' });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---

## 🆚 PASETO vs JWT

| Feature               | JWT                | PASETO                         |
| --------------------- | ------------------ | ------------------------------ |
| Algorithms            | Many (some unsafe) | Only safe defaults             |
| Encryption            | Optional           | Clear `local` (encrypted) mode |
| Misconfiguration risk | High               | Low                            |
| Simplicity            | Medium             | High                           |
| Adoption              | Very High          | Growing                        |

---

## ✅ Summary

| Step | Description                         |
| ---- | ----------------------------------- |
| 1    | Install the PASETO library          |
| 2    | Generate keys (`public` or `local`) |
| 3    | Create and sign/encrypt tokens      |
| 4    | Verify or decrypt them              |
| 5    | Follow security best practices      |
| 6    | Integrate into your API             |

---

Would you like me to extend this with a **real-world example**, like implementing **user authentication with PASETO** in Node.js or Java?

## References

1. https://paseto.io/
2. https://github.com/paragonie/paseto
3. https://permify.co/post/jwt-paseto/
4. https://github.com/panva/paseto
5. 