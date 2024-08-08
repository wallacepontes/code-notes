# Micro frontend

The concept you are referring to is known as "Micro Frontends." It's a design approach that extends the principles of microservices to the frontend development. The idea is to decompose a frontend into individual, loosely-coupled parts which can be developed, tested, and deployed independently. This allows different teams to work on different parts of the frontend, using different technologies if needed, while still maintaining a cohesive user experience.

In a micro frontends architecture, each sub-part of the UI corresponds to a separate deployable unit, often developed by different teams. These units can be developed using different technologies and frameworks, as long as they can be integrated seamlessly. The integration is typically achieved through an integration layer or a shell application that orchestrates the rendering of these independent parts. This approach provides flexibility, scalability, and enables independent development and deployment of different parts of the application.

Some popular approaches and technologies that enable micro frontends are:

1. Web Components: Using standards like Web Components allows you to create self-contained custom elements that can be reused across different projects and frameworks.

2. Single-SPA: Single-SPA is a JavaScript framework that allows you to build micro frontends that can work together. It provides utilities for orchestrating the lifecycle of different micro frontends and allows them to communicate with each other.

3. Module Federation: Module Federation, often used with webpack, enables the sharing of code between different applications. This can be useful when multiple teams are working on different micro frontends, and there is a need to share common code.

4. Iframes: Although not the most favored approach due to some drawbacks, iframes can be used to isolate different parts of the frontend. Each micro frontend is loaded into an iframe, which provides a high level of isolation but can also introduce some complexity.

When implementing a micro frontends architecture, it's important to consider the trade-offs and challenges, such as maintaining a consistent user experience, handling cross-cutting concerns such as authentication and routing, and managing the communication between different micro frontends. However, when implemented correctly, micro frontends can provide significant benefits in terms of scalability, flexibility, and team autonomy.



## Videos

 * [Micro-Frontends Course - Beginner to Expert?](https://www.youtube.com/watch?v=lKKsjpH09dU)
	> [<img src="https://img.youtube.com/vi/lKKsjpH09dU/0.jpg" width="200">](https://www.youtube.com/watch?v=lKKsjpH09dU "Don't know what a Micro-Frontend, but you've heard the buzz and you want to learn it. Let me take you from just starting out, to understanding asynchronous loading, error handling, shared state, cross platform micro-frontends, how to route multiple applications together, and even how to test Micro-Frontend code. by freeCodeCamp.org 371,153 views 1 hour, 56 minutes and 43 seconds")

    * [Micro-Frontends: What, why and how](https://www.youtube.com/watch?v=w58aZjACETQ)
	> [<img src="https://img.youtube.com/vi/w58aZjACETQ/0.jpg" width="200">](https://www.youtube.com/watch?v=w58aZjACETQ "Find out all about Micro Frontends at the high level. Why are they interesting? What are the advantages and disadvantages? And how would you get started in moving your site into a Micro-FE architecture. by Jack Herrington 144k views 9 minutes and 33 seconds")