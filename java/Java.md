# Java

## What is the difference between JAX-RS and JAX-WS?

JAX-RS and JAX-WS are both Java APIs used for building web services, but they have different focuses and purposes.

JAX-RS (Java API for RESTful Web Services) is used for building RESTful web services, which use the HTTP protocol and its methods (GET, POST, PUT, DELETE, etc.) to interact with resources over the web. JAX-RS is designed to be lightweight, easy to use, and scalable, and it provides a simple way to map HTTP requests to Java methods and objects. JAX-RS also supports the use of annotations to simplify the process of creating and configuring web services.

On the other hand, JAX-WS (Java API for XML Web Services) is used for building SOAP-based web services, which use XML messages to communicate between applications over the web. JAX-WS is designed to be more complex than JAX-RS, and it provides a more powerful set of features for building enterprise-level web services, including support for transactions, security, and reliability. JAX-WS also provides tools for generating client-side code that can be used to consume web services.

In summary, the main difference between JAX-RS and JAX-WS is that JAX-RS is used for building lightweight RESTful web services, while JAX-WS is used for building more complex SOAP-based web services with enterprise-level features.

## And compared with org.springframework.web.reactive.function.client.WebClient

The `org.springframework.web.reactive.function.client.WebClient` is a client-side API in the Spring WebFlux framework, which is used for building reactive web applications. It is designed to be used with non-blocking, asynchronous code and supports a variety of reactive programming features.

Compared to JAX-RS and JAX-WS, `WebClient` is a different type of API altogether, as it is not specifically focused on building web services, but rather on consuming them from a client perspective. `WebClient` provides a reactive way to make HTTP requests to RESTful web services, and it is designed to work well with Spring's reactive programming model.

One key advantage of using `WebClient` is that it is well-suited for building reactive, non-blocking web applications that can handle a large number of concurrent requests. `WebClient` also provides a flexible API that can be used to easily configure and customize HTTP requests, and it supports a variety of data formats and protocols.

In summary, while JAX-RS and JAX-WS are focused on building web services, `WebClient` is a client-side API designed for consuming web services in a reactive and non-blocking way, and it is part of the Spring WebFlux framework.

## Why some classes in Java need to be final?

In Java, marking a class as `final` means that it cannot be subclassed. There are several reasons why you might want to make a class `final`:

1. **Security**: By marking a class as `final`, you prevent other classes from overriding its methods, which can help maintain the integrity and security of the class's functionality. This is particularly useful for classes that are part of security-critical systems.

2. **Performance**: Final classes and methods can often be optimized more aggressively by the Java compiler and runtime environment. When the compiler knows that a method or class cannot be overridden, it can make certain assumptions and optimizations that can improve performance.

3. **Design**: Sometimes, a class is designed to have a specific role or functionality that should not be altered or extended. Marking the class as `final` communicates this design intent to other developers and prevents unintended subclassing, which could lead to misuse or misunderstanding of the class's behavior.

4. **Thread safety**: In multithreaded environments, making a class `final` can help ensure thread safety. If a class is not intended to be subclassed, it reduces the risk of concurrency issues that could arise from subclass methods being overridden in unpredictable ways.

5. **API stability**: For library or framework classes, marking classes as `final` can provide stability to the API. Once a class is marked as `final`, you guarantee that its behavior will not change due to subclassing, which can be important for maintaining backward compatibility.

However, it's important to note that making a class `final` restricts its extensibility, so it should be used judiciously. In some cases, you might want to allow subclassing for flexibility or customization.

## Why sealed interface java is need?

In Java, a sealed interface restricts which classes can implement it, providing more control over the inheritance hierarchy. This helps in designing APIs where you want to limit the extensibility of your interface to specific classes, ensuring better encapsulation and reducing the risk of unintended subclassing. It's particularly useful for creating stable and secure APIs in libraries or frameworks.

### Example

Sure, here's a simple example to illustrate the use of sealed interfaces in Java:

```java
// Sealed interface
sealed interface Shape permits Circle, Rectangle {
    double area();
}

// Subclasses
final class Circle implements Shape {
    private final double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

final class Rectangle implements Shape {
    private final double length;
    private final double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double area() {
        return length * width;
    }
}

// Attempting to create a subclass not permitted by the sealed interface
// This would result in a compilation error
// class Triangle implements Shape {
//     ...
// }
```

In this example, the `Shape` interface is declared as sealed, and it explicitly permits only the `Circle` and `Rectangle` classes to implement it. Any attempt to create a subclass of `Shape` that is not permitted, like `Triangle`, would result in a compilation error. This ensures that only specific subclasses defined within the same module or package can extend the interface, providing better control over the inheritance hierarchy.

