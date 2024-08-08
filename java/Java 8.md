# Java 8

## Functional programming paradigms
Yes, functional programming features were introduced in Java 8. Some of the key functional programming features included in Java 8 are:

1. **Lambda Expressions**: These are anonymous functions that can be treated as instances of functional interfaces. They provide a clear and concise way to represent one method interface using an expression.

   ```java
   (parameters) -> expression
   ```

   Example:
   ```java
   List<String> list = Arrays.asList("one", "two", "three");
   list.forEach(s -> System.out.println(s));
   ```

2. **Functional Interfaces**: These are interfaces that have a single abstract method. Java 8 introduced several functional interfaces in the `java.util.function` package, such as `Predicate`, `Function`, `Consumer`, and `Supplier`.

   Example:
   ```java
   @FunctionalInterface
   public interface MyFunctionalInterface {
       void execute();
   }
   ```

3. **Streams API**: The Streams API allows processing sequences of elements in a functional style. It supports operations like map, filter, reduce, and collect.

   Example:
   ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   List<Integer> squaredNumbers = numbers.stream()
                                         .map(n -> n * n)
                                         .collect(Collectors.toList());
   ```

4. **Method References**: These provide a way to refer to methods of existing classes or objects in a more concise manner than using lambda expressions.

   Example:
   ```java
   list.forEach(System.out::println);
   ```

5. **Optional Class**: This is a container object which may or may not contain a non-null value. It provides methods to check the presence of a value and perform actions based on its presence.

   Example:
   ```java
   Optional<String> optional = Optional.of("Hello");
   optional.ifPresent(System.out::println);
   ```

These features greatly enhance Java's ability to support functional programming paradigms, making code more readable and concise.

## Java Optionals

The `Optional` class in Java was introduced in Java 8 as part of the `java.util` package. It is used to represent a value that may or may not be present. This helps in avoiding `null` checks and reduces the chances of encountering `NullPointerException`.

Here's a breakdown of how `Optional` can be used effectively:

### Creating an Optional

1. **Empty Optional:**
   ```java
   Optional<String> emptyOptional = Optional.empty();
   ```

2. **Optional with a Non-Null Value:**
   ```java
   Optional<String> nonEmptyOptional = Optional.of("Hello, World!");
   ```

3. **Optional with a Nullable Value:**
   ```java
   Optional<String> nullableOptional = Optional.ofNullable(someString);
   ```

### Checking the Presence of a Value

1. **isPresent():** Checks if there is a value present.
   ```java
   if (nonEmptyOptional.isPresent()) {
       System.out.println("Value is present");
   }
   ```

2. **ifPresent():** Executes a block of code if a value is present.
   ```java
   nonEmptyOptional.ifPresent(value -> System.out.println("Value: " + value));
   ```

### Retrieving the Value

1. **get():** Returns the value if present, otherwise throws `NoSuchElementException`.
   ```java
   String value = nonEmptyOptional.get();
   ```

2. **orElse():** Returns the value if present, otherwise returns the provided default value.
   ```java
   String value = nullableOptional.orElse("Default Value");
   ```

3. **orElseGet():** Returns the value if present, otherwise invokes the provided `Supplier` and returns its result.
   ```java
   String value = nullableOptional.orElseGet(() -> "Default Value");
   ```

4. **orElseThrow():** Returns the value if present, otherwise throws the provided exception.
   ```java
   String value = nullableOptional.orElseThrow(() -> new IllegalStateException("Value is absent"));
   ```

### Transforming the Value

1. **map():** Applies a function to the value if present, and returns an `Optional` of the result.
   ```java
   Optional<Integer> length = nonEmptyOptional.map(String::length);
   ```

2. **flatMap():** Similar to `map()`, but the function must return an `Optional`.
   ```java
   Optional<Integer> flatMappedLength = nonEmptyOptional.flatMap(str -> Optional.of(str.length()));
   ```

### Filtering the Value

1. **filter():** Returns an `Optional` describing the value if it matches the provided predicate, otherwise returns an empty `Optional`.
   ```java
   Optional<String> filtered = nonEmptyOptional.filter(str -> str.startsWith("H"));
   ```

### Example Usage

Here's a comprehensive example demonstrating the usage of `Optional`:

```java
public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> optional = Optional.ofNullable("Hello, Optional!");

        // Checking presence and retrieving value
        optional.ifPresent(System.out::println);

        // Transforming the value
        Optional<Integer> length = optional.map(String::length);
        length.ifPresent(len -> System.out.println("Length: " + len));

        // Providing default values
        String result = optional.orElse("Default Value");
        System.out.println(result);

        // Throwing exceptions
        try {
            String value = optional.orElseThrow(() -> new Exception("Value not present"));
            System.out.println(value);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

By using `Optional`, you can write more robust and readable code, reducing the risk of `NullPointerException` and making it clear when a value might be absent.

## Java Streams

Java Streams, introduced in Java 8, provide a modern, functional-style way to process collections of objects. They allow for efficient and concise operations on sequences of elements, such as filtering, mapping, and reducing. Here’s a detailed look at the Streams API:

### Creating a Stream

1. **From a Collection:**
   ```java
   List<String> list = Arrays.asList("a", "b", "c");
   Stream<String> stream = list.stream();
   ```

2. **From an Array:**
   ```java
   String[] array = {"a", "b", "c"};
   Stream<String> stream = Arrays.stream(array);
   ```

3. **From Values:**
   ```java
   Stream<String> stream = Stream.of("a", "b", "c");
   ```

### Stream Operations

Streams support various operations that can be categorized into intermediate and terminal operations.

#### Intermediate Operations

Intermediate operations transform a stream into another stream and are lazy, meaning they are not executed until a terminal operation is invoked.

1. **filter:** Filters elements based on a predicate.
   ```java
   Stream<String> filteredStream = stream.filter(s -> s.startsWith("a"));
   ```

2. **map:** Transforms elements using a function.
   ```java
   Stream<Integer> mappedStream = stream.map(String::length);
   ```

3. **flatMap:** Transforms elements using a function that returns a stream and flattens the result.
   ```java
   Stream<String> flatMappedStream = stream.flatMap(s -> Stream.of(s.split("")));
   ```

4. **distinct:** Removes duplicate elements.
   ```java
   Stream<String> distinctStream = stream.distinct();
   ```

5. **sorted:** Sorts elements either naturally or using a comparator.
   ```java
   Stream<String> sortedStream = stream.sorted();
   Stream<String> customSortedStream = stream.sorted(Comparator.reverseOrder());
   ```

6. **peek:** Performs an action for each element (usually for debugging).
   ```java
   Stream<String> peekedStream = stream.peek(System.out::println);
   ```

#### Terminal Operations

Terminal operations produce a result or a side-effect and trigger the processing of the stream.

1. **forEach:** Performs an action for each element.
   ```java
   stream.forEach(System.out::println);
   ```

2. **collect:** Collects elements into a collection or other container.
   ```java
   List<String> collectedList = stream.collect(Collectors.toList());
   ```

3. **reduce:** Combines elements into a single result using an associative accumulation function.
   ```java
   Optional<String> reduced = stream.reduce((s1, s2) -> s1 + s2);
   ```

4. **toArray:** Converts the stream into an array.
   ```java
   String[] array = stream.toArray(String[]::new);
   ```

5. **min and max:** Finds the minimum or maximum element according to a comparator.
   ```java
   Optional<String> minElement = stream.min(Comparator.naturalOrder());
   Optional<String> maxElement = stream.max(Comparator.naturalOrder());
   ```

6. **count:** Returns the number of elements in the stream.
   ```java
   long count = stream.count();
   ```

7. **anyMatch, allMatch, noneMatch:** Check if any, all, or none of the elements match a predicate.
   ```java
   boolean anyMatch = stream.anyMatch(s -> s.startsWith("a"));
   boolean allMatch = stream.allMatch(s -> s.length() > 1);
   boolean noneMatch = stream.noneMatch(s -> s.isEmpty());
   ```

### Example Usage

Here’s an example that demonstrates a combination of stream operations:

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class StreamExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

        // Filter, map, and collect
        List<String> result = list.stream()
                                  .filter(s -> s.startsWith("a"))
                                  .map(String::toUpperCase)
                                  .collect(Collectors.toList());

        System.out.println(result); // Output: [APPLE]

        // Reduce example
        Optional<String> concatenated = list.stream()
                                            .reduce((s1, s2) -> s1 + ", " + s2);

        concatenated.ifPresent(System.out::println); // Output: apple, banana, cherry, date, elderberry
    }
}
```

Streams in Java provide a powerful and flexible way to handle collections and sequences of data, making it easier to write concise, readable, and efficient code.

## Videos
 * [Learn Java 8 - Full Tutorial for Beginners](https://www.youtube.com/watch?v=grEKMHGYyns)
	> [<img src="https://img.youtube.com/vi/grEKMHGYyns/0.jpg" width="200">](https://www.youtube.com/watch?v=grEKMHGYyns "Learn Java 8 - Full Tutorial for Beginners by freeCodeCamp.org 3,531,196 views 9 hours, 32 minutes")
 * [Intro to Java Programming - Course for Absolute Beginners](https://www.youtube.com/watch?v=GoXwIVyNvX0)
	> [<img src="https://img.youtube.com/vi/GoXwIVyNvX0/0.jpg" width="200">](https://www.youtube.com/watch?v=GoXwIVyNvX0 "Intro to Java Programming - Course for Absolute Beginners by freeCodeCamp.org 2,704,587 views 3 hours, 48 minutes")
 * [Functional Programming in Java - Full Course](https://www.youtube.com/watch?v=rPSL1alFIjI)
	> [<img src="https://img.youtube.com/vi/rPSL1alFIjI/0.jpg" width="200">](https://www.youtube.com/watch?v=rPSL1alFIjI "Functional Programming in Java - Full Course by freeCodeCamp.org 178,676 views 2 hours, 21 minutes")
 * [Java Optionals | Crash Course](https://www.youtube.com/watch?v=1xCxoOuDZuU)
	> [<img src="https://img.youtube.com/vi/1xCxoOuDZuU/0.jpg" width="200">](https://www.youtube.com/watch?v=1xCxoOuDZuU "Java Optionals | Crash Course by Amigoscode 99,469 views 15 minutes")
 * [Java 8 Essential Training](https://www.youtube.com/watch?v=ZwGYxgic9nc)
	> [<img src="https://img.youtube.com/vi/ZwGYxgic9nc/0.jpg" width="200">](https://www.youtube.com/watch?v=ZwGYxgic9nc "Java 8 Essential Training by MrRobot VPN 699 views 6 hours, 5 minutes")
 * [How To Master Java - Java for Beginners Roadmap](https://www.youtube.com/watch?v=TE3LyYW-AHQ)
	> [<img src="https://img.youtube.com/vi/TE3LyYW-AHQ/0.jpg" width="200">](https://www.youtube.com/watch?v=TE3LyYW-AHQ "How To Master Java - Java for Beginners Roadmap by Amigoscode 580,281 views 12 minutes, 5 seconds")
 * [Building Web Services with Java EE 8: The Course Overview|packtpub.com](https://www.youtube.com/watch?v=VLdUik5W5QU)
	> [<img src="https://img.youtube.com/vi/VLdUik5W5QU/0.jpg" width="200">](https://www.youtube.com/watch?v=VLdUik5W5QU "Building Web Services with Java EE 8: The Course Overview|packtpub.com by Packt  1,368 views 3 minutes, 16 seconds")
 * [Java Streams Tutorial | 2020](https://www.youtube.com/watch?v=Q93JsQ8vcwY)
	> [<img src="https://img.youtube.com/vi/Q93JsQ8vcwY/0.jpg" width="200">](https://www.youtube.com/watch?v=Q93JsQ8vcwY "Java Streams Tutorial | 2020 by Amigoscode 249,493 views 19 minutes")

## Referencias
1. https://en.wikipedia.org/wiki/Java_version_history
2. https://github.com/RameshMF/Java-Free-Resources-By-JavaGuides/blob/master/Java%208%20Interview%20Questions%20and%20Answers.pdf
3. https://www.interviewbit.com/java-8-interview-questions/
4. 