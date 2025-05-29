# Section Review: Using the Set Interface

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using the Set Interface" section.

-----

## Summary S️⃣

This section introduces the `java.util.Set` interface, its core characteristic of not allowing duplicate entries, and the common implementations you need to know: `HashSet`, `LinkedHashSet`, and `TreeSet`.

1.  **`Set` Interface Overview:**

    * A **`Set`** is a collection that **does not allow duplicate elements**.
    * Its primary purpose is to store unique items.
    * The general `Set` interface does not guarantee any specific order of elements, though some implementations do.

2.  **Common `Set` Implementations:**

    * **`HashSet<E>`:**
        * Stores elements in a hash table.
        * Uses the `hashCode()` method of objects for efficient storage and retrieval (to find the "bucket").
        * Uses the `equals()` method to check for actual duplicates within a bucket.
        * **Order:** Does **not** guarantee any specific iteration order; the order can appear arbitrary and may change over time.
        * **Performance:** Offers average time complexity of O(1) for `add`, `remove`, and `contains` operations, assuming a good hash function that distributes elements well.
    * **`LinkedHashSet<E>`:**
        * Extends `HashSet`.
        * Maintains elements in **insertion order** (the order in which elements were added to the set) using an internal doubly linked list connecting the elements.
        * Provides the same no-duplicates guarantee and average O(1) performance for basic operations as `HashSet`, but with slightly more overhead for maintaining the linked list.
        * The text also hints at new Java 21 capabilities (like `SequencedSet`) which `LinkedHashSet` would leverage, allowing predictable iteration and potentially operations related to first/last elements.
    * **`TreeSet<E>`:**
        * Stores elements in a **sorted tree structure** (specifically, a Red-Black tree).
        * Elements are **always kept in sorted order** according to their natural ordering (if they implement `Comparable`) or by a `Comparator` provided at `TreeSet` creation.
        * **Performance:** Offers O(log n) time complexity for `add`, `remove`, and `contains` operations.
        * Does not allow `null` elements if using natural ordering (as `null` cannot be compared to non-null elements). A `Comparator` could potentially handle `null`s.

3.  **Creating Sets with Factory Methods (Immutable):**

    * **`Set.of(E... elements)` (Java 9+):**
        * Returns an **immutable `Set`**.
        * **Duplicate elements** provided in the varargs will cause an **`IllegalArgumentException`** at the time of creation.
    * **`Set.copyOf(Collection<? extends E> coll)` (Java 10+):**
        * Returns an **immutable `Set`** containing the unique elements from the given collection.
        * If the source collection contains duplicates, they are silently ignored, and only one instance of each unique element is included in the resulting set.

4.  **Working with `Set` Methods (Behavior of `Collection` methods with Sets):**

    * **`boolean add(E element)`:**
        * Returns `true` if the element was successfully added (i.e., it was not already present in the set).
        * Returns `false` if the element was already present (as sets do not allow duplicates). This is a key way `Set.add()` differs from `List.add()`.
    * **`equals(Object object)`:**
        * For sets, `equals()` compares two sets for equality. Two sets are equal if they have the same size and contain the same elements, regardless of order (for `HashSet` and `LinkedHashSet` when compared to another `Set`). `TreeSet` equality would also imply the same sort order if compared to another `SortedSet`.
    * Other methods like `remove()`, `isEmpty()`, `size()`, `clear()`, `contains()`, `forEach()`, `removeIf()` behave as expected for a collection of unique items.

5.  **Importance of `hashCode()` and `equals()`:**

    * For objects stored in `HashSet` or `LinkedHashSet`, a correct and consistent implementation of both `hashCode()` and `equals()` methods is crucial for the set to function correctly (to identify duplicates and locate elements efficiently).

-----

## Conceptual Questions

1.  What is the single most defining characteristic of a `Set` compared to a `List`?
2.  Briefly describe the main difference between `HashSet`, `LinkedHashSet`, and `TreeSet` in terms of how they store elements and the order (if any) in which elements are iterated.
3.  Why is a good implementation of `hashCode()` and `equals()` methods important for objects stored in a `HashSet`?
4.  What happens if you attempt to add an element to a `HashSet` that is already present in the set? What does the `add()` method return in this case?
5.  How does a `TreeSet` determine the order of its elements? What is required of the elements added to a `TreeSet` if no explicit `Comparator` is provided?
6.  Explain the difference in behavior between `Set.of("A", "B", "A")` and `Set.copyOf(List.of("A", "B", "A"))` during their creation.
7.  Can you add `null` elements to a `TreeSet`? What about a `HashSet` or `LinkedHashSet`?

-----

## Scenario-Based Questions

1.  You need to store a collection of unique student ID numbers and be able to check for the existence of an ID very quickly. The order in which IDs are stored or retrieved does not matter. Which `Set` implementation would likely be the most efficient choice?
2.  You are processing a log file and want to collect all unique IP addresses that accessed your server, maintaining the order in which each unique IP address first appeared. Which `Set` implementation would be most suitable?
3.  You have a collection of custom `Book` objects that you want to store in a `TreeSet`. What must be true about the `Book` class for this to work without providing a separate `Comparator`?
4.  If `Set<String> s1 = Set.of("a", "b");` and `Set<String> s2 = Set.of("b", "a");`, what would `s1.equals(s2)` return? Why?
5.  If `Set<Integer> numbers = new HashSet<>(); numbers.add(5); numbers.add(10);`, will the elements always be iterated in the order 5, then 10?

-----

## Coding Problems

1.  **Unique Word Counter:**
    * Write a program that reads a sentence (a `String`).
    * Use a `HashSet<String>` to store all the unique words from the sentence (case-insensitive).
    * Print the number of unique words and then print each unique word.
2.  **Insertion-Ordered Set:**
    * Create a `LinkedHashSet<String>`.
    * Add the names "Charlie", "Alice", "Bob", "Alice" in this order.
    * Iterate through the set and print each name. Observe the order and the handling of duplicates.
3.  **Sorted Fruit Basket:**
    * Create a `TreeSet<String>` to store names of fruits.
    * Add "Apple", "Orange", "Banana", "Grape", "Kiwi", "Apple".
    * Print the set. What do you observe about the order and duplicates?
    * Now, try to add `null` to this `TreeSet` (conceptually, what would happen based on your understanding, and then you can verify if you were to run it).

-----