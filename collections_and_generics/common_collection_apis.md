# Section Review: Using Common Collection APIs

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using Common Collection APIs" section.

-----

## Summary 🧺

This section introduces the Java Collections Framework (JCF), its main interfaces, the concept of generics for type safety, and common methods available through the `Collection` interface.

1.  **Java Collections Framework (JCF) Overview:**

      * A set of classes and interfaces in `java.util` for storing and manipulating groups of objects.
      * **Main Interfaces:**
          * **`List`**: An ordered collection allowing duplicate elements. Elements are accessible by an integer index.
          * **`Set`**: A collection that does not allow duplicate elements.
          * **`Queue`**: A collection typically ordering elements for processing (e.g., FIFO). `Deque` is a subinterface allowing additions/removals from both ends.
          * **`Map`**: Stores key-value pairs. Does not allow duplicate keys. (Note: `Map` is part of the JCF but does not extend the `Collection` interface).
      * Java 21 introduced **Sequenced Collections** (`SequencedSet`, `SequencedCollection`, `SequencedMap`).

2.  **Understanding Generic Types:**

      * **Generics (`<Type>`):** Provide a way to use parameterized types, enabling type safety for collections.
      * **Benefits:**
          * Prevents adding unrelated objects to a collection at compile time.
          * Eliminates the need for explicit casts when retrieving elements.
          * Example: `List<Integer>` ensures only `Integer` objects (or `int`s via autoboxing) can be added and retrieved elements are known to be `Integer`.

3.  **Shortening Generics Code:**

      * **Diamond Operator (`<>`):** Used on the right side of an assignment to infer the generic type from the left side's declaration (e.g., `List<Integer> list = new ArrayList<>();`). Cannot be used on the left side for variable declaration or as a type in method parameters.
      * **`var` Keyword:** Infers the variable type from the right side of the assignment. The generic type must be specified on the right side if using `var` (e.g., `var list = new ArrayList<Integer>();`).
      * **Using both `var` and `<>`:** `var map = new HashMap<>();` compiles and infers the types as `Object` (e.g., `HashMap<Object, Object>`).

4.  **Common `Collection<E>` Interface Methods:**

      * **`boolean add(E element)`:**
          * Inserts an element. Returns `true` if the collection was modified.
          * For `List`, usually always returns `true` (as duplicates are allowed).
          * For `Set`, returns `false` if the element is a duplicate and already present.
      * **`boolean remove(Object object)`:**
          * Removes a single instance of the specified element (using `equals()` for comparison).
          * Returns `true` if an element was removed.
          * For `List`, removes only the first occurrence if duplicates exist.
      * **`boolean isEmpty()`:** Returns `true` if the collection contains no elements.
      * **`int size()`:** Returns the number of elements in the collection.
      * **`void clear()`:** Removes all elements from the collection.
      * **`boolean contains(Object object)`:**
          * Returns `true` if the collection contains the specified element (uses `equals()` for comparison).
      * **`boolean removeIf(Predicate<? super E> filter)`:**
          * Removes all elements that satisfy the given predicate.
          * Takes a `Predicate` functional interface (e.g., a lambda or method reference).
      * **`void forEach(Consumer<? super E> action)`:**
          * Performs the given action for each element in the collection.
          * Takes a `Consumer` functional interface.
      * **Iteration Alternatives:** Enhanced for-loop (`for (Type element : collection)`), and using an `Iterator` (`hasNext()`, `next()`).

5.  **`equals(Object object)` for Collections:**

      * Collections implement `equals()` to compare type and contents.
      * For `List`, order of elements matters.
      * For `Set`, order does not matter.
      * Comparing a `List` to a `Set` (even with same elements) returns `false` due to different types.

6.  **Unboxing `null` from Collections:**

      * It's permissible to add `null` to a collection of wrapper types (e.g., `ArrayList<Integer>`).
      * However, attempting to unbox this `null` value into a primitive variable (e.g., `int x = list.get(0);` where `list.get(0)` returns `null`) will result in a `NullPointerException` at runtime.

