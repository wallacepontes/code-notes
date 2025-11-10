# GraphQL Tradeoffs

## Table of Contents
- [GraphQL Tradeoffs](#graphql-tradeoffs)
  - [Table of Contents](#table-of-contents)
  - [GraphQL Tradeoffs: Benefits vs. Burdens](#graphql-tradeoffs-benefits-vs-burdens)
    - [1. Benefits (Why You Might Choose GraphQL)](#1-benefits-why-you-might-choose-graphql)
    - [2. Burdens (The Complexity and Risks of GraphQL)](#2-burdens-the-complexity-and-risks-of-graphql)
    - [Conclusion: Should We Use GraphQL?](#conclusion-should-we-use-graphql)
  - [References](#references)

That is correct; software engineering is fundamentally about trade-offs. Whether you should use GraphQL depends entirely on weighting its benefits against the significant operational complexity and security burdens it introduces. There is **no one right answer**.

Here is a comprehensive discussion of the tradeoffs presented in the sources:

## GraphQL Tradeoffs: Benefits vs. Burdens

### 1. Benefits (Why You Might Choose GraphQL)

GraphQL was developed to solve issues inherent in traditional REST APIs, making it a powerful choice in specific contexts:

| Pro | Detail and Citation |
| :--- | :--- |
| **Efficiency and Precision** | GraphQL **prioritizes giving clients exactly the data they request and no more**. Clients can request all data needed in a **single request**, reducing the number of round trips to the server and improving efficiency. This reduces overfetching and underfetching, where the client receives more or less data than it needs. |
| **Flexibility and Complex Requests** | GraphQL is better suited for applications requiring **complex data requests**, especially those with **nested fields** or those needing to pull data from **multiple data sources**. It allows clients to specify exactly the data they need, making the API more flexible and able to accommodate a wide range of client needs. |
| **API Evolution** | GraphQL allows the API to **evolve more flexibly**. Developers can add new fields and types without breaking existing clients because clients request only the fields they need and ignore the rest. This means GraphQL APIs generally **do not require versioning** when the data shape changes, unlike REST APIs which typically require using a different URL for each version. |
| **Development Experience** | Many developers, including the author of the critical article, initially found GraphQL a **"breath of fresh air"** compared to untyped JSON REST APIs, valuing its self-documenting, type-safe nature. |

### 2. Burdens (The Complexity and Risks of GraphQL)

The primary reason to avoid GraphQL, as argued by an author with six years of experience, is that **non-functional requirements** like **security, performance, and maintainability** become far greater concerns. Mitigating these issues adds **significant complexity** to the codebase.

| Con | Detail and Citation |
| :--- | :--- |
| **Increased Attack Surface** | Exposing a **query language** to untrusted clients inherently **increases the attack surface** of the application. Mitigating this requires a substantial burden. |
| **Authorization Complexity** | Authorization is the **most widely understood risk**. Developers must ensure that **every field resolved** is authorized appropriately to the context in which it is being fetched. This contrasts sharply with the REST world, where you generally only need to authorize **every endpoint**, which is a "far smaller task". |
| **Rate Limiting Difficulty** | Since there is **no limit to how big a query can be**, requests cannot be assumed to impose equal load. Mitigation requires calculating the complexity of resolving **every single field in the schema**. Schema cycles (e.g., tags related to tags) can cause the complexity estimate to compound **exponentially**, leading to large inaccuracies. Compare this to REST, where a simple bucketed rate limiter preventing a user from exceeding a fixed number of requests per minute across all endpoints is often sufficient. |
| **Query Parsing Attacks** | Crafting an invalid, syntactically complex query string (e.g., using thousands of directives) can cause the server to OOM (Out of Memory) during error response construction, consuming **2,000x more memory** than the query string itself due to memory amplification. There is **no REST equivalent to this attack of this severity**. |
| **N+1 Performance Problems** | Because GraphQL is a query language, clients can modify a query to **create an N+1 problem** without any backend changes. Developers must defensively introduce the Dataloader pattern **everywhere**, adding boilerplate. |
| **Authorization N+1 Issues** | Integrating authorization frameworks leads to a new N+1 problem. If authorization code (e.g., checking if two users are friends) requires hitting the database, this check runs $N$ times for a list of $N$ items. This can cause queries to spend **more time authorizing data than fetching it**. This issue simply **does not exist in the REST world**. |
| **Coupling and Maintainability** | In a mature GraphQL codebase, business logic is **forced into the transport layer** via authorization rules, Dataloaders, and custom connection objects. Testing requires extensive integration tests using GraphQL queries, which can be painful, and debugging is harder because critical processes happen inside the framework. |
| **Caching Difficulty** | REST APIs can be cached **more easily** because the response from a specific endpoint is always the same, leveraging HTTP GET's well-defined caching behavior. GraphQL uses a single endpoint (often HTTP POST by default), making standard HTTP caching **more difficult**. |
| **Tooling Cost** | GraphQL requires **heavier tooling support** on both the client and server side, requiring a **sizable upfront investment**, especially for simple CRUD APIs. |

### Conclusion: Should We Use GraphQL?

The choice hinges on whether the benefits of flexibility and reducing overfetching outweigh the **significant complexity added by necessary security and performance mitigations**.

The critical recommendation from the sources is that GraphQL should **not be recommended to most people**.

An **OpenAPI 3.0+ compliant JSON REST API** is proposed as a better alternative if you meet the following criteria:
1.  You **control all your clients**.
2.  You have **$\leq 3$ clients**.
3.  You have a client written in a **statically typed language**.
4.  You are using **$>1$ language** across the server and clients.

This alternative allows developers to gain the key benefit frontend developers liked about GraphQL—its **self-documenting, type-safe nature**—without inheriting the high complexity needed for mitigating the security and performance drawbacks of exposing a fully functional query language.

## References
1. https://notebooklm.google.com/notebook/161c01db-15b2-406c-a11a-07677f71d43f
2. 