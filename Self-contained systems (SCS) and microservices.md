# Self-contained systems (SCS) and microservices

## Table of Contents
- [Self-contained systems (SCS) and microservices](#self-contained-systems-scs-and-microservices)
  - [Table of Contents](#table-of-contents)
  - [Architectural styles](#architectural-styles)
  - [Combination of microservices with a micro-frontend deployed together](#combination-of-microservices-with-a-micro-frontend-deployed-together)
  - [API exposed](#api-exposed)
  - [Preventing monolithic issues](#preventing-monolithic-issues)

## Architectural styles

Self-contained systems (SCS) and microservices are both architectural styles for building software systems, but they differ in their approach to modularity and deployment.

1. **Self-contained systems (SCS):**
   - SCS are essentially autonomous units that encompass both the frontend and backend components necessary to provide a specific set of functionality.
   - They are designed to be independent and self-sufficient, meaning they can run and be deployed as standalone entities.
   - SCS often encapsulate a complete business capability, including user interface, backend logic, and data storage.
   - They are typically larger in scope compared to microservices and may consist of multiple modules or services internally.

2. **Microservices:**
   - Microservices architecture breaks down a system into smaller, loosely coupled services that focus on performing a single function.
   - Each microservice is responsible for a specific task and communicates with other services through APIs, often over a network.
   - Microservices are designed to be small, lightweight, and independently deployable, promoting agility and scalability.
   - They encourage a decentralized approach to development and deployment, allowing teams to work on different services simultaneously.

In summary, while both SCS and microservices promote modularity and independence, SCS are more self-contained and encompass broader functionality, while microservices are smaller, more focused units that communicate with each other to accomplish tasks. Choosing between them depends on factors such as the size and complexity of the system, organizational structure, and scalability requirements.

## Combination of microservices with a micro-frontend deployed together

Yes, you could think of a Self-contained System (SCS) as a combination of microservices with a micro-frontend deployed together. SCS encapsulates both the frontend and backend components necessary to provide a specific set of functionality, similar to a microservice architecture where each service handles a specific task. 

The key difference is that in an SCS, the frontend and backend are tightly coupled within the same system, whereas in a microservices architecture, they are typically decoupled and communicate with each other via APIs over a network. 

So, in a way, an SCS provides a more cohesive and self-contained approach by bundling both frontend and backend components together, while still maintaining modularity and independence.

## API exposed

Yes, in a Self-contained System (SCS), it's possible to have APIs exposed. While the frontend and backend components are tightly integrated within an SCS, there might still be a need for external systems or clients to interact with it. In such cases, the SCS can expose APIs to allow external access.

These APIs can be used for various purposes, such as integrating with other systems, allowing third-party developers to build applications on top of the SCS, or enabling communication between different SCS instances within a larger system.

So, even though an SCS encapsulates both frontend and backend components, it can still provide interfaces for external communication, which may include RESTful APIs, GraphQL endpoints, messaging queues, or other communication mechanisms.

## Preventing monolithic issues

Preventing a Self-contained System (SCS) from growing and becoming monolithic involves maintaining modularity, enforcing boundaries, and continuously monitoring and refactoring the system as needed. Here are some strategies:

1. **Clear Module Boundaries:** Define clear boundaries between modules or components within the SCS. Each module should have a well-defined responsibility and interface, and communication between modules should be minimized to prevent tight coupling.

2. **Decomposition:** Regularly review the SCS architecture and look for opportunities to decompose it into smaller, more focused units. Identify cohesive modules that can be extracted and developed independently, following the principles of microservices or modular design.

3. **Single Responsibility Principle (SRP):** Ensure that each module or component within the SCS adheres to the SRP, meaning it should have only one reason to change. This helps prevent modules from becoming overly complex and encourages maintainability.

4. **Continuous Refactoring:** Regularly refactor the codebase to eliminate duplication, improve clarity, and maintain a clean and modular architecture. This includes removing unused code, simplifying complex logic, and optimizing performance.

5. **Automated Testing:** Implement comprehensive automated testing, including unit tests, integration tests, and end-to-end tests, to ensure that changes to the system do not introduce regressions or break existing functionality. This provides confidence when making modifications and allows for easier refactoring.

6. **Monitoring and Metrics:** Monitor the health and performance of the SCS using metrics and monitoring tools. Keep track of key indicators such as response times, error rates, and resource utilization. This helps identify areas of the system that may be becoming overly complex or resource-intensive.

7. **Team Collaboration:** Foster a culture of collaboration and communication within the development team. Encourage knowledge sharing, code reviews, and architectural discussions to ensure that everyone understands the system's architecture and contributes to its improvement.

By following these strategies and principles, you can prevent a Self-contained System from growing into a monolithic architecture and maintain its modularity, scalability, and maintainability over time.

