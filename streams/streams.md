# Section Review: Using Streams

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using Streams" section.

-----

## Summary 🌊

This section introduces Java Streams, a sequence of data processed through a pipeline of operations. It covers the conceptual flow, creation of streams (finite and infinite), and common terminal and intermediate operations.

1.  **Understanding the Stream Pipeline Flow:**

    * A stream pipeline processes a sequence of data, analogous to an assembly line.
    * **Three Parts:**
        1.  **Source:** Where the stream originates (e.g., a Collection, an array, I/O channel, generator function).
        2.  **Intermediate Operations (Zero or More):** Transform a stream into another stream (e.g., `filter()`, `map()`, `sorted()`). They are **lazy**; they don't execute until a terminal operation is invoked.
        3.  **Terminal Operation (Exactly One):** Produces a result or a side effect (e.g., `count()`, `forEach()`, `collect()`). Consumes the stream, meaning the stream cannot be reused after a terminal operation.
    * **Lazy Evaluation:** Intermediate operations are not performed until the terminal operation is called. Data is processed on demand, element by element through the pipeline where possible.
    * **Short-circuiting:** Some operations (e.g., `limit()`, `findAny()`, `anyMatch()`) can allow the pipeline to complete without processing all elements from the source, especially with infinite streams.

2.  **Differences: Intermediate vs. Terminal Operations (Table 10.2):**

    * **Intermediate:** Not required for a useful pipeline, can be multiple, returns a stream, lazy execution, stream still valid after call.
    * **Terminal:** Required, only one, returns non-stream (or void), eager execution (triggers pipeline), stream invalid after call.

3.  **Creating Stream Sources (`java.util.stream.Stream<T>`):**

    * **Finite Streams:**
        * `Stream.empty()`: Creates an empty stream.
        * `Stream.of(T... values)`: Creates a stream from a varargs array of elements.
        * `Stream.of(T t)`: Creates a stream with a single element.
        * `collection.stream()`: Creates a sequential stream from any `Collection`.
        * `collection.parallelStream()`: Creates a parallel stream from a `Collection` (allows concurrent processing, but with coordination overhead).
    * **Infinite Streams:**
        * `Stream.generate(Supplier<T> s)`: Creates an infinite stream by repeatedly calling the `Supplier`'s `get()` method.
        * `Stream.iterate(T seed, UnaryOperator<T> f)`: Creates an infinite stream starting with `seed`, then `f(seed)`, `f(f(seed))`, and so on.
        * `Stream.iterate(T seed, Predicate<T> hasNext, UnaryOperator<T> next)` (Java 9+): Creates a stream that can be finite or infinite. Starts with `seed`, continues as long as `hasNext` is true for the current element, generating the next element using `next`.
    * Printing a stream object directly (e.g., `System.out.print(stream)`) shows an internal representation, not the elements.

4.  **Common Terminal Operations (Table 10.4):**

    * **Reductions (combine stream into single result):**
        * `long count()`: Returns number of elements. (Hangs on infinite streams).
        * `Optional<T> min(Comparator)`, `Optional<T> max(Comparator)`: Return min/max element. (Hangs on infinite streams).
        * `T reduce(T identity, BinaryOperator<T> accumulator)`
        * `Optional<T> reduce(BinaryOperator<T> accumulator)`
        * `<U> U reduce(U identity, BiFunction<U,? super T,U> accumulator, BinaryOperator<U> combiner)`: For combining elements, potentially in parallel. (Hangs on infinite streams).
        * `<R,A> R collect(Collector<? super T, A,R> collector)`
        * `<R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner)`: Mutable reduction, e.g., collecting into a `List` or `StringBuilder`. (Hangs on infinite streams).
    * **Non-Reductions (may not process all elements, or return void):**
        * `Optional<T> findAny()`, `Optional<T> findFirst()`: Return an element. (Can terminate on infinite streams).
        * `boolean allMatch(Predicate)`, `boolean anyMatch(Predicate)`, `boolean noneMatch(Predicate)`: Test elements against a predicate. (Sometimes terminate on infinite streams).
        * `void forEach(Consumer)`: Performs an action for each element. (Hangs on infinite streams). A stream cannot be used in an enhanced for-loop.

5.  **Common Intermediate Operations:**

    * Return a new stream and are lazy.
    * `Stream<T> filter(Predicate<? super T> predicate)`: Selects elements matching the predicate.
    * `Stream<T> distinct()`: Removes duplicate elements (using `equals()`).
    * `Stream<T> limit(long maxSize)`: Truncates the stream. Can make an infinite stream finite.
    * `Stream<T> skip(long n)`: Discards the first `n` elements.
    * `<R> Stream<R> map(Function<? super T, ? extends R> mapper)`: Transforms each element one-to-one.
    * `<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)`: Transforms each element into a stream of zero or more elements, then flattens these streams into a single stream.
    * `Stream<T> sorted()`, `Stream<T> sorted(Comparator<? super T> comparator)`: Sorts elements. (Will hang on an un-limited infinite stream if it needs all elements to sort).
    * `Stream<T> peek(Consumer<? super T> action)`: Performs an action on each element as it passes through, primarily for debugging. Should not be used for side effects that modify the stream's data source if it affects the pipeline result.
    * **Static `Stream.concat(Stream a, Stream b)`:** Concatenates two streams.

