# Microservices

## Introduction

Microservices architecture has become increasingly popular in recent years due to its ability to help organizations overcome the challenges of building large, complex software applications. This article will provide an overview of the key topics related to microservices architecture, including its characteristics, benefits, and challenges. Additionally, we will explore how to design, deploy, and monitor microservices, and how they fit into the broader context of DevOps.

Characteristics of Microservices Architecture

Microservices architecture is an approach to building software applications as a suite of independently deployable, modular services. Each service is small, has a single responsibility, and communicates with other services via lightweight mechanisms. Microservices can be designed to be resilient, flexible, and scalable. However, they come with their own set of challenges, such as increased complexity in terms of deployment and management, the need for sophisticated testing and monitoring, and the issue of data consistency across multiple services.

Benefits of Microservices Architecture

Microservices architecture offers several benefits, including faster development and deployment, improved fault isolation and recovery, increased scalability, and the ability to use different technologies and programming languages for different services. Additionally, microservices architecture aligns with the principles of DevOps, including faster and more frequent deployments, increased collaboration between development and operations teams, and a focus on automation and monitoring.

Designing Microservices

When designing microservices, it is important to start with a domain-driven design approach, identifying service boundaries based on business capabilities. Each service should have a clear responsibility and interface. In order to decompose a monolithic application into microservices, it is important to identify areas of the application that can be separated into services, create a service inventory, and address data consistency and transactional boundaries.

Communication between Microservices

Microservices can communicate with each other using different patterns, such as synchronous and asynchronous communication. It is important to choose the appropriate pattern based on the specific needs of the application. Additionally, it is important to ensure that communication between services is secure and reliable.

Deploying and Scaling Microservices

Microservices can be deployed using different strategies, such as continuous delivery, blue-green deployment, and canary releases. It is important to choose the appropriate strategy based on the specific needs of the application. Additionally, microservices can be scaled horizontally and vertically using containerization technologies like Docker and Kubernetes.

Testing Microservices

Testing microservices can be challenging due to their distributed nature. However, it is important to ensure that each service is tested thoroughly, including unit testing, integration testing, and end-to-end testing. Additionally, contract testing and chaos engineering can be used to ensure the resilience of microservices.

Monitoring Microservices

Monitoring microservices is critical to ensuring the health and availability of the application. It is important to monitor service availability, response times, and error rates. Additionally, distributed tracing and logging can be used to gain visibility into the behavior of microservices.

Microservices and DevOps

Microservices architecture aligns with the principles of DevOps, including faster and more frequent deployments, increased collaboration between development and operations teams, and a focus on automation and monitoring. Microservices can be deployed using DevOps practices, including continuous delivery and deployment, infrastructure as code, and automated testing and monitoring.

Conclusion

Microservices architecture has become increasingly popular due to its ability to help organizations overcome the challenges of building large, complex software applications. Microservices architecture offers several benefits, including faster development and deployment, improved fault isolation and recovery, increased scalability, and the ability to use different technologies and programming languages for different services. However, microservices architecture comes with its own set of challenges, such as increased complexity in terms of deployment and management, the need for sophisticated testing and monitoring, and the issue of data consistency across multiple services. To overcome these challenges, it is important to design, deploy,

### Martin Fowler introduces the concept of microservices architecture

1. Introduction to Microservices Architecture:
In this article, Martin Fowler introduces the concept of microservices architecture, which is an approach to building software applications as a suite of independently deployable, modular services. He explains how this architecture differs from the traditional monolithic architecture and how it can help overcome some of the challenges associated with building large, complex applications.

2. Characteristics of Microservices Architecture:
Fowler describes the key characteristics of microservices architecture, such as being small, independently deployable, having a single responsibility, and communicating via lightweight mechanisms. He also discusses how microservices can be designed to be resilient, flexible, and scalable.

3. Benefits of Microservices Architecture:
The article highlights several benefits of microservices architecture, including faster development and deployment, improved fault isolation and recovery, increased scalability, and the ability to use different technologies and programming languages for different services.

4. Challenges of Microservices Architecture:
Fowler acknowledges that microservices architecture comes with its own set of challenges, such as increased complexity in terms of deployment and management, a need for sophisticated testing and monitoring, and the need to address the issue of data consistency across multiple services.

5. Designing Microservices:
The article provides guidance on how to design microservices, such as starting with a domain-driven design approach, identifying service boundaries based on business capabilities, and ensuring each service has a clear responsibility and interface.

6. Decomposing Monolithic Applications:
Fowler describes how to decompose a monolithic application into microservices, including identifying areas of the application that can be separated into services, creating a service inventory, and addressing data consistency and transactional boundaries.

7. Communication between Microservices:
The article explains the different communication patterns that can be used between microservices, such as synchronous and asynchronous communication, and provides guidance on when to use each pattern.

8. Deploying and Scaling Microservices:
Fowler discusses various deployment strategies for microservices, including continuous delivery, blue-green deployment, and canary releases. He also explains how to scale microservices horizontally and vertically and discusses the use of containerization technologies like Docker and Kubernetes.

9. Testing Microservices:
The article provides guidance on how to test microservices, including unit testing, integration testing, and end-to-end testing. It also discusses the use of contract testing and chaos engineering to ensure the resilience of microservices.

10. Monitoring Microservices:
Fowler explains the importance of monitoring microservices and provides guidance on what to monitor, such as service availability, response times, and error rates. He also discusses the use of distributed tracing and logging to gain visibility into the behavior of microservices.

11. Microservices and DevOps:
The article discusses how microservices architecture aligns with the principles of DevOps, including faster and more frequent deployments, increased collaboration between development and operations teams, and a focus on automation and monitoring.

12. Conclusion and Future Directions of Microservices Architecture:
Fowler concludes by summarizing the benefits and challenges of microservices architecture and discussing its future directions, including the use of serverless computing and the emergence of new tools and platforms designed specifically for microservices architecture.

## What Are Microservices Really All About?

Microservices is an architectural style for building and designing applications as a collection of small, independent services, each running its own process and communicating with lightweight mechanisms, typically an HTTP-based API. The goal of microservices is to improve scalability and fault tolerance, as well as allow for a more agile development process by breaking down a monolithic application into smaller, modular components.

## What does a typical microservices architecture look like?

A typical microservices architecture consists of several loosely-coupled services, each with its own responsibility and functionality. The services communicate with each other through APIs over the network. Each microservice runs in its own process, and is typically deployed and managed independently.

A typical microservices architecture might include:

- API Gateway: serves as the entry point for clients making requests to the microservices.

- Services: each microservice implements a specific business capability and exposes a set of APIs.

- Service Registry: a registry of all available services, allowing services to discover and communicate with each other.

- Load Balancer: distributes incoming requests to multiple instances of a service, to ensure reliability and scalability.

- Database: each microservice may have its own database, or a shared database, to persist its data.

