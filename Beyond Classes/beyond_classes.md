# Chapter Summary: Beyond Classes

This summary covers advanced and alternative class-like structures in Java, including interfaces, enums, sealed types, records, and nested classes. These features are essential for modern, robust, and expressive Java applications.

## 1. Implementing Interfaces

* **Core Concepts:**
    * An interface is an abstract data type defining a contract of methods and constants.
    * A class `implements` one or more interfaces, while an interface `extends` one or more interfaces.
    * A class must provide a concrete implementation for all inherited abstract methods.
    * Interfaces cannot be instantiated directly or marked `final`.

* **Implicit Modifiers:**
    * **Interface Declaration:** Implicitly `abstract`.
    * **Variables:** Implicitly `public`, `static`, and `final` (constants).
    * **Abstract Methods:** Implicitly `public` and `abstract`.

* **Concrete Methods in Interfaces (Java 8+):**
    * **`default` Methods:** Provide a default implementation that implementing classes inherit. They are implicitly `public` and can be overridden. If a class inherits two default methods with the same signature, it must override the method to resolve the ambiguity.
    * **`static` Methods:** Belong to the interface itself and are **not inherited** by implementing classes. They must be called using the interface name (e.g., `MyInterface.staticMethod()`).
    * **`private` Methods (Java 9+):** Helper methods with a body, marked `private` or `private static`, that can only be called by other methods inside the same interface.

## 2. Working with Enums

* **Core Concepts:**
    * An `enum` is a special type representing a fixed set of type-safe constants.
    * Enums cannot be extended, can implement interfaces, and their constants can be compared with `==`.
* **Built-in Methods:**
    * `values()`: Returns an array of all constants.
    * `valueOf(String)`: Returns the constant matching the name (case-sensitive). Throws `IllegalArgumentException` if no match.
    * `name()`: Returns the constant's name as a `String`.
    * `ordinal()`: Returns the constant's zero-based declaration position.
* **Complex Enums:**
    * Can have fields, methods, and constructors. The list of constants must come first, followed by a semicolon `;`.
    * **Constructors:** Are always implicitly `private`. They are called once for each constant when the enum is first initialized on its first active use.
    * **Constant-Specific Methods:** An enum can have an `abstract` method, which forces each constant to provide its own implementation.

## 3. Sealing Classes and Interfaces

* **Core Concept:** A `sealed` class or interface restricts which other types may extend or implement it.
* **Keywords:**
    * **`sealed`:** Restricts inheritance/implementation.
    * **`permits`:** Lists the only allowed direct subtypes.
    * **`non-sealed`:** Allows a subtype to be freely extended again, opening the hierarchy back up.
* **The Subclass Mandate:** Every direct subtype of a `sealed` type **must** be declared as `final`, `sealed`, or `non-sealed`.
* **`permits` Clause:** Is optional if all direct subtypes are in the same `.java` file or are nested within the sealed class. Otherwise, it is required.
* **`switch` Expressions:** Sealed types allow for exhaustive `switch` statements without a `default` branch if all permitted subtypes are handled as cases.

## 4. Encapsulating Data with Records

* **Concept:** A `record` is a concise, immutable, data-oriented class that drastically reduces boilerplate code.
* **Compiler-Generated Members:** For `record Person(String name, int age)`, the compiler generates:
    * A `public` "canonical" constructor that accepts all components (`String`, `int`).
    * `public` accessor methods for each component (`name()`, `age()`).
    * Implementations of `equals()`, `hashCode()`, and `toString()`.
* **Rules and Customization:**
    * Records and their components are implicitly `final`.
    * You cannot add extra non-static instance fields or instance initializers.
    * **Constructors:** Can be customized with a `compact` constructor (for validation), a `long` constructor (explicitly declared), or `overloaded` constructors (which must call `this()`).
* **Pattern Matching:** Allows deconstructing a record to access its components directly (e.g., `if (obj instanceof Person(var n, var a))`). The pattern must be exhaustive, component order must match, and types must be compatible.

## 5. Creating Nested Classes

* **Definition:** A class defined within another class. There are four types.
* **1. Inner Class:** A non-static member class. It is tied to an instance of the outer class and can access all its members. It is instantiated via `outerInstance.new Inner()`.
* **2. Static Nested Class:** A `static` member class. It is not tied to an outer instance and cannot access the outer class's instance members. It is instantiated like a namespaced class: `new Outer.StaticNested()`.
* **3. Local Class:** A class defined inside a method. Its scope is limited to the method. It can access local variables only if they are `final` or `effectively final`.
* **4. Anonymous Class:** An unnamed local class that is declared and instantiated in one statement. It must extend exactly one class or implement exactly one interface.