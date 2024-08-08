# SOLID

## What is SOLID? Code more maintainable, extensible, and testable

### Single Responsibility principle

The Single Responsibility Principle (SRP) is a software design principle that states that a class should have only one reason to change. In other words, a class should have only one responsibility and should only be responsible for one thing.

The idea behind SRP is that a class that does too much can become difficult to understand, maintain, and test. By limiting a class to a single responsibility, you can make it easier to understand and modify, and reduce the likelihood of introducing bugs when making changes.

Here's an example in Java to illustrate the Single Responsibility Principle:

Suppose you have a class called Order that represents an order in an e-commerce system. This class might have methods for adding items to the order, calculating the total price of the order, and generating a receipt for the order.

However, this class violates the SRP because it has multiple responsibilities: managing the items in the order and generating a receipt. Instead, we can split this class into two separate classes: one responsible for managing the items in the order, and another responsible for generating a receipt. Here's what the code might look like:

```java
class Order {
    private List<OrderItem> items;

    public void addItem(OrderItem item) {
        items.add(item);
    }

    public double calculateTotal() {
        double total = 0;
        for (OrderItem item : items) {
            total += item.getPrice();
        }
        return total;
    }
}

class Receipt {
    private Order order;

    public Receipt(Order order) {
        this.order = order;
    }

    public String generate() {
        // Generate a receipt for the order
    }
}

class OrderItem {
    private String name;
    private double price;

    public OrderItem(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() {
        return price;
    }
}
```

With this design, the Order class is responsible only for managing the items in the order, while the Receipt class is responsible only for generating a receipt. This makes each class simpler and easier to understand, and reduces the likelihood of introducing bugs when making changes.

### Open–closed principle open to extension and closed to all modification

The Open-Closed Principle (OCP) is a software design principle that suggests that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In other words, you should be able to add new functionality to a system without changing the existing code.

The idea behind the OCP is that as software systems grow and evolve, it becomes increasingly difficult and risky to make changes to existing code. By designing your code to be open for extension but closed for modification, you can avoid the need to change existing code, thereby reducing the risk of introducing bugs or breaking existing functionality.

Here's an example in Java to illustrate the Open-Closed Principle:

Suppose you have a system that needs to render different shapes on a screen. You might have a Shape class with subclasses for different shapes, such as Circle, Square, and Triangle. Each subclass might have a method called draw() that knows how to draw that shape.

Now suppose you want to add a new shape to the system, such as a Rectangle. With the OCP, you should be able to add this new shape without changing any of the existing code. One way to achieve this is to create a new subclass of Shape called Rectangle, and implement its draw() method to draw a rectangle. Here's what the code might look like:

```java
abstract class Shape {
    abstract void draw();
}

class Circle extends Shape {
    void draw() {
        // Draw a circle
    }
}

class Square extends Shape {
    void draw() {
        // Draw a square
    }
}

class Triangle extends Shape {
    void draw() {
        // Draw a triangle
    }
}

class Rectangle extends Shape {
    void draw() {
        // Draw a rectangle
    }
}
```

With this design, you can add new shapes to the system simply by creating new subclasses of Shape, without having to modify any of the existing code. This makes the system more flexible and easier to maintain as it grows and evolves over time.

### Liskov Substitution principle

The Liskov Substitution Principle (LSP) is a fundamental principle in object-oriented programming that states that any instance of a parent class should be able to be replaced by an instance of one of its subtypes without altering the correctness of the program. In other words, any method that works for the parent class should also work for its subtypes.

The LSP is one of the SOLID principles of object-oriented design, and it helps to ensure that a software system is maintainable, extensible, and robust.

To illustrate the Liskov Substitution Principle in Java, let's consider an example involving a Shape class hierarchy:

```java
public abstract class Shape {
    public abstract double getArea();
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double getArea() {
        return width * height;
    }
}

public class Square extends Shape {
    private double side;

    public Square(double side) {
        this.side = side;
    }

    @Override
    public double getArea() {
        return side * side;
    }
}
```

