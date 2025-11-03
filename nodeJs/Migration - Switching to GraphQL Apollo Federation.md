# Migration - Switching to GraphQL/Apollo Federation

## Table of Contents
- [Migration - Switching to GraphQL/Apollo Federation](#migration---switching-to-graphqlapollo-federation)
  - [Table of Contents](#table-of-contents)
  - [Switching to GraphQL/Apollo Federation for the Migration](#switching-to-graphqlapollo-federation-for-the-migration)
    - [1. High-Level Architectural Changes](#1-high-level-architectural-changes)
    - [2. Code Structure Changes](#2-code-structure-changes)
      - [Updated Project Structure](#updated-project-structure)
      - [Package.json Changes](#packagejson-changes)
    - [3. SOA/Apigee Integration Changes](#3-soaapigee-integration-changes)
    - [4. Deployment and CI/CD Changes](#4-deployment-and-cicd-changes)
    - [5. Testing and Documentation Changes](#5-testing-and-documentation-changes)
    - [6. Benefits and Challenges for Your Project](#6-benefits-and-challenges-for-your-project)
    - [7. Next Steps](#7-next-steps)


## Switching to GraphQL/Apollo Federation for the Migration

That's a significant and exciting pivot for your telecom project's migration from the legacy Java 8 WOA application with 322 REST URIs! GraphQL with Apollo Federation is a natural evolution, especially given your DDD-based structure, serverless Node.js setup, TypeScript adoption, and Apigee integration for SOA access. It aligns beautifully with your Bounded Contexts (e.g., `SelfService`, `Sales`, `Credit`) by treating them as **subgraphs**—federated services that contribute to a unified "supergraph" API. This replaces the fragmented REST endpoints with a single, flexible GraphQL endpoint, reducing client over/under-fetching and simplifying maintenance for the 90% of URIs that proxy to SOA.

Based on best practices for migrating REST to GraphQL Federation (e.g., incremental adoption, schema design over mirroring REST, and using tools like Apollo Connectors for existing APIs), I'll outline the key changes to your project. These build on our prior discussions (e.g., event-driven architecture, Apigee token handling, global errors) while introducing GraphQL-specific adjustments. The migration can be **incremental**: Start by wrapping one Bounded Context's REST logic into a subgraph, then federate others, ensuring backward compatibility for clients during rollout.

---

### 1. High-Level Architectural Changes
- **From REST to GraphQL Schema**:
  - **REST**: 322 discrete URIs (e.g., `GET /self-service/account/{id}`), each handling fixed payloads and potential over-fetching.
  - **GraphQL Federation**: A single endpoint (e.g., `https://api.telecom.com/graphql`) where clients query flexible schemas. Each Bounded Context becomes a **subgraph** (a federated GraphQL service) that exposes types, queries, mutations, and resolvers. Apollo Router (the federation gateway) composes them into a **supergraph**.
  - **Why it fits DDD**: Subgraphs map directly to Bounded Contexts, enforcing domain boundaries. For example, `SelfService` subgraph handles customer/account queries, while `Sales` manages orders—resolvers reference entities across subgraphs via `@key` directives.
  - **Migration Strategy**: Don't mirror all 322 URIs 1:1. Instead, design a demand-driven schema based on client needs (e.g., consolidate related billing URIs into a `Customer` type with nested fields). Use Apollo Connectors to wrap existing REST/SOA calls initially, then refactor to native resolvers.

- **Serverless Deployment**:
  - **Previous**: Separate Lambda functions per Bounded Context (e.g., `self-service/handler.ts` with Express routes).
  - **New**: Each subgraph as a dedicated AWS Lambda (or AppSync for managed GraphQL), with Apollo Router as a central Lambda or ECS service. Deploy via Serverless Framework/AWS SAM, but add `apollo-federation` plugins for schema composition.
  - **Apigee Integration**: Apigee now fronts the GraphQL gateway (Router), handling auth/tokens. Subgraphs call Apigee-proxied SOA endpoints in resolvers—your token refresh logic remains unchanged but is invoked per resolver.

- **Client Impact**: Clients (e.g., mobile/web apps) switch from multiple REST calls to batched GraphQL queries, reducing round-trips. Tools like Apollo Client ease the transition.

---

### 2. Code Structure Changes
Your DDD folder structure evolves to GraphQL-centric, with resolvers replacing routes/services for query handling. TypeScript shines here for schema typing.

#### Updated Project Structure
```
serverless-openapi/  # Rename to serverless-graphql-federation?
├── src/
│   ├── self-service/  # Now a subgraph
│   │   ├── schema.graphql  # Subgraph schema (types, queries, @extends/@key)
│   │   ├── resolvers.ts    # Resolver functions (replaces routes/services)
│   │   ├── models.ts       # Domain models (e.g., Customer type)
│   │   ├── adapters.ts     # Apigee/SOA calls (unchanged)
│   │   ├── errors.ts       # Domain errors (integrate with GraphQL errors)
│   │   └── index.ts        # Subgraph server setup (ApolloServer with federation)
│   ├── sales/             # Similar for other Bounded Contexts
│   │   ├── schema.graphql
│   │   ├── resolvers.ts
│   │   └── ...
│   ├── router/            # New: Apollo Router (gateway)
│   │   ├── supergraph-schema.graphql  # Composed schema
│   │   └── router.ts      # Router config
│   ├── shared/
│   │   ├── apigee.ts      # Token management (unchanged, used in resolvers)
│   │   ├── errors.ts      # ApiError → GraphQL error formatting
│   │   ├── logger.ts      # CloudWatch integration for resolver errors
│   │   └── events.ts      # EventEmitter for domain events (e.g., post-resolver)
├── package.json
├── tsconfig.json
├── serverless.yml         # Updated for subgraphs + router
└── tests/                 # Add schema validation tests
```

- **Key Additions**:
  - **Schema Files** (`.graphql`): Define types/queries per subgraph. Example for `self-service/schema.graphql`:
    ```graphql
    extend schema
      @link(url: "https://specs.apolloserver.com/federation/v2.0", import: ["@key", "@provides", "@requires"])

    type Customer @key(fields: "id") {
      id: ID!
      name: String!
      account: Account  # Reference to another subgraph (e.g., Billing)
    }

    type Query {
      customer(id: ID!): Customer
    }

    type Mutation {
      updateCustomer(id: ID!, input: UpdateCustomerInput!): Customer
    }
    ```
    - **Federation Directives**: `@key` enables entity resolution across subgraphs (e.g., `Sales` extends `Customer`).

  - **Resolvers** (replaces routes/services): Functions that fetch data, using your adapters. Example in `self-service/resolvers.ts`:
    ```typescript
    import { Resolvers } from '../generated/graphql';  // Auto-generated from schema
    import { getCustomerFromApigee } from './adapters';
    import { CustomerNotFoundError } from './errors';
    import { formatError } from 'graphql';  // For error handling

    const resolvers: Resolvers = {
      Query: {
        customer: async (_, { id }) => {
          try {
            return await getCustomerFromApigee(id);  // Calls Apigee/SOA
          } catch (err) {
            if (err instanceof CustomerNotFoundError) {
              throw new Error(err.message);  // GraphQL errors
            }
            throw err;
          }
        },
      },
      Mutation: {
        updateCustomer: async (_, { id, input }) => {
          // Business logic + SOA call
          const updated = await updateCustomerViaApigee(id, input);
          // Emit domain event
          emitter.emit('CustomerUpdated', { id, changes: input });
          return updated;
        },
      },
      Customer: {
        account: async (parent) => {
          // Resolver for nested field; fetch from Billing subgraph if federated
          return { __typename: 'Account', id: parent.accountId };
        },
      },
    };

    export default resolvers;
    ```
    - **Event-Driven Tie-In**: Emit events post-resolver (e.g., via AWS SNS), leveraging Node.js's strengths.

  - **Subgraph Server** (`self-service/index.ts`):
    ```typescript
    import { ApolloServer } from '@apollo/server';
    import { startStandaloneServer } from '@apollo/server/standalone';
    import { buildFederatedSchema } from '@apollo/federation';
    import typeDefs from './schema.graphql';
    import resolvers from './resolvers';
    import { apolloServerConfig } from '../shared/config';  // Plugins, etc.

    const server = new ApolloServer({
      schema: buildFederatedSchema([
        { typeDefs, resolvers },
      ]),
      ...apolloServerConfig,  // Error formatting, logging
    });

    // For serverless: Export handler
    export const handler = startStandaloneServer(server, { listen: { port: process.env.PORT || 4001 } });
    ```
    - **Global Errors**: Format errors in `apolloServerConfig` (e.g., map `ApiError` to GraphQL `formatError` for consistent JSON responses).

- **Router (Gateway)** (`router/router.ts`):
  - Uses Apollo Router to compose subgraphs. Config via YAML or code:
    ```yaml
    # router-config.yaml
    supergraph:
      listen:
        - port: 4000
    include_subgraph_errors: false  # Centralized error handling
    ```
  - Deploy as a separate Lambda/ECS service.

#### Package.json Changes
Add GraphQL dependencies:
```json
{
  "dependencies": {
    "@apollo/server": "^4.10.0",
    "@apollo/gateway": "^2.8.0",  // For Router
    "@apollo/federation-instrumenter": "^4.0.0",
    "graphql": "^16.9.0",
    "graphql-tools": "^9.0.0"  // For schema building
  },
  "devDependencies": {
    "@apollo/rover": "^0.32.0",  # Schema composition tool
    "graphql-codegen": "^5.0.0"  # Auto-generate TypeScript types from schema
  }
}
```
- Install: `npm install` and add scripts like `"generate": "graphql-codegen"`.

---

### 3. SOA/Apigee Integration Changes
- **Minimal Disruption**: Your `shared/apigee.ts` token management stays the same—resolvers call adapters like `getCustomerFromApigee(id)`, handling 401/403 retries.
- **Enhancements**:
  - Use **Apollo Connectors** (or GraphQL Mesh) to wrap any remaining REST/SOA calls as GraphQL directives, e.g., `@rest` for quick integration without full resolvers.
    Example in schema:
    ```graphql
    type Customer @rest(endpoint: "https://apigee-endpoint/self-service/account/{id}") {
      id: ID!
      name: String!
    }
    ```
  - Apigee proxies the supergraph endpoint, enforcing policies (e.g., rate limiting per subgraph).

---

### 4. Deployment and CI/CD Changes
- **Serverless.yml** (Updated for Federation):
  ```yaml
  service: telecom-graphql
  provider:
    name: aws
    runtime: nodejs20.x
    stage: ${opt:stage, 'dev'}
  functions:
    selfServiceSubgraph:
      handler: src/self-service/index.handler
      events:
        - httpApi:  # Switch to HTTP API for GraphQL
            path: /graphql/self-service
            method: POST  # GraphQL uses POST
    salesSubgraph:
      handler: src/sales/index.handler
      events:
        - httpApi:
            path: /graphql/sales
            method: POST
    router:  # Gateway
      handler: src/router/router.handler
      events:
        - httpApi:
            path: /graphql
            method: POST
  plugins:
    - serverless-offline
    - serverless-apollo-federation  # Custom plugin for schema build
  ```
  - **AWS SAM Alternative**: Define `AWS::Serverless::GraphQLApi` resources for subgraphs.
  - **Schema Registry**: Use Apollo Studio (free tier) or AWS S3 for composing schemas across stages (dev/qa/uat/prod).

- **CI/CD Pipeline** (GitHub Actions Example):
  - Add steps: `npx rover supergraph compose` (composes schemas), `npm run generate` (TypeScript types), and deploy subgraphs independently.
  - Incremental: Deploy one subgraph at a time, updating the Router config.

- **Local Development**:
  - Use `npm run offline` with `serverless-offline` + Rover for local federation simulation.
  - Tools: Apollo Sandbox/Studio for schema exploration and testing queries.

---

### 5. Testing and Documentation Changes
- **OpenAPI → GraphQL Schema**: Replace Swagger with GraphQL introspection + Apollo Studio for docs. Use `@graphql-codegen` to generate client code.
- **Testing**:
  - Unit: Test resolvers with `graphql-testing` or Jest mocks for adapters.
  - Integration: Use `mercurius` or Apollo's test utils for end-to-end subgraph queries.
  - Example: `npm run test` includes schema validation (`rover subgraph check`).
- **Error Handling**: GraphQL errors are per-field; format via `formatError` to match your `ApiError` (e.g., `{ errors: [{ message: 'Customer not found', extensions: { code: 'NOT_FOUND' } }] }`).

---

### 6. Benefits and Challenges for Your Project
- **Benefits**:
  - **Efficiency**: Clients fetch exactly needed data (e.g., one query for customer + orders vs. multiple REST calls).
  - **DDD Synergy**: Subgraphs = Bounded Contexts; federation enables cross-domain references (e.g., `Customer` in `SelfService` extended by `Credit`).
  - **Scalability**: Independent subgraph scaling in Lambda; event-driven resolvers integrate with AWS SNS for domain events.
  - **Migration Ease**: Wrap 90% SOA-proxied URIs via connectors initially, refactor gradually.

- **Challenges**:
  - **Learning Curve**: Schema design > REST mirroring; start with Apollo's interactive course on REST-to-GraphQL.
  - **Overhead**: Router adds a hop; monitor with CloudWatch.
  - **Subscriptions**: If real-time (e.g., order updates), Federation supports them via WebSockets—integrate with AWS AppSync if needed.
  - **Performance**: Use DataLoader in resolvers for N+1 query batching on SOA calls.

---

### 7. Next Steps
- **Proof of Concept**: Migrate 10-20 URIs from one Bounded Context (e.g., `SelfService` billing endpoints) into a subgraph. Test with Apollo Studio.
- **Resources**:
  - Apollo Docs: [Federation Quickstart](https://www.apollographql.com/docs/federation) and [REST Integration](https://www.apollographql.com/docs/federation/connectors/rest).
  - Migration Guide: Treat your existing DDD services as the first subgraph, then federate.
- **Team Alignment**: Discuss schema governance (e.g., who owns types?) and Apigee policies for the supergraph.

This shift streamlines your 322 URIs into a cohesive graph while preserving your event-driven, TypeScript foundation. If you share specifics (e.g., sample URI or Apigee config), I can provide a tailored subgraph example or updated `serverless.yml`. What's your first focus—schema design, a POC, or deployment tweaks?