- Monitoring and Logging: monitoring and logging tools are used to monitor the performance and health of the microservices and to assist with troubleshooting and debugging.

The architecture and components can vary based on the needs of the specific application and organization.

## And when should we use it?

Microservices architecture can be useful in various situations, including:

1. Large and complex applications: where a monolithic architecture can become unwieldy, microservices can provide a more scalable and maintainable solution by breaking the application into smaller, independent components.

2. Agile development: microservices allow for more rapid and agile development, as services can be developed, tested, and deployed independently of each other.

3. Scalability: microservices allow for more fine-grained scaling, as individual services can be scaled up or down independently based on demand.

4. Resilience: the loose coupling of services in a microservices architecture makes it easier to build a fault-tolerant system, as the failure of one service does not bring down the entire application.

5. Different technologies: microservices allows for the use of different technologies for different services, based on their specific requirements.

However, microservices are not a silver bullet and can add complexity to the overall system. It is important to carefully consider the trade-offs before deciding to use a microservices architecture and to have a clear understanding of the requirements and goals of the application.

## Designing functions with well-defined interfaces

Designing functions with well-defined interfaces is a critical aspect of both serverless and microservices architecture, as it allows for loose coupling between components and facilitates communication and integration between them. Here are some key steps to designing functions with well-defined interfaces:

1. Identify the inputs and outputs: Define the inputs and outputs of each function, including the data format, data structure, and any constraints or requirements. This will help ensure that the function can be called and used by other components in a predictable and consistent manner.

2. Document the interface: Document the interface of each function, including the inputs, outputs, and any errors or exceptions that may be returned. This documentation can be in the form of API documentation, code comments, or other technical documentation.

3. Use a common data format: Consider using a common data format, such as JSON or XML, for all inputs and outputs. This will make it easier for different functions to communicate and integrate with each other.

4. Validate inputs: Validate inputs to ensure that they are in the expected format and meet any constraints. This will help prevent errors and ensure that the function operates as intended.

5. Define error handling: Define how errors and exceptions will be handled, including any error codes or messages that will be returned. This will help ensure that errors can be effectively handled and resolved by calling components.

6. Consider versioning: Consider versioning the interface, especially if the function is part of a larger system and may change over time. This will allow different components to continue to work with the function even if its interface changes.

By following these steps, functions can be designed with well-defined interfaces that promote loose coupling, facilitate communication and integration, and allow for 
scalable and maintainable systems.

## Advantages of Microservices Architecture

Microservices architecture is a modern software development approach that offers a number of advantages and disadvantages, as outlined in your description:

**Advantages of Microservices Architecture:**

1. **Independent Components:** Each microservice operates independently and can be developed, deployed, and scaled separately. This makes it easier to manage and update individual services without affecting the entire application.

2. **Easier Understanding:** Smaller, focused services are easier to understand and maintain compared to a monolithic application, as each service has a specific purpose and functionality.

3. **Scalability:** Microservices can be scaled horizontally by adding more instances of a particular service as needed. This provides greater flexibility in handling varying workloads.

4. **Faster Deployments:** With independent services, you can deploy changes or updates more quickly, as you don't need to redeploy the entire application. This can lead to faster development cycles and quicker responses to user feedback.

5. **Flexibility in Choosing Technology:** Different services within a microservices architecture can use different programming languages, frameworks, and libraries that are best suited to their specific requirements. This flexibility can enhance development efficiency.

6. **Fault Tolerance:** Since microservices are independent and isolated, failures in one service are less likely to impact the overall system. You can implement redundancy and failover strategies for critical services to improve fault tolerance.

**Disadvantages of Microservices Architecture:**

1. **Complexity:** Managing a system composed of multiple microservices can be complex. You need to handle service discovery, load balancing, monitoring, and coordination between services, which can increase system complexity.

2. **System Distribution:** The distributed nature of microservices can introduce challenges related to network latency, data consistency, and communication between services. Proper design and architecture are required to address these issues.

3. **System Integration:** Integrating various microservices into a cohesive application can be challenging. You need to define clear and well-documented APIs, and ensure that services work seamlessly together.

4. **Testing:** Testing a microservices-based application can be more complex than testing a monolithic application. You must consider end-to-end testing, service mocking, and ensuring that different services interact correctly.

It's important to note that the decision to adopt a microservices architecture should be based on the specific needs and goals of a project. While microservices can offer significant benefits in terms of scalability and flexibility, they also introduce complexities that need to be managed effectively. Careful planning, architectural design, and ongoing maintenance are crucial for the success of a microservices-based application.

## 𝗠𝗶𝗰𝗿𝗼𝘀𝗲𝗿𝘃𝗶𝗰𝗲𝘀 𝗔𝗿𝗰𝗵𝗶𝘁𝗲𝗰𝘁𝘂𝗿𝗲 𝗶𝘀 𝗻𝗼𝘁 𝗮𝗹𝘄𝗮𝘆𝘀 𝘁𝗵𝗲 𝗯𝗲𝘀𝘁 𝗰𝗵𝗼𝗶𝗰𝗲 🤔

Microservices architecture has gained significant attention for its ability to provide scalability, flexibility, and independent deployment of services.

However, it's important to note that while microservices offer numerous benefits, they might not always be the best fit for every situation.

Here are some considerations to keep in mind:

🧩 𝗖𝗼𝗺𝗽𝗹𝗲𝘅𝗶𝘁𝘆 𝗢𝘃𝗲𝗿𝗵𝗲𝗮𝗱: Microservices introduce a higher level of complexity, requiring careful management of multiple services and communication between them.

🔧 𝗢𝗽𝗲𝗿𝗮𝘁𝗶𝗼𝗻𝗮𝗹 𝗖𝗼𝗺𝗽𝗹𝗲𝘅𝗶𝘁𝘆: Operating and monitoring a distributed system of microservices can be challenging, requiring additional resources and tools.

⏱️ 𝗜𝗻𝗰𝗿𝗲𝗮𝘀𝗲𝗱 𝗟𝗮𝘁𝗲𝗻𝗰𝘆: Communication between microservices over the network can lead to higher latency compared to in-memory communication in monolithic architectures.

💻 𝗗𝗲𝘃𝗲𝗹𝗼𝗽𝗺𝗲𝗻𝘁 𝗮𝗻𝗱 𝗧𝗲𝘀𝘁𝗶𝗻𝗴 𝗢𝘃𝗲𝗿𝗵𝗲𝗮𝗱: Developing and testing microservices demands additional effort to ensure proper integration and compatibility.

💡 𝗟𝗲𝗮𝗿𝗻𝗶𝗻𝗴 𝗖𝘂𝗿𝘃𝗲: Transitioning to microservices requires developers to learn new tools and best practices, potentially slowing down the initial development process.


While microservices offer benefits, they also come with challenges like data consistency, organizational changes, and resource-intensive demands. The decision to adopt a microservices architecture should be based on the specific needs of your project.


