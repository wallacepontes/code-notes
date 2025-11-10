# Faker.js

## Table of Contents
- [Faker.js](#fakerjs)
  - [Table of Contents](#table-of-contents)
  - [🧠 What Is Faker.js?](#-what-is-fakerjs)
  - [⚙️ 1. Install Faker.js](#️-1-install-fakerjs)
  - [🧩 2. Basic Usage Example](#-2-basic-usage-example)
  - [🧱 3. Creating Structured Fake Objects](#-3-creating-structured-fake-objects)
  - [🧪 4. Generating Lists of Fake Data](#-4-generating-lists-of-fake-data)
  - [🌍 5. Locales (Different Languages)](#-5-locales-different-languages)
  - [🧰 6. Useful Faker Modules](#-6-useful-faker-modules)
  - [💾 7. Seeding Data for Consistency](#-7-seeding-data-for-consistency)
  - [🧱 8. Using Faker in Tests](#-8-using-faker-in-tests)
  - [🚀 9. Bonus: Generate Mock APIs with Express](#-9-bonus-generate-mock-apis-with-express)
  - [🧾 Summary](#-summary)
  - [References](#references)

Here’s a **complete beginner-friendly tutorial** to get you started with **Faker.js**, the popular library for generating fake data in JavaScript projects — perfect for testing, seeding databases, or creating mock APIs.

---

## 🧠 What Is Faker.js?

**Faker.js** is a JavaScript library that helps you generate fake (but realistic) data — such as names, addresses, emails, company names, lorem text, and much more.

The project was originally abandoned but now lives as a modern package called **`@faker-js/faker`**, actively maintained by the community.

---

## ⚙️ 1. Install Faker.js

First, initialize a Node.js project (if you don’t have one yet):

```bash
mkdir faker-demo
cd faker-demo
npm init -y
```

Then install Faker:

```bash
npm install @faker-js/faker
```

---

## 🧩 2. Basic Usage Example

Create a new file named `index.js`:

```js
// Import faker
import { faker } from '@faker-js/faker';

// Generate fake data
console.log('Name:', faker.person.fullName());
console.log('Email:', faker.internet.email());
console.log('Phone:', faker.phone.number());
console.log('Address:', faker.location.streetAddress());
console.log('Company:', faker.company.name());
console.log('Lorem text:', faker.lorem.sentence());
```

If your Node.js version doesn’t support ES Modules, use:

```js
const { faker } = require('@faker-js/faker');
```

Then run:

```bash
node index.js
```

You’ll see random but realistic outputs each time.

---

## 🧱 3. Creating Structured Fake Objects

You can use Faker to mock more complex data structures:

```js
const user = {
  id: faker.string.uuid(),
  name: faker.person.fullName(),
  email: faker.internet.email(),
  phone: faker.phone.number(),
  address: faker.location.streetAddress(),
  company: faker.company.name(),
  registeredAt: faker.date.past(),
};

console.log(user);
```

---

## 🧪 4. Generating Lists of Fake Data

Let’s say you need a list of 10 users:

```js
const users = Array.from({ length: 10 }, () => ({
  id: faker.string.uuid(),
  name: faker.person.fullName(),
  email: faker.internet.email(),
  city: faker.location.city(),
  country: faker.location.country(),
}));

console.table(users);
```

---

## 🌍 5. Locales (Different Languages)

Faker supports **multiple locales**, so you can generate data in different languages or country formats.

```js
import { Faker, en, pt_BR } from '@faker-js/faker';

// Create a faker instance with Brazilian Portuguese locale
const fakerBR = new Faker({ locale: [pt_BR, en] });

console.log(fakerBR.person.fullName());
console.log(fakerBR.location.city());
```

---

## 🧰 6. Useful Faker Modules

Here are some of the most useful categories:

| Category         | Example                                                |
| ---------------- | ------------------------------------------------------ |
| `faker.person`   | `faker.person.firstName()`, `faker.person.lastName()`  |
| `faker.internet` | `faker.internet.email()`, `faker.internet.url()`       |
| `faker.location` | `faker.location.country()`, `faker.location.zipCode()` |
| `faker.company`  | `faker.company.name()`                                 |
| `faker.phone`    | `faker.phone.number()`                                 |
| `faker.string`   | `faker.string.alphanumeric(10)`                        |
| `faker.lorem`    | `faker.lorem.paragraph()`                              |
| `faker.date`     | `faker.date.future()`                                  |

Full API reference: [https://fakerjs.dev/api/](https://fakerjs.dev/api/)

---

## 💾 7. Seeding Data for Consistency

You can make Faker produce **repeatable data** using a seed value:

```js
faker.seed(123);

console.log(faker.person.fullName());
console.log(faker.person.fullName()); // Same output every time
```

---

## 🧱 8. Using Faker in Tests

Example with **Jest**:

```js
import { faker } from '@faker-js/faker';
import { createUser } from './userService.js';

test('should create a new user with valid data', () => {
  const fakeUser = {
    name: faker.person.fullName(),
    email: faker.internet.email(),
  };

  const result = createUser(fakeUser);
  expect(result).toHaveProperty('id');
});
```

---

## 🚀 9. Bonus: Generate Mock APIs with Express

You can even combine **Faker** with **Express** to mock an API endpoint:

```js
import express from 'express';
import { faker } from '@faker-js/faker';

const app = express();

app.get('/api/users', (req, res) => {
  const users = Array.from({ length: 5 }, () => ({
    id: faker.string.uuid(),
    name: faker.person.fullName(),
    email: faker.internet.email(),
  }));
  res.json(users);
});

app.listen(3000, () => console.log('Mock API running on http://localhost:3000'));
```

Now visit `http://localhost:3000/api/users` and see fake JSON user data.

---

## 🧾 Summary

| Step | Description                    |
| ---- | ------------------------------ |
| 1    | Install `@faker-js/faker`      |
| 2    | Generate simple fake data      |
| 3    | Create structured fake objects |
| 4    | Generate lists of fake data    |
| 5    | Use locales                    |
| 6    | Explore Faker modules          |
| 7    | Use seeds for reproducibility  |
| 8    | Integrate with tests           |
| 9    | Create mock APIs               |

---

Would you like me to extend this tutorial with a **“Database Seeder Example”** (for populating mock users into a PostgreSQL/MySQL database)?

## References
1. https://fakerjs.dev/