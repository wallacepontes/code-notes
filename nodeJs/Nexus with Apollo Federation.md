# Nexus with Apollo Federation

## Table of Contents
- [Nexus with Apollo Federation](#nexus-with-apollo-federation)
  - [Table of Contents](#table-of-contents)
  - [Apollo Federation for GraphQL](#apollo-federation-for-graphql)
    - [What is Apollo Federation for GraphQL?](#what-is-apollo-federation-for-graphql)
    - [Starting Local Development](#starting-local-development)
      - [Prerequisites](#prerequisites)
      - [Step 1: Set Up a Mock Apigee Server](#step-1-set-up-a-mock-apigee-server)
      - [Step 2: Set Up Subgraphs](#step-2-set-up-subgraphs)
      - [Step 3: Set Up the GraphQL Gateway](#step-3-set-up-the-graphql-gateway)
      - [Step 4: Run Everything Locally](#step-4-run-everything-locally)
      - [Step 5: Towards AWS Deployment](#step-5-towards-aws-deployment)
      - [Tips for Development](#tips-for-development)
  - [Nexus with Apollo Federation](#nexus-with-apollo-federation-1)
    - [Why Use Nexus with Apollo Federation?](#why-use-nexus-with-apollo-federation)
    - [Updated Setup Overview](#updated-setup-overview)
    - [Prerequisites](#prerequisites-1)
    - [Step 1: Mock Apigee Server](#step-1-mock-apigee-server)
    - [Step 2: Set Up Subgraphs with Nexus](#step-2-set-up-subgraphs-with-nexus)
      - [Subgraph: Accounts (Port 4001)](#subgraph-accounts-port-4001)
      - [Subgraph: Inventory (Port 4002)](#subgraph-inventory-port-4002)
    - [Step 3: Gateway (Unchanged)](#step-3-gateway-unchanged)
    - [Step 4: Run Locally](#step-4-run-locally)
    - [Step 5: AWS Deployment Considerations](#step-5-aws-deployment-considerations)
    - [Tips for Nexus + Federation](#tips-for-nexus--federation)
    - [Example: Adding a New Endpoint](#example-adding-a-new-endpoint)
    - [Resources](#resources)
  - [Authorization Complexity in GraphQL + Apollo Federation](#authorization-complexity-in-graphql--apollo-federation)
    - [1. The Core Problem – **Field-Level Granularity**](#1-the-core-problem--field-level-granularity)
    - [2. Where Authorization Lives in a Federated Architecture](#2-where-authorization-lives-in-a-federated-architecture)
    - [3. Real-World Complexity Examples](#3-real-world-complexity-examples)
    - [4. Practical Authorization Patterns](#4-practical-authorization-patterns)
      - [4.1 **Auth Directive (Declarative)** – *Recommended for most teams*](#41-auth-directive-declarative--recommended-for-most-teams)
      - [4.2 **Resolver Guard (Imperative)** – *Fine-grained control*](#42-resolver-guard-imperative--fine-grained-control)
      - [4.3 **Context Propagation** – *The glue*](#43-context-propagation--the-glue)
      - [4.4 **Authorization Service (Central Policy Engine)**](#44-authorization-service-central-policy-engine)
    - [5. Federation-Specific Gotchas \& Mitigations](#5-federation-specific-gotchas--mitigations)
    - [6. Tooling Ecosystem (2025)](#6-tooling-ecosystem-2025)
    - [7. Step-by-Step Local Setup (Nexus + Federation + Auth)](#7-step-by-step-local-setup-nexus--federation--auth)
    - [8. Production Checklist (AWS)](#8-production-checklist-aws)
    - [9. TL;DR – Action Plan](#9-tldr--action-plan)
  - [Video](#video)
  - [References](#references)

## Apollo Federation for GraphQL

### What is Apollo Federation for GraphQL?

Apollo Federation is an open-source architecture and specification for building modular, distributed GraphQL APIs. It allows you to decompose a large GraphQL schema into smaller, independent services called **subgraphs** (or sub-domains), which can be developed, deployed, and scaled separately by different teams. These subgraphs collectively form a **supergraph**—a unified GraphQL API that clients query via a single endpoint.

Key benefits include:
- **Separation of concerns**: Each subgraph owns a domain (e.g., users, products) and can use any language or framework, as long as it conforms to the federation spec.
- **Automatic composition**: A **GraphQL gateway** (like Apollo Gateway or Apollo Router) fetches schemas from subgraphs, composes them into the supergraph schema, and routes queries across subgraphs at runtime.
- **Entity resolution**: Subgraphs use directives like `@key` to define how entities (e.g., a `User` type) are resolved across services, enabling efficient joins without manual stitching.
- **No client changes**: Clients query the supergraph as if it were a monolith, without knowing about the underlying subgraphs.

This fits your architecture perfectly: Your GraphQL Gateway acts as the federation router, subgraphs handle domain-specific logic (and can proxy to mocked Apigee/SOA), and the whole thing deploys to AWS (e.g., via ECS, Lambda, or EKS for subgraphs; API Gateway + Lambda for the gateway).

For production on AWS, you could use AWS AppSync for subgraphs (it supports federation natively) or self-managed Apollo Servers on Lambda/EC2, with the gateway on Lambda behind API Gateway. For local dev, we'll focus on Node.js with Apollo Server (easy to prototype and deploy to AWS later).

### Starting Local Development

To get started locally, we'll set up:
- **Subgraphs**: Simple Node.js Apollo Servers with federated schemas. You'll develop endpoints (types, queries, mutations) here. Assume two subgraphs for example: `accounts` (user-related) and `inventory` (product-related). Each will have a resolver that proxies to a mocked Apigee/SOA endpoint.
- **Gateway**: An Apollo Gateway that composes the subgraphs into a supergraph.
- **Mocking Apigee**: Since Apigee is an API gateway for SOA (likely SOAP/REST backends), mock it with a simple Node.js Express mock server or SoapUI. I'll use Express for simplicity—it's lightweight and runs alongside your GraphQL services.

#### Prerequisites
- Node.js (v18+ recommended).
- Install the Apollo CLI (Rover) globally: `npm install -g @apollo/rover` (optional but useful for schema publishing/testing; free tier works for local dev).
- Basic GraphQL knowledge (schemas, resolvers).

#### Step 1: Set Up a Mock Apigee Server
Create a simple mock for Apigee that simulates proxying to SOA (e.g., returns dummy JSON/SOAP responses). This runs on `http://localhost:3001`.

Create a new directory `mock-apigee` and run:
```bash
mkdir mock-apigee && cd mock-apigee
npm init -y
npm install express
```

Create `server.js`:
```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/soa-endpoint', (req, res) => {
  // Simulate SOA response (e.g., for a user or product query)
  res.json({ 
    success: true, 
    data: { id: req.body.id || 1, name: 'Mock SOA Data', status: 'active' } 
  });
});

app.listen(3001, () => console.log('Mock Apigee running on http://localhost:3001'));
```

Run it: `node server.js`.  
In your subgraphs, you'll proxy to `http://localhost:3001/soa-endpoint` using a library like `axios` (install via `npm i axios` in subgraph dirs).

For SoapUI: Download from soapui.org, create a mock service project, and define SOAP mocks. Point subgraphs to its endpoint (e.g., `http://localhost:8088/mockSoa`). Express is faster for JSON/SOA simulation.

#### Step 2: Set Up Subgraphs
Each subgraph is an independent Apollo Server. Create a root dir `federation-project`, then subdirs `subgraph-accounts` and `subgraph-inventory`.

**General Subgraph Setup** (repeat for each):
- In `subgraph-accounts`: `npm init -y && npm i @apollo/server graphql apollo-server-core axios`
- Create `schema.graphql` (federated schema with `@key` for entities).
- Create `index.js` (server with resolvers proxying to mock).

Example for `subgraph-accounts` (port 4001):

`schema.graphql`:
```
extend schema
  @link(url: "https://specs.apolloserver.com/federation/v2.0", import: ["@key", "@shareable"])

type Query {
  me: User
}

type User @key(fields: "id") {
  id: ID!
  name: String!
  email: String!
}

type Mutation {
  createUser(name: String!, email: String!): User!
}
```

`index.js`:
```javascript
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
const { buildSubgraphSchema } = require('@apollo/subgraph');
const { readFileSync } = require('fs');
const axios = require('axios');

const typeDefs = readFileSync('./schema.graphql', 'utf-8');
const server = new ApolloServer({
  schema: buildSubgraphSchema({ typeDefs }),
  // Add plugins for federation if needed
});

const resolvers = {
  Query: {
    me: async () => {
      // Proxy to mock Apigee/SOA
      const response = await axios.post('http://localhost:3001/soa-endpoint', { id: 1 });
      return response.data.data;
    },
  },
  Mutation: {
    createUser: async (_, { name, email }) => {
      const response = await axios.post('http://localhost:3001/soa-endpoint', { name, email });
      return response.data.data;
    },
  },
  User: {
    __resolveReference: (user, { dataLoader }) => {
      // Entity resolution for federation (fetch from SOA if needed)
      return axios.post('http://localhost:3001/soa-endpoint', { id: user.id }).then(r => r.data.data);
    },
  },
};

startStandaloneServer(server, { listen: { port: 4001 }, resolvers }).then(({ url }) => {
  console.log(`Accounts subgraph ready at ${url}`);
});
```

Run: `node index.js`.  
Repeat for `subgraph-inventory` (port 4002), with types like `Product @key(fields: "upc")`, queries like `product(upc: ID!): Product`, and similar proxy resolvers.

Develop endpoints here: Add more types/queries/mutations in `schema.graphql` and resolvers in `index.js`. Test individually with GraphQL Playground (add to server config) or `curl`.

#### Step 3: Set Up the GraphQL Gateway
The gateway composes subgraphs and exposes the supergraph on port 4000.

In root `federation-project`: `npm init -y && npm i @apollo/gateway @apollo/server graphql`

`gateway.js`:
```javascript
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
const { ApolloGateway } = require('@apollo/gateway');
const { IntrospectAndCompose } = require('@apollo/gateway');
const { readFile } = require('fs/promises');

const gateway = new ApolloGateway({
  // For local dev: Introspect subgraphs automatically
  supergraphSdl: new IntrospectAndCompose({
    subgraphs: [
      { name: 'accounts', url: 'http://localhost:4001/graphql' },
      { name: 'inventory', url: 'http://localhost:4002/graphql' },
    ],
  }),
  // For production (AWS): Use managed federation
  // serviceList: [...] or fetch from Apollo Studio
});

const server = new ApolloServer({ gateway });

startStandaloneServer(server, { listen: { port: 4000 } }).then(({ url }) => {
  console.log(`Gateway ready at ${url}`);
});
```

Run: `node gateway.js`.  
Query the supergraph: Visit `http://localhost:4000` (Apollo Sandbox opens). Example query:
```
query {
  me {
    name
    email
  }
  product(upc: "1") {
    name
    price
  }
}
```
The gateway routes to subgraphs and resolves entities across them.

#### Step 4: Run Everything Locally
1. Start mock: `cd mock-apigee && node server.js`
2. Start subgraphs: In separate terminals, `cd subgraph-accounts && node index.js`, then `cd subgraph-inventory && node index.js`
3. Start gateway: `cd federation-project && node gateway.js`
4. Test: Query `http://localhost:4000`. Develop by adding resolvers/endpoints in subgraphs and restarting them.

#### Step 5: Towards AWS Deployment
- **Subgraphs**: Package as Lambda functions (use `serverless` framework) or Docker/ECS. Expose via internal ALB.
- **Gateway**: Deploy as Lambda behind API Gateway, or ECS. Switch to managed federation: Sign up for Apollo Studio (free), publish schemas with `rover subgraph publish <GRAPH_REF> --name accounts --schema ./schema.graphql --routing-url <AWS_ENDPOINT>`, then configure gateway with `APOLLO_KEY`.
- **Mock to Real**: Replace axios calls with real Apigee endpoints (e.g., via AWS API Gateway integration).
- **Composition**: Use Rover for supergraph validation: `rover supergraph compose > supergraph.graphql`.

#### Tips for Development
- **Schema Checks**: Use `rover subgraph check <GRAPH_REF> --schema ./schema.graphql` to validate federation compatibility.
- **Debugging**: Enable query plans in gateway logs; use Apollo Studio for visualization.
- **Scaling Locally**: Use Docker Compose for all services (see Apollo's supergraph-demo repo).
- **Errors**: Ensure subgraphs include the federation link in schema. If using v1, upgrade to v2 for better performance.

## Nexus with Apollo Federation

To integrate **Nexus** with **Apollo Federation** in your setup (GraphQL Gateway -> Subgraphs -> Mocked Apigee/SOA, deployed on AWS), you'll use Nexus to define the schemas for your subgraphs in a code-first manner, then configure them to comply with Apollo Federation specifications. This approach replaces the SDL-first `schema.graphql` files with JavaScript/TypeScript code, making schema definition more programmatic and type-safe, especially useful for teams comfortable with code over static schema files. Below, I’ll explain how to adapt your local development setup to use Nexus for the subgraphs (`accounts` and `inventory`), keep the Apollo Gateway as is, and ensure compatibility with your mocked Apigee/SOA and AWS deployment plans.

### Why Use Nexus with Apollo Federation?
- **Code-First**: Define types, queries, and mutations in JavaScript/TypeScript, with auto-generated SDL that’s compatible with federation.
- **Type Safety**: Leverage TypeScript for compile-time checks, reducing runtime errors.
- **Modularity**: Nexus’s plugin system (e.g., `nexus-plugin-federation`) simplifies defining federated entities with directives like `@key`.
- **Developer Experience**: Autocompletion, less boilerplate, and easier refactoring compared to SDL-first.

### Updated Setup Overview
- **Subgraphs**: Use Nexus to define schemas for `accounts` (port 4001) and `inventory` (port 4002). Each subgraph proxies to the mock Apigee server (`http://localhost:3001/soa-endpoint`) and uses federation directives.
- **Gateway**: Remains unchanged (Apollo Gateway on port 4000, using `IntrospectAndCompose` for local dev).
- **Mock Apigee**: Same Express-based mock server (or SoapUI if preferred).
- **AWS Deployment**: Nexus-generated schemas work seamlessly with Apollo’s federation tools for AWS (e.g., AppSync, Lambda, ECS).

### Prerequisites
- Same as before: Node.js (v18+), `npm i -g @apollo/rover` (optional for schema publishing).
- Add for each subgraph: `npm i nexus @apollo/server @apollo/subgraph axios`.
- Optionally, use TypeScript (`npm i typescript ts-node @types/node` and `tsconfig.json`).

### Step 1: Mock Apigee Server
No changes needed—use the same Express mock server from your original setup (`mock-apigee/server.js` on port 3001). It simulates SOA responses:
```javascript
const express = require('express');
const app = express();
app.use(express.json());
app.post('/soa-endpoint', (req, res) => {
  res.json({ success: true, data: { id: req.body.id || 1, name: 'Mock SOA Data', status: 'active' } });
});
app.listen(3001, () => console.log('Mock Apigee running on http://localhost:3001'));
```

### Step 2: Set Up Subgraphs with Nexus
For each subgraph (`subgraph-accounts` and `subgraph-inventory`), replace the SDL-based `schema.graphql` with Nexus-based schema definitions. Below is the setup for `subgraph-accounts`. Repeat similarly for `subgraph-inventory`.

#### Subgraph: Accounts (Port 4001)
In `subgraph-accounts`:
```bash
npm init -y
npm install nexus @apollo/server @apollo/subgraph axios
```

Create `schema.js` (or `schema.ts` for TypeScript):
```javascript
const { makeSchema, objectType, queryType, mutationType, nonNull, stringArg } = require('nexus');
const { buildFederatedSchema } = require('@apollo/subgraph');

const Query = queryType({
  definition(t) {
    t.field('me', {
      type: 'User',
      resolve: async () => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { id: 1 });
        return response.data.data;
      },
    });
  },
});

const User = objectType({
  name: 'User',
  definition(t) {
    t.id('id', { description: 'User ID' });
    t.nonNull.string('name', { description: 'User name' });
    t.nonNull.string('email', { description: 'User email' });
    // Federation-specific: Define @key for entity resolution
    t.string('__resolveReference', {
      resolve: async (user) => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { id: user.id });
        return response.data.data;
      },
    });
  },
});

const Mutation = mutationType({
  definition(t) {
    t.field('createUser', {
      type: 'User',
      args: {
        name: nonNull(stringArg()),
        email: nonNull(stringArg()),
      },
      resolve: async (_, { name, email }) => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { name, email });
        return response.data.data;
      },
    });
  },
});

// Generate federated schema
const schema = makeSchema({
  types: [Query, User, Mutation],
  plugins: [],
  outputs: {
    schema: __dirname + '/generated/schema.graphql', // Optional: For inspection
    typegen: __dirname + '/generated/nexus-typegen.ts', // Optional: For TypeScript
  },
});

module.exports = buildFederatedSchema({
  typeDefs: schema.typeDefs,
  resolvers: schema.resolvers,
});
```

Create `index.js`:
```javascript
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
const schema = require('./schema');

const server = new ApolloServer({ schema });

startStandaloneServer(server, { listen: { port: 4001 } }).then(({ url }) => {
  console.log(`Accounts subgraph ready at ${url}`);
});
```

**Key Points**:
- **Federation Directives**: Nexus automatically handles `@key(fields: "id")` via `buildFederatedSchema`. The `User` type’s `__resolveReference` resolver enables entity resolution across subgraphs.
- **Resolvers**: Same logic as your SDL setup—proxy to the mock Apigee server.
- **Output**: Nexus generates an SDL file (`generated/schema.graphql`) compatible with Apollo Federation v2, which the gateway will introspect.

#### Subgraph: Inventory (Port 4002)
Follow the same structure for `subgraph-inventory`. Example `schema.js`:
```javascript
const { makeSchema, objectType, queryType, nonNull, stringArg } = require('nexus');
const { buildFederatedSchema } = require('@apollo/subgraph');

const Query = queryType({
  definition(t) {
    t.field('product', {
      type: 'Product',
      args: { upc: nonNull(stringArg()) },
      resolve: async (_, { upc }) => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { id: upc });
        return { upc, ...response.data.data };
      },
    });
  },
});

const Product = objectType({
  name: 'Product',
  definition(t) {
    t.id('upc', { description: 'Product UPC' });
    t.nonNull.string('name', { description: 'Product name' });
    t.float('price', { description: 'Product price' });
    t.string('__resolveReference', {
      resolve: async (product) => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { id: product.upc });
        return { upc: product.upc, ...response.data.data };
      },
    });
  },
});

const schema = makeSchema({
  types: [Query, Product],
  outputs: {
    schema: __dirname + '/generated/schema.graphql',
    typegen: __dirname + '/generated/nexus-typegen.ts',
  },
});

module.exports = buildFederatedSchema({
  typeDefs: schema.typeDefs,
  resolvers: schema.resolvers,
});
```

`index.js` (same as accounts, but port 4002):
```javascript
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
const schema = require('./schema');

const server = new ApolloServer({ schema });

startStandaloneServer(server, { listen: { port: 4002 } }).then(({ url }) => {
  console.log(`Inventory subgraph ready at ${url}`);
});
```

**Developing Endpoints**:
- Add queries/mutations in `Query` or `Mutation` types (e.g., `t.field('allProducts', { type: list('Product'), resolve: ... })`).
- Use Nexus’s `field`, `arg`, `list`, etc., for complex types/args.
- Proxy to `http://localhost:3001/soa-endpoint` as needed.

### Step 3: Gateway (Unchanged)
The Apollo Gateway setup remains identical, as it introspects the subgraphs’ schemas (now generated by Nexus). Use the same `gateway.js`:
```javascript
const { ApolloServer } = require('@apollo/server');
const { startStandaloneServer } = require('@apollo/server/standalone');
const { ApolloGateway } = require('@apollo/gateway');
const { IntrospectAndCompose } = require('@apollo/gateway');

const gateway = new ApolloGateway({
  supergraphSdl: new IntrospectAndCompose({
    subgraphs: [
      { name: 'accounts', url: 'http://localhost:4001/graphql' },
      { name: 'inventory', url: 'http://localhost:4002/graphql' },
    ],
  }),
});

const server = new ApolloServer({ gateway });

startStandaloneServer(server, { listen: { port: 4000 } }).then(({ url }) => {
  console.log(`Gateway ready at ${url}`);
});
```

### Step 4: Run Locally
1. **Mock Apigee**: `cd mock-apigee && node server.js`.
2. **Subgraphs**: In separate terminals:
   - `cd subgraph-accounts && node index.js`
   - `cd subgraph-inventory && node index.js`
3. **Gateway**: `cd federation-project && node gateway.js`.
4. **Test**: Open `http://localhost:4000` in Apollo Sandbox. Example query:
   ```graphql
   query {
     me {
       id
       name
       email
     }
     product(upc: "1") {
       upc
       name
       price
     }
   }
   ```

### Step 5: AWS Deployment Considerations
- **Subgraphs**: Package `schema.js` and `index.js` into Lambda functions (use `serverless` framework) or Docker containers for ECS/EKS. Nexus’s generated SDL is federation-compliant, so no changes are needed. Expose subgraphs via internal ALB or AppSync (AppSync supports federation natively).
- **Gateway**: Deploy as Lambda behind API Gateway or as an ECS service. For managed federation, publish Nexus-generated schemas to Apollo Studio:
  ```bash
  rover subgraph publish <GRAPH_REF> --name accounts --schema ./subgraph-accounts/generated/schema.graphql --routing-url <AWS_ENDPOINT>
  ```
  Configure the gateway with `APOLLO_KEY` from Studio.
- **Apigee Integration**: Replace mock axios calls with real Apigee endpoints (e.g., via AWS API Gateway or direct Apigee URLs provided by your team).
- **Nexus-Specific**: Ensure `generated/schema.graphql` is included in your deployment artifacts if using Rover/Studio.

### Tips for Nexus + Federation
- **Federation Directives**: If you need advanced directives (e.g., `@provides`, `@requires`), use Nexus’s `extendType` or custom resolvers. The `@apollo/subgraph` package handles `@key` automatically.
- **TypeScript**: For type safety, initialize with `npx ts-node` and add to `tsconfig.json`:
  ```json
  {
    "compilerOptions": {
      "target": "ES2018",
      "module": "commonjs",
      "outDir": "./dist",
      "strict": true,
      "esModuleInterop": true
    }
  }
  ```
- **Debugging**: Inspect `generated/schema.graphql` to verify federation compatibility. Use `rover subgraph check` for validation.
- **Performance**: Nexus’s generated schemas are lean, but avoid over-fetching in resolvers. Cache Apigee responses if needed (e.g., using Redis on AWS).
- **Extending Schemas**: Add more types/queries/mutations in `schema.js` (e.g., `orderType`, `queryType` for `allOrders`). Nexus’s API is intuitive for scaling.

### Example: Adding a New Endpoint
To add a `allUsers` query to `subgraph-accounts`:
```javascript
const Query = queryType({
  definition(t) {
    t.field('me', { /* existing */ });
    t.list.field('allUsers', {
      type: 'User',
      resolve: async () => {
        const axios = require('axios');
        const response = await axios.post('http://localhost:3001/soa-endpoint', { action: 'listUsers' });
        return response.data.data; // Assume SOA returns [{ id, name, email }, ...]
      },
    });
  },
});
```

Update `schema.js`, restart the subgraph, and the gateway will pick it up automatically.

### Resources
- Apollo Federation Docs: [federation.apollo.dev](https://www.apollographql.com/docs/federation/) 
- Nexus Docs: [nexusjs.org](https://nexusjs.org/docs/)
- Apollo’s Supergraph Demo: [github.com/apollographql/supergraph-demo](https://github.com/apollographql/supergraph-demo)

## Authorization Complexity in GraphQL + Apollo Federation  
*(Why it’s “harder than REST” and how to tame it)*  

---

### 1. The Core Problem – **Field-Level Granularity**

| Aspect | REST | GraphQL (single service) | **Federated GraphQL** |
|--------|------|--------------------------|------------------------|
| **Authorization surface** | One endpoint → one policy | Every **field** can be resolved independently | Every **field** in **any subgraph** can be resolved independently |
| **Request shape** | Fixed path + method | Client-defined projection (`{ user { id name email } }`) | Same, but **resolution hops across services** |
| **Policy enforcement point** | Middleware before handler | Resolver-level or directive | **Resolver in subgraph** + **gateway** (optional) |
| **Error surface** | 403 on whole call | Partial success (`null` + `errors`) | Partial success **across subgraphs** |

> **Quote from the community**:  
> *“In REST you secure 10 doors. In GraphQL you secure 10 000 light switches.”* – @sashko (Apollo)

---

### 2. Where Authorization Lives in a Federated Architecture

```
Client
   ↓
[Gateway] ──→ (Query Plan) ──→ Subgraph A (accounts)
   │                                 ↓
   │                             Resolver → AuthZ
   ↓
   ↓──→ Subgraph B (inventory)
               ↓
           Resolver → AuthZ
```

| Layer | What it can enforce | Typical pattern |
|-------|---------------------|-----------------|
| **Gateway** | Global rules (rate-limit, API key, coarse roles) | `@auth` directive on root fields, `ApolloGateway` plugin |
| **Subgraph** | **Domain-specific** rules (e.g., “only owner can read email”) | Resolver guard, `@requiresAuth`, `@authorized` directive |
| **Entity resolvers** (`__resolveReference`) | Cross-subgraph entity access | Same as normal field resolvers |

---

### 3. Real-World Complexity Examples

| Scenario | Why it’s tricky |
|----------|-----------------|
| **User → Orders → OrderItems** | `User` lives in `accounts`, `Order` in `orders`, `OrderItem` in `inventory`. Each hop must re-validate the caller’s rights on the **same user context**. |
| **Field-level role** | `admin` sees `user.ssn`, `user` sees only `user.name`. The `User` type is **shared** across subgraphs. |
| **Batch loading** | DataLoader fetches 100 users; you must filter out unauthorized ones **without leaking IDs**. |
| **Partial failures** | One subgraph returns `null + error` while others succeed → client sees mixed data. |

---

### 4. Practical Authorization Patterns

#### 4.1 **Auth Directive (Declarative)** – *Recommended for most teams*

```ts
// schema.ts (Nexus)
import { schema } from 'nexus'
import { authorized } from 'nexus-plugin-auth'

schema.directive('authorized', authorized({
  roles: ['ADMIN', 'USER']
}))
```

```graphql
type User @key(fields: "id") {
  id: ID!
  name: String!
  email: String @authorized(roles: ["ADMIN"])
}
```

- **Pros**: Single source of truth, works in **any subgraph**.
- **Cons**: Directive must be interpreted in **every resolver** (plugin needed).

#### 4.2 **Resolver Guard (Imperative)** – *Fine-grained control*

```ts
function requireRole(roles: string[]) {
  return (_: any, __: any, ctx: Context) => {
    if (!roles.some(r => ctx.user?.roles.includes(r))) {
      throw new Error('Forbidden')
    }
  }
}

// In resolver
me: requireRole(['USER']), async (_, __, ctx) => { ... }
```

#### 4.3 **Context Propagation** – *The glue*

```ts
// Gateway → Subgraph
const gateway = new ApolloGateway({
  buildService({ name, url }) {
    return new RemoteGraphQLDataSource({
      url,
      willSendRequest({ request, context }) {
        request.http?.headers.set('authorization', context.authToken)
        request.http?.headers.set('x-user-id', context.userId)
      },
    })
  },
})
```

- **Pass**: `userId`, `roles`, `tenantId`, JWT.
- **Validate once in gateway** (optional), **re-validate in subgraph** (defense-in-depth).

#### 4.4 **Authorization Service (Central Policy Engine)**

```
Subgraph → AuthZ Service (OPA, Cerbos, Permify)
```

```ts
async function can(access: string, resource: any, ctx: Context) {
  const policy = await cerbos.checkResource({
    principal: { id: ctx.userId, roles: ctx.roles },
    resource: { kind: 'user', id: resource.id },
    actions: [access],
  })
  return policy.isAllowed(access)
}
```

- **Pros**: Single policy language, audit, hot-reload.
- **Cons**: Extra network hop (cache aggressively).

---

### 5. Federation-Specific Gotchas & Mitigations

| Issue | Mitigation |
|-------|------------|
| **Entity leakage via `@key`** | Never expose sensitive keys. Use opaque IDs or hash. |
| **`__resolveReference` bypass** | Always apply same auth as normal fields. |
| **Query plan re-ordering** | Gateway may fetch `User` before `Order`. Ensure **subgraph auth doesn’t depend on order**. |
| **Persisted Queries + Cached Plans** | Include auth context in plan cache key (Apollo Router v1+). |
| **Schema stitching errors** | Use `@inaccessible` (Federation 2.4+) to hide fields from unauthorized clients. |

```graphql
type User @key(fields: "id") {
  id: ID!
  name: String!
  ssn: String @inaccessible
}
```

---

### 6. Tooling Ecosystem (2025)

| Tool | Purpose | Federation-ready |
|------|---------|------------------|
| **Apollo Router** (Rust) | High-perf gateway, built-in `@auth` | Yes |
| **GraphQL Shield** | Middleware rules | Yes (subgraph) |
| **Cerbos** | Policy-as-code | Yes |
| **OPA Gatekeeper** | Rego policies | Yes |
| **AWS AppSync + Cognito** | Managed federation + IAM | Yes |
| **Apollo Studio** | Schema registry + `@requiresScopes` | Yes |

---

### 7. Step-by-Step Local Setup (Nexus + Federation + Auth)

```bash
# 1. Mock Apigee (unchanged)
cd mock-apigee && node server.js

# 2. Subgraph – accounts (Nexus + @authorized)
cd subgraph-accounts
npm i nexus @apollo/subgraph apollo-server graphql cerbos-sdk
```

`schema.ts` (TypeScript):
```ts
import { makeSchema, objectType, queryType } from 'nexus'
import { buildFederatedSchema } from '@apollo/subgraph'
import { authorized } from 'nexus-plugin-auth' // custom or shield
import Cerbos from '@cerbos/core'

const cerbos = new Cerbos({ hostname: 'http://localhost:3593' })

const User = objectType({
  name: 'User',
  definition(t) {
    t.id('id')
    t.string('name')
    t.string('email', {
      authorize: async (parent, _, ctx) =>
        await cerbos.isAllowed({
          principal: { id: ctx.user.id, roles: ctx.user.roles },
          resource: { kind: 'user', id: parent.id },
          action: 'read:email',
        }),
    })
    t.key('id') // Federation @key
  },
})
```

`index.ts`:
```ts
import { ApolloServer } from '@apollo/server'
import { startStandaloneServer } from '@apollo/server/standalone'
import schema from './schema'

const server = new ApolloServer({ schema })
startStandaloneServer(server, { port: 4001 })
```

Repeat for `inventory` subgraph (e.g., `product.price` only for `ADMIN`).

**Gateway** – add auth context:
```ts
const gateway = new ApolloGateway({
  buildService({ url }) {
    return new RemoteGraphQLDataSource({
      url,
      willSendRequest({ request, context }) {
        request.http.headers.set('x-user', JSON.stringify(context.user))
      },
    })
  },
})
```

---

### 8. Production Checklist (AWS)

| Item | Implementation |
|------|----------------|
| **IAM / Cognito** | JWT → `context.user` in Lambda authorizer |
| **Subgraph isolation** | VPC + Security Groups; each subgraph its own Lambda |
| **Policy cache** | Redis (ElastiCache) for Cerbos/OPA results |
| **Observability** | Apollo Studio **field-level usage** + **auth errors** |
| **Rate-limit per user** | API Gateway usage plans + `context.user.id` |
| **Schema registry** | Apollo Studio + `@inaccessible` for PII |

---

### 9. TL;DR – Action Plan

1. **Decide policy location**  
   - Global → gateway  
   - Domain → subgraph (preferred)  
   - Central → external PDP (Cerbos/OPA)

2. **Pick a pattern**  
   - **Directive** (`@authorized`, `@requiresScopes`) → least code  
   - **Guard** → max flexibility  
   - **PDP** → audit & reuse

3. **Propagate context** from gateway to every subgraph.

4. **Mark sensitive fields** with `@inaccessible` or hide via directives.

5. **Test with Apollo Sandbox** → try queries as different roles.

6. **Automate schema checks**  
   ```bash
   rover subgraph check MYGRAPH@prod --schema ./generated/schema.graphql
   ```

---

**Bottom line**:  
Authorization in federated GraphQL is *field-level* and *distributed*. Treat **every resolver as a security boundary**, propagate auth context reliably, and prefer **declarative directives** or a **central policy engine** to keep the explosion of checks maintainable.  


## Video
 * [Your First Federated Schema with Apollo Server](https://www.youtube.com/watch?v=v_1bn2sHdk4)
	> [<img src="https://img.youtube.com/vi/v_1bn2sHdk4/0.jpg" width="200">](https://www.youtube.com/watch?v=v_1bn2sHdk4 "Talk by Mandi Wise, Author of Advanced GraphQL with Apollo & React and Owner, 8-Bit Press Inc by Apollo GraphQL 53k views 26 minutes, 22 seconds")

 * [Getting started with Apollo MCP Server](https://www.youtube.com/watch?v=QTvMYUP4E30)
	> [<img src="https://img.youtube.com/vi/QTvMYUP4E30/0.jpg" width="200">](https://www.youtube.com/watch?v=QTvMYUP4E30 "In a world racing to connect AI systems with enterprise data, we've built the bridge using the Model Context Protocol (MCP). by Apollo GraphQL 3.7k views 4 minutes, 16 seconds")

## References
1. 
