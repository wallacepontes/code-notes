# Serverless

## Serverless architecture is not a silver bullet

According to the video [Serverless Was A Mistake | Prime Reacts](https://www.youtube.com/watch?v=BcMm0aaqnnI), serverless architecture is not a silver bullet and there are cases where monolithic architecture is a better choice.

Here are the key points from the video:

* Serverless architecture does not mean you are not using servers. You are still using servers provided by the cloud vendor.
* Serverless can be expensive because in addition to the cost of the servers, you also need to pay for the additional services required to run serverless applications.
* Monolithic architecture can be more cost-effective for some applications, especially if you can optimize it.
* There is no one-size-fits-all solution when it comes to cloud architecture. The best architecture for your application will depend on your specific needs and trade-offs.

### Serverless architecture and monolithic architecture

This video [Serverless was a big mistake... says Amazon](https://www.youtube.com/watch?v=qQk94CjRvIs) is about serverless architecture and monolithic architecture.

The speaker claims that serverless is a big lie because you are still using servers that you don't manage yourself. Serverless architecture can be expensive as shown by an Amazon Prime Video case study. They switched from serverless microservices to a monolithic architecture and saved 90% on their AWS bill. This contradicts the main claims of serverless architecture, which is to save money by scaling infrastructure more efficiently.

The video also talks about the pros and cons of monolithic architecture. Monolithic architecture is simpler to develop and maintain but can be difficult to scale. Microservices, on the other hand, are more complex to develop and maintain but can scale more easily.

In conclusion, there is no perfect solution and the best architecture depends on your specific needs. If you are a small business like the speaker, serverless architecture can be a good option because it is easy to use and you don't have to worry about managing servers. However, if you are a large company like Amazon, a monolithic architecture may be more cost-effective.
http://googleusercontent.com/youtube_content/4

## Serverless vs Microservices - Which Architecture to Choose?

The choice between serverless and microservices architecture depends on the specific requirements and goals of the application. Here are some key considerations:

1. Scalability: serverless architecture is highly scalable, as it automatically allocates resources as needed. Microservices can also be scalable, but the scaling needs to be managed and orchestrated.

2. Cost: serverless can be cost-effective, as you only pay for what you use, with no need to pay for idle resources. However, microservices can also be cost-effective, especially if you have a large amount of reuse between services.

3. Complexity: serverless can be simpler to manage, as the underlying infrastructure is abstracted away. Microservices can add complexity, as the service orchestration and management needs to be handled by the development team.

4. Latency: serverless can have higher latency, as the cold start time for a function can be longer. Microservices can have lower latency, as the services can be kept running and ready to respond to requests.

5. Flexibility: microservices offer more flexibility, as each service can be developed and deployed independently and can be written in different programming languages. Serverless is more limited in this regard, as functions typically need to be written in the same language.

Ultimately, the choice between serverless and microservices depends on the specific needs and requirements of the application. In some cases, a combination of the two architectures may be appropriate.

## Videos

 * [Serverless: Do Zero ao Deploy com API Gateway, Lambda e DynamoDB](https://www.youtube.com/watch?v=kpjkSLXHN5E)
	> [<img src="https://img.youtube.com/vi/kpjkSLXHN5E/0.jpg" width="200">](https://www.youtube.com/watch?v=kpjkSLXHN5E "Serverless: Do Zero ao Deploy com API Gateway, Lambda e DynamoDB by Full Cycle 8,937 views 1 hour")
 * [Monolith vs Microservices vs Serverless](https://www.youtube.com/watch?v=1A9tPOfp6NA)
	> [<img src="https://img.youtube.com/vi/1A9tPOfp6NA/0.jpg" width="200">](https://www.youtube.com/watch?v=1A9tPOfp6NA "Monolith vs Microservices vs Serverless by Code With Ryan 71,669 views 23 minutes")
 * [How to Create an API with TypeScript on AWS with Serverless](https://www.youtube.com/watch?v=HhgXwKFUzT8)
	> [<img src="https://img.youtube.com/vi/HhgXwKFUzT8/0.jpg" width="200">](https://www.youtube.com/watch?v=HhgXwKFUzT8 "How to Create an API with TypeScript on AWS with Serverless by Complete Coding - Master AWS Serverless  21,673 views 24 minutes")
 * [Amazon Prime Video Ditches AWS Serverless, Saves 90%](https://www.youtube.com/watch?v=JTp0TY_2hXM)
	> [<img src="https://img.youtube.com/vi/JTp0TY_2hXM/0.jpg" width="200">](https://www.youtube.com/watch?v=JTp0TY_2hXM "Amazon Prime Video Ditches AWS Serverless, Saves 90% by ByteByteGo 65,855 views 4 minutes, 15 seconds")
 * [FREE AWS Cloud Project Bootcamp (Week 8) - Serverless Image Processing](https://www.youtube.com/watch?v=YiSNlK4bk90)
	> [<img src="https://img.youtube.com/vi/YiSNlK4bk90/0.jpg" width="200">](https://www.youtube.com/watch?v=YiSNlK4bk90 "FREE AWS Cloud Project Bootcamp (Week 8) - Serverless Image Processing by ExamPro 2,228 views 2 hours, 4 minutes")
 * [WASM vs Docker Containers vs Kubernetes vs Serverless: The Battle for Cloud Native Supremacy](https://www.youtube.com/watch?v=uZ8xI26sno8)
	> [<img src="https://img.youtube.com/vi/uZ8xI26sno8/0.jpg" width="200">](https://www.youtube.com/watch?v=uZ8xI26sno8 "WASM vs Docker Containers vs Kubernetes vs Serverless: The Battle for Cloud Native Supremacy by DevOps Toolkit 10,171 views 27 minutes")
 * [Serverless for Frontends - Florian Rappl](https://www.youtube.com/watch?v=QKPVGdJ32yQ)
	> [<img src="https://img.youtube.com/vi/QKPVGdJ32yQ/0.jpg" width="200">](https://www.youtube.com/watch?v=QKPVGdJ32yQ "Serverless for Frontends - Florian Rappl by TechDay Pakistan 143 views 42 minutes")
 * [Serverless Was A Mistake | Prime Reacts](https://www.youtube.com/watch?v=BcMm0aaqnnI)
	> [<img src="https://img.youtube.com/vi/BcMm0aaqnnI/0.jpg" width="200">](https://www.youtube.com/watch?v=BcMm0aaqnnI "Serverless Was A Mistake | Prime Reacts by ThePrimeTime 197,099 views 13 minutes, 40 seconds")
 * [Serverless was a big mistake... says Amazon](https://www.youtube.com/watch?v=qQk94CjRvIs)
	> [<img src="https://img.youtube.com/vi/qQk94CjRvIs/0.jpg" width="200">](https://www.youtube.com/watch?v=qQk94CjRvIs "Serverless was a big mistake... says Amazon by Fireship 1,479,917 views 3 minutes, 48 seconds")

