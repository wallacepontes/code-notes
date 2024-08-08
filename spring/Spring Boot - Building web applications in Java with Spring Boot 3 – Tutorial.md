# Building web applications in Java with Spring Boot 3 – Tutorial

Learn how to build web applications in Java with Spring Boot 3. You'll learn about Spring's fundamentals by creating a REST API that communicates with a database and is supported by a comprehensive suite of tests. By the end of this course you will have learned what you need to start building your own web applications with Spring Boot 3.

## Spring : Outcomes
Spring is a popular open-source framework for building enterprise Java applications. It provides comprehensive infrastructure support for developing Java applications, making it easier to create robust and scalable applications.

Here's a breakdown of the topics you mentioned:

1. **What is Spring and What It Can Do**:
   - Spring provides a wide range of features and functionalities, including:
     - Dependency Injection: Spring's core feature is dependency injection, which allows for loose coupling between components in your application.
     - Aspect-Oriented Programming (AOP): AOP allows you to modularize cross-cutting concerns like logging, security, and transaction management.
     - Spring MVC: A powerful web framework for building web applications.
     - Spring Boot: A tool built on top of the Spring framework, providing a streamlined way to create stand-alone, production-grade Spring-based applications.
     - Spring Security: A powerful and customizable authentication and access control framework for securing Spring-based applications.
     - Spring Data: Provides a consistent and easy way to work with various data access technologies, including relational databases, NoSQL databases, and more.

2. **Building a Web Application with Spring Boot**:
   - Spring Boot simplifies the process of creating Spring-based applications by providing defaults and auto-configuration. Here's how to build a web application with Spring Boot:
     - Start by creating a new Spring Boot project using either Spring Initializr or Spring Boot CLI.
     - Define your application's dependencies, such as Spring Web for building web applications.
     - Write your application code, including controllers, services, and repositories.
     - Run your Spring Boot application either from your IDE or using the command-line interface.
     - Access your web application through the specified endpoints.

3. **Testing a Spring Boot Application**:
   - Spring Boot applications are typically tested using unit tests and integration tests.
   - For unit testing, you can use frameworks like JUnit and Mockito to test individual components in isolation.
   - For integration testing, Spring Boot provides utilities for testing controllers and other components in the context of a running Spring application.
   - You can also use tools like Spring Test for testing Spring applications and Spring Boot Test for testing Spring Boot applications.

4. **Using Spring Data to Interact with a Database**:
   - Spring Data provides a powerful abstraction layer on top of various data access technologies, making it easier to interact with databases in your Spring applications.
   - To use Spring Data, you typically define repository interfaces that extend Spring Data interfaces like JpaRepository (for JPA) or MongoRepository (for MongoDB).
   - Spring Data automatically generates implementations for these repositories at runtime, reducing the amount of boilerplate code you need to write.
   - You can then use these repositories to perform CRUD operations and execute custom queries against your database.

By mastering these concepts, you'll be well-equipped to build robust web applications using Spring and Spring Boot, test them effectively, and interact with databases using Spring Data.


## What is Spring?