In this example, we have an abstract Shape class that defines a single method, getArea(), which returns the area of the shape. We also have two concrete classes, Rectangle and Square, that inherit from Shape and provide their own implementation of getArea().

According to the Liskov Substitution Principle, we should be able to replace any instance of Shape with an instance of Rectangle or Square without affecting the correctness of the program. For example, if we have a method that takes a Shape parameter and calculates its area, it should work correctly regardless of whether the actual object passed in is a Rectangle or a Square:

```java
public double calculateTotalArea(List<Shape> shapes) {
    double totalArea = 0;
    for (Shape shape : shapes) {
        totalArea += shape.getArea();
    }
    return totalArea;
}
```

Here, the method takes a list of Shape objects and calculates the total area of all the shapes in the list by calling their getArea() method. We can use this method with a list that contains both Rectangle and Square objects, and it should work correctly thanks to the Liskov Substitution Principle:

```java
List<Shape> shapes = new ArrayList<>();
shapes.add(new Rectangle(2, 3));
shapes.add(new Square(2));

double totalArea = calculateTotalArea(shapes);
System.out.println(totalArea); // outputs "10.0"
```

In this example, we create a list that contains both a Rectangle and a Square object, and pass it to the calculateTotalArea() method. The method correctly calculates the total area of both shapes, thanks to the fact that both Rectangle and Square are subtypes of Shape and implement the getArea() method.

### Interface Segregation principle

The Interface Segregation Principle (ISP) is one of the SOLID principles of object-oriented design. It states that a client should not be forced to depend on methods it does not use. In other words, instead of creating a large interface that contains many methods, it is better to create smaller interfaces that are more specific to the needs of the client.

The ISP is particularly useful in situations where you have a large interface that is being implemented by many different classes, but each class only needs a subset of the methods provided by the interface. By breaking down the large interface into smaller, more focused interfaces, each class can implement only the methods it needs, rather than being burdened with a large number of methods that it doesn't use.

Here's an example in Java:

Suppose you have an interface called `Document` that represents a document in a word processing application. The interface has several methods such as `open()`, `close()`, `save()`, `print()`, and `cut()`. However, you have a class called `Printer` that only needs to print documents, and doesn't need to implement the other methods.

To follow the ISP, you can create a separate interface called `Printable` that contains only the `print()` method. The `Printer` class can then implement this `Printable` interface, rather than the larger `Document` interface.

Here's how the code would look like:

```java
public interface Document {
    void open();
    void close();
    void save();
    void print();
    void cut();
}

public interface Printable {
    void print();
}

public class Printer implements Printable {
    public void print() {
        // Code to print the document
    }
}
```

By separating the `print()` method into a separate interface, we have made the code more modular and flexible. Now, if we need to add a new class that only needs to save documents, we can create a separate interface called `Saveable` that contains only the `save()` method, and have that class implement the `Saveable` interface. This way, each class only depends on the methods that it needs, and the code is easier to maintain and modify in the future.

### Dependency Inversion Principle (DIP)

The Dependency Inversion Principle (DIP) is a software design principle that states that high-level modules should not depend on low-level modules. Instead, both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. In other words, the principle states that the code should depend on abstractions, not on concrete implementations.

The goal of the Dependency Inversion Principle is to create a flexible and loosely coupled system. This makes it easier to change and maintain the code over time.

One common way to implement DIP in Java is through the use of interfaces. By defining interfaces for classes and having those classes implement those interfaces, you can create a separation between the interface and the implementation. This allows for more flexibility in the system and easier testing.

Here's an example in Java:

```java
interface ILogger {
    void log(String message);
}

class ConsoleLogger implements ILogger {
    @Override
    public void log(String message) {
        System.out.println(message);
    }
}

class FileLogger implements ILogger {
    @Override
    public void log(String message) {
        // write message to a file
    }
}

class Client {
    private ILogger logger;

    public Client(ILogger logger) {
        this.logger = logger;
    }

    public void doSomething() {
        // do something
        logger.log("Something was done");
        // do something else
    }
}
```