Remember, there's no one-size-fits-all solution, and a careful evaluation of your project's requirements will help determine whether microservices are the right choice.

Have you considered these factors in your architecture decisions? Share your thoughts! 👇

### considerations

The considerations you've highlighted are indeed crucial when making decisions about adopting a microservices architecture. It's essential for organizations to carefully assess their specific needs, constraints, and development capabilities before committing to such an architectural approach. Here are some additional thoughts on these considerations:

📈 **Scalability vs. Complexity:** Microservices excel in scalability, but it's important to gauge whether your application truly requires this level of scalability. For smaller or less complex projects, the added complexity of microservices might not be justified.

🌐 **Network Overhead:** Microservices often involve network communication between services, which can introduce latency and potential points of failure. Understanding the trade-offs between network overhead and service isolation is crucial.

🛠️ **Operational Overhead:** The operational complexity of managing multiple microservices should not be underestimated. DevOps practices and tooling become essential to handle the deployment, monitoring, and maintenance of these services effectively.

⚙️ **Tooling and Infrastructure:** Microservices often require robust infrastructure and tooling for service discovery, load balancing, and monitoring. Ensure that your organization is equipped to handle the additional technical requirements.

🔄 **Data Management:** Handling data consistency, transactions, and inter-service communication patterns are significant challenges in microservices. Assess how your application manages data and whether a microservices approach aligns with your data requirements.

👥 **Team Structure:** Microservices might necessitate changes in your team's structure, with teams responsible for individual services. Consider the impact on your organization's culture and collaboration patterns.

💬 **Communication Overhead:** Effective communication between teams working on different microservices is crucial. Establish clear communication channels and documentation practices to avoid misunderstandings.

📚 **Learning Curve:** Prepare for a learning curve as developers and operations teams adapt to the microservices paradigm. Investing in training and knowledge sharing can mitigate this challenge.

Ultimately, microservices are a powerful architectural pattern that can deliver significant benefits, but they also come with trade-offs. It's essential to make an informed decision based on your project's specific requirements, your team's expertise, and your organization's ability to manage the complexity introduced by microservices. In some cases, a hybrid approach that combines microservices with monolithic components might be a suitable compromise.

## the key advantages of microservices architecture

You're absolutely right, and you've highlighted some of the key advantages of microservices architecture, which include:

1. **Decoupled Functionality:** Microservices allow for the decoupling of different parts of an application, making it easier to develop, maintain, and evolve each component independently. This decoupling reduces the risk of changes in one microservice impacting the entire system.

2. **Reduced Risk of Downtime:** Since microservices are independently deployable, updates or improvements to one service can be made without affecting the availability of the entire system. This isolation mitigates the risk of system-wide downtime.

3. **Simplified Maintenance:** Smaller, more focused microservices are easier to understand, maintain, and troubleshoot compared to a monolithic application. This ease of maintenance is particularly beneficial as the application grows.

4. **Team Scalability:** Microservices allow for the scaling of teams based on the services they are responsible for. This can lead to more efficient development, with specialized teams working on specific areas of expertise.

5. **Agile Adaptation:** Adopting microservices often goes hand-in-hand with agile development practices, which emphasize iterative development, continuous delivery, and rapid adaptation to changing requirements. This agility is well-suited to the microservices approach.

It's important to note that while microservices can address many challenges associated with monolithic applications, they introduce their own set of complexities, particularly in terms of managing the distributed nature of the system and ensuring effective communication between services. These complexities can be managed with the right tools, practices, and a well-thought-out architecture.

As you mentioned, careful management and an agile framework are essential when transitioning to a microservices architecture. Planning, documentation, and clear communication within and between teams become critical to the success of the project. Additionally, monitoring and observability tools are vital for identifying and addressing issues in a distributed environment.

In summary, microservices offer significant advantages in terms of scalability, maintenance, and flexibility, but they should be adopted with a clear understanding of the complexities involved and a commitment to managing those complexities effectively. When implemented thoughtfully, microservices can empower organizations to build and evolve complex applications with greater agility and reliability.

