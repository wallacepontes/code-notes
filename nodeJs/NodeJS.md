# NodeJS

## Table of Contents
- [NodeJS](#nodejs)
	- [Table of Contents](#table-of-contents)
	- [NodeJS](#nodejs-1)
		- [What is Node.js used for?](#what-is-nodejs-used-for)
		- [why use Node.js and npm with nginx in docker?](#why-use-nodejs-and-npm-with-nginx-in-docker)
		- [Videos](#videos)

## NodeJS

[NodeJS on dockerhub](https://hub.docker.com/_/node)

### What is Node.js used for?

Node.js is an open-source, cross-platform JavaScript runtime environment that allows developers to build scalable network applications. It uses an event-driven, non-blocking I/O model, making it lightweight and efficient, particularly for real-time applications that run across distributed devices. Node.js is commonly used for various purposes, including:

1. **Server-Side Web Development**: Node.js is widely used for server-side web development to create dynamic web pages and web servers. It allows developers to build fast, scalable network applications using JavaScript on the server side.

2. **Building APIs**: It is commonly used for creating robust and efficient APIs (Application Programming Interfaces) that enable communication between different software applications. Many modern web applications use Node.js to power their backend APIs.

3. **Real-Time Applications**: Node.js is well-suited for building real-time applications such as chat applications, collaborative tools, and online gaming platforms. Its event-driven architecture makes it ideal for handling multiple concurrent connections and providing seamless real-time communication.

4. **Command Line Tools**: It can be used to build command line tools and utilities, leveraging its ability to handle I/O operations efficiently. Many developers use Node.js to create command line interfaces and build custom command line tools for various tasks.

5. **Microservices**: Node.js is often used for building microservices-based architectures due to its lightweight and scalable nature. It allows developers to create small, independent services that can be developed, deployed, and scaled independently.

6. **Desktop Applications**: With the help of frameworks like Electron, Node.js can be used to build cross-platform desktop applications, allowing developers to create powerful applications using web technologies.

Node.js has gained popularity for its performance, scalability, and the ability to handle a large number of concurrent connections, making it a popular choice for developing various types of applications and software solutions.

### why use Node.js and npm with nginx in docker?

Using Node.js, npm, and Nginx together in a Docker environment can provide a powerful combination for building and deploying modern web applications. Each component serves a specific purpose within the software development and deployment process:

1. **Node.js and npm**: Node.js and npm are commonly used for building and managing JavaScript-based applications. They provide the tools and environment needed for server-side JavaScript development and offer a vast ecosystem of packages and libraries that can be used to build robust and scalable applications.

2. **Nginx**: Nginx is a high-performance, open-source web server, and reverse proxy. It is known for its efficiency in handling concurrent connections and serving web content. When used as a reverse proxy, Nginx can help improve the performance and scalability of web applications by handling load balancing, caching, and serving static content.

3. **Docker**: Docker provides a way to package applications and their dependencies into containers, ensuring consistency across different environments and making it easier to deploy and manage applications in various settings.

When used together in a Docker environment, Node.js and npm can be used to build and manage the application's backend and frontend components, while Nginx can be used as a reverse proxy to handle load balancing and serve static content. Docker helps in packaging and deploying these components together as a unified application, making it easier to manage the entire application stack as a single unit.

Using this combination allows developers to build and deploy scalable, performant, and easily manageable web applications that leverage the strengths of each component. Additionally, Docker provides a consistent and reliable way to package, deploy, and manage the entire application stack across different environments.

### Videos
 * [Node.js: The Documentary | An origin story](https://www.youtube.com/watch?v=LB8KwiiUGy0)
	> [<img src="https://img.youtube.com/vi/LB8KwiiUGy0/0.jpg" width="200">](https://www.youtube.com/watch?v=LB8KwiiUGy0 "Node.js: The Documentary | An origin story by Honeypot 192,421 views 1 hour, 2 minutes")
 * [How to Detect Keypress in Node.js | Capture Keypress Nodejs](https://www.youtube.com/watch?v=MO6RjlB81P8)
	> [<img src="https://img.youtube.com/vi/MO6RjlB81P8/0.jpg" width="200">](https://www.youtube.com/watch?v=MO6RjlB81P8 "How to Detect Keypress in Node.js | Capture Keypress Nodejs by Coder Gautam 3,063 views 1 minute, 45 seconds")
 * [CURSO DE NODEJS - Como criar uma api com Node.js do zero |  AULA 0](https://www.youtube.com/watch?v=Rbm_C7rqkFc)
	> [<img src="https://img.youtube.com/vi/Rbm_C7rqkFc/0.jpg" width="200">](https://www.youtube.com/watch?v=Rbm_C7rqkFc "CURSO DE NODEJS - Como criar uma api com Node.js do zero |  AULA 0 by Luaverse 107 views 19 minutes")
 * [Como criar um robô que faz login e da like? | Robô com JavaScript, NodeJS e Puppeteer](https://www.youtube.com/watch?v=Ltdp9-ZTAzM)
	> [<img src="https://img.youtube.com/vi/Ltdp9-ZTAzM/0.jpg" width="200">](https://www.youtube.com/watch?v=Ltdp9-ZTAzM "Como criar um robô que faz login e da like? | Robô com JavaScript, NodeJS e Puppeteer by Mario Souto - Dev Soutinho 52,293 views 16 minutes")
 * [The Hidden Cost Of GraphQL And NodeJS](https://www.youtube.com/watch?v=i0YfiQlzv6M)
	> [<img src="https://img.youtube.com/vi/i0YfiQlzv6M/0.jpg" width="200">](https://www.youtube.com/watch?v=i0YfiQlzv6M "The Hidden Cost Of GraphQL And NodeJS by ThePrimeTime 170,345 views 28 minutes")
 * [Worker queues for Node.js - Piotr Cholewa | MonteTalks: Node.js](https://www.youtube.com/watch?v=wAEMXVcRbgU)
	> [<img src="https://img.youtube.com/vi/wAEMXVcRbgU/0.jpg" width="200">](https://www.youtube.com/watch?v=wAEMXVcRbgU "Worker queues for Node.js - Piotr Cholewa | MonteTalks: Node.js by monterail 15,840 views 27 minutes")
 * [Why node.js is the wrong choice for APIs (and what to use instead)](https://www.youtube.com/watch?v=HSkRNRG8dco)
	> [<img src="https://img.youtube.com/vi/HSkRNRG8dco/0.jpg" width="200">](https://www.youtube.com/watch?v=HSkRNRG8dco "Why node.js is the wrong choice for APIs (and what to use instead) by DevOps For Developers 66,322 views 5 minutes, 48 seconds")
 * [Node.js Rocks in Docker, 2022 Edition](https://www.youtube.com/watch?v=rRMTYkRqUww)
	> [<img src="https://img.youtube.com/vi/rRMTYkRqUww/0.jpg" width="200">](https://www.youtube.com/watch?v=rRMTYkRqUww "Node.js Rocks in Docker, 2022 Edition by Docker 3,583 views 27 minutes")
 * [🧠 Refatoramos um App NODE.JS para BUN (E TESTAMOS A PERFORMANCE)](https://www.youtube.com/watch?v=ImzqKFU0d_U)
	> [<img src="https://img.youtube.com/vi/ImzqKFU0d_U/0.jpg" width="200">](https://www.youtube.com/watch?v=ImzqKFU0d_U "🧠 Refatoramos um App NODE.JS para BUN (E TESTAMOS A PERFORMANCE) by Código Fonte TV 42,649 views 15 minutes")
 * [Bun vs Node.js - Everything You Need To Know!](https://www.youtube.com/watch?v=YpZff07df-Q)
	> [<img src="https://img.youtube.com/vi/YpZff07df-Q/0.jpg" width="200">](https://www.youtube.com/watch?v=YpZff07df-Q "Bun vs Node.js - Everything You Need To Know! by Codevolution 33,160 views 23 minutes")
 * [Princípios SOLID em uma API REST com Node.js e TypeScript | Code/Drops #44](https://www.youtube.com/watch?v=vAV4Vy4jfkc)
	> [<img src="https://img.youtube.com/vi/vAV4Vy4jfkc/0.jpg" width="200">](https://www.youtube.com/watch?v=vAV4Vy4jfkc "Princípios SOLID em uma API REST com Node.js e TypeScript | Code/Drops #44 by Rocketseat 201,538 views 48 minutes")
 * [Node.js is a serious thing now… (2023)](https://www.youtube.com/watch?v=_Im4_3Z1NxQ)
	> [<img src="https://img.youtube.com/vi/_Im4_3Z1NxQ/0.jpg" width="200">](https://www.youtube.com/watch?v=_Im4_3Z1NxQ "Node.js is a serious thing now… (2023) by Code With Ryan 610,025 views 8 minutes, 18 seconds")
 * [Node.js // Dicionário do Programador](https://www.youtube.com/watch?v=vYekSMBCCiM)
	> [<img src="https://img.youtube.com/vi/vYekSMBCCiM/0.jpg" width="200">](https://www.youtube.com/watch?v=vYekSMBCCiM "Node.js // Dicionário do Programador by Código Fonte TV 171,996 views 10 minutes, 33 seconds")
 * [Node.js // Dicionário do Programador](https://www.youtube.com/watch?v=vYekSMBCCiM)
	> [<img src="https://img.youtube.com/vi/vYekSMBCCiM/0.jpg" width="200">](https://www.youtube.com/watch?v=vYekSMBCCiM "Node.js // Dicionário do Programador by Código Fonte TV 171,996 views 10 minutes, 33 seconds")
 * [Node.js / Express Course - Build 4 Projects](https://www.youtube.com/watch?v=qwfE7fSVaZM)
	> [<img src="https://img.youtube.com/vi/qwfE7fSVaZM/0.jpg" width="200">](https://www.youtube.com/watch?v=qwfE7fSVaZM "Node.js / Express Course - Build 4 Projects by freeCodeCamp.org 1,213,310 views 10 hours")
 * [Criando APIs com NodeJs - Aula 40: Conclusão](https://www.youtube.com/watch?v=f4BiBAuZEUA)
	> [<img src="https://img.youtube.com/vi/f4BiBAuZEUA/0.jpg" width="200">](https://www.youtube.com/watch?v=f4BiBAuZEUA "Criando APIs com NodeJs - Aula 40: Conclusão by balta.io 7,128 views 3 minutes, 32 seconds")
 * [Criando APIs com NodeJs - Aula 1: Instalação Node, NPM e VS Code](https://www.youtube.com/watch?v=wDWdqlYxfcw)
	> [<img src="https://img.youtube.com/vi/wDWdqlYxfcw/0.jpg" width="200">](https://www.youtube.com/watch?v=wDWdqlYxfcw "Criando APIs com NodeJs - Aula 1: Instalação Node, NPM e VS Code by balta.io 103,820 views 4 minutes, 3 seconds")
 * [Learn the MERN Stack - Full Tutorial (MongoDB, Express, React, Node.js)](https://www.youtube.com/watch?v=7CqJlxBYj-M)
	> [<img src="https://img.youtube.com/vi/7CqJlxBYj-M/0.jpg" width="200">](https://www.youtube.com/watch?v=7CqJlxBYj-M "Learn the MERN Stack - Full Tutorial (MongoDB, Express, React, Node.js) by freeCodeCamp.org 1,619,292 views 1 hour, 47 minutes")
 * [Integrando um App React com NodeJS - Passo a Passo Completo](https://www.youtube.com/watch?v=edeoWfH_70U)
	> [<img src="https://img.youtube.com/vi/edeoWfH_70U/0.jpg" width="200">](https://www.youtube.com/watch?v=edeoWfH_70U "Integrando um App React com NodeJS - Passo a Passo Completo by Danki Code 14,675 views 12 minutes, 53 seconds")
