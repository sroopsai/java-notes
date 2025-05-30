# Section Review: Working with Generics

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Working with Generics" section.

-----

## Summary 🧬

This section delves into creating and using generic classes, methods, and records, understanding type erasure, and working with bounded wildcards to enhance flexibility and type safety.

1.  **Creating Generic Classes and Records:**

    * **Syntax:** Declare a formal type parameter in angle brackets after the class/record name (e.g., `public class Crate<T> {}`, `public record Box<T>(T content) {}`).
    * **Naming Conventions:** Typically single uppercase letters (E for Element, K for Key, V for Value, N for Number, T for Type, S/U/V for multiple types).
    * **Purpose:** Allows a class or record to work with various data types specified at instantiation, without sacrificing type safety. The type parameter `T` acts as a placeholder for the actual type.
    * **Instantiation:** `Crate<Elephant> crate = new Crate<>();`.
    * **Multiple Type Parameters:** Allowed (e.g., `public class SizeLimitedCrate<T, U> {}`).
    * **Generic Records:** Also support type parameters (e.g., `public record CrateRecord<T>(T contents) {}`).

2.  **Understanding Type Erasure:**

    * Generics are primarily a **compile-time feature** for type checking and safety.
    * During compilation, the generic type information is largely **erased**, and type parameters (like `T`) are typically replaced with `Object` (or their bound if specified, e.g., `Number` for `T extends Number`).
    * This means at runtime, there's only one version of the class file (e.g., `Crate.class`), not separate versions for `Crate<Elephant>` and `Crate<Robot>`.
    * The compiler inserts necessary **casts** automatically when retrieving elements from generic structures, maintaining type safety for the programmer.
    * Type erasure ensures backward compatibility with older Java versions that didn't have generics.

3.  **Generics and Method Overloading/Overriding:**

    * **Overloading:** Methods cannot be overloaded if their signatures become identical after type erasure. For example, `void process(List<String> list)` and `void process(List<Integer> list)` both erase to `void process(List list)`, leading to a compile error. However, if raw types differ (e.g., `List` vs. `ArrayList`), they can be overloaded.
    * **Overriding Generic Return Types:** When overriding a method with a generic return type, the return type in the subclass must be covariant. This means the base type can be a subtype (e.g., `ArrayList` for `List`), but the **generic type parameter itself must match exactly** (e.g., if super returns `List<CharSequence>`, override can be `ArrayList<CharSequence>` but not `List<String>`). Non-generic return types follow standard covariance (e.g., `String` can override `CharSequence`).

4.  **Implementing Generic Interfaces:**

    * A class can implement a generic interface in several ways:
        1.  **Specify the generic type:** `class RobotCrate implements Shippable<Robot> { public void ship(Robot t) {} }`
        2.  **Make the class generic:** `class GenericCrate<U> implements Shippable<U> { public void ship(U t) {} }`
        3.  **Use raw types (legacy, not recommended):** `class RawCrate implements Shippable { public void ship(Object t) {} }` (generates compiler warnings).

5.  **Limitations with Generic Types (due to Type Erasure):**

    * Cannot instantiate a generic type parameter directly: `new T()` is not allowed.
    * Cannot create an array of a generic type: `new T[size]` is not allowed (results in `Object[]`).
    * Cannot use `instanceof` with a generic type parameter: `obj instanceof T` is not allowed (except for unbounded wildcards like `obj instanceof List<?>`).
    * Cannot use primitive types as generic type parameters (use their wrapper classes, e.g., `Integer` for `int`).
    * Cannot create `static` variables of a generic type parameter `T` (static members belong to the class, not instances with specific types).
    * Cannot `catch` an exception of a generic type `T`.

6.  **Writing Generic Methods:**

    * Type parameters can be declared at the method level, usually for `static` methods or when a method's generic type is independent of class-level generics.
    * **Syntax:** The formal type parameter is declared before the method's return type: `public static <T> Crate<T> ship(T t) { ... }`.
    * The compiler infers the type `T` from the arguments passed during the method call.
    * Optional explicit type invocation: `MyClass.<String>myGenericMethod("hello");`.
    * Method-level type parameters can shadow class-level type parameters if they share the same name.

