# Chapter Summary: Lambdas and Functional Interfaces

This summary covers the core concepts of functional programming in Java, including the syntax for lambdas, the rules for functional interfaces, method references, and how lambdas interact with variables.

## 1. Writing Simple Lambdas

* **What is a Lambda Expression?**
    * An unnamed block of code with parameters and a body that can be passed around to be executed later. It allows you to treat behavior as data.
    * Its primary purpose is to reduce boilerplate code by providing a direct implementation for a functional interface.

* **Functional Interfaces (The "Context"):**
    * A lambda can only be used where a **functional interface** is expected.
    * A functional interface is an interface with **exactly one abstract method** (also known as the SAM rule).
    * The compiler uses the functional interface's single abstract method to infer the types for the lambda's parameters and its return value.

* **Lambda Syntax Rules:**
    * **Basic Structure:** `(parameters) -> { body }`.
    * **Parameters `()`:** Parentheses are required for zero or multiple parameters. They are optional for a single parameter *only if* its type is not explicitly declared.
    * **Body `{}`:** Braces are optional for a single-expression body (the `return` keyword must be omitted). Braces are required for multi-statement bodies (the `return` keyword is required if the method returns a value).
* **`var` Restriction:** You cannot assign a lambda directly to a variable declared with `var` due to a lack of type context (e.g., `var p = s -> s.isEmpty();` is illegal).

## 2. Coding Functional Interfaces

* **Definition (The SAM Rule):** A functional interface must contain **exactly one abstract method**.
* **`@FunctionalInterface` Annotation:**
    * An optional annotation that triggers a compile-time check to ensure the interface adheres to the SAM rule.
* **What Counts as an Abstract Method?**
    * Abstract methods inherited from a parent interface count towards the total.
    * `default`, `static`, and `private` methods **do not** count.
    * **`Object` Class Exception:** An abstract method declaration that matches a `public` method from `java.lang.Object` (e.g., `toString()`, `equals(Object)`) **does not** count towards the total. The signature must match exactly.

## 3. Using Method References

* **What is a Method Reference?**
    * A shorthand syntax for a lambda expression that does nothing but call a single, existing method. It uses the `::` operator.
* **The Four Types:**
    1.  **Static Method:** `ClassName::staticMethod` (e.g., `Math::round` is like `x -> Math.round(x)`).
    2.  **Instance Method on a Specific Object:** `instanceRef::instanceMethod` (e.g., `myString::isEmpty` is like `() -> myString.isEmpty()`).
    3.  **Instance Method on a Parameter:** `ClassName::instanceMethod` (e.g., `String::isEmpty` is like `s -> s.isEmpty()`. The first lambda parameter becomes the instance).
    4.  **Constructor Reference:** `ClassName::new` (e.g., `ArrayList::new` is like `() -> new ArrayList()`).

## 4. Working with Built-in Functional Interfaces

Java provides a suite of common interfaces in the `java.util.function` package.

| Functional Interface | Purpose | Method Name | Parameters | Return Type |
| :--- | :--- | :--- | :--- | :--- |
| **`Supplier<T>`** | Supplies a value | `get()` | 0 | `T` |
| **`Consumer<T>`** | Uses a value | `accept(T t)` | 1 | `void` |
| **`BiConsumer<T,U>`**| Uses two values | `accept(T t, U u)`| 2 | `void` |
| **`Predicate<T>`** | A true/false test | `test(T t)` | 1 | `boolean` |
| **`BiPredicate<T,U>`**| A true/false test on two values|`test(T t, U u)` | 2 | `boolean` |
| **`Function<T,R>`** | Transforms a T into an R | `apply(T t)` | 1 | `R` |
| **`BiFunction<T,U,R>`**| Transforms T and U into an R|`apply(T t, U u)` | 2 | `R` |
| **`UnaryOperator<T>`** | A function where all types are the same | `apply(T t)` | 1 | `T` |
| **`BinaryOperator<T>`**| A BiFunction where all types are the same |`apply(T t1, T t2)`| 2 | `T` |

* **Primitive Interfaces:** Special versions exist for primitives (`IntPredicate`, `DoubleSupplier`, etc.) to avoid the performance cost of autoboxing.

## 5. Working with Variables in Lambdas

* **Lambda Parameters:** All parameters in a lambda must use a consistent style: all have explicit types, all use `var`, or all have no types. Mixing styles is a compiler error.
* **Local Variables Inside Lambdas:** A local variable declared inside a lambda cannot "shadow" (have the same name as) a lambda parameter or a variable from the enclosing scope.
* **Variable Capture (Accessing Outside Variables):**
    * A lambda can freely access and modify **instance** and **static** variables.
    * A lambda can only access **local variables** and **method parameters** if they are `final` or `effectively final` (i.e., their value is never changed after initial assignment).