### Example 2

Certainly! In a banking application, you might have an interface representing different types of accounts, and you may want to restrict which classes can implement this interface to ensure proper handling of different account types. Let's consider an example:

```java
// Sealed interface representing different types of bank accounts
sealed interface BankAccount permits SavingsAccount, CheckingAccount {
    double getBalance();
    void deposit(double amount);
    void withdraw(double amount);
}

// Subclasses representing specific types of bank accounts
final class SavingsAccount implements BankAccount {
    private double balance;

    public SavingsAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    @Override
    public double getBalance() {
        return balance;
    }

    @Override
    public void deposit(double amount) {
        balance += amount;
    }

    @Override
    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        } else {
            System.out.println("Insufficient funds");
        }
    }
}

final class CheckingAccount implements BankAccount {
    private double balance;

    public CheckingAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    @Override
    public double getBalance() {
        return balance;
    }

    @Override
    public void deposit(double amount) {
        balance += amount;
    }

    @Override
    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        } else {
            System.out.println("Insufficient funds");
        }
    }
}

// Example usage
public class Main {
    public static void main(String[] args) {
        BankAccount savingsAccount = new SavingsAccount(1000);
        BankAccount checkingAccount = new CheckingAccount(2000);
        
        System.out.println("Savings Account Balance: " + savingsAccount.getBalance());
        System.out.println("Checking Account Balance: " + checkingAccount.getBalance());
        
        savingsAccount.deposit(500);
        checkingAccount.withdraw(100);
        
        System.out.println("Updated Savings Account Balance: " + savingsAccount.getBalance());
        System.out.println("Updated Checking Account Balance: " + checkingAccount.getBalance());
    }
}
```

In this example, the `BankAccount` interface is declared as sealed, permitting only `SavingsAccount` and `CheckingAccount` classes to implement it. This ensures that only these specific types of bank accounts can be created, providing better control over the types of accounts within the banking system.

