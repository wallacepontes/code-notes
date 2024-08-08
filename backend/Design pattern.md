# Design pattern

## Behavioral 

### Command design pattern

The Command design pattern is a behavioral pattern that encapsulates a request as an object, allowing for deferred execution or manipulation of requests. It is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

Here's an example of the Command pattern in Java:

```java
interface Command {
    void execute();
}

class Light {
    public void turnOn() {
        System.out.println("The light is on");
    }
}

class TurnOnLightCommand implements Command {
    private Light light;

    public TurnOnLightCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}

class Client {
    public static void main(String[] args) {
        Light livingRoomLight = new Light();
        Command turnOnLivingRoomLight = new TurnOnLightCommand(livingRoomLight);

        RemoteControl remote = new RemoteControl();
        remote.setCommand(turnOnLivingRoomLight);
        remote.pressButton();
    }
}
```
This example defines a `Light` class, a `TurnOnLightCommand` class that implements the `Command` interface, and a `RemoteControl` class that uses a `Command` object to turn on a light. The `Client` class creates an instance of the `Light` class and an instance of the `TurnOnLightCommand` class, and then it uses the `RemoteControl` class to turn on the light. By encapsulating the request in an object, you can pass around the request with all the information necessary to execute it, rather than just the request itself.

## MVP architecture 

MVP (Model-View-Presenter) is an architectural pattern used in software development. It is an extension of the MVC (Model-View-Controller) pattern, with the main difference being that the presenter takes on the responsibilities of the controller in the MVC pattern.

In MVP architecture, the model represents the data and business logic of the application. The view is responsible for rendering the user interface, displaying the data from the model, and handling user input. The presenter acts as a mediator between the model and the view, taking input from the view and providing it to the model for processing, and updating the view when the model changes.

The MVP pattern helps to improve the separation of concerns in the application, as the view is decoupled from the model and the presenter acts as an intermediary between the two. This makes it easier to unit test the different components of the application and to make changes to the application without affecting other parts of the code. It is particularly useful in applications with complex user interfaces, as it allows developers to focus on the logic of the application without having to worry about the details of the user interface.

## resilient design patterns

Resilient design patterns are strategies and approaches that are used to design systems that are able to withstand and recover from failures and disruptions. These patterns are particularly important in the design of critical systems, such as financial systems, power grids, and transportation networks, where the consequences of failure can be severe.

There are many different types of resilient design patterns, and the specific pattern or patterns that are used will depend on the needs and constraints of the system being designed. Some common resilient design patterns include:

- Redundancy: Adding extra components or capacity to a system to allow it to continue functioning even if one or more components fail.

- Load balancing: Distributing workloads across multiple components to ensure that no single component is overwhelmed, and to allow the system to continue operating even if one or more components fail.

- Fault tolerance: Designing components and systems to be able to detect and recover from failures, either automatically or with human intervention.

- Disaster recovery: Developing plans and procedures for responding to and recovering from major disruptions or disasters, such as natural disasters or cyber attacks.

Overall, resilient design patterns are an important aspect of system design, and can help to ensure that critical systems are able to continue operating even in the face of failures or disruptions.

### Additional strategies and approaches that can be used to design resilient systems

- Autoscaling: Automatically adjusting the capacity and resources of a system in response to changes in workload or demand. This can help to ensure that the system has the resources it needs to meet the needs of users, while minimizing the cost of resources that are not being fully utilized.

- Health checks: Periodically testing the status and performance of system components to ensure that they are functioning correctly. This can help to identify potential problems before they become serious, and can also be used to trigger failover or other recovery actions.

- Failover strategies: Planning and implementing strategies for how the system should respond when a component fails or becomes unavailable. This can include automatically switching to a backup component, or routing traffic around a failed component.

- Avoiding cascading failures: Designing systems to be resilient to failures that might propagate and cause additional failures. This can include designing systems with redundant components, or implementing isolation mechanisms to prevent failures in one part of the system from affecting other parts of the system.

- Operational isolation: Separating different parts of the system, or different types of workloads, in order to reduce the risk of failures or disruptions propagating from one part of the system to another. This can include separating critical and non-critical workloads, or isolating different geographic regions.

Overall, these strategies and approaches can be used in combination to design resilient systems that are able to withstand and recover from failures and disruptions.

## TOGAF

