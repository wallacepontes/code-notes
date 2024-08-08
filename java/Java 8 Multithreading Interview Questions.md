# Java 8 Multithreading Interview Questions

## 1. What are the new features introduced in Java 8 for multithreading?

Java 8 introduced several key features that significantly improved the ease and efficiency of working with multithreading:

### Java 8

1. **Lambda Expressions**:
   - Simplified the process of writing anonymous classes that implement interfaces, making it easier to create instances of `Runnable` and other functional interfaces.
   - Enabled concise and readable multithreaded code.

2. **Streams API**:
   - Allows functional-style operations on collections and other data sources.
   - Supports parallel processing with `parallelStream()`, which splits the source data and processes it in parallel, taking advantage of multiple cores for improved performance.

3. **CompletableFuture**:
   - A flexible Future implementation that supports non-blocking, asynchronous programming.
   - Provides a rich API for combining multiple asynchronous tasks, handling results, and managing errors.
   - Methods like `thenApply`, `thenAccept`, `thenCompose`, and `thenCombine` make it easy to build complex asynchronous workflows.

4. **Enhancements to the Executor Framework**:
   - **ForkJoinPool**: Improved the existing `ForkJoinPool` introduced in Java 7, which is designed for work-stealing algorithms, making it more efficient for divide-and-conquer tasks.
   - The common pool, a shared instance of `ForkJoinPool`, can be used by default for parallel streams and other parallel operations.

### Detailed Overview

#### Lambda Expressions
```java
// Example of using lambda with Runnable
Runnable task = () -> {
    System.out.println("Task is running in a separate thread");
};
new Thread(task).start();
```

#### Streams API
```java
// Example of using parallelStream
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
int sum = numbers.parallelStream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(Integer::intValue)
                 .sum();
System.out.println("Sum of even numbers: " + sum);
```

#### CompletableFuture
```java
// Example of using CompletableFuture
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    // Simulate long-running task
    try {
        Thread.sleep(2000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("Task completed");
});

future.thenRun(() -> System.out.println("Continuation task"));
```

#### ForkJoinPool
```java
// Example of using ForkJoinPool
ForkJoinPool pool = new ForkJoinPool();
pool.submit(() -> {
    IntStream.range(1, 100).parallel().forEach(i -> {
        System.out.println(Thread.currentThread().getName() + " " + i);
    });
}).join();
```

These features have made it significantly easier to write, manage, and understand multithreaded code in Java, promoting more efficient use of system resources and better performance in concurrent applications.

## 2. How does the ForkJoinPool class work?

Indeed, the `ForkJoinPool` is a crucial component of the Java concurrency framework, designed to efficiently handle tasks that can be broken down into smaller subtasks. Let's delve deeper into the mechanics and benefits of the `ForkJoinPool`.

### Key Concepts of ForkJoinPool

1. **Fork-Join Framework**:
   - **Fork**: A task splits itself into smaller subtasks (recursively if necessary).
   - **Join**: The subtasks are executed and their results are combined to produce the final result.

2. **Work Stealing**:
   - When a worker thread completes its tasks, it can "steal" tasks from the queues of other worker threads that are still busy.
   - This helps in balancing the load across all available threads, ensuring that all CPU cores are utilized effectively.

### Example of ForkJoinPool

Here's an example demonstrating the `ForkJoinPool` with a simple task that recursively calculates the sum of an array of numbers:

```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

class SumTask extends RecursiveTask<Integer> {
    private static final int THRESHOLD = 20; // Threshold for splitting tasks
    private final int[] array;
    private final int start;
    private final int end;

    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= THRESHOLD) {
            int sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            int mid = (start + end) / 2;
            SumTask leftTask = new SumTask(array, start, mid);
            SumTask rightTask = new SumTask(array, mid, end);
            leftTask.fork(); // Split into subtasks
            int rightResult = rightTask.compute(); // Compute the right task directly
            int leftResult = leftTask.join(); // Wait for the left task to complete
            return leftResult + rightResult;
        }
    }
}

public class ForkJoinExample {
    public static void main(String[] args) {
        int[] array = new int[1000];
        for (int i = 0; i < array.length; i++) {
            array[i] = i + 1; // Fill array with values 1 to 1000
        }
        
        ForkJoinPool pool = new ForkJoinPool();
        SumTask task = new SumTask(array, 0, array.length);
        
        int result = pool.invoke(task);
        System.out.println("Sum: " + result); // Should print the sum of numbers from 1 to 1000
    }
}
```

### Benefits of ForkJoinPool

1. **Efficient Resource Utilization**:
   - By breaking tasks into smaller chunks and using work-stealing, `ForkJoinPool` maximizes CPU utilization.

2. **Scalability**:
   - As the number of tasks increases, the pool can efficiently distribute work among available threads, making it suitable for large-scale parallel processing.

3. **Simplifies Parallel Processing**:
   - The `RecursiveTask` and `RecursiveAction` classes provide a simple API for dividing tasks, making it easier to implement complex parallel algorithms.

### Practical Use Cases

1. **Parallel Sorting Algorithms**:
   - Implementing parallel versions of sorting algorithms like quicksort or mergesort.

2. **Matrix Multiplications**:
   - Efficiently multiplying large matrices by breaking them into smaller submatrices.

3. **Parallel Search Algorithms**:
   - Searching large datasets by dividing the search space among multiple threads.

By leveraging the `ForkJoinPool`, developers can write highly efficient, scalable, and maintainable concurrent applications in Java, taking full advantage of modern multi-core processors.

## References
1. https://www.javatpoint.com/java-8-multithreading-interview-questions
2. 