6.  **Putting Together the Pipeline:**

    * Operations are chained. Example: `list.stream().filter().sorted().limit().forEach()`.
    * Lazy evaluation and short-circuiting (like `limit()`) optimize processing.
    * The order of operations matters, especially with infinite streams (e.g., `limit()` should usually come before `sorted()` on an infinite stream).
    * A pipeline can be broken into parts by collecting to an intermediate collection and then creating a new stream from it.

-----

## Conceptual Questions

1.  What are the three main parts of a stream pipeline, and what is the role of each?
2.  Explain the concept of "lazy evaluation" in the context of stream intermediate operations. When do these operations actually execute?
3.  What is the key difference between an intermediate operation and a terminal operation in terms of what they return and whether the stream can be reused?
4.  List three ways to create a finite `Stream` and two ways to create an infinite `Stream` in Java.
5.  What is the main difference between `collection.stream()` and `collection.parallelStream()`?
6.  Why do terminal operations like `count()`, `min()`, and `max()` generally not terminate when called on an infinite stream, while operations like `findAny()` or `anyMatch()` might?
7.  What does the `reduce()` terminal operation achieve? Describe its simplest form (identity and accumulator).
8.  What is a "mutable reduction," and which terminal operation is typically used for it? Give an example of what you might use it for.
9.  Explain the difference between the `map()` and `flatMap()` intermediate operations. When would you use `flatMap()`?
10. Why is the order of operations in a stream pipeline important, especially when dealing with infinite streams or operations like `sorted()` and `limit()`?
11. What is the purpose of the `peek()` intermediate operation, and what is a common caution regarding its use?

-----

## Scenario-Based Questions

1.  If you have a `List<String> names`, and you call `names.stream().filter(s -> s.startsWith("A")).count()`, is `filter()` an intermediate or terminal operation? What about `count()`?
2.  You create an infinite stream using `Stream.generate(() -> "hello")`. If you chain `.limit(5).forEach(System.out::println)` to it, will the program hang? Why or why not?
3.  What if you call `Stream.generate(() -> "hello").sorted().forEach(System.out::println)`? What would happen?
4.  You have a `Stream<String> words`. You want to get a `List<Integer>` containing the lengths of each word. Which intermediate operation would you primarily use?
5.  If you want to find if *any* string in a `Stream<String>` is longer than 10 characters, which terminal operation would be most efficient?
6.  Consider `Stream.iterate(0, n -> n < 10, n -> n + 1)`. Is this stream finite or infinite? What values will it produce?
7.  If you have `Stream.of(List.of(1,2), List.of(3,4), List.of(5))`, and you want a single stream of integers `Stream<Integer>` containing `1, 2, 3, 4, 5`, which intermediate operation would be most suitable?

-----

## Coding Problems

1.  **Simple Stream Pipeline:**
    * Given a `List<String> names = List.of("Alice", "Bob", "Charlie", "David", "Eve", "Anna");`.
    * Write a stream pipeline to:
        * Filter names that start with "A".
        * Convert these names to uppercase.
        * Collect the results into a new `List<String>`.
        * Print the resulting list.
2.  **Infinite Stream with Limit:**
    * Create an infinite stream of integers starting from 1 and incrementing by 1 (`Stream.iterate`).
    * Use intermediate operations to:
        * Keep only the even numbers.
        * Take the first 10 such even numbers.
    * Use a terminal operation to print each of these 10 even numbers.
3.  **Reduction Operation:**
    * Given a `List<Integer> numbers = List.of(1, 2, 3, 4, 5);`.
    * Use the `reduce()` method to calculate the sum of all numbers in the list.
    * Use the `reduce()` method to calculate the product of all numbers in the list.
4.  **Using `flatMap()`:**
    * You have a `List<List<String>> nestedLists = List.of(List.of("a", "b"), List.of("c", "d", "e"), List.of("f"));`.
    * Use `flatMap()` to convert this into a single `Stream<String>` (or `List<String>`) containing all individual strings ("a", "b", "c", "d", "e", "f").
    * Print the elements of the flattened stream.
5.  **`peek()` for Debugging (Conceptual):**
    * Write a stream pipeline for a `List<String>` that filters strings longer than 3 characters, then maps them to their lengths, and then finds the maximum length.
    * Describe where you would insert `peek()` operations (and what they would print) to debug or understand the flow of data at each step if the result was unexpected.

-----