## Videos
 * [Value Objects in Valhalla](https://www.youtube.com/watch?v=a3VRwz4zbdw)
	> [<img src="https://img.youtube.com/vi/a3VRwz4zbdw/0.jpg" width="200">](https://www.youtube.com/watch?v=a3VRwz4zbdw "Value Objects in Valhalla #JVMLS by Java 14,168 views 51 minutes")
 * [Why you should use my high-performance-java-persistence GitHub repository](https://www.youtube.com/watch?v=U8MoOe8uMYA)
	> [<img src="https://img.youtube.com/vi/U8MoOe8uMYA/0.jpg" width="200">](https://www.youtube.com/watch?v=U8MoOe8uMYA "Why you should use my high-performance-java-persistence GitHub repository by Vlad Mihalcea 5,493 views 5 minutes, 4 seconds")
 * [Java Class Design: When to use a Primitive vs Reference Type](https://www.youtube.com/watch?v=bQG5lzlzo6I)
	> [<img src="https://img.youtube.com/vi/bQG5lzlzo6I/0.jpg" width="200">](https://www.youtube.com/watch?v=bQG5lzlzo6I "Java Class Design: When to use a Primitive vs Reference Type by Dan Vega 4,922 views 17 minutes")
	> [<img src="https://img.youtube.com/vi/5gfZDx6IOD4/0.jpg" width="200">](https://www.youtube.com/watch?v=5gfZDx6IOD4 "OpenJDK Project Wakefield - The Wayland Desktop for JDK on Linux by Java 6,651 views 27 minutes")

 * [Modern Java in Action](https://www.youtube.com/watch?v=4UDwrfTNmo4)
	> [<img src="https://img.youtube.com/vi/4UDwrfTNmo4/0.jpg" width="200">](https://www.youtube.com/watch?v=4UDwrfTNmo4 "Modern Java in Action by Java 26,290 views 50 minutes")
 * [Why you should use my high-performance-java-persistence GitHub repository](https://www.youtube.com/watch?v=U8MoOe8uMYA)
	> [<img src="https://img.youtube.com/vi/U8MoOe8uMYA/0.jpg" width="200">](https://www.youtube.com/watch?v=U8MoOe8uMYA "Why you should use my high-performance-java-persistence GitHub repository by Vlad Mihalcea 5,493 views 5 minutes, 4 seconds")
 * [Data Structures and Algorithms with Visualizations – Full Course (Java)](https://www.youtube.com/watch?v=2ZLl8GAk1X4)
	> [<img src="https://img.youtube.com/vi/2ZLl8GAk1X4/0.jpg" width="200">](https://www.youtube.com/watch?v=2ZLl8GAk1X4 "Data Structures and Algorithms with Visualizations – Full Course (Java) by freeCodeCamp.org 306,244 views 47 hours")
 * [Why you should use my high-performance-java-persistence GitHub repository](https://www.youtube.com/watch?v=U8MoOe8uMYA)
	> [<img src="https://img.youtube.com/vi/U8MoOe8uMYA/0.jpg" width="200">](https://www.youtube.com/watch?v=U8MoOe8uMYA "Why you should use my high-performance-java-persistence GitHub repository by Vlad Mihalcea 5,493 views 5 minutes, 4 seconds")
 * [Does Java 22 Kill Build Tools? - Inside Java Newscast #63](https://www.youtube.com/watch?v=q2MFE3DVkH0)
	> [<img src="https://img.youtube.com/vi/q2MFE3DVkH0/0.jpg" width="200">](https://www.youtube.com/watch?v=q2MFE3DVkH0 "Does Java 22 Kill Build Tools? - Inside Java Newscast #63 by Java 20,466 views 6 minutes, 47 seconds")
 * [5 Java Libraries You Should Know](https://www.youtube.com/watch?v=sHZGv0PklQ0)
	> [<img src="https://img.youtube.com/vi/sHZGv0PklQ0/0.jpg" width="200">](https://www.youtube.com/watch?v=sHZGv0PklQ0 "5 Java Libraries You Should Know by SivaLabs 1,516 views 15 minutes")
 * [Java and JBoss in Containers. One .war File Per Container?](https://www.youtube.com/watch?v=orZ3OvEIMT0)
	> [<img src="https://img.youtube.com/vi/kuzjX_efuDs/0.jpg" width="200">](https://www.youtube.com/watch?v=kuzjX_efuDs "Optimizing your equals() methods with Pattern Matching - JEP Cafe #21 by Java 14,496 views 32 minutes")

 * [Java&#39;s Hidden Gems: Tools and Libraries by Johan Janssen](https://www.youtube.com/watch?v=ABm0KhsZJ0c)
	> [<img src="https://img.youtube.com/vi/ABm0KhsZJ0c/0.jpg" width="200">](https://www.youtube.com/watch?v=ABm0KhsZJ0c "Java&#39;s Hidden Gems: Tools and Libraries by Johan Janssen by Devoxx UK 13,000 views 50 minutes")
 * [Alcançando a especialização em backend Java: O passo a passo](https://www.youtube.com/watch?v=XYNu7WX7_a0)
	> [<img src="https://img.youtube.com/vi/XYNu7WX7_a0/0.jpg" width="200">](https://www.youtube.com/watch?v=XYNu7WX7_a0 "Alcançando a especialização em backend Java: O passo a passo by Elder Moraes 17,557 views 18 minutes")
 * [Brian Goetz Answers Your Java Questions](https://www.youtube.com/watch?v=mE4iTvxLTC4)
	> [<img src="https://img.youtube.com/vi/mE4iTvxLTC4/0.jpg" width="200">](https://www.youtube.com/watch?v=mE4iTvxLTC4 "Brian Goetz Answers Your Java Questions by Java 15,233 views 33 minutes")

 * [Know your Java? By Venkat Subramaniam](https://www.youtube.com/watch?v=q2T9NlROLqw)
	> [<img src="https://img.youtube.com/vi/q2T9NlROLqw/0.jpg" width="200">](https://www.youtube.com/watch?v=q2T9NlROLqw "Know your Java? By Venkat Subramaniam by Devoxx 23,607 views 2 hours, 39 minutes")

 * [Asynchronous Programming in Java: Options to Choose from By Venkat Subramaniam](https://www.youtube.com/watch?v=1zSF1259s6w)
	> [<img src="https://img.youtube.com/vi/1zSF1259s6w/0.jpg" width="200">](https://www.youtube.com/watch?v=1zSF1259s6w "Asynchronous Programming in Java: Options to Choose from By Venkat Subramaniam by Devoxx 31,601 views 2 hours, 39 minutes")
 * [CHEGOU A HORA DE FALAR DE JAVA!](https://www.youtube.com/watch?v=BoHl-grg8JU)
	> [<img src="https://img.youtube.com/vi/BoHl-grg8JU/0.jpg" width="200">](https://www.youtube.com/watch?v=BoHl-grg8JU "CHEGOU A HORA DE FALAR DE JAVA! by Código Fonte TV 131,844 views 28 minutes")
 * [Java Functional Programming | Full Course](https://www.youtube.com/watch?v=VRpHdSFWGPs)
	> [<img src="https://img.youtube.com/vi/VRpHdSFWGPs/0.jpg" width="200">](https://www.youtube.com/watch?v=VRpHdSFWGPs "Java Functional Programming | Full Course by Amigoscode 544,976 views 2 hours, 22 minutes")
 * [What is the Java Job delusion?](https://www.youtube.com/watch?v=BAJnCbavUcU)
	> [<img src="https://img.youtube.com/vi/BAJnCbavUcU/0.jpg" width="200">](https://www.youtube.com/watch?v=BAJnCbavUcU "What is the Java Job delusion? by Stefan Mischook 92,252 views 12 minutes, 23 seconds")
 * [Porque Não Uso Mais Java (Como Senior Mobile Developer)](https://www.youtube.com/watch?v=tQTdBxoNhXA)
	> [<img src="https://img.youtube.com/vi/tQTdBxoNhXA/0.jpg" width="200">](https://www.youtube.com/watch?v=tQTdBxoNhXA "Porque Não Uso Mais Java (Como Senior Mobile Developer) by Tiago Aguiar 4,834 views 1 minute, 45 seconds")
 * [COMO UTILIZAR LOG NO JAVA | Feltex](https://www.youtube.com/watch?v=52iaLpBkYC0)
	> [<img src="https://img.youtube.com/vi/52iaLpBkYC0/0.jpg" width="200">](https://www.youtube.com/watch?v=52iaLpBkYC0 "COMO UTILIZAR LOG NO JAVA | Feltex by Feltex 8,699 views 17 minutes")
 * [Como instalar o IntelliJ IDEA no Windows | Para Programar em Java e Kotlin - Guia Completo](https://www.youtube.com/watch?v=xRBd2l580Ac)
	> [<img src="https://img.youtube.com/vi/xRBd2l580Ac/0.jpg" width="200">](https://www.youtube.com/watch?v=xRBd2l580Ac "Como instalar o IntelliJ IDEA no Windows | Para Programar em Java e Kotlin - Guia Completo by Stack Mobile 36,316 views 18 minutes")
 * [What does it take to enter the market as a Java Developer?](https://www.youtube.com/watch?v=ilxcVePPH64)
	> [<img src="https://img.youtube.com/vi/ilxcVePPH64/0.jpg" width="200">](https://www.youtube.com/watch?v=ilxcVePPH64 "What does it take to enter the market as a Java Developer? by Michelli Brito 69,055 views 9 minutes, 34 seconds")
 * [O que está Acontecendo com o Java?!](https://www.youtube.com/watch?v=Zs68mVQ-Tuk)
	> [<img src="https://img.youtube.com/vi/Zs68mVQ-Tuk/0.jpg" width="200">](https://www.youtube.com/watch?v=Zs68mVQ-Tuk "O que está Acontecendo com o Java?! by Área Tech Brasil 42,522 views 6 minutes, 33 seconds")
 * [Drools Tutorial Part - 1| Java based Rule Engine | Drools getting started | Basic Setup](https://www.youtube.com/watch?v=zQhDe_PT60Y)
	> [<img src="https://img.youtube.com/vi/zQhDe_PT60Y/0.jpg" width="200">](https://www.youtube.com/watch?v=zQhDe_PT60Y "Drools Tutorial Part - 1| Java based Rule Engine | Drools getting started | Basic Setup by Binod Suman Academy 60,278 views 17 minutes")
 * [5 Java concepts you MUST KNOW!!](https://www.youtube.com/watch?v=BJxozKJlDvg)
	> [<img src="https://img.youtube.com/vi/BJxozKJlDvg/0.jpg" width="200">](https://www.youtube.com/watch?v=BJxozKJlDvg "5 Java concepts you MUST KNOW!! by Amigoscode 294,364 views 11 minutes, 50 seconds")
 * [Should you still LEARN Java in 2023](https://www.youtube.com/watch?v=9yzMKaKcoC0)
	> [<img src="https://img.youtube.com/vi/9yzMKaKcoC0/0.jpg" width="200">](https://www.youtube.com/watch?v=9yzMKaKcoC0 "Should you still LEARN Java in 2023 by Amigoscode 257,726 views 8 minutes, 16 seconds")
 * [TDD na prática com Java usando @MockBean](https://www.youtube.com/watch?v=4VmbETu-dcA)
	> [<img src="https://img.youtube.com/vi/4VmbETu-dcA/0.jpg" width="200">](https://www.youtube.com/watch?v=4VmbETu-dcA "TDD na prática com Java usando @MockBean by Michelli Brito 46,059 views 27 minutes")