## Videos
 * [O que é arquitetura de microsserviços ? Como ela funciona ?](https://www.youtube.com/watch?v=_Oyy5PFOIcU)
	> [<img src="https://img.youtube.com/vi/_Oyy5PFOIcU/0.jpg" width="200">](https://www.youtube.com/watch?v=_Oyy5PFOIcU "O que é arquitetura de microsserviços ? Como ela funciona ? by Douglas Mugnos 17,035 views 10 minutes, 39 seconds")
 * [Event source, Arquitetura baseada em eventos, SOA vs Microsserviços](https://www.youtube.com/watch?v=9y9BAUourSY)
	> [<img src="https://img.youtube.com/vi/9y9BAUourSY/0.jpg" width="200">](https://www.youtube.com/watch?v=9y9BAUourSY "Event source, Arquitetura baseada em eventos, SOA vs Microsserviços by Full Cycle 6,522 views 23 minutes")
 * [Microsserviços: O que restou. Erros e acertos](https://www.youtube.com/watch?v=Zu-D9A_L5Qo)
	> [<img src="https://img.youtube.com/vi/Zu-D9A_L5Qo/0.jpg" width="200">](https://www.youtube.com/watch?v=Zu-D9A_L5Qo "Microsserviços: O que restou. Erros e acertos by Full Cycle 54,680 views 22 minutes")
 * [Comunicação entre #Microsserviços usando : Orquestração vs Coreografia](https://www.youtube.com/watch?v=K_laUuqf2fc)
	> [<img src="https://img.youtube.com/vi/K_laUuqf2fc/0.jpg" width="200">](https://www.youtube.com/watch?v=K_laUuqf2fc "Comunicação entre #Microsserviços usando : Orquestração vs Coreografia by Emerging Code 3,007 views 36 minutes")
 * [Quem ganha? Sistemas Monolíticos vs Microsserviços](https://www.youtube.com/watch?v=UgmRiqL0sbA)
	> [<img src="https://img.youtube.com/vi/UgmRiqL0sbA/0.jpg" width="200">](https://www.youtube.com/watch?v=UgmRiqL0sbA "Quem ganha? Sistemas Monolíticos vs Microsserviços by Full Cycle 47,376 views 23 minutes")
 * [MICROSSERVIÇOS JAVA COM DDD, CQRS E EVENT SOURCING - PARTE 2](https://www.youtube.com/watch?v=HnxpOcwKEFA)
	> [<img src="https://img.youtube.com/vi/HnxpOcwKEFA/0.jpg" width="200">](https://www.youtube.com/watch?v=HnxpOcwKEFA "MICROSSERVIÇOS JAVA COM DDD, CQRS E EVENT SOURCING - PARTE 2 by Lucas Gertel 7,146 views 21 minutes")
 * [MICROSSERVIÇOS JAVA COM DDD, CQRS E EVENT SOURCING - PARTE 1](https://www.youtube.com/watch?v=xhn2wW0XShA)
	> [<img src="https://img.youtube.com/vi/xhn2wW0XShA/0.jpg" width="200">](https://www.youtube.com/watch?v=xhn2wW0XShA "MICROSSERVIÇOS JAVA COM DDD, CQRS E EVENT SOURCING - PARTE 1 by Lucas Gertel 12,619 views 25 minutes")

 * [CONSTRUINDO MICROSERVIÇOS COM JAVA](https://www.youtube.com/watch?v=yACzWg9gUGM)
	> [<img src="https://img.youtube.com/vi/yACzWg9gUGM/0.jpg" width="200">](https://www.youtube.com/watch?v=yACzWg9gUGM "CONSTRUINDO MICROSERVIÇOS COM JAVA by Fernanda Kipper | Dev 24,032 views 2 hours, 6 minutes")
 * [Designing a microservices architecture with DDD | Is DDD still useful?](https://www.youtube.com/watch?v=gtZIaSxRkS4)
	> [<img src="https://img.youtube.com/vi/gtZIaSxRkS4/0.jpg" width="200">](https://www.youtube.com/watch?v=gtZIaSxRkS4 "Designing a microservices architecture with DDD | Is DDD still useful? by Marco Lenzo 20,624 views 7 minutes, 40 seconds")
 * [Building Prod Ready Microservice using Express JS Project #07](https://www.youtube.com/watch?v=aCH6jnUNrEs)
	> [<img src="https://img.youtube.com/vi/aCH6jnUNrEs/0.jpg" width="200">](https://www.youtube.com/watch?v=aCH6jnUNrEs "Building Prod Ready Microservice using Express JS Project #07 by Code with tkssharma 753 views 37 minutes")
 * [How Event-Based Communication enhances Microservices Architecture? (Intro to Microservices - Part 2)](https://www.youtube.com/watch?v=bpP4GX8oxjk)
	> [<img src="https://img.youtube.com/vi/bpP4GX8oxjk/0.jpg" width="200">](https://www.youtube.com/watch?v=bpP4GX8oxjk "How Event-Based Communication enhances Microservices Architecture? (Intro to Microservices - Part 2) by sudoCODE 11,441 views 12 minutes, 6 seconds")
 * [Service Discovery in Microservices | Microservices Primer Course](https://www.youtube.com/watch?v=3UDJvF0CMkQ)
	> [<img src="https://img.youtube.com/vi/3UDJvF0CMkQ/0.jpg" width="200">](https://www.youtube.com/watch?v=3UDJvF0CMkQ "Service Discovery in Microservices | Microservices Primer Course by sudoCODE 13,237 views 7 minutes, 54 seconds")
 * [Types of coupling in microservices and how to reduce it ? Microservices Primer Course](https://www.youtube.com/watch?v=bQvLj71-0Mk)
	> [<img src="https://img.youtube.com/vi/bQvLj71-0Mk/0.jpg" width="200">](https://www.youtube.com/watch?v=bQvLj71-0Mk "Types of coupling in microservices and how to reduce it ? Microservices Primer Course by sudoCODE 6,671 views 8 minutes, 46 seconds")
 * [Micro Frontend - Aplicando o conceito de micro-serviços no Front-end, por Mateus Wilding](https://www.youtube.com/watch?v=SC1QkRfFccE)
	> [<img src="https://img.youtube.com/vi/SC1QkRfFccE/0.jpg" width="200">](https://www.youtube.com/watch?v=SC1QkRfFccE "Micro Frontend - Aplicando o conceito de micro-serviços no Front-end, por Mateus Wilding by Codeminer42 4,545 views 22 minutes")
 * [Introduction to Microservices for absolute beginners | Yogita Sharma | sudoCODE](https://www.youtube.com/watch?v=mgCUmdH5LNk)
	> [<img src="https://img.youtube.com/vi/mgCUmdH5LNk/0.jpg" width="200">](https://www.youtube.com/watch?v=mgCUmdH5LNk "Introduction to Microservices for absolute beginners | Yogita Sharma | sudoCODE by sudoCODE 21,792 views 17 minutes")
 * [Micro Frontend - Aplicando o conceito de micro-serviços no Front-end, por Mateus Wilding](https://www.youtube.com/watch?v=SC1QkRfFccE)
	> [<img src="https://img.youtube.com/vi/SC1QkRfFccE/0.jpg" width="200">](https://www.youtube.com/watch?v=SC1QkRfFccE "Micro Frontend - Aplicando o conceito de micro-serviços no Front-end, por Mateus Wilding by Codeminer42 4,545 views 22 minutes")
 * [The HIDDEN Challenge of Microservices: UI Composition](https://www.youtube.com/watch?v=ILbjKR1FXoc)
	> [<img src="https://img.youtube.com/vi/ILbjKR1FXoc/0.jpg" width="200">](https://www.youtube.com/watch?v=ILbjKR1FXoc "The HIDDEN Challenge of Microservices: UI Composition by CodeOpinion 12,743 views 11 minutes, 11 seconds")
 * [Microservices Architecture Patterns | SAGA Choreography Explained &amp; Project Creation | JavaTechie](https://www.youtube.com/watch?v=aOen1-pQLZg)
	> [<img src="https://img.youtube.com/vi/aOen1-pQLZg/0.jpg" width="200">](https://www.youtube.com/watch?v=aOen1-pQLZg "Microservices Architecture Patterns | SAGA Choreography Explained &amp; Project Creation | JavaTechie by Java Techie 111,709 views 18 minutes")
 * [How to do Distributed Transactions the RIGHT way? Microservices](https://www.youtube.com/watch?v=vGOEO6mO674)
	> [<img src="https://img.youtube.com/vi/vGOEO6mO674/0.jpg" width="200">](https://www.youtube.com/watch?v=vGOEO6mO674 "How to do Distributed Transactions the RIGHT way? Microservices by sudoCODE 29,903 views 13 minutes, 6 seconds")
 * [Microservices explained - the What, Why and How?](https://www.youtube.com/watch?v=rv4LlmLmVWk)
	> [<img src="https://img.youtube.com/vi/rv4LlmLmVWk/0.jpg" width="200">](https://www.youtube.com/watch?v=rv4LlmLmVWk "Microservices explained - the What, Why and How? by TechWorld with Nana 746,521 views 18 minutes")
 * [The ONLY TWO good reasons for MICROSERVICES](https://www.youtube.com/watch?v=m92iMj1x6q4)
	> [<img src="https://img.youtube.com/vi/m92iMj1x6q4/0.jpg" width="200">](https://www.youtube.com/watch?v=m92iMj1x6q4 "The ONLY TWO good reasons for MICROSERVICES by Marco Lenzo 1,594 views 7 minutes, 13 seconds")
 * [Stop Creating Microservices | Prime Reacts](https://www.youtube.com/watch?v=ivjPzOoPZsM)
	> [<img src="https://img.youtube.com/vi/ivjPzOoPZsM/0.jpg" width="200">](https://www.youtube.com/watch?v=ivjPzOoPZsM "Stop Creating Microservices | Prime Reacts by ThePrimeTime 181,393 views 33 minutes")
 * [Which is better : microservices or monolithic architecture | Detailed analysis](https://www.youtube.com/watch?v=Q-w-hM1rGdg)
	> [<img src="https://img.youtube.com/vi/Q-w-hM1rGdg/0.jpg" width="200">](https://www.youtube.com/watch?v=Q-w-hM1rGdg "Which is better : microservices or monolithic architecture | Detailed analysis by IT k Funde 78,181 views 20 minutes")
 * [Mastering Chaos - A Netflix Guide to Microservices](https://www.youtube.com/watch?v=CZ3wIuvmHeM)
	> [<img src="https://img.youtube.com/vi/CZ3wIuvmHeM/0.jpg" width="200">](https://www.youtube.com/watch?v=CZ3wIuvmHeM "Mastering Chaos - A Netflix Guide to Microservices by InfoQ 2,271,548 views 53 minutes")
 * [Distributed isn&#39;t Microservices, In-Process isn&#39;t a Monolith](https://www.youtube.com/watch?v=qndSXhknxRc)
	> [<img src="https://img.youtube.com/vi/qndSXhknxRc/0.jpg" width="200">](https://www.youtube.com/watch?v=qndSXhknxRc "Distributed isn&#39;t Microservices, In-Process isn&#39;t a Monolith by CodeOpinion 15,344 views 8 minutes, 40 seconds")
 * [Using sagas to maintain data consistency in a microservice architecture by Chris Richardson](https://www.youtube.com/watch?v=YPbGW3Fnmbc)
	> [<img src="https://img.youtube.com/vi/YPbGW3Fnmbc/0.jpg" width="200">](https://www.youtube.com/watch?v=YPbGW3Fnmbc "Using sagas to maintain data consistency in a microservice architecture by Chris Richardson by Devoxx 291,651 views 49 minutes")
 * [Microservices gets it WRONG defining Service Boundaries](https://www.youtube.com/watch?v=Uc7SLJbKAGo)
	> [<img src="https://img.youtube.com/vi/Uc7SLJbKAGo/0.jpg" width="200">](https://www.youtube.com/watch?v=Uc7SLJbKAGo "Microservices gets it WRONG defining Service Boundaries by CodeOpinion 11,453 views 10 minutes, 15 seconds")
 * [Comunicação entre Microservices](https://www.youtube.com/watch?v=o71dBV1nZt4)
	> [<img src="https://img.youtube.com/vi/o71dBV1nZt4/0.jpg" width="200">](https://www.youtube.com/watch?v=o71dBV1nZt4 "Comunicação entre Microservices by Michelli Brito 29,153 views 10 minutes, 56 seconds")
 * [Arquitetura de Microservices](https://www.youtube.com/watch?v=zd5iI9tLKm8)
	> [<img src="https://img.youtube.com/vi/zd5iI9tLKm8/0.jpg" width="200">](https://www.youtube.com/watch?v=zd5iI9tLKm8 "Arquitetura de Microservices by Michelli Brito 20,851 views 13 minutes, 59 seconds")
 * [Coding Microservice From Scratch (Part 13) | JAX-RS Done Right! | Head Crashing Informatics 79](https://www.youtube.com/watch?v=FiwRpVXLYo4)
	> [<img src="https://img.youtube.com/vi/FiwRpVXLYo4/0.jpg" width="200">](https://www.youtube.com/watch?v=FiwRpVXLYo4 "Coding Microservice From Scratch (Part 13) | JAX-RS Done Right! | Head Crashing Informatics 79 by Markus Karg 109 views 16 minutes")
 * [Microservices explained - the What, Why and How?](https://www.youtube.com/watch?v=rv4LlmLmVWk)
	> [<img src="https://img.youtube.com/vi/rv4LlmLmVWk/0.jpg" width="200">](https://www.youtube.com/watch?v=rv4LlmLmVWk "Microservices explained - the What, Why and How? by TechWorld with Nana 746,521 views 18 minutes")
 * [How to seed a new Microservice with data?](https://www.youtube.com/watch?v=RcVf-R7RZcY)
	> [<img src="https://img.youtube.com/vi/RcVf-R7RZcY/0.jpg" width="200">](https://www.youtube.com/watch?v=RcVf-R7RZcY "How to seed a new Microservice with data? by CodeOpinion 7,366 views 9 minutes, 36 seconds")
 * [Lesson 162 - Microservices Architecture](https://www.youtube.com/watch?v=UZQMUiVqpFs)
	> [<img src="https://img.youtube.com/vi/UZQMUiVqpFs/0.jpg" width="200">](https://www.youtube.com/watch?v=UZQMUiVqpFs "Lesson 162 - Microservices Architecture by Mark Richards 6,093 views 11 minutes, 55 seconds")
 * [Monolith vs Microservices vs Serverless](https://www.youtube.com/watch?v=1A9tPOfp6NA)
	> [<img src="https://img.youtube.com/vi/1A9tPOfp6NA/0.jpg" width="200">](https://www.youtube.com/watch?v=1A9tPOfp6NA "Monolith vs Microservices vs Serverless by Code With Ryan 71,669 views 23 minutes")
 * [Microservices are evil!](https://www.youtube.com/watch?v=98IUhK6IGUA)
	> [<img src="https://img.youtube.com/vi/98IUhK6IGUA/0.jpg" width="200">](https://www.youtube.com/watch?v=98IUhK6IGUA "Microservices are evil! by James Luterek 14,618 views 4 minutes, 2 seconds")
 * [Prime Video Swaps Microservices for Monolith: 90% Cost Reduction](https://www.youtube.com/watch?v=dV3wAe8HV7Q)
	> [<img src="https://img.youtube.com/vi/dV3wAe8HV7Q/0.jpg" width="200">](https://www.youtube.com/watch?v=dV3wAe8HV7Q "Prime Video Swaps Microservices for Monolith: 90% Cost Reduction by Hussein Nasser 155,397 views 35 minutes")
 * [Microservices // Dicionário do Programador](https://www.youtube.com/watch?v=_2bDOCTnbKc)
	> [<img src="https://img.youtube.com/vi/_2bDOCTnbKc/0.jpg" width="200">](https://www.youtube.com/watch?v=_2bDOCTnbKc "Microservices // Dicionário do Programador by Código Fonte TV 73,746 views 9 minutes, 51 seconds")
 * [Microservices // Dicionário do Programador](https://www.youtube.com/watch?v=_2bDOCTnbKc)
	> [<img src="https://img.youtube.com/vi/_2bDOCTnbKc/0.jpg" width="200">](https://www.youtube.com/watch?v=_2bDOCTnbKc "Microservices // Dicionário do Programador by Código Fonte TV 73,746 views 9 minutes, 51 seconds")
 * [REST APIs for Microservices? Beware!](https://www.youtube.com/watch?v=_4gyR6CBkUE)
	> [<img src="https://img.youtube.com/vi/_4gyR6CBkUE/0.jpg" width="200">](https://www.youtube.com/watch?v=_4gyR6CBkUE "REST APIs for Microservices? Beware! by CodeOpinion 48,702 views 11 minutes, 49 seconds")
 * [Developing microservices with aggregates - Chris Richardson](https://www.youtube.com/watch?v=7kX3fs0pWwc)
 	> [<img src="https://img.youtube.com/vi/7kX3fs0pWwc/0.jpg" width="200">](https://www.youtube.com/watch?v=7kX3fs0pWwc "Developing microservices with aggregates - Chris Richardson by SpringDeveloper 273,589 views 1 hour, 9 minutes")
 * [Monolithic vs Microservice Architecture: Which To Use and When?](https://www.youtube.com/watch?v=NdeTGlZ__Do)
	> [<img src="https://img.youtube.com/vi/NdeTGlZ__Do/0.jpg" width="200">](https://www.youtube.com/watch?v=NdeTGlZ__Do "Monolithic vs Microservice Architecture: Which To Use and When? by Alex Hyett 46,271 views 10 minutes, 43 seconds")
 * [Comunicação entre Microservices](https://www.youtube.com/watch?v=8ij7uCSr4T4)
	> [<img src="https://img.youtube.com/vi/8ij7uCSr4T4/0.jpg" width="200">](https://www.youtube.com/watch?v=8ij7uCSr4T4 "Comunicação entre Microservices by Ramon Durães 2,065 views 7 minutes, 9 seconds")
 * [Moving from MONOLITHS to MICROSERVICES 🎂 → 🍰🍰🍰](https://www.youtube.com/watch?v=rckfN7xFig0)
	> [<img src="https://img.youtube.com/vi/rckfN7xFig0/0.jpg" width="200">](https://www.youtube.com/watch?v=rckfN7xFig0 "Moving from MONOLITHS to MICROSERVICES 🎂 → 🍰🍰🍰 by Gaurav Sen 206,227 views 19 minutes")
 * [What is a MICROSERVICE ARCHITECTURE and what are its advantages?](https://www.youtube.com/watch?v=qYhRvH9tJKw)
	> [<img src="https://img.youtube.com/vi/qYhRvH9tJKw/0.jpg" width="200">](https://www.youtube.com/watch?v=qYhRvH9tJKw "What is a MICROSERVICE ARCHITECTURE and what are its advantages? by Gaurav Sen 673,613 views 8 minutes, 19 seconds")
 * [We Need to Discuss the Microservices Madness - Scaling with Common Sense](https://www.youtube.com/watch?v=NsIeAV5aFLE)
	> [<img src="https://img.youtube.com/vi/NsIeAV5aFLE/0.jpg" width="200">](https://www.youtube.com/watch?v=NsIeAV5aFLE "We Need to Discuss the Microservices Madness - Scaling with Common Sense by Hussein Nasser 30,443 views 46 minutes")
 * [Client Side Load Balancing | Microservices Architecture Pattern | Tech Primers](https://www.youtube.com/watch?v=-PbnWGddmcM)
	> [<img src="https://img.youtube.com/vi/-PbnWGddmcM/0.jpg" width="200">](https://www.youtube.com/watch?v=-PbnWGddmcM "Client Side Load Balancing | Microservices Architecture Pattern | Tech Primers by Tech Primers 45,231 views 6 minutes, 13 seconds")
 * [What are Microservices? | Tech Primers](https://www.youtube.com/watch?v=-luHIZJ6or0)
	> [<img src="https://img.youtube.com/vi/-luHIZJ6or0/0.jpg" width="200">](https://www.youtube.com/watch?v=-luHIZJ6or0 "What are Microservices? | Tech Primers by Tech Primers 53,685 views 2 minutes, 56 seconds")
 * [HA vs Fault Tolerance | How Swiggy handles Faults in Microservices? | Tech Primers](https://www.youtube.com/watch?v=7Q73wgGUpZM)
	> [<img src="https://img.youtube.com/vi/7Q73wgGUpZM/0.jpg" width="200">](https://www.youtube.com/watch?v=7Q73wgGUpZM "HA vs Fault Tolerance | How Swiggy handles Faults in Microservices? | Tech Primers by Tech Primers 6,416 views 15 minutes")
 * [CQRS is probably the cause of the Microservices madness](https://www.youtube.com/watch?v=DQ3D_mplIgY)
	> [<img src="https://img.youtube.com/vi/DQ3D_mplIgY/0.jpg" width="200">](https://www.youtube.com/watch?v=DQ3D_mplIgY "CQRS is probably the cause of the Microservices madness by Hussein Nasser 67,318 views 6 minutes, 31 seconds")
 * [How to Secure Your Microservices Architecture With JSON Web Tokens](https://www.youtube.com/watch?v=n-3J6RuL9Gk)
	> [<img src="https://img.youtube.com/vi/n-3J6RuL9Gk/0.jpg" width="200">](https://www.youtube.com/watch?v=n-3J6RuL9Gk "How to Secure Your Microservices Architecture With JSON Web Tokens by Techstrong TV 22,702 views 1 hour")
 * [Microservices Security Architecture  (+ Cybersecurity basics)](https://www.youtube.com/watch?v=m438IXxaQJ4)
	> [<img src="https://img.youtube.com/vi/m438IXxaQJ4/0.jpg" width="200">](https://www.youtube.com/watch?v=m438IXxaQJ4 "Microservices Security Architecture  (+ Cybersecurity basics) by A Dev&#39; Story 21,321 views 8 minutes, 2 seconds")
 * [The Saga Pattern in Microservices (EDA - part 2)](https://www.youtube.com/watch?v=C0rGwyJkDTU)
	> [<img src="https://img.youtube.com/vi/C0rGwyJkDTU/0.jpg" width="200">](https://www.youtube.com/watch?v=C0rGwyJkDTU "The Saga Pattern in Microservices (EDA - part 2) by A Dev&#39; Story 134,091 views 7 minutes, 59 seconds")
 * [The Saga Pattern in Microservices (EDA - part 2)](https://www.youtube.com/watch?v=C0rGwyJkDTU)
	> [<img src="https://img.youtube.com/vi/C0rGwyJkDTU/0.jpg" width="200">](https://www.youtube.com/watch?v=C0rGwyJkDTU "The Saga Pattern in Microservices (EDA - part 2) by A Dev&#39; Story 134,091 views 7 minutes, 59 seconds")
 * [Pragmatic Event-Driven Microservices • Allard Buijze • GOTO 2018](https://www.youtube.com/watch?v=vSd_0zGxsIU)
	> [<img src="https://img.youtube.com/vi/vSd_0zGxsIU/0.jpg" width="200">](https://www.youtube.com/watch?v=vSd_0zGxsIU "Pragmatic Event-Driven Microservices • Allard Buijze • GOTO 2018 by GOTO Conferences 39,420 views 48 minutes")
 * [Organizing (Commands, Events &amp; Handlers) in Microservices/SOA](https://www.youtube.com/watch?v=8j5ETvSSNpc)
	> [<img src="https://img.youtube.com/vi/8j5ETvSSNpc/0.jpg" width="200">](https://www.youtube.com/watch?v=8j5ETvSSNpc "Organizing (Commands, Events &amp; Handlers) in Microservices/SOA by CodeOpinion 7,202 views 8 minutes, 22 seconds")
 * [REST APIs for Microservices? Beware!](https://www.youtube.com/watch?v=_4gyR6CBkUE)
	> [<img src="https://img.youtube.com/vi/_4gyR6CBkUE/0.jpg" width="200">](https://www.youtube.com/watch?v=_4gyR6CBkUE "REST APIs for Microservices? Beware! by CodeOpinion 48,703 views 11 minutes, 49 seconds")
 * [Microservices with FastAPI – Full Course](https://www.youtube.com/watch?v=Cy9fAvsXGZA)
	> [<img src="https://img.youtube.com/vi/Cy9fAvsXGZA/0.jpg" width="200">](https://www.youtube.com/watch?v=Cy9fAvsXGZA "Microservices with FastAPI – Full Course by freeCodeCamp.org 232,802 views 1 hour, 28 minutes")
 * [Microservice Architecture and System Design with Python &amp; Kubernetes – Full Course](https://www.youtube.com/watch?v=hmkF77F9TLw)
	> [<img src="https://img.youtube.com/vi/hmkF77F9TLw/0.jpg" width="200">](https://www.youtube.com/watch?v=hmkF77F9TLw "Microservice Architecture and System Design with Python &amp; Kubernetes – Full Course by freeCodeCamp.org 363,303 views 5 hours, 4 minutes")
 * [Do Microservices require Containers/Docker/Kubernetes?](https://www.youtube.com/watch?v=kDVcAcCSvdI)
	> [<img src="https://img.youtube.com/vi/kDVcAcCSvdI/0.jpg" width="200">](https://www.youtube.com/watch?v=kDVcAcCSvdI "Do Microservices require Containers/Docker/Kubernetes? by CodeOpinion 8,371 views 10 minutes, 33 seconds")
 * [Does CAP Theorem apply to Microservices?](https://www.youtube.com/watch?v=PgHMtMmSn9s)
	> [<img src="https://img.youtube.com/vi/PgHMtMmSn9s/0.jpg" width="200">](https://www.youtube.com/watch?v=PgHMtMmSn9s "Does CAP Theorem apply to Microservices? by CodeOpinion 12,497 views 11 minutes, 38 seconds")
 * [Troubleshooting Kafka with 2000 Microservices | Event Driven](https://www.youtube.com/watch?v=dLpCFwR4Eac)
	> [<img src="https://img.youtube.com/vi/dLpCFwR4Eac/0.jpg" width="200">](https://www.youtube.com/watch?v=dLpCFwR4Eac "Troubleshooting Kafka with 2000 Microservices | Event Driven by CodeOpinion 16,256 views 12 minutes, 39 seconds")
 * [CREATING AN EMAIL SENDING MICROSERVICE](https://www.youtube.com/watch?v=ZBleZzJf6ro)
	> [<img src="https://img.youtube.com/vi/ZBleZzJf6ro/0.jpg" width="200">](https://www.youtube.com/watch?v=ZBleZzJf6ro "CREATING AN EMAIL SENDING MICROSERVICE by Michelli Brito 56,911 views 37 minutes")
 * [ASYNCHRONOUS COMMUNICATION BETWEEN MICROSERVICES](https://www.youtube.com/watch?v=V-PqR0BxA8c)
	> [<img src="https://img.youtube.com/vi/V-PqR0BxA8c/0.jpg" width="200">](https://www.youtube.com/watch?v=V-PqR0BxA8c "ASYNCHRONOUS COMMUNICATION BETWEEN MICROSERVICES by Michelli Brito 32,123 views 25 minutes")
 * [Abordagem SAGA para Transações entre Microservices ft. Alvaro Valle](https://www.youtube.com/watch?v=MzzzGQFyTLs)
	> [<img src="https://img.youtube.com/vi/MzzzGQFyTLs/0.jpg" width="200">](https://www.youtube.com/watch?v=MzzzGQFyTLs "Abordagem SAGA para Transações entre Microservices ft. Alvaro Valle by Ramon Durães 2,710 views 37 minutes")
 * [MICROSERVICES ECOSYSTEM](https://www.youtube.com/watch?v=3B6nL9xeXhI)
	> [<img src="https://img.youtube.com/vi/3B6nL9xeXhI/0.jpg" width="200">](https://www.youtube.com/watch?v=3B6nL9xeXhI "MICROSERVICES ECOSYSTEM by Michelli Brito 15,146 views 18 minutes")
 * [Microservices: Vantagens x Desvantagens](https://www.youtube.com/watch?v=EjYoS6o6FvQ)
	> [<img src="https://img.youtube.com/vi/EjYoS6o6FvQ/0.jpg" width="200">](https://www.youtube.com/watch?v=EjYoS6o6FvQ "Microservices: Vantagens x Desvantagens by Michelli Brito 5,885 views 12 minutes, 7 seconds")
 * [Java e Microservices na Prática (Cloud Native &amp; DevOps) // Mão no Código by Red Hat](https://www.youtube.com/watch?v=ooLEpm4CXBY)
	> [<img src="https://img.youtube.com/vi/ooLEpm4CXBY/0.jpg" width="200">](https://www.youtube.com/watch?v=ooLEpm4CXBY "Java e Microservices na Prática (Cloud Native &amp; DevOps) // Mão no Código by Red Hat by Código Fonte TV 25,979 views 24 minutes")
 * [The HIDDEN Challenge of Microservices: UI Composition](https://www.youtube.com/watch?v=ILbjKR1FXoc)
	> [<img src="https://img.youtube.com/vi/ILbjKR1FXoc/0.jpg" width="200">](https://www.youtube.com/watch?v=ILbjKR1FXoc "The HIDDEN Challenge of Microservices: UI Composition by CodeOpinion 12,743 views 11 minutes, 11 seconds")
 * [Troubleshooting Kafka with 2000 Microservices | Event Driven](https://www.youtube.com/watch?v=dLpCFwR4Eac)
	> [<img src="https://img.youtube.com/vi/dLpCFwR4Eac/0.jpg" width="200">](https://www.youtube.com/watch?v=dLpCFwR4Eac "Troubleshooting Kafka with 2000 Microservices | Event Driven by CodeOpinion 16,256 views 12 minutes, 39 seconds")
 * [The HIDDEN Challenge of Microservices: UI Composition](https://www.youtube.com/watch?v=ILbjKR1FXoc)
	> [<img src="https://img.youtube.com/vi/ILbjKR1FXoc/0.jpg" width="200">](https://www.youtube.com/watch?v=ILbjKR1FXoc "The HIDDEN Challenge of Microservices: UI Composition by CodeOpinion 12,743 views 11 minutes, 11 seconds")
 * [What Are Microservices Really All About? (And When Not To Use It)](https://www.youtube.com/watch?v=lTAcCNbJ7KE)
	> [<img src="https://img.youtube.com/vi/lTAcCNbJ7KE/0.jpg" width="200">](https://www.youtube.com/watch?v=lTAcCNbJ7KE "What Are Microservices Really All About? (And When Not To Use It) by ByteByteGo 579,020 views 4 minutes, 45 seconds")
 * [Messaging in Microservices: Commands vs Events](https://www.youtube.com/watch?v=QVd3t5CqNjA)
	> [<img src="https://img.youtube.com/vi/QVd3t5CqNjA/0.jpg" width="200">](https://www.youtube.com/watch?v=QVd3t5CqNjA "Messaging in Microservices: Commands vs Events by Michelli Brito 7,627 views 17 minutes")
 * [Microservices Explained in 5 Minutes](https://www.youtube.com/watch?v=lL_j7ilk7rc)
	> [<img src="https://img.youtube.com/vi/lL_j7ilk7rc/0.jpg" width="200">](https://www.youtube.com/watch?v=lL_j7ilk7rc "Microservices Explained in 5 Minutes by 5 Minutes or Less 626,749 views 5 minutes, 17 seconds")
 * [Microservice Architecture and System Design with Python &amp; Kubernetes – Full Course](https://www.youtube.com/watch?v=hmkF77F9TLw)
	> [<img src="https://img.youtube.com/vi/hmkF77F9TLw/0.jpg" width="200">](https://www.youtube.com/watch?v=hmkF77F9TLw "Microservice Architecture and System Design with Python &amp; Kubernetes – Full Course by freeCodeCamp.org 363,305 views 5 hours, 4 minutes")
 * [What Are Microservices Really All About? (And When Not To Use It)](https://www.youtube.com/watch?v=lTAcCNbJ7KE)
	> [<img src="https://img.youtube.com/vi/lTAcCNbJ7KE/0.jpg" width="200">](https://www.youtube.com/watch?v=lTAcCNbJ7KE "What Are Microservices Really All About? (And When Not To Use It) by ByteByteGo 579,020 views 4 minutes, 45 seconds")
 * [O que são Microsserviços? (Microservices) #HipstersPontoTube](https://www.youtube.com/watch?v=jSnLOoGjQ80)
	> [<img src="https://img.youtube.com/vi/jSnLOoGjQ80/0.jpg" width="200">](https://www.youtube.com/watch?v=jSnLOoGjQ80 "O que são Microsserviços? (Microservices) #HipstersPontoTube by Alura 77,143 views 15 minutes")

Tecnologia: Microservices, docker e Kubernetes!!! https://www.youtube.com/watch?v=GaAB66dR9uI
Os primeiros passos na arquitetura de Microservices https://www.youtube.com/watch?v=Y6eP6SWCUDw
Microservices | André Faria | Papo Reto | T1E83 https://www.youtube.com/watch?v=ANPvIMUkYQ0
AWS re:Invent 2014 | (PFC304) Interprocess Comms in Cloud: Pros, Cons of Microservices Architectures https://www.youtube.com/watch?v=CriDUYtfrjs
TDC - Trilha Microservices | Alexandre Martins | Papo Reto | T6E34 https://www.youtube.com/watch?v=_dnMYFIXbUs
Microservices, o que usar? (Coreografia ou Orquestração) https://www.youtube.com/watch?v=QwDY0wla13Y

https://www.youtube.com/watch?v=QWOgkI4DuE8&list=PLwvrYc43l1MzeA2bBYQhCWr2gvWLs9A7S&index=12
https://www.youtube.com/watch?v=-luHIZJ6or0&list=PLTyWtrsGknYdZlO7LAZFEElWkEk59Y2ak
https://www.youtube.com/watch?v=LW-N44fZ1wk&list=PL8iIphQOyG-Dp037UnFG0x8aduelvZZWE

	> You are a senior solution architect in microservices who had read a lot articles and books related, generate an article with those topics above.

## References

1. https://amigoscode.com/p/microservices
2. https://aws.amazon.com/pt/blogs/architecture/data-caching-across-microservices-in-a-serverless-architecture/
3. https://docs.devprime.tech/how-to/asynchronous-microservices-communication/
4. https://docs.microsoft.com/en-us/dotnet/architecture/microservices/multi-container-microservice-net-applications/rabbitmq-event-bus-development-test-environment
5. https://dzone.com/articles/adopting-microservices-netflix
6. https://en.wikipedia.org/wiki/Microservices
7. https://github.com/AxonFramework/AxonFramework
8. https://github.com/JavatoDev-com/internet-banking-concept-microservices
9. https://github.com/javatodev/internet-banking-concept-microservices
10. https://github.com/rnott/microservice-archetype
11. https://javatodev.com/building-microservices-with-spring-boot-free-course-with-practical-project/
12. https://javatodev.com/building-microservices-with-spring-boot-free-course-with-practical-project/
13. https://javatodev.com/microservices-communication-with-spring-cloud-openfeign/
14. https://javatodev.com/microservices-exception-handling/
15. https://javatodev.com/microservices-service-registration-and-discovery-with-spring-cloud-netflix-eureka/
16. https://martinfowler.com/articles/microservices.html
17. https://medium.com/@vivekmadurai/agile-squad-for-building-microservices-54a05469ece9
18. https://microservices.io/
19. https://openliberty.io/guides/circuit-breaker.html
20. https://programmaticponderings.com/2022/04/14/monolith-to-microservices-refactoring-relational-databases%EF%BF%BC/
21. https://swagger.io/blog/api-strategy/microservices-apis-and-swagger/
22. https://www.coursera.org/learn/applications-development-microservices-serverless-openshift
23. https://www.infoq.com/building-microservices-in-java-article-series/
24. https://www.nginx.com/blog/event-driven-data-management-microservices/
25. https://www.slideshare.net/INPAY/autonomous-microservices-for-a-financial-system
26. https://medium.com/@marcelomg21/event-sourcing-es-em-uma-arquitetura-de-microsservi%C3%A7os-852f6ce04595 
27. https://medium.com/design-microservices-architecture-with-patterns/microservices-architecture-for-enterprise-large-scaled-application-825436c9a78a
28. https://learn.microsoft.com/en-us/dotnet/architecture/cloud-native/introduce-eshoponcontainers-reference-app
29. https://github.com/spring-petclinic/spring-petclinic-microservices
30. https://medium.com/design-microservices-architecture-with-patterns/microservices-architecture-for-enterprise-large-scaled-application-825436c9a78a
31. 