The Open Group Architecture Framework (TOGAF) is a framework for enterprise architecture that provides a common language and set of tools for designing and managing the structure of an organization's information systems. TOGAF is widely used in the field of enterprise architecture, and is recognized as a leading industry standard.

TOGAF is based on a four-stage architecture development process, which includes:

1. Architecture development cycle: This is the overall process of using TOGAF to develop and maintain an enterprise architecture.

2. Architecture content framework: This defines the types of information and artifacts that make up an enterprise architecture, and how they relate to each other.

3. Architecture development method: This provides a step-by-step guide for using TOGAF to develop an enterprise architecture.

4. TOGAF reference models: These are models that provide a common vocabulary and set of concepts for use in developing an enterprise architecture.

TOGAF is a comprehensive and flexible framework that can be tailored to the needs of a specific organization. It is designed to support the ongoing evolution and development of an enterprise architecture, and to ensure that it remains aligned with the business strategy and goals of the organization. 

## Statistical Analytical System

A statistical analytical system (SAS) is a software platform that is used for statistical analysis, data management, and business intelligence. SAS is widely used in a variety of industries, including finance, healthcare, and government, and is known for its powerful and flexible capabilities for data manipulation, analysis, and visualization.

SAS consists of a number of different components, including:

- SAS language: A programming language that is used to manipulate and analyze data, create reports and graphics, and automate tasks.

- SAS Studio: A web-based interface for accessing and using SAS tools and resources.

- SAS Data Management: Tools and utilities for organizing and managing data, including data integration, quality, and governance.

- SAS Visual Analytics: A tool for creating interactive dashboards and visualizations of data.

- SAS Enterprise Miner: A tool for data mining and machine learning.

Overall, SAS is a comprehensive and powerful platform for statistical analysis and data management, and is widely used by organizations to gain insights from their data and make informed business decisions.

## How Visitor Pattern works?

The Visitor Pattern is a behavioral design pattern that allows you to separate algorithms from the objects on which they operate. It enables you to add new operations to existing object structures without modifying those structures.

Here's how it works:

1. **Define the Visitor interface**: This interface declares a set of visit methods, each corresponding to a different class of elements in the object structure.

2. **Implement Concrete Visitors**: Concrete Visitor classes implement the Visitor interface and provide specific implementations for the visit methods corresponding to each element class.

3. **Define the Visitable interface**: This interface declares an accept method that accepts a visitor as an argument. Each element class in the object structure implements this interface.

4. **Implement Concrete Visitables**: Concrete Visitable classes implement the Visitable interface and provide an implementation for the accept method. This method typically calls the appropriate visit method on the visitor object.

5. **Use the Visitor Pattern**: Clients can then create instances of Concrete Visitors and pass them to instances of Concrete Visitables. When the accept method is called on a Visitable object, it invokes the appropriate visit method on the Visitor, allowing the Visitor to perform its operation on the Visitable object.

Here's a simplified example to illustrate the Visitor Pattern:

```java
// Visitor interface
interface Visitor {
    void visitCircle(Circle circle);
    void visitSquare(Square square);
}

// Concrete Visitor
class AreaCalculator implements Visitor {
    @Override
    public void visitCircle(Circle circle) {
        System.out.println("Calculating area of circle");
        // Calculate area of circle
    }

    @Override
    public void visitSquare(Square square) {
        System.out.println("Calculating area of square");
        // Calculate area of square
    }
}

// Visitable interface
interface Shape {
    void accept(Visitor visitor);
}

// Concrete Visitables
class Circle implements Shape {
    @Override
    public void accept(Visitor visitor) {
        visitor.visitCircle(this);
    }
}

class Square implements Shape {
    @Override
    public void accept(Visitor visitor) {
        visitor.visitSquare(this);
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        Visitor areaCalculator = new AreaCalculator();

        Shape circle = new Circle();
        Shape square = new Square();

        circle.accept(areaCalculator);
        square.accept(areaCalculator);
    }
}
```

In this example, the Visitor (`AreaCalculator`) defines visit methods for different types of shapes (`Circle` and `Square`). Each shape implements the `Shape` interface and provides an implementation for the `accept` method, which calls the appropriate visit method on the visitor. When the client code invokes the accept method on a shape object, the corresponding visit method is called on the visitor, allowing it to perform its operation on the shape object.

