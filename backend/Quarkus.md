# Quarkus 

Quarkus is a Kubernetes-native Java framework tailored for GraalVM and OpenJDK HotSpot, optimized for fast startup time and low memory footprint. It's designed to help Java developers build highly efficient, lightweight, and scalable microservices and cloud-native applications.

## Quarkus vs spring boot

Quarkus and Spring Boot are both popular frameworks for building Java applications, but they have different focuses and characteristics:

1. **Performance:** Quarkus is optimized for fast startup time and low memory usage, making it well-suited for serverless computing and microservices where fast boot time and minimal resource consumption are essential. Spring Boot, on the other hand, emphasizes developer productivity and convention-over-configuration, which may result in slightly slower startup times and higher memory usage compared to Quarkus.

2. **Ecosystem:** Spring Boot has a mature and extensive ecosystem with a wide range of libraries, tools, and community support. Quarkus, being relatively newer, has a growing ecosystem, but it may not have as many third-party integrations and resources as Spring Boot.

3. **Programming Model:** Spring Boot follows the Spring framework's programming model, which emphasizes dependency injection, aspect-oriented programming, and convention over configuration. Quarkus leverages standards like CDI (Contexts and Dependency Injection) and JAX-RS (Java API for RESTful Web Services) but provides a more opinionated and streamlined development experience.

4. **Compatibility:** Spring Boot has broader compatibility with existing Spring ecosystem components and libraries, making it a natural choice for developers already familiar with the Spring framework. Quarkus, while compatible with many Java standards and libraries, may require adjustments for certain existing Spring-based applications or libraries.

In summary, if you prioritize performance, especially in cloud-native environments, and prefer a lightweight, Kubernetes-native approach, Quarkus might be a better fit. However, if you value a mature ecosystem, extensive community support, and developer productivity, Spring Boot remains a strong choice. Ultimately, the decision depends on your specific project requirements, familiarity with the frameworks, and performance considerations.

### Is Spring boot version > 3.2 having similar capabilities in terms of performance when compared to Quarkus ?

Spring Boot and Quarkus are both known for their performance, but they target different use cases:

* **Spring Boot:** Offers a wide range of features and flexibility, making it a good choice for complex enterprise applications. However, this comprehensiveness can introduce some overhead.

* **Quarkus:** Focuses on speed and efficiency, making it ideal for microservices and applications deployed in resource-constrained environments like serverless or containerized deployments.

Recent versions of Spring Boot (including versions above 3.2) have made significant improvements in performance, but Quarkus generally has the edge in startup time, memory usage, and overall execution speed.

Here's a breakdown of key performance factors:

* **Startup time:** Quarkus excels due to its ability to generate native executables that start up much faster than Spring Boot's JVM-based approach.

* **Memory usage:** Quarkus tends to have a smaller footprint than Spring Boot because it uses extensions selectively and avoids unnecessary features.

* **Requests per second:**  In benchmark tests, Quarkus typically handles more requests per second than Spring Boot under load.

**Choosing the right framework depends on your priorities:**

* If you need a feature-rich framework for a complex application and performance is not a critical concern, Spring Boot might be sufficient.

* If you have strict performance requirements, resource limitations, or plan to deploy in a cloud-native environment, Quarkus is likely a better choice.

###  Spring Boot and Quarkus
 This video is about comparing two Java frameworks, Spring Boot and Quarkus [1]. The video discusses the use cases, performance, developer experience, and ecosystem of each framework to help viewers decide which one to use for their project [1].

Spring Boot is an opinionated framework for creating standalone and production-ready Spring applications [1]. It is popular because it gets rid of a lot of boilerplate code and comes with a vast ecosystem of libraries and tools [1]. Spring Boot is generally used to speed up the development process  for Java applications [2].

Quarkus is a newer Java framework designed for delivering container-optimized and cloud-native applications [1]. It has a faster boot time and better resource utilization than Spring Boot [1]. It combines both imperative and reactive programming models and has a lot of tools to improve developer productivity [1]. Quarkus is ideal for serverless or containerized applications where minimal resource consumption is a priority [7].

Here's a table summarizing the key differences between Spring Boot and Quarkus:

| Feature | Spring Boot | Quarkus |
|---|---|---|
| Use case | Speed up development of Java applications | Develop cloud-native applications |
| Performance | Slower boot time, higher resource consumption | Faster boot time, lower resource consumption |
| Developer experience | Large community, extensive documentation | Live coding, continual testing, developer UI |
| Ecosystem | Larger, more mature | Smaller, but growing rapidly |

