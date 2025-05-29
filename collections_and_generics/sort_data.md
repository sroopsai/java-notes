# Section Review: Sorting Data

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Sorting Data" section.

-----

## Summary ⇅

This section covers how to sort collections of objects in Java, primarily focusing on the `Comparable` and `Comparator` interfaces, their methods, and their use with `Collections.sort()` and sorted collections like `TreeSet`. It also touches upon binary search.

1.  **Introduction to Sorting:**

    * Java provides mechanisms to sort collections of objects.
    * Order for numbers is numerical; for `String`s, it's lexicographical (Unicode based: numbers \< uppercase \< lowercase).
    * `Collections.sort(List<T> list)` sorts a list in place (returns `void`).

2.  **The `Comparable<T>` Interface:**

    * **Package:** `java.lang`
    * **Purpose:** Allows objects of a class to define their "natural ordering." A class implements `Comparable` to indicate its instances can be compared to each other.
    * **Single Method:** `public int compareTo(T o)`
        * Returns `0` if `this` object is equal to `o`.
        * Returns a negative integer if `this` object is less than `o`.
        * Returns a positive integer if `this` object is greater than `o`.
    * **Implementation:** The class itself implements this interface and provides the logic for `compareTo()`.
        * Example: Sorting `Duck` records by `name` using `name.compareTo(d.name)`.
        * Example: Sorting `ZooDuck` records by `id` using `id - d.id` (ascending) or `d.id - id` (descending).
    * **Generics:** Using `Comparable<T>` (where `T` is the type of the class) avoids casts in the `compareTo` method. Raw `Comparable` (resulting in `compareTo(Object o)`) requires a cast.
    * **Null Handling:** The `compareTo` method should ideally handle `null` arguments (e.g., by throwing `IllegalArgumentException` or defining a consistent order for nulls).
    * **Consistency with `equals()`:** It is strongly recommended that the natural ordering defined by `compareTo()` be *consistent with equals*. That is, `x.compareTo(y) == 0` should have the same boolean value as `x.equals(y)`. Inconsistent implementations can lead to unpredictable behavior in sorted collections like `TreeSet` and `TreeMap`.

3.  **The `Comparator<T>` Interface:**

    * **Package:** `java.util`
    * **Purpose:** Defines an alternative ordering for objects, or provides ordering for classes that do not implement `Comparable`, or when multiple different sort orders are needed. A `Comparator` is external to the class whose objects are being compared.
    * **Functional Interface:** Has one abstract method: `public int compare(T o1, T o2)`.
        * Returns `0` if `o1` is equal to `o2`.
        * Returns a negative integer if `o1` is less than `o2`.
        * Returns a positive integer if `o1` is greater than `o2`.
    * **Implementation:** Can be implemented using a separate class, an anonymous class, or (most commonly) a lambda expression.
    * **Helper Methods for Creating `Comparator`s (in `Comparator` interface):**
        * **Static methods:**
            * `Comparator.comparing(Function<T, U> keyExtractor)`: Creates a comparator that compares by a key extracted by the function (where `U` must be `Comparable`).
            * `Comparator.comparingInt(ToIntFunction<T> keyExtractor)`, `comparingDouble`, `comparingLong`: Similar, but for primitive keys.
            * `Comparator.naturalOrder()`: Uses the natural `Comparable` order of the elements.
            * `Comparator.reverseOrder()`: Uses the reverse of the natural `Comparable` order.
        * **Default (chainable) methods:**
            * `thenComparing(Function<T, U> keyExtractor)`: For secondary sorting if the primary comparator results in equality.
            * `thenComparingInt`, `thenComparingDouble`, `thenComparingLong`.
            * `reversed()`: Reverses the order of the current comparator.
    * **`Comparable` vs. `Comparator` (Table 9.8):**
        * `Comparable`: `java.lang`, implemented by the class itself, method `compareTo(T o)`, 1 parameter, not typically a lambda.
        * `Comparator`: `java.util`, separate class/lambda, method `compare(T o1, T o2)`, 2 parameters, commonly a lambda.