-----

## Conceptual Questions

1.  What are the four main interfaces of the Java Collections Framework mentioned, and what is a key characteristic of each (e.g., ordered, allows duplicates, key-value pairs)?
2.  Why is the `Map` interface considered part of the Java Collections Framework even though it doesn't extend the `Collection` interface?
3.  What is the primary benefit of using generics (e.g., `List<String>`) with collections? How does it compare to using raw types (e.g., `List`)?
4.  Explain the purpose of the diamond operator (`<>`) and how it differs from using `var` in terms of type inference for generic collections. What happens if you use both `var list = new ArrayList<>();`?
5.  The `add(E element)` method in the `Collection` interface returns a `boolean`. Why is this return value useful, and how might its behavior differ between a `List` and a `Set`?
6.  Describe the behavior of the `remove(Object object)` method. If a `List` contains duplicate elements, which one does `remove()` target?
7.  What is the difference between `isEmpty()` and `size() == 0`?
8.  What does the `removeIf(Predicate<? super E> filter)` method do? What kind of argument does it expect?
9.  How does the `equals()` method typically behave for `List` implementations versus `Set` implementations when comparing two collections?
10. What is the potential runtime issue when retrieving an element from a collection of wrapper types (like `List<Integer>`) and assigning it to a primitive variable if the retrieved element was `null`?

-----

## Scenario-Based Questions (for this section's content - answers to be provided later)

1.  If you have `Collection<String> coll = new ArrayList<>();` and you execute `coll.add("hello"); coll.add("world"); coll.add("hello");`, what would `coll.size()` return?
2.  You have a `Set<Integer> numbers = new HashSet<>();`. What would `numbers.add(10);` return? What would `numbers.add(10);` return the second time?
3.  If `List<String> items = new ArrayList<>(); items.add("apple"); items.add("banana");` and you call `items.remove("orange");`, what is the return value? What if you then call `items.remove("apple");`?
4.  You have `List<String> data = new ArrayList<>(List.of("a", "b", "c"));`. If you call `data.clear();`, what will `data.isEmpty()` return?
5.  Given `Collection<Integer> nums = new HashSet<>(Set.of(10, 20, 30));`, what does `nums.contains(20)` return? What about `nums.contains(Integer.valueOf(20))`?
6.  If `List<String> words = new ArrayList<>(List.of("apple", "apricot", "banana"));` and you execute `words.removeIf(s -> s.length() > 5);`, what would be the content of `words`?
7.  What would be printed if you executed `List.of("A", "B").forEach(s -> System.out.print(s + "-"));`?

-----

## Coding Problems (for this section's content - solutions to be provided later)

1.  **Basic Collection Operations:**
      * Create an `ArrayList` of `String` called `colors`.
      * Add "Red", "Green", "Blue", "Green" to it. Print the list and its size.
      * Check if it contains "Yellow". Print the result.
      * Remove the first occurrence of "Green". Print the list.
      * Use `removeIf` to remove all colors that have a length less than 4. Print the list.
      * Clear the list and print if it's empty.
2.  **Set Behavior:**
      * Create a `HashSet` of `Integer` called `uniqueNumbers`.
      * Try to add the numbers 10, 20, 10, 30, 20.
      * Iterate through the set using `forEach` and print each number.
      * Print the final size of the set.
3.  **Generics and Type Safety:**
      * Create a `List` without using generics (a raw type `List list = new ArrayList();`).
      * Add an `Integer` (e.g., `100`), a `String` (e.g., `"Hello"`), and a `Double` (e.g., `3.14`) to this list.
      * Now, try to iterate through the list and retrieve elements, attempting to cast them to `Integer` to perform an arithmetic operation (e.g., multiply by 2). Observe what happens (you'll likely need a try-catch for `ClassCastException` to make it run without crashing).
      * Discuss how using `List<Integer>` would prevent these issues at compile time.

-----