# Apollo Federation

# Table of Contents
- [Apollo Federation](#apollo-federation)
- [Table of Contents](#table-of-contents)
  - [🧠 What is Apollo Federation?](#-what-is-apollo-federation)
  - [⚙️ Federation Architecture Overview](#️-federation-architecture-overview)
  - [🧩 Core Components](#-core-components)
  - [🧪 Example: Local Setup (Node.js)](#-example-local-setup-nodejs)
    - [1️⃣ Create the first subgraph (`accounts`)](#1️⃣-create-the-first-subgraph-accounts)
    - [2️⃣ Create the second subgraph (`products`)](#2️⃣-create-the-second-subgraph-products)
    - [3️⃣ Create the **Apollo Gateway**](#3️⃣-create-the-apollo-gateway)
    - [🧠 Example Query](#-example-query)
  - [🧱 How Docker Fits In](#-how-docker-fits-in)
  - [🚀 Use Cases](#-use-cases)
  - [Video](#video)
  - [References](#references)

Excellent question — **Apollo Federation** is one of the most elegant solutions for scaling GraphQL architectures in large or distributed systems. Let’s go step by step so you get the full picture — conceptually and practically 👇

---

## 🧠 What is Apollo Federation?

**Apollo Federation** is an architecture that allows you to **compose multiple GraphQL services into a single unified graph**.

It’s the *GraphQL equivalent* of **microservices**, where each team owns part of the graph (called a **subgraph**) — and all are stitched together by a **Gateway**.

So instead of having **one big monolithic GraphQL server**, you can split it into smaller services like:

* **Accounts service** → users, profiles
* **Products service** → catalog, inventory
* **Orders service** → purchases, transactions

Each exposes its own **GraphQL schema**, and Apollo Federation **combines them automatically** into one global schema available to clients.

---

## ⚙️ Federation Architecture Overview

```
          ┌────────────────────────┐
          │      Client Apps       │
          └────────────┬───────────┘
                       │
               (Unified GraphQL API)
                       │
           ┌───────────▼───────────┐
           │   Apollo Gateway      │
           │ (Federation Router)   │
           └───────┬─────┬────────┘
                   │     │
     ┌─────────────▼┐ ┌──▼─────────────┐
     │ Accounts API │ │ Products API   │
     └──────────────┘ └────────────────┘
```

* Each **subgraph service** runs independently.
* The **Apollo Gateway**:

  * Fetches the subgraph schemas.
  * Composes them into one **federated supergraph**.
  * Routes queries to the right subgraphs automatically.

---

## 🧩 Core Components

| Component                    | Description                                             |
| ---------------------------- | ------------------------------------------------------- |
| **Subgraph**                 | A microservice exposing a partial GraphQL schema        |
| **@key directive**           | Defines unique identifier fields for federated entities |
| **Apollo Gateway / Router**  | Central entry point combining all subgraphs             |
| **Apollo Studio (optional)** | Cloud tool to manage, publish, and observe your graph   |

---

## 🧪 Example: Local Setup (Node.js)

Let’s see how you’d **use it locally** to experiment.

---

### 1️⃣ Create the first subgraph (`accounts`)

**`accounts.js`**

```js
import { ApolloServer } from '@apollo/server';
import { buildSubgraphSchema } from '@apollo/subgraph';
import gql from 'graphql-tag';

const typeDefs = gql`
  type User @key(fields: "id") {
    id: ID!
    name: String
  }

  type Query {
    me: User
  }
`;

const resolvers = {
  Query: {
    me: () => ({ id: '1', name: 'Wallace' }),
  },
  User: {
    __resolveReference: (user) => ({ id: user.id, name: 'Wallace' }),
  },
};

const server = new ApolloServer({
  schema: buildSubgraphSchema({ typeDefs, resolvers }),
});

server.listen({ port: 4001 }).then(({ url }) => {
  console.log(`🚀 Accounts subgraph ready at ${url}`);
});
```

Run it:

```bash
node accounts.js
```

---

### 2️⃣ Create the second subgraph (`products`)

**`products.js`**

```js
import { ApolloServer } from '@apollo/server';
import { buildSubgraphSchema } from '@apollo/subgraph';
import gql from 'graphql-tag';

const typeDefs = gql`
  type Product @key(fields: "id") {
    id: ID!
    name: String
    price: Float
  }

  type Query {
    topProducts: [Product]
  }
`;

const resolvers = {
  Query: {
    topProducts: () => [
      { id: '1', name: 'Phone', price: 699 },
      { id: '2', name: 'Laptop', price: 1299 },
    ],
  },
};

const server = new ApolloServer({
  schema: buildSubgraphSchema({ typeDefs, resolvers }),
});

server.listen({ port: 4002 }).then(({ url }) => {
  console.log(`🚀 Products subgraph ready at ${url}`);
});
```

---

### 3️⃣ Create the **Apollo Gateway**

**`gateway.js`**

```js
import { ApolloServer } from '@apollo/server';
import { ApolloGateway } from '@apollo/gateway';

const gateway = new ApolloGateway({
  serviceList: [
    { name: 'accounts', url: 'http://localhost:4001' },
    { name: 'products', url: 'http://localhost:4002' },
  ],
});

const server = new ApolloServer({
  gateway,
  subscriptions: false,
});

server.listen({ port: 4000 }).then(({ url }) => {
  console.log(`🚀 Gateway ready at ${url}`);
});
```

Run:

```bash
node gateway.js
```

Now you have a **federated GraphQL API** running locally 🎉
Try querying it at: [http://localhost:4000](http://localhost:4000)

---

### 🧠 Example Query

```graphql
query {
  me {
    id
    name
  }
  topProducts {
    id
    name
    price
  }
}
```

Response:

```json
{
  "data": {
    "me": { "id": "1", "name": "Wallace" },
    "topProducts": [
      { "id": "1", "name": "Phone", "price": 699 },
      { "id": "2", "name": "Laptop", "price": 1299 }
    ]
  }
}
```

---

## 🧱 How Docker Fits In

You can **Dockerize** each subgraph and the gateway easily — they’re just Node.js services.
Example `Dockerfile`:

```Dockerfile
FROM node:22-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "accounts.js"]
```

Then you can use **Docker Compose** to orchestrate all subgraphs + gateway locally.

---

## 🚀 Use Cases

* Large organizations with multiple domain teams.
* Need for modular, scalable API architecture.
* Different backends (SQL, REST, microservices) exposed as one GraphQL endpoint.

---

If you want, I can show you a **Docker Compose setup** to run this full Apollo Federation environment locally (gateway + 2 subgraphs).
Would you like me to generate that next?

## Video

 * [Your First Federated Schema with Apollo Server](https://www.youtube.com/watch?v=v_1bn2sHdk4)
	> [<img src="https://img.youtube.com/vi/v_1bn2sHdk4/0.jpg" width="200">](https://www.youtube.com/watch?v=v_1bn2sHdk4 "Talk by Mandi Wise, Author of Advanced GraphQL with Apollo & React and Owner, 8-Bit Press Inc. by Apollo GraphQL 53 k views 26 minutes, 22 seconds")

## References
1. 