4.  **Sorting and Searching with `Comparable` and `Comparator`:**

    * **`Collections.sort(List<T> list)`:** Sorts a list whose elements implement `Comparable`. Throws a compile-time error if elements are not `Comparable`.
    * **`Collections.sort(List<T> list, Comparator<? super T> c)`:** Sorts a list using the given `Comparator`.
    * **`List.sort(Comparator<? super E> c)`:** An instance method on `List` (Java 8+) that sorts the list itself using a `Comparator`.
    * **`Collections.reverse(List<?> list)`:** Reverses the order of elements.
    * **`Collections.binarySearch(List list, Object key)`:**
        * **Precondition:** The list **must be sorted** in ascending natural order.
        * Returns the index of the search key if found.
        * Returns `(-(insertion point) - 1)` if the key is not found.
    * **`Collections.binarySearch(List list, Object key, Comparator c)`:**
        * **Precondition:** The list **must be sorted** according to the order defined by the given `Comparator`.
        * The result is undefined if the list is not sorted according to this comparator.
    * **Sorted Collections (`TreeSet`, `TreeMap`):**
        * If elements added to a `TreeSet` do not implement `Comparable` and no `Comparator` is provided to the `TreeSet`'s constructor, a `ClassCastException` is thrown at runtime when an attempt is made to compare incompatible elements (usually upon adding the first or second element that requires comparison).
        * A `Comparator` can be passed to the `TreeSet` constructor to define a custom sort order.

5.  **`final` Methods and Overriding/Hiding:**

    * Methods declared `final` cannot be overridden (for instance methods) or hidden (for static methods). This also applies in the context of sorting if, for example, a `compareTo` method crucial for a natural order was unwisely marked `final` in a superclass you wished to refine. (This point was more related to the previous section "Inheriting Members" but is generally relevant to method behavior in hierarchies).

-----

## Conceptual Questions

1.  What is the primary difference between the `Comparable` interface and the `Comparator` interface in terms of where the comparison logic is defined and how they are used for sorting?
2.  What are the three possible integer value categories returned by `compareTo(T o)` and `compare(T o1, T o2)` methods, and what does each category signify about the relationship between the objects being compared?
3.  Why is it "strongly encouraged" for a class implementing `Comparable` to ensure its `compareTo()` method is "consistent with equals"? What does this consistency mean?
4.  If a class does not implement `Comparable`, can you still sort a `List` of its objects? If so, how?
5.  What is the main precondition for using `Collections.binarySearch()` effectively on a `List`? What happens if this precondition is not met?
6.  When adding elements to a `TreeSet`, what happens if the elements do not implement `Comparable` and no `Comparator` was provided to the `TreeSet`'s constructor?
7.  How can you sort a `List` of objects based on multiple criteria (e.g., sort by name, then by age if names are the same) using the `Comparator` interface's helper methods?
8.  Can a lambda expression be directly used to implement the `Comparable` interface for a class? Why or why not, based on the typical use of `Comparable`?

-----

## Scenario-Based Questions

1.  If `duck1.compareTo(duck2)` returns `-5`, what does this imply about `duck1` relative to `duck2` in terms of their natural order?
2.  You have a `List<String>` that you want to sort in reverse alphabetical order. Which static method from `Comparator` could you use directly with `Collections.sort()` or `List.sort()`?
3.  A `List<Integer>` contains `[10, 2, 7]`. If you call `Collections.sort(list);` and then `Collections.binarySearch(list, 7);`, what will be the result of the binary search? What if you searched for `5`?
4.  If you define `Comparator<String> byLength = (s1, s2) -> s1.length() - s2.length();`, how would this sort a list of strings?
5.  You have a `TreeSet<Person>` where `Person` does not implement `Comparable`. What must you do when creating the `TreeSet` to avoid a runtime exception when adding `Person` objects?

-----

## Coding Problems

1.  **`Song` Comparability:**
    * Create a `Song` class (or record) with fields `title` (String), `artist` (String), and `durationInSeconds` (int).
    * Make the `Song` class implement `Comparable<Song>` such that songs are naturally ordered by `title` alphabetically.
    * In a `main` method, create a `List<Song>`, add a few songs, sort it using `Collections.sort()`, and print the sorted list.
2.  **`Song` Comparators:**
    * Using the `Song` class from problem 1 (it can still implement `Comparable` for its natural order).
    * Create two separate `Comparator<Song>` instances using lambda expressions or `Comparator.comparing()` helper methods:
        * One that sorts songs by `artist` alphabetically.
        * One that sorts songs by `durationInSeconds` (ascending).
    * In a `main` method, create a `List<Song>`, add a few songs.
    * Sort and print the list using the artist comparator.
    * Sort and print the list using the duration comparator.
3.  **Multi-Criteria Sorting:**
    * Using the `Song` class and the list of songs from the previous problems.
    * Create a `Comparator<Song>` that sorts songs first by `artist` alphabetically, and then for songs by the same artist, by `durationInSeconds` in descending order. (Hint: use `thenComparing` and `reversed`).
    * Sort and print the list using this multi-criteria comparator.
4.  **Binary Search with Custom Objects:**
    * Using the `Song` class that implements `Comparable` (sorted by title).
    * Create a sorted `List<Song>`.
    * Perform a `Collections.binarySearch()` to find a specific song by its title.
    * Perform a `Collections.binarySearch()` for a song title that doesn't exist and interpret the result.

-----