# Section Review: Using the List Interface

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using the List Interface" section.

-----

## Summary

This section focuses on the `java.util.List` interface, its characteristics, common implementations (`ArrayList` and `LinkedList`), factory methods for creating lists, and specific `List` methods.

1.  **`List` Interface Overview:**

    * Represents an **ordered collection** of elements (also known as a sequence).
    * Allows **duplicate entries**.
    * Elements can be accessed, inserted, or removed based on their **integer index** (zero-based).
    * Implementations can often change in size dynamically.

2.  **Common `List` Implementations:**

    * **`ArrayList<E>`:**
        * Implements `List`.
        * Backed by a resizable array.
        * **Strengths:** Fast random access to elements using `get(index)` (constant time - O(1)).
        * **Weaknesses:** Adding or removing elements (especially in the middle) can be slower as it may require shifting elements (linear time - O(n)).
        * Generally a good default choice when unsure.
    * **`LinkedList<E>`:**
        * Implements both `List` and `Deque` (double-ended queue).
        * **Strengths:** Fast additions and removals at the beginning or end of the list (constant time - O(1)).
        * **Weaknesses:** Accessing an element by an arbitrary index is slower (linear time - O(n)) as it may involve traversing the list.
        * A good choice when frequent operations at the ends are needed or when it's used as a queue/stack.

3.  **Creating Lists with Factory Methods (Immutable/Fixed-Size):**

    * **`Arrays.asList(T... a)`:**
        * Returns a **fixed-size `List`** backed directly by the specified array.
        * **Modifications:**
            * Cannot add or remove elements (throws `UnsupportedOperationException`).
            * Can modify existing elements using `set(index, element)`, which also modifies the original array.
            * Changes to the original array are reflected in the list, and vice-versa.
    * **`List.of(E... elements)` (Java 9+):**
        * Returns an **immutable `List`**.
        * Cannot add, remove, or replace elements (all attempts throw `UnsupportedOperationException`).
    * **`List.copyOf(Collection<? extends E> coll)` (Java 10+):**
        * Returns an **immutable `List`** containing the elements of the given collection.
        * It's a true copy, so modifications to the original collection after creating the copy do not affect the immutable list.

4.  **Creating Lists with Constructors:**

    * Most `List` implementations (like `ArrayList`, `LinkedList`) provide:
        * A no-argument constructor: `new ArrayList<String>()` (creates an empty list).
        * A constructor that accepts another `Collection`: `new ArrayList<String>(anotherList)` (creates a new list containing all elements from `anotherList`).
    * `ArrayList` has an additional constructor: `new ArrayList<String>(int initialCapacity)` (creates an empty list with a specified initial internal capacity).

5.  **Common `List<E>` Interface Methods (beyond `Collection` methods):**

    * **`void add(int index, E element)`:** Inserts `element` at the specified `index`, shifting subsequent elements.
    * **`E get(int index)`:** Returns the element at the specified `index`.
    * **`int indexOf(Object o)`:** Returns the index of the first occurrence of `o`, or -1 if not found. Uses `equals()`.
    * **`int lastIndexOf(Object o)`:** Returns the index of the last occurrence of `o`, or -1 if not found. Uses `equals()`.
    * **`E remove(int index)`:** Removes and returns the element at the specified `index`. Shifts subsequent elements. Throws `IndexOutOfBoundsException` if `index` is out of range.
    * **`default void replaceAll(UnaryOperator<E> operator)`:** Replaces each element with the result of applying the `operator`.
    * **`E set(int index, E element)`:** Replaces the element at `index` with `element`, and returns the element previously at that position. Throws `IndexOutOfBoundsException` if `index` is out of range.
    * **`default void sort(Comparator<? super E> c)`:** Sorts the list according to the order induced by the specified `Comparator` (or natural ordering if `c` is `null` and elements are `Comparable`).

6.  **Overloaded `remove()` Methods - Distinction:**

    * `boolean remove(Object o)` (from `Collection`): Removes the first occurrence of the specified **element**.
    * `E remove(int index)` (from `List`): Removes the element at the specified **index**.
    * **Caution with `List<Integer>`:** `list.remove(2)` calls `remove(int index)`. `list.remove(Integer.valueOf(2))` calls `remove(Object o)`.

7.  **Converting `List` to Array:**

    * **`Object[] toArray()`:** Returns an array of `Object` containing all elements.
    * **`T[] toArray(T[] a)`:** Returns an array of type `T`.
        * If `a` (the provided array) is large enough, it's used.
        * Otherwise (e.g., if `a` is `new T[0]`), a new array of the correct size and runtime type is allocated and returned.
    * The created array is a copy; modifications to the original list after conversion do not affect the array.

-----

## Conceptual Questions

1.  What are the two defining characteristics of a `List` interface compared to a generic `Collection`?
2.  Compare `ArrayList` and `LinkedList` in terms of their performance characteristics for:
    * Accessing an element by index (`get(index)`).
    * Adding/removing an element at the beginning/end.
    * Adding/removing an element in the middle.
3.  Describe the key behavioral differences between a `List` created by `Arrays.asList()`, `List.of()`, and `List.copyOf()`, especially concerning mutability and fixed size.
4.  What happens if you try to `add` or `remove` elements from a `List` created by `Arrays.asList()`? What if you use `set()`?
5.  Besides the common `Collection` methods like `add(E element)` and `remove(Object o)`, name three methods specific to the `List` interface that work with indices.
6.  Explain the difference between `list.remove(10)` and `list.remove(Integer.valueOf(10))` when `list` is an `ArrayList<Integer>`.
7.  What does the `replaceAll(UnaryOperator<E> operator)` method do for a `List`?
8.  When converting a `List<String>` to a `String[]` array using the `toArray(T[] a)` method, what is the common practice for the argument array `a` to ensure an array of the correct size and type is returned?

-----

## Scenario-Based Questions

1.  You need a collection to store a sequence of user actions where the order of actions is critical and you might need to access the 5th action frequently. Would `ArrayList` or `LinkedList` be a more suitable primary choice? Why?
2.  If `String[] data = {"x", "y"}; List<String> list = Arrays.asList(data);` is executed, and then `data[0] = "z";` is executed, what will `list.get(0)` return?
3.  If `List<String> immutableList = List.of("a", "b");`, what happens if you try to execute `immutableList.add("c");`?
4.  You have an `ArrayList<Integer>` and want to double the value of every element in it. Which `List` method introduced in this section could you use with a lambda expression to achieve this?
5.  If `List<String> myList = new ArrayList<>(); myList.add("A"); myList.add("B"); myList.add("A");`, what would `myList.indexOf("A")` return? What would `myList.lastIndexOf("A")` return?

-----

## Coding Problems

1.  **List Manipulation:**
    * Create an `ArrayList<String>` and add the elements: "Alpha", "Beta", "Gamma".
    * Insert "Delta" at index 1.
    * Replace the element at index 2 with "Omega".
    * Remove the element "Alpha".
    * Print the element at index 0.
    * Print the final list.
2.  **Factory Methods Exploration:**
    * Create a list using `Arrays.asList("A", "B", "C")`. Try to `add` an element "D" to it (observe the exception in a comment). Try to `set` the element at index 0 to "X". Print the list.
    * Create a list using `List.of("X", "Y", "Z")`. Try to `add` an element "W" to it (observe the exception in a comment).
3.  **Integer List `remove()` Ambiguity:**
    * Create an `ArrayList<Integer>` and add the numbers 10, 20, 30, 20, 40.
    * Remove the element at index 2. Print the list.
    * Remove the first occurrence of the element with value 20. Print the list.

-----