## Videos
* [Design Patterns #16: Facade](https://www.youtube.com/watch?v=PuApVFSLRZE)
	> [<img src="https://img.youtube.com/vi/PuApVFSLRZE/0.jpg" width="200">](https://www.youtube.com/watch?v=PuApVFSLRZE "Design Patterns #16: Facade by Luiz Alberto 4,056 views 8 minutes, 48 seconds")
 * [JAVA DTO Pattern Tutorial | Simplify Your Code](https://www.youtube.com/watch?v=5yquJa2x3Ko)
	> [<img src="https://img.youtube.com/vi/5yquJa2x3Ko/0.jpg" width="200">](https://www.youtube.com/watch?v=5yquJa2x3Ko "JAVA DTO Pattern Tutorial | Simplify Your Code by Amigoscode 175,557 views 19 minutes")
 * [10 Design Patterns Explained in 10 Minutes](https://www.youtube.com/watch?v=tv-_1er1mWI)
	> [<img src="https://img.youtube.com/vi/tv-_1er1mWI/0.jpg" width="200">](https://www.youtube.com/watch?v=tv-_1er1mWI "10 Design Patterns Explained in 10 Minutes by Fireship 2,096,339 views 11 minutes, 4 seconds")
 * [10 Architecture Patterns Used In Enterprise Software Development Today](https://www.youtube.com/watch?v=BrT3AO8bVQY)
	> [<img src="https://img.youtube.com/vi/BrT3AO8bVQY/0.jpg" width="200">](https://www.youtube.com/watch?v=BrT3AO8bVQY "10 Architecture Patterns Used In Enterprise Software Development Today by Coding Tech 239,955 views 11 minutes")
 * [Anti-Corruption Layer Pattern (Padrão da Camada de Anti-Corrupção)](https://www.youtube.com/watch?v=on9FiB18A6Q)
	> [<img src="https://img.youtube.com/vi/on9FiB18A6Q/0.jpg" width="200">](https://www.youtube.com/watch?v=on9FiB18A6Q "Anti-Corruption Layer Pattern (Padrão da Camada de Anti-Corrupção) by André Secco 1,837 views 29 minutes")
 * [Identifique Quando e Como Usar o Design Pattern Strategy na Prática](https://www.youtube.com/watch?v=WPdrnuSHAQs)
	> [<img src="https://img.youtube.com/vi/WPdrnuSHAQs/0.jpg" width="200">](https://www.youtube.com/watch?v=WPdrnuSHAQs "Identifique Quando e Como Usar o Design Pattern Strategy na Prática by Código Fonte TV 44,155 views 10 minutes, 27 seconds")
 * [Um Programador Pleno já deveria saber usar esse Design Pattern (tutorial linha a linha)](https://www.youtube.com/watch?v=arAz2Ff8s88)
	> [<img src="https://img.youtube.com/vi/arAz2Ff8s88/0.jpg" width="200">](https://www.youtube.com/watch?v=arAz2Ff8s88 "Um Programador Pleno já deveria saber usar esse Design Pattern (tutorial linha a linha) by Filipe Deschamps 372,904 views 11 minutes, 57 seconds")
 * [Aprenda Design Patterns](https://www.youtube.com/watch?v=tvIwTK0dwkg)
	> [<img src="https://img.youtube.com/vi/tvIwTK0dwkg/0.jpg" width="200">](https://www.youtube.com/watch?v=tvIwTK0dwkg "Aprenda Design Patterns by Full Cycle 47,510 views 29 minutes")
 * [Tudo sobre o Design Pattern Singleton](https://www.youtube.com/watch?v=IXgE3jmwdk0)
	> [<img src="https://img.youtube.com/vi/IXgE3jmwdk0/0.jpg" width="200">](https://www.youtube.com/watch?v=IXgE3jmwdk0 "Tudo sobre o Design Pattern Singleton by Full Cycle 8,386 views 28 minutes")
 * [Padrões de Projeto (Design Patterns - GoF) - Introdução - Parte 1/45](https://www.youtube.com/watch?v=MqddY6Ochkc&list=PLbIBj8vQhvm0VY5YrMrafWaQY2EnJ3j8H)
	> [<img src="https://img.youtube.com/vi/MqddY6Ochkc/0.jpg" width="200">](https://www.youtube.com/watch?v=MqddY6Ochkc&list=PLbIBj8vQhvm0VY5YrMrafWaQY2EnJ3j8H "Padrões de Projeto (Design Patterns - GoF) - Introdução - Parte 1/45 by Otávio Miranda 65,000 views 12 minutes, 24 seconds")
* [Aggregate (Root) Design: Separate Behavior & Data for Persistence](https://www.youtube.com/watch?v=GtWVGJp061A)
	> [<img src="https://img.youtube.com/vi/GtWVGJp061A/0.jpg" width="200">](https://www.youtube.com/watch?v=GtWVGJp061A "Aggregate (Root) Design: Separate Behavior & Data for Persistence by CodeOpinion 60,000 views 10 minutes, 46 seconds")
* [Aggregate Design: Using Invariants as a Guide](https://www.youtube.com/watch?v=64ngP-aUYPc)
	> [<img src="https://img.youtube.com/vi/64ngP-aUYPc/0.jpg" width="200">](https://www.youtube.com/watch?v=64ngP-aUYPc "Aggregate Design: Using Invariants as a Guide by CodeOpinion 36,000 views 8 minutes, 34 seconds")
* [Create a new Maven project for our Java microservice](https://www.youtube.com/watch?v=-XdcIkXIorc)
	> [<img src="https://img.youtube.com/vi/-XdcIkXIorc/0.jpg" width="200">](https://www.youtube.com/watch?v=-XdcIkXIorc "Create a new Maven project for our Java microservice by Deege 23,000 views 4 minutes, 07 seconds")

* [Patterns and Frameworks for Asynchronous Event Handling (Part 1)](https://www.youtube.com/watch?v=2rGOX8Oy7-Q)
	> [<img src="https://img.youtube.com/vi/2rGOX8Oy7-Q/0.jpg" width="200">](https://www.youtube.com/watch?v=2rGOX8Oy7-Q "Patterns and Frameworks for Asynchronous Event Handling (Part 1) by Douglas Schmidt 4,200 views 11 minutes, 30 seconds")

## References

1. https://developers.redhat.com/blog/2018/10/01/patterns-for-distributed-transactions-within-a-microservices-architecture#possible_solutions
2. https://deviq.com/domain-driven-design/aggregate-pattern
3. https://deviq.com/principles/open-closed-principle
4. https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines
5. https://github.com/wallacepontes/code-o-share/tree/master/design-patterns/src/main/java/org/codeoshare/designpatterns/behavioral/visitor
6. https://github.com/wallacepontes/code-o-share/tree/master/design-patterns/src/main/java/org/codeoshare/designpatterns/behavioral/visitor
7. https://java-design-patterns.com/patterns/service-layer/
8. https://levelup.gitconnected.com/microservices-design-patterns-in-practice-using-java-maven-and-sprint-boot-step-1-building-a-dfe18c6a1206
9. https://martinfowler.com/bliki/ValueObject.html
10. https://medium.com/@_jesus_rafael/making-http-client-more-resilient-in-go-d24c66a64bd1
11. https://medium.com/@eduardolanfredi/inje%C3%A7%C3%A3o-de-depend%C3%AAncia-ff0372a1672
12. https://medium.com/@engnogueirawgn/princ%C3%ADpio-da-segrega%C3%A7%C3%A3o-de-interfaces-isp-8344ce3c5b4e
13. https://medium.com/codigorefinado/strangle-design-pattern-o-padr%C3%A3o-estrangulamento-f93c05e9061d
14. https://medium.com/design-microservices-architecture-with-patterns/how-to-choose-a-database-for-microservices-cap-theorem-d1585bf40ecd
15. https://medium.com/design-microservices-architecture-with-patterns/how-to-choose-a-database-for-microservices-cap-theorem-d1585bf40ecd
16. https://medium.com/tonaserasa/outbox-pattern-saga-transa%C3%A7%C3%B5es-distribu%C3%ADdas-com-microservices-c9c294b7a045
17. https://microservices.io/patterns/data/cqrs.html
18. https://microservices.io/patterns/data/saga.html
19. https://microservices.io/patterns/microservices.html
20. https://microservices.io/patterns/reliability/circuit-breaker.html
21. https://nextcodeblock.com/posts/introduction-to-the-microservices-architecture-pattern/
22. https://www.databricks.com/blog/2023/02/01/design-patterns-batch-processing-financial-services.html
23. https://www.devmedia.com.br/inversao-de-controle-x-injecao-de-dependencia/18763
24. https://www.enterpriseintegrationpatterns.com/patterns/conversation/RequestResponse.html
25. https://www.hostgator.com.br/blog/clean-code-o-que-e/
26. https://www.infoq.com/br/articles/strangle-Pattern-strategy/