1. [Spring Academy](https://spring.academy/)
Learn from the experts. Build like a boss.
2. [Why Spring?](https://spring.io/why-spring)
3. [Guides](https://spring.io/guides)

## What Spring can do?

Spring is a comprehensive framework that offers a wide range of features and capabilities to developers. Here's a breakdown of what Spring can do:

1. **Microservices**: Quickly deliver production‑grade features with independently evolvable microservices.
   - Spring provides tools and frameworks that support the development of microservices architectures. With Spring Boot, developers can quickly create independently deployable microservices that can be scaled and managed efficiently.

2. **Reactive**: Spring's asynchronous, nonblocking architecture means you can get more from your computing resources.
   - Spring embraces reactive programming, allowing developers to build applications that are asynchronous and non-blocking. This architecture enables applications to handle more concurrent connections and efficiently utilize computing resources.

3. **Cloud**: Your code, any cloud—we’ve got you covered. Connect and scale your services, whatever your platform.
   - Spring offers robust support for cloud-native development, making it easy to deploy Spring applications to various cloud platforms. Whether you're using AWS, Azure, Google Cloud, or others, Spring provides libraries and integrations to simplify cloud development and deployment.

4. **Web Apps**: Frameworks for fast, secure, and responsive web applications connected to any data store.
   - Spring provides frameworks like Spring MVC and Spring WebFlux for building fast, secure, and responsive web applications. These frameworks support various web technologies and integrations, allowing developers to connect their applications to any data store and deliver rich user experiences.

5. **Serverless**: The ultimate flexibility. Scale up on demand and scale to zero when there’s no demand.
   - With Spring, developers can build serverless applications that scale up or down based on demand. Spring Cloud Function and Spring Cloud Stream provide abstractions for building serverless functions and processing streaming data, enabling ultimate flexibility in resource utilization.

6. **Event Driven**: Integrate with your enterprise. React to business events. Act on your streaming data in realtime.
   - Spring facilitates event-driven architectures, allowing applications to integrate with enterprise systems, react to business events, and process streaming data in real-time. Spring Cloud Stream and Spring Integration are examples of tools that support event-driven development in Spring.

7. **Batch**: Automated tasks. Offline processing of data at a time to suit you.
   - Spring Batch is a framework for building batch processing applications. It automates the execution of repetitive tasks, such as data processing and reporting, at scheduled intervals or on-demand. Spring Batch simplifies the development of batch applications by providing components for job scheduling, transaction management, and error handling.

Overall, Spring provides developers with a powerful and versatile toolkit for building a wide range of applications, from microservices to web apps, serverless functions, and batch processing systems. Its modular architecture, extensive ecosystem, and strong community support make it a popular choice for Java developers.

### [Projects](https://spring.io/projects)
From configuration to security, web apps to big data—whatever the infrastructure needs of your application may be, there is a Spring Project to help you build it. Start small and use just what you need—Spring is modular by design.

The Spring ecosystem consists of various projects tailored to different aspects of application development. Here's a brief overview of some key projects:

1. **Spring Boot**: *Takes an opinionated view of building Spring applications and gets you up and running as quickly as possible.* 
    * Simplifies the setup and configuration of Spring applications by providing defaults and conventions, enabling rapid development and deployment. 

2. **Spring Framework**: *Provides core support for dependency injection, transaction management, web apps, data access, messaging, and more.*
    * Offers core features for dependency injection, transaction management, web applications, data access, messaging, and more, serving as the foundation for most Spring-based projects.

3. **Spring Data**: *Provides a consistent approach to data access – relational, non-relational, map-reduce, and beyond.*
    * Provides a unified approach to data access, supporting various data storage technologies including relational databases, NoSQL databases, and more.

4. **Spring Cloud**: *Provides a set of tools for common patterns in distributed systems. Useful for building and deploying microservices.* 
    * Offers tools and libraries for building and deploying distributed systems, particularly useful for microservices architectures.

5. **Spring Cloud Data Flow**: *Provides an orchestration service for composable data microservice applications on modern runtimes.* 
    * Facilitates the development and orchestration of data microservice applications on modern runtimes.

6. **Spring Security**: *Protects your application with comprehensive and extensible authentication and authorization support.* 
    * Provides comprehensive authentication and authorization support to secure Spring-based applications.

7. **Spring Authorization Server**: *Provides a secure, light-weight, and customizable foundation for building OpenID Connect 1.0 Identity Providers and OAuth2 Authorization Server products.* 
    * Offers a lightweight and customizable foundation for building OAuth2 Authorization Server and OpenID Connect Identity Providers.

8. **Spring for GraphQL**: *Provides support for Spring applications built on GraphQL Java.* 
    * Provides support for integrating GraphQL into Spring applications.

9. **Spring Session**: *Provides an API and implementations for managing a user’s session information.* 
    * Offers APIs and implementations for managing user session information in Spring-based applications.

10. **Spring Integration**: *Supports the well-known Enterprise Integration Patterns through lightweight messaging and declarative adapters.* 
    * Supports Enterprise Integration Patterns through lightweight messaging and declarative adapters.

11. **Spring HATEOAS**: *Simplifies creating REST representations that follow the HATEOAS principle.* 
    * Simplifies the creation of REST representations following the HATEOAS principle.

12. **Spring Modulith**: *Allows developers to build well-structured Spring Boot applications and guides developers in finding and working with application modules driven by the domain.*

13. **Spring REST Docs**: *Lets you document RESTful services by combining hand-written documentation with auto-generated snippets produced with Spring MVC Test or REST Assured.*
    * Allows documentation of RESTful services by combining hand-written documentation with auto-generated snippets.

14. **Spring AI**: *It is an application framework for AI engineering.* 
    * An application framework for AI engineering.

15. **Spring Batch**: *Simplifies and optimizes the processing of high-volume batch operations.*

16. **Spring CLI**: *A CLI focused on developer productivity*

17. **Spring AMQP**: *Applies Spring concepts to the development of AMQP-based messaging solutions.*

18. **Spring Flo**: *Provides a JavaScript library that offers a basic embeddable HTML5 visual builder for pipelines and simple graphs.*
    * Provides an embeddable HTML5 visual builder for pipelines and simple graphs.

19. **Spring for Apache Kafka**: *Provides Familiar Spring Abstractions for Apache Kafka.*
    * Offers familiar Spring abstractions for Apache Kafka.

20. **Spring LDAP**: *Simplifies the development of LDAP-based applications using Spring's template-based approach.*

21. **Spring for Apache Pulsar**: *Provides familiar Spring abstractions for Apache Pulsar.*

22. **Spring Shell**: *Makes writing and testing RESTful applications easier with CLI-based resource discovery and interaction.*
    * Facilitates the development and testing of RESTful applications with CLI-based resource discovery and interaction.

23. **Spring Statemachine**: *Provides a framework for application developers to use state machine concepts with Spring applications.* 
    * Offers a framework for utilizing state machine concepts in Spring applications.

24. **Spring Web Flow**: *Supports building web applications that feature controlled navigation, such as checking in for a flight or applying for a loan.*
    * Supports building web applications with controlled navigation, suitable for scenarios like flight check-ins or loan applications.

25. **Spring Web Services**: *Facilitates the development of contract-first SOAP web services.*

These projects demonstrate the breadth and depth of the Spring ecosystem, catering to various infrastructure needs and providing solutions for modern application development challenges. Developers can leverage these projects based on their specific requirements to build robust and scalable applications efficiently.

## What is the difference between the new JDBC Client in Spring Boot 3.2 and Spring Data JDBC?

The new JDBC Client in Spring Boot 3.2 and Spring Data JDBC are both tools for working with databases in Spring applications, but they target different levels of complexity and offer distinct functionalities:

**JDBC Client**

* **Lower-level API:** It provides a more fundamental way to interact with databases. You write more code to perform operations like creating connections, executing queries, and processing results.
* **Manual Mapping:** You're responsible for converting data between database rows and Java objects. 
* **Fine-grained Control:**  Offers greater control over database interactions, making it useful for specific needs or complex operations.

**Spring Data JDBC**

* **Higher-level Abstraction:** Built on top of the JDBC API, it simplifies data access. You write less code and focus more on what data you need.
* **Automatic Mapping:** Handles mapping database rows to objects and vice versa automatically, reducing boilerplate code. 
* **Repository Pattern and CRUD:** Provides pre-built functionality for common database operations like Create, Read, Update, and Delete (CRUD), following the repository design pattern. This promotes cleaner and more concise code.

**In summary:**

* Use the JDBC Client when you need low-level control and customization for specific database interactions.
* Use Spring Data JDBC for a more productive and declarative approach to database access, especially for common CRUD operations.

### [Video](https://www.youtube.com/watch?v=qLDrfebeXS0)
According to the video, Spring Data JDBC is a better option for those new to Spring as it simplifies data access compared to JDBC.

The speaker, Dan Vega, starts the video by explaining different ways to ask questions about Spring topics including emailing him, leaving comments on YouTube videos, reaching him on Twitter, and attending Spring office hours.

Then he dives into the main topic. He says that Spring Data JDBC is a better option for those new to Spring as it simplifies data access compared to JDBC.

Next, he demonstrates the code for a simple blog application built with Spring Boot 3.2. Here are the steps to create a Spring Boot application with JDBC:

1. Create a new project using Spring Initializr.
2. Chooseが必要です (hapiyōsu - required)  Spring Web and JDBC API as dependencies.
3. Configure the database connection details in the application.properties file.
4. Create a schema.sql file to set up the database schema.
5. Create a Post class to represent a blog post record.
6. Use JdbcTemplate to query the database for posts.

Spring Data JDBC simplifies the code further by using a repository abstraction. Here are the steps to use Spring Data JDBC:

1. Include the Spring Boot Data JDBC dependency in the pom.xml file.
2. Create a PostRepository interface that extends JpaRepository.
3. Use the repository to save and find posts.

Overall, Spring Data JDBC provides a more convenient way to interact with databases in Spring applications compared to plain JDBC. It offers a higher level of abstraction that reduces boilerplate code.



## Video 
## ⌨️ (0:02:00) Module 1: Course Introduction

## ⌨️ (0:19:25) Module 2: Create your project

## ⌨️ (0:49:44) Module 3: REST API

## ⌨️ (1:33:12) Module 4: Working with Databases

## ⌨️ (2:24:12) Module 5: Rest Clients

## ⌨️ (2:44:55) Module 6: Testing

## ⌨️ (3:27:50) Conclusion 

## References

1. [GitHub for the course](https://github.com/danvega/fcc-spring-boot-3.git)
2. [Video](https://www.youtube.com/watch?v=31KTdfRH6nY&t=2316s)