In this example, we have an interface `ILogger` that defines a method `log`. We have two classes that implement this interface: `ConsoleLogger` and `FileLogger`. The `Client` class takes an instance of `ILogger` in its constructor, and uses that instance to log messages.

By using an interface, the `Client` class is decoupled from the specific implementation of the logger. This means that we can easily switch out the logger implementation without having to change the `Client` class. For example, we could use the `FileLogger` implementation instead of the `ConsoleLogger` implementation by passing in a `FileLogger` instance to the `Client` constructor. This makes our code more flexible and easier to maintain over time.

## Interface Segregation Principle (ISP) is not in conflict with the Single Responsibility Principle (SRP)

No, the Interface Segregation Principle (ISP) is not in conflict with the Single Responsibility Principle (SRP). While both principles are concerned with designing cohesive and maintainable software, they address different aspects of software design.

The Single Responsibility Principle states that a class should have only one reason to change. It focuses on ensuring that each class has a single, well-defined responsibility or concern. This principle promotes high cohesion and reduces coupling between classes.

On the other hand, the Interface Segregation Principle focuses on the design of interfaces between objects. It states that clients should not be forced to depend on methods they do not use. This principle aims to break large interfaces into smaller and more specific ones, which are tailored to the needs of each client. By doing so, it reduces the coupling between the clients and the implementation of the interface.

While it is true that implementing the Interface Segregation Principle can result in classes that implement multiple interfaces, each interface should still represent a single responsibility. Thus, the ISP does not conflict with the SRP. Instead, they complement each other by promoting a design that is both cohesive and modular.

## benefits of applying solid principle

The SOLID principles are a set of five design principles that help software developers create more flexible, maintainable, and extensible code. Here are some of the benefits of applying SOLID principles in software development:

1. Improved maintainability: By following SOLID principles, developers can create code that is easier to maintain over time. This is because the code is more modular and less tightly coupled, which makes it easier to make changes without affecting other parts of the codebase.

2. Increased flexibility: SOLID principles help developers create code that is more flexible and adaptable to changes in requirements. This is because the code is designed to be easily extended or modified without affecting other parts of the system.

3. Better testability: SOLID principles promote the use of interfaces and abstractions, which make it easier to write automated tests for the code. This leads to higher quality code that is more reliable and easier to debug.

4. Lower coupling: By following SOLID principles, developers can reduce the coupling between different parts of the system. This makes it easier to make changes to one part of the system without affecting other parts.

5. Higher cohesion: SOLID principles promote high cohesion, which means that code is organized into logical units that perform a single, well-defined task. This makes the code easier to understand and maintain.

Overall, applying SOLID principles in software development can lead to higher quality code that is easier to maintain, extend, and test.

## Peace of mind

Applying SOLID principles can also give developers peace of mind because they know that their code is more robust, reliable, and maintainable. When code is designed with SOLID principles in mind, it is less likely to contain bugs or have unexpected behavior, which can reduce the risk of errors or system failures.

Additionally, SOLID principles can help developers avoid technical debt, which is the accumulation of code that is difficult to maintain, understand, or change. Technical debt can be a major source of stress and frustration for developers, as it can slow down development and make it harder to deliver new features or fixes.

By following SOLID principles, developers can reduce technical debt and ensure that their codebase remains manageable and maintainable over time. This can give them greater peace of mind and confidence in their work, knowing that they have created a solid foundation for future development.

## which design patterns are strongly related to solid principles?

There is a strong relationship between design patterns and SOLID principles in software design. SOLID is an acronym for five design principles: Single Responsibility Principle (SRP), Open/Closed Principle (OCP), Liskov Substitution Principle (LSP), Interface Segregation Principle (ISP), and Dependency Inversion Principle (DIP).

Here are some design patterns that are strongly related to SOLID principles:

1. Factory Method Pattern - The Factory Method Pattern supports the Open/Closed Principle (OCP) by allowing for the addition of new product types without modifying existing code. The Factory Method Pattern also supports the Single Responsibility Principle (SRP) by encapsulating the creation of objects in a single class.

