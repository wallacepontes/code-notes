# Spring Tips: Spring Boot 3.2

## Data Oriented Programming in Java 21

## Visitor Pattern

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

## Reference

1. [[Backend] Spring Tips: Spring Boot 3.2.md](https://www.youtube.com/watch?v=dMhpDdR6nHw)

2. [Data Oriented Programming in Java](https://www.infoq.com/articles/data-oriented-programming-java/)

3. Visitor Pattern (https://en.wikipedia.org/wiki/Visitor_pattern)