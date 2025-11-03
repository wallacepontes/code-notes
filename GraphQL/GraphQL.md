# GraphQL

## Table of Contents
- [GraphQL](#graphql)
	- [Table of Contents](#table-of-contents)
	- [What Is GraphQL? REST vs. GraphQL](#what-is-graphql-rest-vs-graphql)
		- [When should we use it?](#when-should-we-use-it)
		- [How is GraphQL the same as REST? How are they different?](#how-is-graphql-the-same-as-rest-how-are-they-different)
		- [So, should we use GraphQL?](#so-should-we-use-graphql)
		- [It's all about tradeoffs](#its-all-about-tradeoffs)
		- [Next, let's discuss some drawbacks of GraphQL](#next-lets-discuss-some-drawbacks-of-graphql)
		- [The beauty of REST is that we don't need special libraries to consume someone else's API](#the-beauty-of-rest-is-that-we-dont-need-special-libraries-to-consume-someone-elses-api)
		- [about these GraphQL requirements, what specific libraries they are](#about-these-graphql-requirements-what-specific-libraries-they-are)
		- [So, GraphQL requires heavier tooling support, both on the client and server sides. This requires a sizable upfront investment.](#so-graphql-requires-heavier-tooling-support-both-on-the-client-and-server-sides-this-requires-a-sizable-upfront-investment)
		- [Another criticism of GraphQL is that is more difficult to cache.](#another-criticism-of-graphql-is-that-is-more-difficult-to-cache)
		- [The final concern we have with GraphQL is that while GraphQL allows clients to query for just the data they need, this also poses a great danger.](#the-final-concern-we-have-with-graphql-is-that-while-graphql-allows-clients-to-query-for-just-the-data-they-need-this-also-poses-a-great-danger)
		- [Imagine this example where a mobile application shipped a new feature that causes an unexpected table scan of a critical database table of a backend service. This could bring the database down as soon as the new application goes live.](#imagine-this-example-where-a-mobile-application-shipped-a-new-feature-that-causes-an-unexpected-table-scan-of-a-critical-database-table-of-a-backend-service-this-could-bring-the-database-down-as-soon-as-the-new-application-goes-live)
		- [The cost to safeguard risks like this must be factored in when considering GraphQL.](#the-cost-to-safeguard-risks-like-this-must-be-factored-in-when-considering-graphql)
		- [How about mutation and subscription](#how-about-mutation-and-subscription)
	- [Videos](#videos)
	- [References](#references)

## What Is GraphQL? REST vs. GraphQL

GraphQL is a query language for your API, and a runtime for executing those queries against your data. It was developed by Facebook and is often used as an alternative to REST (Representational State Transfer) for building web services. 

REST is an architectural style for building web services, and it is based on the idea of using HTTP methods (such as GET, POST, PUT, and DELETE) to manipulate resources. RESTful web services typically return data in a predefined format, such as JSON or XML, and the client is responsible for understanding the structure of the data.

GraphQL, on the other hand, allows the client to specify exactly what data it needs, and the server will return only that data. This allows for more efficient data retrieval, as the client can request only the data it needs, rather than receiving a predefined set of data. Additionally, GraphQL allows for more flexibility in the structure of the data returned, as the client can ask for different fields on the same type of resource.

In summary, REST is an architectural style for building web services, while GraphQL is a query language for your API that allows for more efficient and flexible data retrieval.

### When should we use it?

There are a few situations where using GraphQL may be beneficial:

1. When you have a client-heavy application: GraphQL allows the client to specify exactly what data it needs, which can lead to more efficient data retrieval and reduce the amount of data sent over the network.

2. When you have multiple clients with different needs: With GraphQL, each client can specify the specific data it needs, rather than having to build a separate endpoint for each client's needs.

3. When you have a rapidly evolving schema: GraphQL allows the client to request new fields without having to update the server-side code. This allows for a more flexible schema that can evolve over time.

4. When you have a complex data graph: GraphQL allows you to query multiple resources in a single request, which can be useful when working with complex data graphs.

5. When you have a need to access data from multiple sources: GraphQL allows you to connect to multiple data sources and query them in a single request.

It's worth noting that GraphQL is not a replacement of REST but it's an alternative, so it's important to evaluate the requirements of your application and decide which approach is the best fit.


### How is GraphQL the same as REST? How are they different?

GraphQL and REST are both approaches to building web services, but they have some key differences.

Similarities:

- Both GraphQL and REST are based on the idea of sending requests and receiving responses.
- Both GraphQL and REST can return data in a structured format, such as JSON.
- Both GraphQL and REST can be used to create web services that can be consumed by various types of clients, including web browsers, mobile apps, and other servers.

Differences:

- In REST, the client typically retrieves data from a fixed set of endpoints, and the structure of the data is predefined. In GraphQL, the client can specify exactly what data it needs, and the server will return only that data.
- RESTful web services use HTTP methods to manipulate resources, while GraphQL uses a single endpoint and a query language to retrieve data.
- GraphQL allows for more flexibility in the structure of the data returned, as the client can ask for different fields on the same type of resource, while RESTful web services typically return a fixed set of data.
- In RESTful web services, the client is responsible for understanding the structure of the data returned, while in GraphQL, the client can ask for specific fields and the server will handle the data structure.
- GraphQL allows for multiple queries or mutations to be executed in a single request, while RESTful web services typically require one request per action.

In summary, REST is an architectural style for building web services, which focuses on the idea of using HTTP methods to manipulate resources. GraphQL, on the other hand, is a query language for your API, which allows for more efficient and flexible data retrieval.

### So, should we use GraphQL?

Whether or not to use GraphQL depends on the specific requirements of your application.

GraphQL can be a good choice in situations where:

- You have a client-heavy application and need to optimize data retrieval
- You have multiple clients with different needs and want to reduce the number of endpoints required
- You have a rapidly evolving schema and need to add new fields without updating server-side code
- You have a complex data graph and need to query multiple resources in a single request
- You have a need to access data from multiple sources

However, it's worth noting that GraphQL does have some limitations. It can be more complex to implement, especially when working with complex data graphs or when trying to optimize performance. Additionally, it may not be the best choice for simple, read-only applications or for applications where security is a top priority.

It's important to evaluate the requirements of your application and decide which approach is the best fit. GraphQL is a powerful tool, but it may not always be the right solution for every situation. It's always good to weigh the pros and cons of each approach before making a decision.


### It's all about tradeoffs

Yes, that's correct. Choosing between GraphQL and REST is a trade-off between different factors such as flexibility, performance, ease of implementation, and security.

GraphQL allows for more flexibility in the structure of the data returned and can optimize data retrieval in a client-heavy application. However, it may be more complex to implement, especially when working with complex data graphs or when trying to optimize performance.

REST, on the other hand, is a simpler architectural style for building web services, which focuses on the idea of using HTTP methods to manipulate resources. It may be more straightforward to implement and is suitable for simple, read-only applications or for applications where security is a top priority.

It's important to evaluate the requirements of your application and decide which approach is the best fit. The trade-offs between these two approaches will vary depending on the specific needs of the application.

### Next, let's discuss some drawbacks of GraphQL

While GraphQL has many benefits, it also has some drawbacks that should be considered:

1. Learning Curve: GraphQL has a steeper learning curve than REST. It requires a good understanding of the query language and the underlying data model to effectively use it.

2. Performance: GraphQL can be less performant than REST in certain scenarios, especially when dealing with large or complex data sets. Because GraphQL allows clients to retrieve any data they want, it can be more difficult to optimize the queries for performance.

3. Security: GraphQL can be more difficult to secure than REST, as it allows clients to query any data they want. This can make it harder to prevent unauthorized access to sensitive data.

4. Caching: Caching can be more challenging in GraphQL as it allows for more flexibility in the structure of the data returned. It can be difficult to cache the responses as the structure of the data returned is based on the client's request.

5. Error handling: Error handling can be more complex in GraphQL as it allows for more flexibility in the structure of the data returned. It can be difficult to handle errors that occur on the server side, and it may be harder to identify the exact cause of the error.

6. Versioning: Versioning can be harder in GraphQL as it allows for more flexibility in the structure of the data returned. It can be difficult to handle changes in the schema without breaking clients that rely on the old schema.

It's worth noting that GraphQL is relatively new and these drawbacks may be mitigated by future developments. However, It's always good to be aware of these drawbacks when considering GraphQL for your application.

### The beauty of REST is that we don't need special libraries to consume someone else's API

Yes, that's one of the benefits of REST. Because REST is built on top of standard HTTP, it doesn't require any special libraries or tooling to consume an API.

Clients can interact with a RESTful API using standard HTTP methods (such as GET, POST, PUT, and DELETE) and can parse the responses using standard libraries. This means that clients can be built using any programming language that can send HTTP requests and parse JSON or XML responses.

This also means that RESTful API's are easily understood and consumed by developers. This is because developers are already familiar with how to use HTTP methods and there is no need to learn a new query language or data model.

In contrast, GraphQL requires a specific query language and specific libraries to interact with its API. While this can provide more flexibility and control for the client, it also requires developers to learn a new set of tools and concepts.

### about these GraphQL requirements, what specific libraries they are

When working with GraphQL, there are a few key requirements that developers need to be aware of:

1. GraphQL query language: GraphQL uses a specific query language that allows clients to specify exactly which fields they want to retrieve from the server. This query language is based on a schema that defines the available fields and types, and it's used to validate and execute queries.

2. GraphQL server: To handle GraphQL queries, a server-side component is required. This component is responsible for parsing and validating the queries, executing them against the underlying data source, and returning the results. There are several popular libraries and frameworks for building GraphQL servers, such as Apollo Server, GraphQL-yoga, and Prisma.

3. GraphQL client: To interact with a GraphQL API, a client-side component is required. This component is responsible for sending queries to the server, parsing the results, and handling errors. There are several popular libraries and frameworks for building GraphQL clients, such as Apollo Client, Relay, and urql.

4. Schema definition: In GraphQL, the schema is a blueprint that describes the types of data that can be queried and the relationships between them. It's essential that the schema is well-defined, to guarantee that the client can only retrieve the data they are authorized to.

By using a specific query language and specific libraries, GraphQL can provide more flexibility and control for the client. However, it also requires developers to learn a new set of tools and concepts.

### So, GraphQL requires heavier tooling support, both on the client and server sides. This requires a sizable upfront investment.

Yes, that's correct. Because GraphQL uses a specific query language and requires specific libraries and tooling on both the client and server sides, it does require a larger upfront investment.

Developers need to learn the GraphQL query language and the underlying data model, as well as the specific libraries and frameworks used to build the server and client. This can be more time-consuming and complex than working with a RESTful API, which can be built using standard libraries and HTTP methods.

Additionally, there is also a need for additional operational support to maintain the servers and clients, and to ensure that the schema is well-defined and the security is in place. This is something that needs to be taken into consideration when deciding to use GraphQL.

On the other hand, the benefits of GraphQL such as flexibility, performance, and developer experience, can outweigh the additional development and operational costs, especially for certain types of applications or use cases where data requirements are complex or constantly changing.

### Another criticism of GraphQL is that is more difficult to cache.

Yes, caching can be more difficult with GraphQL than with REST. In REST, resources are identified by URIs and cached based on these URIs. Because the URIs represent specific resources, it's easy to determine when the cached data is stale and needs to be refreshed.

In GraphQL, on the other hand, the same query can return different data depending on the fields that are requested. This means that caching the entire response for a query may not be effective, since the data for some fields may be stale while the data for other fields is still up to date.

Additionally, GraphQL allows the client to request only the fields it needs, which can lead to more granular and fine-grained caching. However, this approach also requires more complexity on the server side to handle caching of individual fields, and also on the client side to manage the cache.

Therefore, while caching can be more difficult in GraphQL, it is not impossible. There are various techniques and libraries that can help to mitigate these challenges, such as caching on the server side, and use of a caching proxy layer.

### The final concern we have with GraphQL is that while GraphQL allows clients to query for just the data they need, this also poses a great danger.

Yes, that's a valid concern. Because GraphQL allows clients to request only the fields they need, it also allows them to request data that they may not be authorized to access. This can be a security concern, as it could potentially allow unauthorized users to access sensitive data.

To mitigate this risk, it's essential to have proper authentication and authorization in place, and to validate requests on the server side before executing them. The GraphQL schema should be well-defined, and the server should be configured to only return the data that the client is authorized to access. 

Additionally, it's also important to validate queries on the client-side to make sure that the client does not request data that they are not authorized to access. This can be achieved by using libraries like Apollo Client or Relay, which provide built-in support for this kind of validation.

It's also important to note that any backend system is vulnerable to security issues if not handled properly, GraphQL is no exception. However, with proper security measures in place, GraphQL can be as secure as REST APIs.

### Imagine this example where a mobile application shipped a new feature that causes an unexpected table scan of a critical database table of a backend service. This could bring the database down as soon as the new application goes live.

Yes, that's a potential issue that could occur if proper testing and monitoring are not in place. A mobile application that causes an unexpected table scan of a critical database table could cause a significant performance issue for the backend service. If the table scan is not optimized, it could put a heavy load on the database and cause it to become slow or even crash.

To avoid such situations, it's essential to have proper testing and monitoring in place before releasing a new feature. The new feature should be thoroughly tested in a staging environment to ensure that it performs well and does not cause any unexpected issues. This would include load testing to ensure that the feature can handle the expected traffic and stress testing to ensure that it can handle unexpected traffic spikes.

Additionally, it's important to monitor the backend service and the database after the new feature is released, to detect and troubleshoot any issues that may arise. This can be done using monitoring tools such as Prometheus, Grafana, or New Relic, which can provide real-time visibility into the performance of the system.

Another approach to prevent this would be to use a feature flag system, in which the feature is introduced slowly and gradually, starting with a small percentage of users and then slowly ramping up over time. This way you would be able to detect any issues early on and fix them before they become critical.

It's also important to have a rollback plan in case of a critical issue, such as a database crash, so that the new feature can be quickly rolled back if necessary.

### The cost to safeguard risks like this must be factored in when considering GraphQL.

Yes, it's important to factor in the cost of safeguarding risks when considering GraphQL. As with any technology, there are costs associated with implementing and maintaining it. In the case of GraphQL, these costs include the upfront investment in learning and implementing the technology, as well as the ongoing costs of testing, monitoring, and maintaining the system.

However, it's also important to consider the benefits that GraphQL can provide, such as improved flexibility and performance for the client, and the ability to handle a wide variety of client needs with a single endpoint. Additionally, GraphQL allows the client to request only the data it needs, which can result in reduced network traffic and faster response times.

It's also worth noting that many of the costs associated with implementing and maintaining GraphQL can be mitigated by using existing libraries and tools, such as Apollo Server and Apollo Client, which provide a lot of the functionality required to implement and maintain a GraphQL API.

Overall, it's important to weigh the costs and benefits of GraphQL and to consider how it will fit into the overall architecture of the system. The trade-offs will be different for every case, and it's important to evaluate the risks and costs associated with GraphQL against the specific requirements and constraints of your application.

### How about mutation and subscription

Mutation and subscription are two important concepts in GraphQL that allow for modifying and real-time updates to the data in a GraphQL API.

A mutation in GraphQL is a way to modify data on the server, such as creating, updating, or deleting data. Mutations are similar to HTTP methods like POST, PUT, and DELETE, but in GraphQL, the client specifies exactly what data it wants to modify and what it wants to receive in response.

A subscription in GraphQL is a way to receive real-time updates from the server whenever a specific event occurs, such as a new data being added or an existing data being updated. This allows the client to stay up-to-date with the latest data in the system without having to constantly poll the server for updates.

Both mutation and subscription are powerful features of GraphQL that allow for a more flexible and efficient data exchange between the client and the server. However, as with any technology, it's important to consider the costs and risks associated with using them, such as the additional complexity and the potential for security vulnerabilities.

It's also important to consider the scalability implications of using subscriptions, as real-time updates can put a heavy load on the server if not handled properly. To mitigate these risks, it's essential to have proper testing and monitoring in place, as well as to design the system with scalability in mind.

Overall, mutations and subscriptions are useful features in GraphQL, but it's important to weigh the costs and benefits of using them, and to implement them in a secure and scalable manner.

## Videos
 * [GraphQL vs REST: Which is Better for APIs?](https://www.youtube.com/watch?v=PTfZcN20fro)
	> [<img src="https://img.youtube.com/vi/PTfZcN20fro/0.jpg" width="200">](https://www.youtube.com/watch?v=PTfZcN20fro "Create, manage, secure, socialize and monetize APIs with IBM API Connect by IBM 263 k views 7 minutes, 30 seconds")

 * [GraphQL Course for Beginners](https://www.youtube.com/watch?v=5199E50O7SI)
	> [<img src="https://img.youtube.com/vi/5199E50O7SI/0.jpg" width="200">](https://www.youtube.com/watch?v=5199E50O7SI "GraphQL Course for Beginners by freeCodeCamp.org 142,762 views 1 hour, 29 minutes")
 * [The Truth About GraphQL](https://www.youtube.com/watch?v=qgdiLcD2RL8)
	> [<img src="https://img.youtube.com/vi/qgdiLcD2RL8/0.jpg" width="200">](https://www.youtube.com/watch?v=qgdiLcD2RL8 "The Truth About GraphQL by Theo - t3․gg 84,845 views 12 minutes, 6 seconds")
 * [REST vs GraphQL | When to choose one over other | Tech Primers](https://www.youtube.com/watch?v=4akSaaEYJqs)
	> [<img src="https://img.youtube.com/vi/4akSaaEYJqs/0.jpg" width="200">](https://www.youtube.com/watch?v=4akSaaEYJqs "REST vs GraphQL | When to choose one over other | Tech Primers by Tech Primers 41,471 views 19 minutes")
 * [GraphQL Full Course - Novice to Expert](https://www.youtube.com/watch?v=ed8SzALpx1Q)
	> [<img src="https://img.youtube.com/vi/ed8SzALpx1Q/0.jpg" width="200">](https://www.youtube.com/watch?v=ed8SzALpx1Q "GraphQL Full Course - Novice to Expert by freeCodeCamp.org 442,049 views 4 hours, 5 minutes")
 * [GraphQL // Dicionário do Programador](https://www.youtube.com/watch?v=xbLpIhCsIdg)
	> [<img src="https://img.youtube.com/vi/xbLpIhCsIdg/0.jpg" width="200">](https://www.youtube.com/watch?v=xbLpIhCsIdg "GraphQL // Dicionário do Programador by Código Fonte TV 52,470 views 6 minutes, 6 seconds")
 * [Is tRPC The End Of REST/GraphQL?](https://www.youtube.com/watch?v=lxnPMB0Jc7E)
	> [<img src="https://img.youtube.com/vi/lxnPMB0Jc7E/0.jpg" width="200">](https://www.youtube.com/watch?v=lxnPMB0Jc7E "Is tRPC The End Of REST/GraphQL? by Web Dev Simplified 131,857 views 13 minutes, 48 seconds")
 * [What Is GraphQL? REST vs. GraphQL](https://www.youtube.com/watch?v=yWzKJPw_VzM)
	> [<img src="https://img.youtube.com/vi/yWzKJPw_VzM/0.jpg" width="200">](https://www.youtube.com/watch?v=yWzKJPw_VzM "What Is GraphQL? REST vs. GraphQL by ByteByteGo 339,460 views 5 minutes, 15 seconds")

## References

1. https://github.com/spring-petclinic/spring-petclinic-graphql
2, https://blog.sourced-bvba.be/article/2021/04/23/why-graphql/
3. https://graphql.org/
4. https://www.linkedin.com/feed/update/urn:li:activity:7011345528659562497/
5. 