2. Dependency Injection Pattern - The Dependency Injection Pattern supports the Dependency Inversion Principle (DIP) by allowing the client to depend on abstractions rather than concrete implementations. 

3. Strategy Pattern - The Strategy Pattern supports the Open/Closed Principle (OCP) by encapsulating algorithms in separate classes, which can be added, removed, or modified without affecting the client code. 

4. Template Method Pattern - The Template Method Pattern supports the Liskov Substitution Principle (LSP) by allowing subclasses to override certain steps of an algorithm while maintaining the overall structure. 

5. Interface Segregation Principle (ISP) - The Interface Segregation Principle is not a design pattern but a principle that can guide the design of interfaces in the Adapter, Bridge, and Composite patterns. These patterns support the Interface Segregation Principle by creating small and cohesive interfaces that are specific to the client's needs.

By using these design patterns, developers can create software that is more maintainable, extensible, and testable, while adhering to SOLID principles.

## Videos
 * [SOLID Design Principles Explained in a Nutshell](https://www.youtube.com/watch?v=69sfWNzxTMc)
	> [<img src="https://img.youtube.com/vi/69sfWNzxTMc/0.jpg" width="200">](https://www.youtube.com/watch?v=69sfWNzxTMc "SOLID Design Principles Explained in a Nutshell by A Dev&#39; Story 149,629 views 6 minutes, 14 seconds")
 * [Será que o Design Pattern mais Popular quebra o SOLID? (Facade na Prática) // Mão no Código #42](https://www.youtube.com/watch?v=8tyNCznvgW8)
	> [<img src="https://img.youtube.com/vi/8tyNCznvgW8/0.jpg" width="200">](https://www.youtube.com/watch?v=8tyNCznvgW8 "Será que o Design Pattern mais Popular quebra o SOLID? (Facade na Prática) // Mão no Código #42 by Código Fonte TV 32,579 views 12 minutes, 29 seconds")
 * [Learn SOLID Principles with CLEAN CODE Examples](https://www.youtube.com/watch?v=_jDNAf3CzeY)
	> [<img src="https://img.youtube.com/vi/_jDNAf3CzeY/0.jpg" width="200">](https://www.youtube.com/watch?v=_jDNAf3CzeY "Learn SOLID Principles with CLEAN CODE Examples by Amigoscode 248,055 views 28 minutes")
 * [SOLID Principles? Nope, just Coupling and Cohesion](https://www.youtube.com/watch?v=YDNR_gfBk0Q)
	> [<img src="https://img.youtube.com/vi/YDNR_gfBk0Q/0.jpg" width="200">](https://www.youtube.com/watch?v=YDNR_gfBk0Q "SOLID Principles? Nope, just Coupling and Cohesion by CodeOpinion 30,144 views 13 minutes, 56 seconds")
 * [DDD, SOLID, Clean Code, Clean Architecture... Isto é para você?](https://www.youtube.com/watch?v=Vyb9w8lOGOI)
	> [<img src="https://img.youtube.com/vi/Vyb9w8lOGOI/0.jpg" width="200">](https://www.youtube.com/watch?v=Vyb9w8lOGOI "DDD, SOLID, Clean Code, Clean Architecture... Isto é para você? by balta.io 23,730 views 1 hour, 15 minutes")
 * [SOLID fica FÁCIL com Essas Ilustrações](https://www.youtube.com/watch?v=6SfrO3D4dHM)
	> [<img src="https://img.youtube.com/vi/6SfrO3D4dHM/0.jpg" width="200">](https://www.youtube.com/watch?v=6SfrO3D4dHM "SOLID fica FÁCIL com Essas Ilustrações by Filipe Deschamps 310,681 views 19 minutes")


## References

1. https://www.bmc.com/blogs/solid-design-principles/
2. https://medium.com/thiago-aragao/solid-princ%C3%ADpios-da-programa%C3%A7%C3%A3o-orientada-a-objetos-ba7e31d8fb25
3. https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design
4. https://profile.es/blog/principios-solid-desarrollo-software-calidad/
5. [SOLID Principles](./pdf/SOLID%20Principle.pdf)