Ultimately, the best framework for your project depends on your specific needs [7]. If you are looking for a mature framework with a vast ecosystem, then Spring Boot is a good choice [7]. If you are looking for a framework that is optimized for cloud-native development and has a faster boot time, then Quarkus is a good choice [7].


## Microservices vs modular monolith 

Microservices and modular monoliths are two architectural approaches for building software systems, each with its own advantages and considerations:

1. **Microservices:**
   - **Decomposition:** Microservices architecture decomposes an application into smaller, independently deployable services, each responsible for a specific business function.
   - **Scalability:** Allows individual services to be scaled independently based on demand, providing flexibility and resource optimization.
   - **Technology Diversity:** Each microservice can be built using different technologies, languages, and frameworks, allowing teams to choose the best tool for each task.
   - **Isolation:** Services are isolated from each other, reducing the risk of cascading failures and allowing for easier fault isolation and recovery.
   - **Complexity:** Introduces additional complexity in terms of distributed system communication, service discovery, and data consistency, requiring robust infrastructure and DevOps practices.

2. **Modular Monolith:**
   - **Modularity:** A modular monolith organizes codebase into loosely coupled modules or components, often with clear boundaries and interfaces, but still deployed as a single unit.
   - **Simplicity:** Easier to develop, test, and deploy compared to microservices due to reduced complexity in distributed system communication and deployment.
   - **Performance:** Typically offers better performance and lower latency compared to microservices since there is no inter-service communication overhead.
   - **Evolvability:** Easier to refactor and evolve since all code resides within the same codebase, making it simpler to make changes across modules.
   - **Scaling:** Requires scaling the entire monolith, which may lead to inefficiencies if certain modules have significantly different resource requirements.

Choosing between microservices and modular monoliths depends on various factors such as the size and complexity of the project, team expertise, scalability requirements, and the need for agility and flexibility. Microservices are often favored for large, complex systems with high scalability and technology diversity requirements, while modular monoliths can be a pragmatic choice for smaller teams or projects with simpler requirements and where the overhead of managing microservices is not justified. Additionally, some organizations may adopt a hybrid approach, starting with a monolith and gradually transitioning towards microservices as the system evolves.

## Video
 * [Java Web App with Quarkus and JPAStreamer – Tutorial](https://www.youtube.com/watch?v=KZnQ5R8Kd4I)
   - Quarkus is a development tool for deploying your Java apps to the cloud. 
   - And in this course, Julia shows you how to use it with JPAStreamer to build a REST app. 
   - She covers connecting to a database, defining REST endpoints, continuous testing, & more.
	> [<img src="https://img.youtube.com/vi/KZnQ5R8Kd4I/0.jpg" width="200">](https://www.youtube.com/watch?v=KZnQ5R8Kd4I "In this course, you'll learn how to use Quarkus and JPAStreamer to build a REST web application. by freeCodeCamp.io 63,761 views 1 hour, 2 minutes, 53 seconds")

 * [Quarkus Insights #63: Quarkus for Spring Developers](https://www.youtube.com/watch?v=RvO8MUfc0kA)
	> [<img src="https://img.youtube.com/vi/RvO8MUfc0kA/0.jpg" width="200">](https://www.youtube.com/watch?v=RvO8MUfc0kA "Eric Deandrea @edeandrea joins us to discuss his new book Quarkus for Spring Developers by Quarkusio 637 views 58 minutes, 51 seconds")

 * [Spring Boot versus Quarkus](https://www.youtube.com/watch?v=mJJpZ70q9M0)
	> [<img src="https://img.youtube.com/vi/mJJpZ70q9M0/0.jpg" width="200">](https://www.youtube.com/watch?v=mJJpZ70q9M0 "Is it possible to write slim, light, fast boot time, low-memory footprint Java applications? Using a Java framework provides reusable pre-written code templates that help speed up development and deployment - but which framework is best? In this video, Cedric Clyburn compares two major frameworks in use today: Spring Boot and Quarkus. by IBM Technology 23K views 5 minutes, 30 seconds")

## References

1. https://code.quarkus.io/
2. https://quarkus.io/books/
3. https://www.youtube.com/@freecodecamp/search?query=quarkus
4. https://www.youtube.com/results?search_query=Eric+Deandrea
5. https://developers.redhat.com/e-books/quarkus-spring-developers
6. https://quarkus.io/blog/decathlon-user-story/
7. 

