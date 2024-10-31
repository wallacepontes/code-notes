# Java Records & Projections

## Table of Contents

- [Java Records \& Projections](#java-records--projections)
  - [Table of Contents](#table-of-contents)
  - [Records](#records)
    - [Java Records: When \& Why to use them](#java-records-when--why-to-use-them)
  - [Lombok](#lombok)
    - [Lombok can create constructors, getters, equals(), hashCode(), and toString() methods automatically.](#lombok-can-create-constructors-getters-equals-hashcode-and-tostring-methods-automatically)
    - [If you really insist on using Lombok](#if-you-really-insist-on-using-lombok)
      - [Example without `@Data`](#example-without-data)
      - [Breakdown](#breakdown)
      - [Benefits](#benefits)
      - [Example with Field Access](#example-with-field-access)
      - [Conclusion](#conclusion)
  - [Projections](#projections)
    - [How you can use projections with Spring Data JPA?](#how-you-can-use-projections-with-spring-data-jpa)
    - [Any completement from this article https://medium.com/@bubu.tripathy/spring-data-jpa-projections-cc5f9289089f](#any-completement-from-this-article-httpsmediumcombubutripathyspring-data-jpa-projections-cc5f9289089f)
  - [JPA does not require setters/getters anymore.](#jpa-does-not-require-settersgetters-anymore)
    - [Key Points](#key-points)
    - [Pros and Cons](#pros-and-cons)
  - [References](#references)

## Records

### Java Records: When & Why to use them

Java records are a feature introduced in Java 14 as a preview feature and became a permanent feature starting from Java 16. They provide a concise way of declaring classes that are essentially data carriers, holding immutable data. Records are immutable by default, meaning once they are created, their state cannot be changed. They are declared using a compact syntax, which automatically generates methods such as constructors, accessors, `equals()`, `hashCode()`, and `toString()` methods, making them suitable for modeling simple data aggregates.

Here's a basic example of a Java record:

```java
public record Person(String name, int age) {}
```

With this declaration, the Java compiler will automatically generate the constructor, accessors for the `name` and `age` fields, `equals()`, `hashCode()`, and `toString()` methods.

Why do we need Java records?

1. **Conciseness**: Java records reduce boilerplate code by providing a compact syntax for declaring simple data carrier classes.

2. **Readability**: They improve code readability by clearly expressing the intention that a class is meant to hold data and nothing else.

3. **Immutability**: Records are immutable by default, which helps in writing safer and more predictable code, especially in multi-threaded environments.

4. **Built-in Methods**: Records automatically generate useful methods like `equals()`, `hashCode()`, and `toString()`, saving developers from writing these methods manually.

5. **Pattern Matching**: While not a direct feature of records, they play well with pattern matching introduced in Java 16, making it easier to destructure and handle data.

Overall, Java records provide a more concise and readable way to define classes that are essentially just containers for data, reducing the verbosity and boilerplate typically associated with such classes in Java.

## Lombok 

### Lombok can create constructors, getters, equals(), hashCode(), and toString() methods automatically.

Yes, you're correct. Lombok is a popular library in the Java ecosystem that helps reduce boilerplate code by automatically generating common methods like constructors, getters, setters, `equals()`, `hashCode()`, and `toString()` methods. It achieves this through the use of annotations.

Here's how you might use Lombok to create a similar class to the earlier Java record example:

```java
import lombok.Data;

@Data
public class Person {
    private final String name;
    private final int age;
}
```

In this example, the `@Data` annotation instructs Lombok to generate getters, `equals()`, `hashCode()`, and `toString()` methods for the `name` and `age` fields. It also generates an all-args constructor.

While Lombok and Java records both address the issue of boilerplate code, they have some differences:

1. **Language Support**: Java records are part of the Java language itself since Java 14 (as a preview feature) and are fully integrated into the language specification starting from Java 16. Lombok, on the other hand, is a third-party library that requires additional dependencies in your project.

2. **Mutability**: Java records are immutable by default, meaning their state cannot be changed after instantiation, whereas with Lombok, mutability depends on how the class is defined. By default, Lombok generates mutable classes, but you can use specific annotations like `@Value` to make them immutable.

3. **Standardization**: Java records are a standardized language feature, meaning they have clear semantics defined by the Java language specification. Lombok, being a library, is not part of the official Java specification and may have differences in behavior or compatibility across different versions or implementations.

Both Java records and Lombok provide ways to reduce boilerplate code, and the choice between them often depends on factors such as project requirements, team preferences, and compatibility concerns.

### If you really insist on using Lombok

Absolutely, I understand the concerns regarding the `@Data` annotation in Lombok. Using `@Data` can indeed be overkill as it generates more than just getters and setters; it includes equals, hashCode, and toString methods, which might not always be desirable.

Instead, Lombok offers more fine-grained annotations such as `@Getter`, `@Setter`, `@NoArgsConstructor`, and `@AllArgsConstructor`, which allow you to generate only what you need. This provides better control and prevents the unnecessary generation of potentially harmful methods.

#### Example without `@Data`

Here’s how you can use Lombok without `@Data` to ensure you only generate what’s necessary:

```java
import lombok.Getter;
import lombok.Setter;
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Person {
    @Id
    @Getter @Setter
    private Long id;

    @Getter @Setter
    private String name;
}
```

#### Breakdown

- `@Getter` and `@Setter`: These annotations are used to generate only the getter and setter methods for each field.
- `@NoArgsConstructor`: Generates a no-argument constructor.
- `@AllArgsConstructor`: Generates a constructor with one argument for each field in the class.

#### Benefits

- **Control**: By using specific Lombok annotations, you have better control over what methods are generated.
- **Simplicity**: Only the necessary methods are generated, reducing potential issues with unintentional method generation.
- **Maintainability**: The class remains clean and maintainable without unnecessary boilerplate code.

#### Example with Field Access

If you prefer to avoid using Lombok entirely and rely on field access with JPA, here’s how you can structure your entity:

```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Person {
    @Id
    private Long id;
    private String name;

    // No explicit getters and setters needed if you use field access with JPA
}
```

#### Conclusion

While Lombok can simplify code by reducing boilerplate, it’s important to use it judiciously. Avoiding `@Data` in favor of more specific annotations like `@Getter` and `@Setter` provides better control and adheres to good coding practices. For projects where you want even more control or to avoid dependencies, relying on JPA's field access can be a viable option.

## Projections

### How you can use projections with Spring Data JPA?

In Spring Data JPA, projections allow you to define interfaces to represent partial views of your entities. Projections are useful when you only need a subset of fields from an entity, or when you want to combine fields from multiple entities into a single view. Here's how you can use projections with Spring Data JPA:

1. **Define Projection Interfaces**: Create interfaces that define the fields you want to project. These interfaces can include getter methods corresponding to the fields you want to retrieve.

```java
public interface UserProjection {
    String getUsername();
    String getEmail();
}
```

2. **Use Projections in Repository Methods**: In your repository interfaces, declare query methods that return instances of your projection interfaces.

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    // Using JPQL
    @Query("SELECT u.username AS username, u.email AS email FROM User u")
    List<UserProjection> findAllProjectedBy();
    
    // Using Spring Data JPA derived query
    List<UserProjection> findByActiveTrue();
}
```

3. **Access Projection Data**: When you call these repository methods, you'll receive instances of your projection interfaces. You can then access the projected fields using the getter methods defined in the projection interface.

```java
List<UserProjection> users = userRepository.findAllProjectedBy();
for (UserProjection user : users) {
    String username = user.getUsername();
    String email = user.getEmail();
    // Do something with the projected data
}
```

4. **Custom Query Methods**: You can define custom query methods in your repository interfaces using JPQL, native queries, or derived queries. These methods can return instances of your projection interfaces.

```java
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u.username AS username, u.email AS email FROM User u WHERE u.active = true")
    List<UserProjection> findActiveUsersProjectedByJPQL();
}
```

By using projections in Spring Data JPA, you can efficiently retrieve only the data you need, which can improve performance and reduce memory usage, especially when dealing with large datasets.

### Any completement from this article https://medium.com/@bubu.tripathy/spring-data-jpa-projections-cc5f9289089f

The article you provided offers a detailed explanation of projections in Spring Data JPA and provides a few additional insights. Here are some points that complement the information provided in the article:

1. **Dynamic Projections**: The article explains how to create dynamic projections using Spring Data JPA's interface-based projections. It demonstrates how you can achieve dynamic projections by defining multiple projection interfaces and selecting the appropriate one based on certain conditions. This technique can be very useful when you have varying requirements for the data you need to fetch.

2. **Using DTOs (Data Transfer Objects)**: While the article primarily focuses on interface-based projections, it briefly mentions the use of DTOs as an alternative approach. DTOs are standalone classes that represent a subset of data from one or more entities. Unlike interface-based projections, DTOs can be more flexible in terms of data transformation and can encapsulate additional logic if needed.

3. **Named Queries**: The article introduces the concept of named queries in Spring Data JPA and demonstrates how to use them with projections. Named queries provide a way to define JPQL queries in repository interfaces using the `@Query` annotation with a name attribute. This can help improve code readability and maintainability by centralizing query definitions.

4. **Projections with Criteria API**: While the article focuses on JPQL-based queries for defining projections, it's worth mentioning that Spring Data JPA also supports using the Criteria API for building dynamic queries, including projections. The Criteria API provides a type-safe and programmatic way to construct queries, which can be beneficial in certain scenarios, especially when query criteria are determined dynamically at runtime.

5. **Performance Considerations**: Although not explicitly mentioned in the article, it's important to consider performance implications when using projections, especially when fetching large datasets. Fetching only the required fields can improve performance by reducing data transfer overhead, but it's essential to monitor query execution times and database performance to ensure that projections are not causing any bottlenecks.

Overall, the article provides a comprehensive overview of projections in Spring Data JPA and demonstrates various techniques for defining and using projections effectively in your applications.

## JPA does not require setters/getters anymore.

Java Persistence API (JPA) traditionally required the use of getters and setters for mapping entity fields to database columns. However, recent updates and best practices have evolved, providing more flexibility. As of the latest versions and with the adoption of more advanced frameworks like Hibernate, Lombok, and records in Java, explicit getters and setters are not strictly necessary in all cases.

### Key Points

1. **Default Access Type**: JPA can use field access rather than property access. This means it can directly access fields via reflection without requiring getters and setters.

    ```java
    @Entity
    public class Person {
        @Id
        private Long id;
        
        private String name;

        // Getters and setters are not strictly required
    }
    ```

2. **Lombok**: Lombok can generate getters, setters, and other boilerplate code at compile-time, making the source code cleaner and easier to maintain.

    ```java
    @Entity
    @Data
    public class Person {
        @Id
        private Long id;
        
        private String name;
    }
    ```

    The `@Data` annotation generates all the necessary getters and setters among other things.

3. **Records**: Java 14 introduced records, which are a concise way to model immutable data classes. JPA support for records is improving, although it may not be fully integrated into all JPA providers yet.

    ```java
    @Entity
    public record Person(@Id Long id, String name) {}
    ```

4. **Direct Field Access**: Some JPA providers allow direct access to fields. By using annotations on fields instead of methods, you can avoid the need for explicit getters and setters.

    ```java
    @Entity
    public class Person {
        @Id
        private Long id;
        
        @Column(name = "name")
        private String name;
    }
    ```

### Pros and Cons

- **Pros**:
  - **Cleaner Code**: Reduces boilerplate code, making the codebase cleaner and more readable.
  - **Performance**: Direct field access can be slightly faster since it avoids the overhead of method calls.
  - **Simplicity**: Easier for small projects or when using advanced features like Lombok or records.

- **Cons**:
  - **Encapsulation**: Bypassing getters and setters can violate encapsulation principles, making the code harder to maintain or evolve.
  - **Tooling**: Some tools and libraries might expect getters and setters, leading to compatibility issues.
  - **Validation and Logic**: Getters and setters are often used to include validation or additional logic which can’t be included if they are omitted.

In conclusion, while JPA does not strictly require getters and setters anymore, whether to use them depends on the specific requirements of your project and the tools and frameworks you are using. For maximum compatibility and maintainability, many developers still prefer using them, but for cleaner and more modern codebases, omitting them with appropriate strategies (like Lombok or records) is becoming more common.

## References

1. [Spring Data JPA Projections with Code Examples](https://medium.com/@bubu.tripathy/spring-data-jpa-projections-cc5f9289089f)

2. [Java Records: When & Why to use them](https://medium.com/javarevisited/java-records-when-why-to-use-them-ebd48de637b6)

3. [How to use Java Records with Spring Data JPA](https://vladmihalcea.com/records-spring-data-jpa/)

4. [Java Records – A Beginner’s Guide](https://vladmihalcea.com/java-records-guide/)

5. [JAVA RECORDS](https://www.happycoders.eu/java/records/)