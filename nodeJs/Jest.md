# Jest

## Table of Contents
- [Jest](#jest)
  - [Table of Contents](#table-of-contents)
  - [🧪 What Is Jest?](#-what-is-jest)
  - [⚙️ 1. Installation](#️-1-installation)
  - [🧰 2. Configure Jest](#-2-configure-jest)
  - [🧩 3. Write Your First Test](#-3-write-your-first-test)
  - [🧠 4. Jest Matchers](#-4-jest-matchers)
  - [🧱 5. Grouping Tests](#-5-grouping-tests)
  - [🧩 6. Mocking \& Spying](#-6-mocking--spying)
  - [📸 7. Snapshot Testing (UI or JSON)](#-7-snapshot-testing-ui-or-json)
  - [📈 8. Code Coverage](#-8-code-coverage)
  - [🧭 9. With TypeScript Example](#-9-with-typescript-example)
  - [🧰 10. Common Jest CLI Options](#-10-common-jest-cli-options)
  - [🧩 11. Example Project Structure](#-11-example-project-structure)
  - [✅ Summary](#-summary)
  - [Jest Resume](#jest-resume)
    - [Key features and concepts of Jest:](#key-features-and-concepts-of-jest)
  - [Videos](#videos)
  - [References](#references)

Here’s a **complete “Getting Started with Jest” tutorial** — perfect if you’re setting up testing for a Node.js or TypeScript project.

---

## 🧪 What Is Jest?

[Jest](https://jestjs.io/) is a **JavaScript testing framework** developed by Facebook, designed to work seamlessly with:

* **Node.js** projects
* **React**, **Vue**, and **Angular** apps
* **TypeScript** (with simple setup)

It’s known for:

* **Zero-config setup**
* **Fast test runs**
* **Mocking**, **snapshot testing**, and **coverage reports** built in.

---

## ⚙️ 1. Installation

First, initialize your Node project:

```bash
mkdir jest-tutorial
cd jest-tutorial
npm init -y
```

Then install Jest:

```bash
npm install --save-dev jest
```

If you’re using **TypeScript**:

```bash
npm install --save-dev typescript ts-jest @types/jest
```

---

## 🧰 2. Configure Jest

Open `package.json` and add a test script:

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

If you’re using TypeScript, initialize Jest config with `ts-jest`:

```bash
npx ts-jest config:init
```

This creates a `jest.config.js` file with defaults:

```js
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
};
```

---

## 🧩 3. Write Your First Test

Create a simple function to test:
**`sum.js`**

```js
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

Now write a test:
**`sum.test.js`**

```js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

Run tests:

```bash
npm test
```

✅ You’ll see:

```
PASS  ./sum.test.js
✓ adds 1 + 2 to equal 3 (2 ms)
```

---

## 🧠 4. Jest Matchers

Jest provides many **assertion helpers**. Examples:

| Matcher         | Example                          | Description     |
| --------------- | -------------------------------- | --------------- |
| `.toBe()`       | `expect(a).toBe(3)`              | Strict equality |
| `.toEqual()`    | `expect(obj).toEqual({x:1})`     | Deep equality   |
| `.toContain()`  | `expect(arr).toContain('apple')` | Contain element |
| `.toThrow()`    | `expect(() => fn()).toThrow()`   | Function throws |
| `.toBeTruthy()` | `expect(val).toBeTruthy()`       | Truthy check    |
| `.toBeNull()`   | `expect(val).toBeNull()`         | Null check      |

Example:

```js
test('object assignment', () => {
  const data = { one: 1 };
  data['two'] = 2;
  expect(data).toEqual({ one: 1, two: 2 });
});
```

---

## 🧱 5. Grouping Tests

You can group tests with `describe()` blocks:

```js
describe('Math operations', () => {
  test('adds numbers', () => {
    expect(1 + 2).toBe(3);
  });

  test('subtracts numbers', () => {
    expect(5 - 2).toBe(3);
  });
});
```

---

## 🧩 6. Mocking & Spying

Mocks let you simulate modules or functions.

Example:

```js
const fetchData = jest.fn(() => 'Hello');

test('mock function', () => {
  expect(fetchData()).toBe('Hello');
  expect(fetchData).toHaveBeenCalled();
});
```

Mocking an imported module:

```js
jest.mock('./api');
const api = require('./api');

api.getUser.mockReturnValue({ id: 1, name: 'Wallace' });

test('gets user data', () => {
  expect(api.getUser()).toEqual({ id: 1, name: 'Wallace' });
});
```

---

## 📸 7. Snapshot Testing (UI or JSON)

Great for React or API outputs.

```js
test('snapshot example', () => {
  const user = { name: 'Alice', age: 30 };
  expect(user).toMatchSnapshot();
});
```

On first run, Jest saves the snapshot. Later runs compare against it.

---

## 📈 8. Code Coverage

Generate a coverage report:

```bash
npm test -- --coverage
```

You’ll get:

```
----------------|----------|----------|----------|----------|-------------------|
File            |  % Stmts | % Branch |  % Funcs |  % Lines | Uncovered Line #s |
----------------|----------|----------|----------|----------|-------------------|
All files       |      100 |      100 |      100 |      100 |                   |
----------------|----------|----------|----------|----------|-------------------|
```

HTML report is generated in `coverage/`.

---

## 🧭 9. With TypeScript Example

**sum.ts**

```ts
export function sum(a: number, b: number): number {
  return a + b;
}
```

**sum.test.ts**

```ts
import { sum } from './sum';

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

Run:

```bash
npm test
```

---

## 🧰 10. Common Jest CLI Options

| Command       | Description                     |
| ------------- | ------------------------------- |
| `--watch`     | Run tests on file change        |
| `--coverage`  | Show code coverage              |
| `--verbose`   | Show each test result           |
| `--runInBand` | Run sequentially (useful in CI) |

---

## 🧩 11. Example Project Structure

```
jest-tutorial/
├── src/
│   ├── sum.ts
│   └── subtract.ts
├── tests/
│   ├── sum.test.ts
│   └── subtract.test.ts
├── jest.config.js
├── package.json
└── tsconfig.json
```

---

## ✅ Summary

| Step | Description                          |
| ---- | ------------------------------------ |
| 1    | Install Jest (`npm i -D jest`)       |
| 2    | Add `"test": "jest"` script          |
| 3    | Write `.test.js` or `.test.ts` files |
| 4    | Run tests with `npm test`            |
| 5    | Use matchers, mocks, and snapshots   |

---

Would you like me to extend this tutorial with **integration testing (e.g. API + Jest + Supertest)** or keep it focused on **unit testing only**?

## Jest Resume

Jest is a popular and widely used open-source JavaScript testing framework, primarily known for its efficiency in unit testing. It was developed by Facebook (now Meta) and is commonly used for testing React applications, though it can be adapted for any JavaScript codebase.

### Key features and concepts of Jest:

- Installation: Jest is typically installed as a development dependency using npm: npm install --save-dev jest.
- Test Script: The package.json file is configured to run Jest using the npm test command.
- Test Files: Test files usually reside in the same directory as the code they test or in a dedicated __tests__ folder and are named with .test.js or .spec.js extensions.
- Test Cases: Individual tests are defined using the test() function (or it(), which is an alias), providing a descriptive name and a callback function containing the test logic.
- Assertions: Jest provides the expect() function and a rich set of "matchers" (e.g., toBe(), toEqual(), toBeTruthy(), toBeFalsy()) to assert the expected outcome of the code being tested.
- Grouping Tests: The describe() function allows for grouping related tests into logical blocks, improving organization and readability.
- Snapshot Testing: Jest supports snapshot testing, which is particularly useful for UI components. It captures a snapshot of a component's rendered output and compares it against subsequent snapshots to detect unexpected changes.
- Code Coverage: Jest can generate coverage reports to show the percentage of code covered by tests, helping identify areas with insufficient testing. This feature is enabled by setting collectCoverage: true in jest.config.json.
- Asynchronous Testing: Jest provides mechanisms for handling asynchronous code, including async/await and .resolves() for promises.
- CLI Options: Jest offers various command-line interface options for running specific tests, sharding tests, and more.
- Integration: Jest integrates well with other tools like Babel and TypeScript (via ts-jest) and various web frameworks.

## Videos

 * [Introduction to Jest Testing | JavaScript Unit Tests](https://www.youtube.com/watch?v=x6NUZ8dc9Qg)
	> [<img src="https://img.youtube.com/vi/x6NUZ8dc9Qg/0.jpg" width="200">](https://www.youtube.com/watch?v=x6NUZ8dc9Qg "Learn JavaScript unit testing with Jest, a popular testing framework. Test functions, compare results, and catch bugs early. by Dave Gray 777 views 25 minutes, 30 seconds")

 * [Aprenda testar seu backend Node JS com Jest](https://www.youtube.com/watch?v=Ru6Tr7Q75IQ)
	> [<img src="https://img.youtube.com/vi/Ru6Tr7Q75IQ/0.jpg" width="200">](https://www.youtube.com/watch?v=Ru6Tr7Q75IQ "Learn JavaScript unit testing with Jest, a popular testing framework. Test functions, compare results, and catch bugs early. by Fernanda Kipper 777 views 37 minutes")

## References

1. https://jestjs.io/
2. https://www.devmedia.com.br/teste-unitario-com-jest/41234
3. https://www.geeksforgeeks.org/javascript/testing-with-jest/
4. https://nextjs.org/docs/app/guides/testing/jest
5. 