7.  **Bounding Generic Types (Wildcards):**

    * **Purpose:** To restrict the range of types that can be used with a generic type parameter, providing more flexibility than exact type matching while maintaining type safety.
    * **Unbounded Wildcard (`?`):**
        * `List<?> list` means "a list of some unknown type."
        * You can assign `List<String>`, `List<Integer>`, etc., to `List<?>`.
        * When reading from `List<?>`, elements are treated as `Object`.
        * **Cannot add elements** (except `null`) to `List<?>` because the compiler doesn't know the actual type and cannot guarantee type safety.
    * **Upper-Bounded Wildcard (`? extends Type`):**
        * `List<? extends Number> list` means "a list of `Number` or any subtype of `Number` (like `Integer`, `Double`)."
        * Useful for methods that **read** from a collection (producer). You know elements are at least of type `Number`.
        * **Cannot add elements** (except `null`) because the exact type is unknown (it could be `List<Integer>`, so adding a `Double` would be unsafe). Makes the list logically immutable for additions.
    * **Lower-Bounded Wildcard (`? super Type`):**
        * `List<? super String> list` means "a list of `String` or any supertype of `String` (like `CharSequence`, `Object`)."
        * Useful for methods that **write** to a collection (consumer). You can safely add instances of `Type` or its subtypes.
        * When reading from `List<? super Type>`, elements are treated as `Object` (as that's the only common guaranteed supertype if you don't know the exact type).
    * **PECS Principle (Producer Extends, Consumer Super):** A mnemonic for choosing bounds. If a generic structure is a *producer* (you only read from it), use `extends`. If it's a *consumer* (you only write to it), use `super`.
    * You cannot instantiate with a wildcard: `new ArrayList<? extends Number>()` is invalid.

Understanding generics, type erasure, and wildcards is crucial for working effectively with Java's Collections Framework and writing flexible, type-safe reusable code.

-----

## Conceptual Questions

1.  What is the primary purpose of declaring a class or interface with a generic type parameter (e.g., `Crate<T>`)?
2.  Explain the concept of "type erasure" in Java generics. What does the generic type `T` typically become after compilation?
3.  Why can't you overload two methods in the same class if their signatures are `void process(List<String> list)` and `void process(List<Integer> list)`?
4.  When overriding a method that returns a generic type like `List<CharSequence>`, what are the rules for the return type in the overriding method regarding the base type (e.g., `List` vs. `ArrayList`) and the generic parameter type (e.g., `CharSequence` vs. `String`)?
5.  List three things you *cannot* do with a generic type parameter `T` due to type erasure (e.g., instantiate it with `new T()`).
6.  What is the syntax for declaring a generic method, and when is it particularly useful to define a method as generic?
7.  What does an unbounded wildcard (`?`) like in `List<?>` signify? What operations are generally restricted on a `List<?>` and why?
8.  Explain what an upper-bounded wildcard (`? extends SomeType`) means. When reading from a collection defined with an upper bound (e.g., `List<? extends Number>`), what do you know about the type of elements? Why can't you typically add arbitrary elements (other than `null`) to such a list?
9.  Explain what a lower-bounded wildcard (`? super SomeType`) means. When writing to a collection defined with a lower bound (e.g., `List<? super String>`), what types of elements can you safely add? What type do you get when you read elements from it?
10. What is the PECS principle, and how does it guide the use of `extends` and `super` in wildcards?

-----

## Scenario-Based Questions

1.  If a generic class is defined as `Box<T>`, and you create an instance `Box<String> stringBox = new Box<>();`, what does `T` effectively become for `stringBox` at compile time? What does `T` become after type erasure at runtime?
2.  You have a method `void printElements(List<?> list)`. Can you call this method with a `List<Integer>`? Can you call it with a `List<String>`? Inside the `printElements` method, if you get an element from the list, what is its guaranteed type?
3.  Consider `List<? extends Number> numbers;`. Can you assign an `ArrayList<Integer>` to it? Can you assign an `ArrayList<Double>` to it? Can you assign an `ArrayList<Object>` to it?
4.  For `List<? extends Number> numbers;`, can you safely call `numbers.add(new Integer(5));`? Why or why not?
5.  Consider `List<? super Integer> intSupers;`. Can you assign an `ArrayList<Integer>` to it? Can you assign an `ArrayList<Number>` to it? Can you assign an `ArrayList<Object>` to it?
6.  For `List<? super Integer> intSupers;`, can you safely call `intSupers.add(new Integer(5));`? Can you safely call `intSupers.add(new Object());`? When you retrieve an element using `get()`, what is its type?

-----

## Coding Problems

1.  **Generic Pair Class:**
    * Create a generic class `Pair<K, V>` that holds two objects of potentially different types (a key `K` and a value `V`).
    * Implement a constructor to initialize the pair.
    * Implement getter methods `getKey()` and `getValue()`.
    * Demonstrate its usage with different types (e.g., `Pair<String, Integer>` and `Pair<Integer, String>`).
2.  **Generic Utility Method:**
    * Write a static generic method `T getLastElement(List<T> list)` that returns the last element of any list. It should return `null` if the list is empty or null.
    * Demonstrate its usage with a `List<String>` and a `List<Integer>`.
3.  **Bounded Wildcards for Processing Numbers:**
    * Create a method `double sumOfList(List<? extends Number> list)` that calculates the sum of all numbers in the list and returns it as a `double`. (Use `number.doubleValue()`).
    * Create another method `void addIntegers(List<? super Integer> list, int count)` that adds `count` new `Integer` objects (e.g., 1, 2, ..., count) to the given list.
    * Demonstrate both methods:
        * Call `sumOfList` with a `List<Integer>` and a `List<Double>`.
        * Call `addIntegers` with a `List<Integer>` and a `List<Number>`.

-----