# Utility Types 

## Table of Contents
- [Utility Types](#utility-types)
  - [Table of Contents](#table-of-contents)
- [Omit](#omit)
  - [🧭 1. What is `Omit<Type, Keys>`?](#-1-what-is-omittype-keys)
  - [🧱 2. Syntax](#-2-syntax)
  - [🧩 3. Basic Example](#-3-basic-example)
  - [⚙️ 4. Omitting Multiple Keys](#️-4-omitting-multiple-keys)
  - [🧠 5. Combining with Other Utility Types](#-5-combining-with-other-utility-types)
    - [Example: Update DTO (Data Transfer Object)](#example-update-dto-data-transfer-object)
  - [🧩 6. Nested Object Example](#-6-nested-object-example)
  - [🧰 7. Advanced Use Case: Creating DTOs or API Types](#-7-advanced-use-case-creating-dtos-or-api-types)
  - [🧮 8. `Omit` + `keyof` for Generic Utility](#-8-omit--keyof-for-generic-utility)
  - [🧩 9. `Omit` in Combination with React Props](#-9-omit-in-combination-with-react-props)
  - [🧾 10. Summary](#-10-summary)
  - [Omit\<Type, Keys\>](#omittype-keys)
    - [Syntax](#syntax)
    - [Example](#example)
    - [Key benefits of using Omit](#key-benefits-of-using-omit)
- [Videos](#videos)
- [References](#references)

# Omit

Here’s a **step-by-step tutorial** to help you get started with TypeScript’s `Omit<Type, Keys>` utility type — from the basics to advanced real-world use cases.

---

## 🧭 1. What is `Omit<Type, Keys>`?

`Omit<Type, Keys>` is a **TypeScript utility type** that lets you **create a new type by excluding** one or more properties (`Keys`) from an existing type (`Type`).

It’s the opposite of `Pick<Type, Keys>`, which *includes only* specific keys.

---

## 🧱 2. Syntax

```typescript
Omit<Type, Keys>
```

* **`Type`** → The original type you want to modify.
* **`Keys`** → One or more property names (as string literals) to remove.

---

## 🧩 3. Basic Example

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  password: string;
};

// Remove password when sending user data to frontend
type PublicUser = Omit<User, "password">;

const user: PublicUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  // ❌ password is not allowed
};
```

✅ **Output Type:**

```typescript
type PublicUser = {
  id: number;
  name: string;
  email: string;
};
```

---

## ⚙️ 4. Omitting Multiple Keys

You can remove more than one field at once by passing a union of keys:

```typescript
type UserWithoutSensitive = Omit<User, "password" | "email">;

const u: UserWithoutSensitive = {
  id: 1,
  name: "Bob",
  // ❌ password and email are omitted
};
```

---

## 🧠 5. Combining with Other Utility Types

You can mix `Omit` with other TypeScript utilities like `Partial`, `Pick`, or `Required`.

### Example: Update DTO (Data Transfer Object)

```typescript
type UserUpdate = Partial<Omit<User, "id" | "password">>;
```

This means:

* All fields except `id` and `password`
* Are **optional**

✅ So the type will allow updates like:

```typescript
const updateData: UserUpdate = { name: "Updated Name" };
```

---

## 🧩 6. Nested Object Example

You can even use `Omit` in **nested types**:

```typescript
type Product = {
  id: string;
  name: string;
  category: {
    id: number;
    label: string;
    description: string;
  };
};

// Exclude the nested description
type SimplifiedProduct = Omit<Product, "category"> & {
  category: Omit<Product["category"], "description">;
};

const p: SimplifiedProduct = {
  id: "p1",
  name: "Phone",
  category: { id: 1, label: "Electronics" },
};
```

---

## 🧰 7. Advanced Use Case: Creating DTOs or API Types

Let’s say your backend uses a `User` entity, but your frontend doesn’t need all fields.

You can define API DTOs like this:

```typescript
// Base domain model
type User = {
  id: string;
  username: string;
  email: string;
  password: string;
  createdAt: Date;
  updatedAt: Date;
};

// DTOs for different contexts
type CreateUserDto = Omit<User, "id" | "createdAt" | "updatedAt">;
type PublicUserDto = Omit<User, "password">;
```

Each derived type fits a specific API layer:

* `CreateUserDto`: what you send when creating
* `PublicUserDto`: what you return publicly

---

## 🧮 8. `Omit` + `keyof` for Generic Utility

You can create **generic functions** that accept `Omit` types dynamically:

```typescript
function withoutKeys<T, K extends keyof T>(obj: T, keys: K[]): Omit<T, K> {
  const clone = { ...obj };
  for (const key of keys) {
    delete clone[key];
  }
  return clone;
}

const original = { id: 1, name: "Alice", password: "1234" };
const safe = withoutKeys(original, ["password"]);

console.log(safe); // { id: 1, name: "Alice" }
```

---

## 🧩 9. `Omit` in Combination with React Props

A common React pattern is to reuse existing component props while excluding or redefining some:

```tsx
type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: "primary" | "secondary";
};

// Reuse ButtonProps, but omit 'onClick'
type LinkButtonProps = Omit<ButtonProps, "onClick"> & {
  href: string;
};

const LinkButton = ({ href, ...rest }: LinkButtonProps) => (
  <a href={href} {...rest}></a>
);
```

---

## 🧾 10. Summary

| Concept              | Description                                         | Example                  |                        |           |
| -------------------- | --------------------------------------------------- | ------------------------ | ---------------------- | --------- |
| `Omit<Type, Keys>`   | Creates a new type without specific keys            | `Omit<User, "password">` |                        |           |
| Multiple keys        | Use a union (`"a"                                   | "b"`)                    | `Omit<User, "password" | "email">` |
| Combine with Partial | `Partial<Omit<User, "id">>`                         | Useful for update DTOs   |                        |           |
| Nested Omit          | `Omit<Type["sub"], "prop">`                         | Exclude nested fields    |                        |           |
| Real use             | API DTOs, React props, domain model transformations | —                        |                        |           |

---

## Omit<Type, Keys>

In TypeScript, Omit<Type, Keys> is a utility type that constructs a new type by taking an existing Type and removing a specified set of Keys from it. This is particularly useful for creating derived types where you need most properties of an existing type but want to exclude certain ones. 
Here's how Omit works and how to use it:

### Syntax

``` TypeScript
type NewType = Omit<OriginalType, 'Key1' | 'Key2' | 'Key3'>;
```

- OriginalType: The existing type from which properties will be omitted.
- 'Key1' | 'Key2' | 'Key3': A union of string literals representing the names of the properties to be excluded from OriginalType.

### Example

Consider an interface Person:

``` TypeScript

interface Person {
  id: number;
  name: string;
  age: number;
  email: string;
}
```

If you need a type for creating a new Person where the id property is not required (as it might be generated by a database), you can use Omit:

``` TypeScript

type NewPerson = Omit<Person, 'id'>;

const newPerson: NewPerson = {
  name: 'Alice',
  age: 30,
  email: 'alice@example.com',
};

// This would result in a TypeScript error because 'id' is not allowed in NewPerson:
// const invalidNewPerson: NewPerson = {
//   id: 1,
//   name: 'Bob',
//   age: 25,
//   email: 'bob@example.com',
// };
```

### Key benefits of using Omit

- Type Safety: Ensures that the new type accurately reflects the desired structure and prevents accidental usage of omitted properties.
- Code Reusability: Avoids duplicating type definitions by allowing you to derive new types from existing ones with minor modifications.
- Improved Readability: Clearly expresses the intent of the new type by specifying which properties are being excluded.

# Videos

 * [Omit no TypeScript: aprenda usar na prática!](https://www.youtube.com/watch?v=_szD9Go1-kE)
	> [<img src="https://img.youtube.com/vi/_szD9Go1-kE/0.jpg" width="200">](https://www.youtube.com/watch?v=_szD9Go1-kE "Esse vídeo faz parte da Playlist e serie de dicas rápidas de Typescript by Rodrigo Gonçalves 777 views 3 minutes, 56 seconds")

# References
1. https://www.typescriptlang.org/docs/handbook/utility-types.html
2. https://refine.dev/blog/typescript-omit-utility-type/
3. 