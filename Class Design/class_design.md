# Chapter Summary: Class Design

This summary covers the core principles of object-oriented class design in Java, including inheritance, constructors, initialization order, abstract classes, and immutability.

## 1. Understanding Inheritance

* **Core Concept:** Inheritance is the process where a new class (subclass or child) derives from an existing class (superclass or parent).
* **`extends` Keyword:** A class is declared as a subclass using the `extends` keyword.
* **Single Inheritance:** Java supports single inheritance; a class can have only one direct parent class.
* **Multi-level Inheritance:** Inheritance is transitive. If `C extends B` and `B extends A`, then `C` is a descendant of `A`.
* **`final` Class Modifier:** A class marked `final` cannot be extended.
* **Access Modifiers:**
    * `public` and `protected` members are inherited by all subclasses.
    * Package-private (default) members are inherited only by subclasses in the same package.
    * `private` members are never inherited.
* **`java.lang.Object`:** Every class in Java implicitly extends `Object`, making it the root of all class hierarchies. If a class does not have an `extends` clause, the compiler adds it.

## 2. Creating Classes and `this`/`super` References

* **Top-Level Class Access:**
    * A `.java` file can have at most **one** `public` top-level class.
    * It can have any number of package-private (no modifier) top-level classes.
    * `protected` and `private` are illegal modifiers for top-level classes.
* **The `this` Reference:**
    * **Purpose:** Refers to the current object instance. Primarily used to distinguish between an instance variable and a method parameter when they have the same name (variable shadowing).
    * **Context:** Can be used in instance methods, constructors, and instance initializers. Cannot be used in a `static` context.
* **The `super` Reference:**
    * **Purpose:** Explicitly accesses members (fields and methods) from the direct superclass.
    * **When to Use:** Primarily used when a subclass has a member with the same name as an inherited member (variable hiding or method overriding).
    * **Key Distinction:** `this` refers to all accessible members (own and inherited). `super` refers *only* to inherited members.

## 3. Declaring Constructors

* **Rules:** A constructor's name must exactly match the class name, and it cannot have a return type.
* **Default Constructor:** If a class has **no** declared constructors, the compiler provides a public, no-argument constructor. The moment any constructor is declared, the default one is no longer provided.
* **`this()` vs. `super()` Calls:**
    * The first statement in any constructor is always a call to another constructor, either `this()` or `super()`. If neither is written, the compiler inserts a no-argument `super()`.
    * **`this()`:** Calls an overloaded constructor in the same class. Must be the first statement.
    * **`super()`:** Calls a constructor in the direct parent class. Must be the first statement.
* **Inheritance "Gotcha":** If a parent class only has constructors with arguments, any subclass must explicitly call `super(...)` with the required arguments, because the compiler-inserted `super()` will fail.

## 4. Initializing Objects

The JVM follows a strict order to initialize classes and objects.

1.  **Static Initialization (once per class):**
    * Initialize the **superclass's** static variables and static initializers.
    * Initialize the **current class's** static variables and static initializers.
2.  **Instance Initialization (for each `new` call):**
    * Initialize the **superclass's instance** (running its instance initializers and constructor).
    * Initialize the **current class's** instance variables and instance initializers.
    * Execute the **current class's** constructor body.
* **`final` Instance Variables:** Must be assigned a value exactly once, either at declaration, in an instance initializer, or in *every* constructor path.

## 5. Inheriting Members

* **Method Overriding (Instance Methods):** A subclass provides a new implementation for an inherited method.
    * **4 Rules:** Must have the same signature, access must be same or more permissive, return type must be covariant (same or a subtype), and cannot throw new or broader checked exceptions.
* **Method Hiding (Static Methods):** A subclass defines a `static` method with the same signature as a parent's `static` method. It follows the same rules as overriding, plus the method must remain `static`. The method called is based on the reference type, not the object type.
* **Hiding Variables:** A subclass variable with the same name as a parent's creates two distinct variables. The variable accessed depends on the reference type.
* **`final` Methods:** A method marked `final` cannot be overridden or hidden.
* **`private` Methods:** A `private` method is never inherited, so a subclass method with the same signature is a new, independent method, not an override.

## 6. Creating Abstract Classes

* **Abstract Class:** Declared with `abstract`, cannot be instantiated directly. Serves as a template for subclasses.
* **Abstract Method:** Declared with `abstract`, has no body, and ends with a semicolon. It defines a contract that concrete subclasses must implement.
* **Concrete Subclass:** The first non-abstract subclass must implement all inherited abstract methods.
* **Illegal Modifier Combinations:** A method or class cannot be `abstract` and `final`. A method cannot be `abstract` and `private`, or `abstract` and `static`.

## 7. Creating Immutable Objects

* **Definition:** An object whose state cannot be modified after creation. They are secure and thread-safe.
* **Strategy for Immutability:**
    1.  Make the class `final`.
    2.  Make all fields `private` and `final`.
    3.  Provide no "setter" methods.
    4.  **Prevent Leaky Getters:** For any mutable fields (like an `ArrayList`), do not return a direct reference. Either return a copy (copy-on-read) or provide delegate methods (e.g., `getItem(index)`).
    5.  **Perform Defensive Copies:** In the constructor, make a private copy of any incoming mutable arguments to prevent the caller from retaining control.