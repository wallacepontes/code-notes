# Immutability in TypeScript

Perfect 👌 — let’s build a **complete, modern tutorial** on how to get started with **immutability in TypeScript**, covering everything from basics to advanced patterns.

This will give you a **single reference guide** — ideal whether you’re doing a small app or building a large-scale architecture.

## Table of Contents
- [Immutability in TypeScript](#immutability-in-typescript)
  - [Table of Contents](#table-of-contents)
- [🧊 Getting Started with Immutability in TypeScript](#-getting-started-with-immutability-in-typescript)
  - [1️⃣ What Is Immutability?](#1️⃣-what-is-immutability)
  - [2️⃣ Using `readonly` — the Built-in Option](#2️⃣-using-readonly--the-built-in-option)
  - [3️⃣ Creating a Global `Immutable<T>` Type](#3️⃣-creating-a-global-immutablet-type)
    - [How to use it:](#how-to-use-it)
  - [4️⃣ Optional: Add Runtime Protection (`deepFreeze`)](#4️⃣-optional-add-runtime-protection-deepfreeze)
  - [5️⃣ Immutability in State Management (React/Redux)](#5️⃣-immutability-in-state-management-reactredux)
  - [6️⃣ When to Use Each Approach](#6️⃣-when-to-use-each-approach)
  - [7️⃣ Example Project Structure](#7️⃣-example-project-structure)
  - [✅ Summary](#-summary)
  - [🚀 Next Steps](#-next-steps)
- [Enforce global immutability in TypeScript](#enforce-global-immutability-in-typescript)
  - [🧩 1. The Problem with `readonly`](#-1-the-problem-with-readonly)
  - [🛠️ 2. Define a Global Immutable Type](#️-2-define-a-global-immutable-type)
  - [🌍 3. Use It Globally](#-3-use-it-globally)
  - [🧠 4. (Optional) Make it Runtime Safe Too](#-4-optional-make-it-runtime-safe-too)
  - [⚙️ 5. Bonus: Enable Strictness](#️-5-bonus-enable-strictness)
  - [✅ Summary](#-summary-1)
- [Mutating State in Code](#mutating-state-in-code)
  - [🧩 If You Mean “Mutating State in Code”](#-if-you-mean-mutating-state-in-code)
  - [🧊 If You’re Using Immutable.js or Immer](#-if-youre-using-immutablejs-or-immer)
  - [🧠 So in Practice](#-so-in-practice)
  - [⚖️ Bottom Line](#️-bottom-line)
- [Can TypeScript's readonly fully replace Immutable.js?](#can-typescripts-readonly-fully-replace-immutablejs)
  - [🧩 What TypeScript’s `readonly` Actually Does](#-what-typescripts-readonly-actually-does)
  - [🧊 What Immutable.js Does](#-what-immutablejs-does)
  - [🔍 Comparison Summary](#-comparison-summary)
  - [💡 When `readonly` Is Enough](#-when-readonly-is-enough)
  - [🧠 When Immutable.js (or similar) Is Better](#-when-immutablejs-or-similar-is-better)
  - [⚖️ Practical Middle Ground](#️-practical-middle-ground)
- [References](#references)

---

# 🧊 Getting Started with Immutability in TypeScript

---

## 1️⃣ What Is Immutability?

**Immutability** means **once data is created, it cannot be changed**.
Instead of mutating objects, you create new versions when updating them.

Why this matters:

* Avoids side effects and “weird bugs”
* Makes debugging and testing easier
* Enables safer concurrency and state management (e.g. React, Redux, etc.)

Example (mutable ❌):

```ts
const user = { name: "Alice", age: 25 };
user.age = 26; // same object modified
```

Example (immutable ✅):

```ts
const user = { name: "Alice", age: 25 };
const newUser = { ...user, age: 26 }; // new object, original untouched
```

---

## 2️⃣ Using `readonly` — the Built-in Option

TypeScript offers the `readonly` modifier to mark properties as immutable:

```ts
interface User {
  readonly name: string;
  readonly age: number;
}

const user: User = { name: "Alice", age: 25 };
user.age = 30; // ❌ compile-time error
```

✅ Pros:

* Simple
* Enforced at compile time

⚠️ Cons:

* **Shallow only** (nested objects can still change)
* **Not enforced at runtime**

Example of shallow limitation:

```ts
interface Address { city: string }
interface Person { readonly address: Address }

const person: Person = { address: { city: "NY" } };
person.address.city = "LA"; // ✅ Allowed! (still mutable)
```

---

## 3️⃣ Creating a Global `Immutable<T>` Type

Let’s make immutability **deep and global** using TypeScript generics.

Create a file: `src/types/global.d.ts`

```ts
// src/types/global.d.ts
declare global {
  type Primitive = string | number | boolean | bigint | symbol | null | undefined;

  type Immutable<T> =
    T extends Primitive ? T :
    T extends Array<infer U> ? ReadonlyArray<Immutable<U>> :
    T extends Map<infer K, infer V> ? ReadonlyMap<Immutable<K>, Immutable<V>> :
    T extends Set<infer U> ? ReadonlySet<Immutable<U>> :
    T extends object ? { readonly [K in keyof T]: Immutable<T[K]> } :
    T;
}

export {};
```

### How to use it:

```ts
interface User {
  name: string;
  tags: string[];
  address: {
    city: string;
    country: string;
  };
}

const user: Immutable<User> = {
  name: "Alice",
  tags: ["dev", "typescript"],
  address: { city: "NY", country: "USA" },
};

// ❌ compile-time errors:
user.name = "Bob";
user.address.city = "LA";
user.tags.push("new");
```

Now your entire structure is **recursively readonly** (deep immutability).

---

## 4️⃣ Optional: Add Runtime Protection (`deepFreeze`)

You can enforce immutability **at runtime** using a helper.

Create `src/utils/deepFreeze.ts`:

```ts
export function deepFreeze<T>(obj: T): Immutable<T> {
  if (obj && typeof obj === "object" && !Object.isFrozen(obj)) {
    Object.freeze(obj);
    Object.getOwnPropertyNames(obj).forEach(prop => {
      const val = (obj as any)[prop];
      if (val && typeof val === "object") deepFreeze(val);
    });
  }
  return obj as Immutable<T>;
}
```

Usage:

```ts
const frozenUser = deepFreeze({
  name: "Alice",
  address: { city: "NY" },
});

// ❌ TypeScript + runtime both protect:
frozenUser.address.city = "LA"; // compile-time error
// At runtime: TypeError: Cannot assign to read-only property 'city'
```

---

## 5️⃣ Immutability in State Management (React/Redux)

If you’re using React or Redux, immutability ensures **predictable UI updates**.

Example using **Immer** (a small library that generates immutable copies automatically):

```bash
npm install immer
```

```ts
import { produce } from "immer";

const user = { name: "Alice", age: 25 };

const newUser = produce(user, draft => {
  draft.age++; // looks mutable
});

console.log(user.age);    // 25
console.log(newUser.age); // 26
```

Immer internally freezes objects, so you don’t have to manage it manually.
It’s a good alternative to Immutable.js if you want **plain objects** + **runtime immutability**.

---

## 6️⃣ When to Use Each Approach

| Use Case                                       | Recommended Tool                |
| ---------------------------------------------- | ------------------------------- |
| Small codebase or static models                | `readonly` or `Immutable<T>`    |
| Complex nested state, performance not critical | `Immutable<T>` + `deepFreeze()` |
| State management in React/Redux                | **Immer**                       |
| Large-scale app needing structural sharing     | **Immutable.js**                |

---

## 7️⃣ Example Project Structure

```
src/
├── types/
│   └── global.d.ts          # Immutable<T> definition
├── utils/
│   └── deepFreeze.ts        # Runtime immutability helper
├── models/
│   └── User.ts              # Your interfaces/types
└── main.ts                  # Example usage
```

And in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true,
    "skipLibCheck": true,
    "baseUrl": "./src",
    "paths": {
      "@types/*": ["types/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "include": ["src"]
}
```

Now every file in your project can use `Immutable<T>` globally.

---

## ✅ Summary

| Goal                                     | Best Approach    |
| ---------------------------------------- | ---------------- |
| Make properties readonly                 | `readonly`       |
| Enforce deep immutability                | `Immutable<T>`   |
| Add runtime safety                       | `deepFreeze()`   |
| Manage immutable state ergonomically     | **Immer**        |
| Large-scale runtime + structural sharing | **Immutable.js** |

---

## 🚀 Next Steps

1. Add `Immutable<T>` globally
2. Use `deepFreeze()` in tests or state initialization
3. Integrate **Immer** or **Immutable.js** if performance/state trees matter
4. Enable `strict` mode in `tsconfig.json` for stronger typing

---

Would you like me to include a **practical example project** (with a simple “user list” that demonstrates immutable updates using all three methods: `Immutable<T>`, `deepFreeze`, and `Immer`)?
That’s a great way to see them in action side by side.

# Enforce global immutability in TypeScript

Perfect — this is a really good direction to take 👍

If you want to **enforce global immutability in TypeScript** (so your types behave like Immutable.js objects, but still use plain JS objects under the hood), the best approach is to define a **recursive utility type** like `Immutable<T>` or `ReadonlyDeep<T>` and use it project-wide.

Let’s go step-by-step 👇

---

## 🧩 1. The Problem with `readonly`

`readonly` is shallow:

```ts
type Person = {
  readonly name: string;
  readonly address: { city: string };
};

const p: Person = { name: "Alice", address: { city: "NY" } };
p.address.city = "LA"; // ✅ Allowed (no deep readonly)
```

We need a **deeply recursive version** that marks every nested property as readonly.

---

## 🛠️ 2. Define a Global Immutable Type

Create a `types/global.d.ts` (or similar) and add:

```ts
type Primitive = string | number | boolean | bigint | symbol | null | undefined;

// Recursively make everything readonly
type Immutable<T> =
  T extends Primitive ? T :
  T extends Array<infer U> ? ReadonlyArray<Immutable<U>> :
  T extends Map<infer K, infer V> ? ReadonlyMap<Immutable<K>, Immutable<V>> :
  T extends Set<infer U> ? ReadonlySet<Immutable<U>> :
  T extends object ? { readonly [K in keyof T]: Immutable<T[K]> } :
  T;
```

This covers:

* **Objects**
* **Arrays**
* **Maps**
* **Sets**
* And primitive “leaf” types

Now you can use:

```ts
interface User {
  name: string;
  tags: string[];
  address: {
    city: string;
    country: string;
  };
}

const user: Immutable<User> = {
  name: "Alice",
  tags: ["dev", "typescript"],
  address: { city: "NY", country: "USA" },
};

// ❌ Type error everywhere
user.address.city = "LA";
user.tags.push("new"); // not allowed
```

---

## 🌍 3. Use It Globally

You can add it to your project’s **global type definitions** by putting it in `src/types/global.d.ts`:

```ts
// src/types/global.d.ts
declare global {
  type Immutable<T> =
    T extends string | number | boolean | bigint | symbol | null | undefined ? T :
    T extends Array<infer U> ? ReadonlyArray<Immutable<U>> :
    T extends Map<infer K, infer V> ? ReadonlyMap<Immutable<K>, Immutable<V>> :
    T extends Set<infer U> ? ReadonlySet<Immutable<U>> :
    T extends object ? { readonly [K in keyof T]: Immutable<T[K]> } :
    T;
}

export {};
```

Now you can use `Immutable<T>` anywhere in your codebase — no import needed.

---

## 🧠 4. (Optional) Make it Runtime Safe Too

If you want extra runtime protection (closer to Immutable.js), you can **freeze** your data using a helper:

```ts
export function deepFreeze<T>(obj: T): Immutable<T> {
  if (obj && typeof obj === "object" && !Object.isFrozen(obj)) {
    Object.freeze(obj);
    Object.getOwnPropertyNames(obj).forEach(prop => {
      const val = (obj as any)[prop];
      if (val && typeof val === "object") deepFreeze(val);
    });
  }
  return obj as Immutable<T>;
}
```

Usage:

```ts
const frozenUser = deepFreeze({
  name: "Alice",
  address: { city: "NY" },
});

// @ts-expect-error — compile-time
frozenUser.address.city = "LA";
// And runtime mutation fails too
```

That gives you **runtime** and **compile-time** immutability with plain TypeScript.

---

## ⚙️ 5. Bonus: Enable Strictness

Make sure your `tsconfig.json` enforces immutability strongly:

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "exactOptionalPropertyTypes": true
  }
}
```

This ensures your type checks catch subtle mutability issues early.

---

## ✅ Summary

| Feature                   | `readonly`        | `Immutable<T>`          |
| ------------------------- | ----------------- | ----------------------- |
| Deep immutability         | ❌ No              | ✅ Yes                   |
| Covers arrays/maps/sets   | ❌ No              | ✅ Yes                   |
| Runtime freeze (optional) | ❌ No              | ✅ With helper           |
| Global usage              | ⚙️ Manual imports | ✅ With `declare global` |

---

Would you like me to show a version of `Immutable<T>` that also **supports functions and class instances safely** (i.e., doesn’t make methods readonly)? That’s a slightly more production-ready refinement.

# Mutating State in Code

Excellent follow-up 👏 — and it depends on *what kind of “state control”* you mean. Let’s unpack that carefully.

---

## 🧩 If You Mean “Mutating State in Code”

If you’re using **TypeScript `readonly`**, then yes — you *do* still need to **worry about state control**, because `readonly`:

* **Doesn’t protect at runtime**, and
* **Doesn’t deeply freeze nested structures**.

That means you could *accidentally* mutate objects if you’re not disciplined, especially in nested data.

Example:

```ts
type User = {
  readonly profile: {
    name: string;
    age: number;
  };
};

const u: User = { profile: { name: "Alice", age: 25 } };

// ✅ This is allowed, even though User is "readonly"
u.profile.age = 26; // mutates nested object
```

So, with plain `readonly`, **you’re responsible** for ensuring your code doesn’t mutate state.
TypeScript won’t stop you deeply — only at the first level.

---

## 🧊 If You’re Using Immutable.js or Immer

Then **no**, you *don’t* need to worry about state control as much:

* **Immutable.js** enforces immutability at runtime — state can’t be changed.
* **Immer** creates immutable copies automatically — you can “mutate” a draft safely.

Example with Immer:

```ts
import { produce } from "immer";

const next = produce(state, draft => {
  draft.user.age++; // looks mutable
});
// But `state` is untouched; `next` is a new copy
```

This makes “state control” automatic — you can’t accidentally mutate the original data.

---

## 🧠 So in Practice

| Scenario                         | Do I need to worry about state control? | Why                                       |
| -------------------------------- | --------------------------------------- | ----------------------------------------- |
| Using `readonly` only            | ✅ Yes                                   | It’s compile-time only, not runtime safe  |
| Using `Object.freeze()` manually | ⚠️ Somewhat less                        | Runtime-safe but shallow freeze           |
| Using Immutable.js               | ❌ No                                    | Deep, runtime immutability                |
| Using Immer                      | ❌ No                                    | Runtime immutable results, mutable syntax |
| Using Redux Toolkit              | ⚙️ Usually no                           | Built on Immer internally                 |

---

## ⚖️ Bottom Line

If your “state control” means *“preventing accidental mutations in application state”*, then:

* `readonly` helps developers write safer code,
* but **does not actually prevent runtime mutations**.

So yes — **you still need to think about state control** if you rely only on TypeScript’s `readonly`.
If you want to forget about it safely, use **Immer** or **Immutable.js** (or a framework that uses them internally).

---

Would you like me to show how to use `Object.freeze()` or a `ReadonlyDeep` utility type to strengthen immutability *without adding a big library*?

# Can TypeScript's readonly fully replace Immutable.js?

Great question — and an important one, because **immutability** is a cornerstone of predictable and maintainable JavaScript/TypeScript applications, especially in complex state management (like React, Redux, etc.).

Let’s break it down clearly 👇

---

## 🧩 What TypeScript’s `readonly` Actually Does

TypeScript’s `readonly` **is purely a compile-time construct** — it prevents mutation in **type-checking**, but **does not enforce immutability at runtime**.

For example:

```ts
type Person = {
  readonly name: string;
  readonly age: number;
};

const p: Person = { name: "Alice", age: 30 };

p.age = 31; // ❌ TypeScript error (compile-time)
```

However:

```ts
const hacked = p as any;
hacked.age = 31; // ✅ Works at runtime, mutates the object!
```

So `readonly`:

* Protects against accidental mutation **during development**
* Does **nothing at runtime**
* Doesn’t deeply freeze nested objects — only applies to the immediate level (unless you use a `ReadonlyDeep` utility type)

---

## 🧊 What Immutable.js Does

Immutable.js, by contrast:

* Provides **actual runtime immutability** (you cannot mutate data)
* Uses **persistent data structures** (structural sharing, efficient updates)
* Offers methods like `.set()`, `.update()`, `.merge()` that **return new instances**
* Ensures immutability **deeply** and **enforces it at runtime**

Example:

```js
import { Map } from "immutable";

const person = Map({ name: "Alice", age: 30 });
const older = person.set("age", 31);

console.log(person.get("age")); // 30
console.log(older.get("age")); // 31
```

Here, immutability is guaranteed both **at runtime** and **deeply**.

---

## 🔍 Comparison Summary

| Feature                              | TypeScript `readonly`       | Immutable.js                 |
| ------------------------------------ | --------------------------- | ---------------------------- |
| **Runtime immutability**             | ❌ No                        | ✅ Yes                        |
| **Deep immutability**                | ⚠️ No (unless custom types) | ✅ Yes                        |
| **Runtime protection**               | ❌ No                        | ✅ Yes                        |
| **Performance (structural sharing)** | ❌ No                        | ✅ Yes                        |
| **API style**                        | Plain JS objects            | Custom API (Map, List, etc.) |
| **Ease of use**                      | ✅ Very simple               | ⚠️ More complex              |
| **Bundle size / overhead**           | ✅ None                      | ⚠️ Adds library size         |

---

## 💡 When `readonly` Is Enough

Use `readonly` when:

* You want **developer guarantees** (compile-time safety)
* You trust your runtime code not to mutate objects recklessly
* You’re using frameworks/libraries that already treat state immutably (e.g., React + Redux Toolkit + Immer)

---

## 🧠 When Immutable.js (or similar) Is Better

Use Immutable.js (or alternatives like **Immer**, **Immer Drafts**, **seamless-immutable**) when:

* You need **real runtime immutability**
* You want to **share data efficiently** across versions (persistent data structures)
* You’re working in **large-scale stateful applications** where accidental mutation can cause subtle bugs

---

## ⚖️ Practical Middle Ground

Most modern projects use **Immer** — it gives you a **mutable syntax** but produces **immutable results**, and works seamlessly with TypeScript:

```ts
import { produce } from "immer";

const next = produce(person, draft => {
  draft.age += 1;
});
```

That gives you the **ease of mutable updates** with **real immutability**, unlike `readonly`.

---

✅ **TL;DR**

> **No, `readonly` cannot fully replace Immutable.js.**
> It provides *compile-time* immutability only, while Immutable.js enforces *runtime* immutability and offers efficient persistent data structures.

If you’re building a small-to-medium app → `readonly` + good discipline is fine.
If you’re managing large state with performance and safety in mind → use **Immutable.js** or **Immer**.

---

Would you like me to show how to create a `ReadonlyDeep<T>` helper in TypeScript to get closer to Immutable.js-like typing?

# References
1. https://stackoverflow.com/questions/55905801/can-typescripts-readonly-fully-replace-immutable-js
2. 