# Beyond Classes

This document provides summaries, conceptual questions, scenario-based questions, and coding problems for the chapter "Beyond Classes," covering Interfaces, Enums, Records, Nested Classes, and Polymorphism.

-----

## Topic 1: Implementing Interfaces

### Summary 🧩

Interfaces in Java define a contract for classes. They are abstract data types that can contain constants, abstract methods, and, with modern Java versions, also default, static, and private methods.

**Key Concepts:**

  * **Definition:** Declared with the `interface` keyword. Cannot be instantiated directly and cannot be marked `final`.
  * **Implicit Modifiers:**
      * Interfaces are implicitly `abstract`.
      * Variables (constants) are implicitly `public static final` and must be initialized.
      * Abstract methods (methods without a body) are implicitly `public abstract`.
      * Non-private methods (abstract, default, static) are implicitly `public`.
  * **Implementation:**
      * A class uses `implements` to adhere to one or more interfaces.
      * A concrete class must provide `public` implementations for all inherited abstract methods.
  * **Extension:**
      * An interface can `extends` multiple other interfaces.
  * **Duplicate Abstract Methods:**
      * Allowed if method signatures are compatible (a single implementation can satisfy all).
      * Compile error if methods with the same name and parameter types have incompatible return types.
  * **Invalid Keyword Usage:**
      * A `class` cannot `extend` an `interface`.
      * An `interface` cannot `implement` another interface (it `extends`).
  * **No `protected` or Package-Private Members:** Interface members are either `public` or `private`.

**Concrete Methods in Interfaces (Java 8+):**

  * **`default` Methods:**
      * Provide a default implementation (`default void myMethod() {...}`). Implicitly `public`.
      * **Purpose:** Backward compatibility, allowing interfaces to evolve.
      * Implementing classes inherit them but can override.
      * **Conflict Resolution:** If a class implements multiple interfaces with conflicting default methods (same signature), the class **must override** the method. Specific super-interface default method can be called using `InterfaceName.super.methodName();`.
  * **`static` Methods:**
      * Belong to the interface itself (`static void myStaticMethod() {...}`). Implicitly `public` (unless `private`).
      * **Not inherited** by implementing classes; must be called using `InterfaceName.staticMethodName();`.
  * **`private` Methods (Java 9+):**
      * Can be `private` or `private static`. Must have a body.
      * **Purpose:** Internal helper logic within the interface. Not accessible or inheritable by implementing classes.
      * `private static` methods can be called by any method in the interface.
      * `private` non-static methods can be called by default methods or other private non-static methods.

### Conceptual Questions

1.  What is an interface in Java, and how does it differ fundamentally from a class in terms of direct instantiation and its capability to be extended or implemented?
2.  Interface variables (constants) have implicit modifiers. What are these modifiers?
3.  If you declare a method in an interface simply as `void performAction();` (with no body and no explicit modifiers like `public` or `abstract`), what are its implicit modifiers?
4.  When a concrete (non-abstract) class implements an interface, what is its obligation regarding the abstract methods defined in that interface? What access modifier must these implementing methods have in the concrete class?
5.  Can an interface `extends` another interface? Can an interface `extends` multiple interfaces?
6.  Can a class `extends` multiple classes? Can a class `implements` multiple interfaces?
7.  What happens if a class attempts to implement two interfaces that have abstract methods with the exact same signature (name and parameter types)? What if those abstract methods have the same name and parameter types but *different, incompatible* return types?
8.  Besides backward compatibility, do `default` methods need to be implemented by a class that implements the interface?
9.  If a class implements two interfaces, and both interfaces provide a `default` method with the exact same signature, what must the implementing class do to resolve this? How can an implementing class, within its own overriding method, specifically call the `default` method from one of its super-interfaces?
10. How must `static` methods in an interface be called? Are they inherited by implementing classes in the same way instance or default methods are?
11. What is the primary purpose of `private` methods (both static and non-static) within an interface? Who is allowed to call these `private` interface methods?
12. Can an interface be declared as `final`? Why or why not?
13. Can methods or variables within an interface be declared with `protected` or package-private (default) access? Explain your reasoning.

### Scenario-Based Questions

1.  If an interface `Configurable` declares `int MAX_USERS = 100;`, can an implementing class change the value of `MAX_USERS`? Why or why not?
2.  Consider:
    ```java
    interface Movable {
        void move();
        default void stop() { System.out.println("Stopping."); }
    }
    abstract class Vehicle implements Movable {
        // Does Vehicle need to implement move() or stop()?
    }
    class Car extends Vehicle {
        // If Vehicle does nothing, what must Car do?
    }
    ```
    What are the obligations of `Vehicle` and `Car` regarding the methods in `Movable`?
3.  If `interface X { static String id() { return "ID-X"; } }` and `class MyClass implements X {}`, is the call `MyClass.id()` valid? Is `new MyClass().id()` valid?

### Coding Problems

1.  **Payment System:**
      * Define an interface `Payable` with an abstract method `double getAmountDue()`.
      * Add a `default` method `String getPaymentInstructions()` that returns "Please pay the amount due."
      * Create two classes, `Invoice` and `SalaryRecipient`, that implement `Payable`.
      * `Invoice` should have fields like `invoiceId` and `amount`.
      * `SalaryRecipient` should have fields like `employeeName` and `monthlySalary`.
      * Implement `getAmountDue()` appropriately for both.
      * Let `SalaryRecipient` override `getPaymentInstructions()` to return "Salary will be auto-deposited."

-----

## Topic 2: Working with Enums

### Summary 🗓️

Enums (enumerations) provide a type-safe way to define a fixed set of named constants.

**Key Concepts:**

  * **Definition:** Declared with `enum` keyword (e.g., `enum Season { WINTER, SPRING, SUMMER, FALL }`).
  * **Type Safety:** Prevents invalid constant values.
  * **Enum Constants:** Named `ALL_UPPERCASE_SNAKE_CASE`. Accessed via `EnumName.CONSTANT_NAME`. `toString()` returns the name. Compared using `==`.
  * **Restrictions:** Cannot be extended (implicitly `final`). Cannot be explicitly `final`. Can implement interfaces.
  * **Common Enum Methods:**
      * `values()`: Returns an array of all enum constants.
      * `name()`: Returns the declared name (String).
      * `ordinal()`: Returns zero-based declaration index (int).
      * `valueOf(String name)`: Returns enum constant for the name; throws `IllegalArgumentException` if no match.
  * **Enums in `switch`:** If all constants handled, `default` not required. `case` labels use constant name (e.g., `case WINTER:`).
  * **Complex Enums:**
      * List of constants first, ends with `;` if other members follow.
      * Can have instance/static fields (preferably `final` for instance fields).
      * **Constructors:** Implicitly `private`. Called once per constant when enum loaded.
      * **Methods:** Can have instance and static methods. Can implement interface methods.
      * **Constant-Specific Method Implementations:** Enum can declare an `abstract` method (each constant must implement it) or provide a concrete method that constants can override.

### Conceptual Questions

1.  What is the primary advantage of using an enum over a set of `public static final String` or `int` constants for representing a fixed set of values?
2.  Can you extend an enum class in Java? Can an enum itself be declared `final`? Explain your reasoning.
3.  How are enum constants typically named (by convention)?
4.  Briefly describe the purpose of the following enum methods: `values()`, `name()`, `ordinal()`, and `valueOf(String name)`. What kind of exception might `valueOf(String name)` throw?
5.  When using an enum in a `switch` statement, is a `default` case always required if you handle all declared enum constants in `case` labels?
6.  Can you use the `ordinal()` value of an enum constant directly in a `case` label of a `switch` statement that is switching on an enum type?
7.  If an enum definition includes members like fields, methods, or constructors (in addition to the list of enum values), what punctuation mark is required after the list of enum values?
8.  What is the implicit access modifier for an enum constructor? Can you declare an enum constructor as `public` or `protected`?
9.  When are the constructors for enum constants actually called by the JVM?
10. Describe the two main ways an enum can allow each of its constants to have a different implementation for a particular method.

### Scenario-Based Questions

1.  If `enum Status { PENDING, ACTIVE, INACTIVE }`, what will `Status.PENDING.toString()` return?
2.  What is the output of `Status.ACTIVE.ordinal()` if `PENDING` is declared first, `ACTIVE` second, and `INACTIVE` third?
3.  If you call `Status.valueOf("active")`, what happens?
4.  Consider `enum TrafficLight { RED("Stop"), GREEN("Go"), YELLOW("Caution"); String action; TrafficLight(String action){ this.action=action;} }`. How would you get the string "Go" from this enum?

### Coding Problems

1.  **Planet Enum:**
      * Create an enum `Planet` with constants for the 8 planets in our solar system.
      * Give each planet instance variables for `mass` (double) and `radius` (double).
      * Create a constructor to initialize these values.
      * Add a `public double getSurfaceGravity()` method (you can use a simplified formula like `G*mass/(radius*radius)`, where G can be a constant).
      * Add a `public static void main(String[] args)` to iterate through `Planet.values()` and print each planet's name and its surface gravity.

-----

## Topic 3: Encapsulating Data with Records

### Summary 💿

This section reviews encapsulation and introduces Java Records as a concise way to create data-oriented classes that automatically implement boilerplate (fields, constructor, accessors, `equals()`, `hashCode()`, `toString()`), promoting immutability.

**Key Concepts:**

  * **Encapsulation:** Protecting class members (usually by making fields `private` and using public getters/setters) to control access and modification.
  * **Records:** A special data-oriented class `record RecordName(Type1 comp1, Type2 comp2) {}`.
      * **Boilerplate Reduction:** Compiler generates:
          * `private final` fields for each component.
          * A `public` canonical constructor matching components.
          * `public` accessor methods (e.g., `comp1()`, `comp2()`).
          * `equals()`, `hashCode()`, and `toString()` based on all components.
      * **Immutability:** Inherently immutable (final fields, no setters). Implicitly `final` (cannot be extended). Can implement interfaces.
      * **Constructors:**
          * **Canonical Constructor:** Matches record components. Can be explicit (then you must assign all fields) or implicit.
          * **Compact Constructor:** Concise syntax (`public RecordName { /* validation */ }`) without parameters in its declaration. Runs before implicit field assignment; used for validation/transforming implicit parameters. Cannot assign to `this.field`.
          * **Overloaded Constructors:** Must call another constructor via `this(...)` as the first statement, eventually chaining to canonical.
      * **Customization:** Can have `static` members, instance/static methods. **Cannot** have additional instance fields (beyond components) or instance initializers.
  * **Pattern Matching with Records (Java 21+):** Allows deconstructing components in `instanceof` and `switch`.
      * Syntax: `if (obj instanceof MyRecord(Type1 c1, Type2 c2)) { ... }`.
      * Order of components in pattern matters. Pattern variable names can differ. Types must be compatible. `var` can be used. Nested patterns supported.

### Conceptual Questions

1.  In your own words, what is encapsulation, and why is it considered a crucial principle in object-oriented design?
2.  How does making instance variables `private` contribute to encapsulation, and what role do getter (accessor) methods typically play in an encapsulated class?
3.  What is the primary problem that Java Records aim to solve when creating data-oriented classes?
4.  Can you list at least four types of members that the Java compiler automatically generates for a simple record declaration (e.g., `public record Point(int x, int y)`)?
5.  How do the automatically generated accessor methods in a record (e.g., for a component `name`) typically differ in naming convention from traditional JavaBean getter methods?
6.  Are the fields corresponding to the record components (e.g., `x` and `y` in `Point(int x, int y)`) mutable or immutable by default in a record? Are records themselves extensible (can they be subclassed)?
7.  What is the main purpose of a "compact constructor" in a record, and how does its syntax differ from a canonical or "long" constructor regarding its parameter list?
8.  If you define an additional, overloaded constructor in a record (one that doesn't match the record components directly), what is the mandatory first statement in that overloaded constructor?
9.  Can you add new instance fields to a record outside of its main component list in the declaration? Can records have `static` fields or `static` initializers?
10. How does the design of records (e.g., no setters, final fields) inherently support the creation of immutable objects?
11. What is the basic idea behind using pattern matching with records (e.g., in an `instanceof` check or a `switch` statement)?
12. When deconstructing a record using a pattern, does the order of components in the pattern matter? Do the variable names used in the pattern need to match the record component names?

### Scenario-Based Questions

1.  If you declare `public record Book(String title, String author) {}`, what is the signature of the automatically generated canonical constructor? How would you call the accessor for `title`?
2.  Is the following compact constructor valid for `record Point(int x, int y)`?
    ```java
    public Point {
        this.x = Math.abs(x); // Assume x is the implicit parameter
        this.y = Math.abs(y); // Assume y is the implicit parameter
    }
    ```
    Why or why not?
3.  If `record User(String name, int age) {}`, which of these `instanceof` checks using pattern matching would compile and correctly deconstruct?
      * `if (obj instanceof User(String n, Integer a))`
      * `if (obj instanceof User(var n, var a))`
      * `if (obj instanceof User(int a, String n))`

### Coding Problems

1.  **Create a record `ColorPoint`** that has an `int x`, `int y`, and a `String color`.
2.  **Add a compact constructor to `ColorPoint`** that validates `x` and `y` to be non-negative (throws `IllegalArgumentException` if not) and ensures `color` is not null (defaults to "black" if null is passed).
3.  **Add an overloaded constructor** to `ColorPoint` that takes only `x` and `y`, and sets the `color` to "transparent".

-----

## Topic 4: Creating Nested Classes

### Summary Nested️

A nested class is a class defined within another class, enhancing encapsulation and logical grouping. There are four types: inner, static nested, local, and anonymous classes.

**Key Concepts:**

  * **Types:**
      * **Inner Class (Member Inner Class):** Non-static, defined at member level. Tied to an instance of the outer class. Can access all outer class members (even private). Instantiated via `outerInstance.new Inner()`.
      * **Static Nested Class:** `static` class at member level. Not tied to an outer class instance. Cannot directly access outer instance members (only static ones). Instantiated like `new OuterClass.StaticNestedClass()`. (Nested records are implicitly static).
      * **Local Class:** Defined within a method body. No access modifiers. Can access enclosing class members and `final` or effectively `final` local variables of the method.
      * **Anonymous Class:** A local class without a name, declared and instantiated in one expression. Must extend a class or implement an interface. No explicit constructors.
  * **Benefits:** Improved encapsulation, logical grouping, specialized helper classes.
  * **Drawbacks:** Can reduce readability if overused, tight coupling (for inner classes).

### Conceptual Questions

1.  What are the four main types of nested classes described in your text?
2.  Conceptually, what are some reasons a programmer might choose to use a nested class instead of a top-level class?
3.  What is the key characteristic of an inner class concerning its relationship with an instance of the outer class? How does this characteristic affect its ability to access members (fields and methods) of the outer class?
4.  If you are inside a `static` method of the outer class, how would you typically create an instance of a non-static inner class named `Inner` (assuming `Outer` is the name of the outer class)?
5.  How does a static nested class fundamentally differ from an inner class regarding its connection to an instance of the outer class and its ability to access the outer class's instance members?
6.  Can a static nested class be declared `private`? If so, what would be the scope of its accessibility?
7.  What limitations are placed on a local class declaration in terms of access modifiers (like `public`, `private`)?
8.  A local class can access local variables of the method it's defined in, but under what specific conditions?
9.  What does it mean for a class to be "anonymous"? When declaring an anonymous class, what must it always do in terms of an existing class or interface?
10. Can an anonymous class have its own explicitly declared constructor with a name? Why or why not?
11. If you need a helper class that will only be used within a single method and needs to access that method's local variables, which type of nested class would be most suitable?
12. If you want to group a class logically with its outer class but it does not require access to the outer class's instance members and should be instantiable independently, which type of nested class would you choose?
13. When might an inner class (non-static) be chosen over a static nested class, even if it means a tighter coupling with the outer class instance?

### Scenario-Based Questions

1.  Consider:
    ```java
    public class Outer {
        private int outerField = 10;
        class Inner {
            void display() { System.out.println(outerField); } // Valid?
        }
        static class StaticNested {
            // void display() { System.out.println(outerField); } // Valid?
        }
    }
    ```
    Is the `display()` method in `Inner` valid? Would a similar `display()` method in `StaticNested` (accessing `outerField`) be valid? Why or why not?
2.  If a method `void process()` declares a local variable `int count = 5;`, and a local class `MyLocal` inside `process()` tries to use `count`. If later in `process()`, but after `MyLocal`'s definition, `count` is changed (e.g., `count = 10;`), can `MyLocal` still access `count`?
3.  Can an anonymous class implement two interfaces simultaneously?

### Coding Problems

1.  **`DataProcessor` with Inner `Parser`:**
      * Create a class `DataProcessor` with a private `String data`.
      * Create a non-static public inner class `Parser` inside `DataProcessor`.
      * The `Parser` should have a method `String getProcessedData()` that can access and modify (e.g., append a suffix from) the `data` field of its `DataProcessor` instance and return it.
      * The `DataProcessor` should have a method that creates and uses its `Parser`.
2.  **`ConfigurationLoader` with Static Nested `Config`:**
      * Create a class `ConfigurationLoader`.
      * Define a `public static class Config` (a static nested class) inside `ConfigurationLoader` with fields like `url` (String) and `timeout` (int). Make `Config` instantiable with a constructor.
      * `ConfigurationLoader` should have a `static Config loadDefaultConfig()` method that creates and returns a `Config` object with default values.
3.  **Event Handler with Anonymous Class:**
      * Define an interface `EventHandler` with a single method `void handleEvent(String eventName)`.
      * In a `Button` class, create a method `setOnClickListener(EventHandler handler)` that stores the handler.
      * Add a `click()` method to `Button` that, if a handler is set, calls its `handleEvent` method with "ButtonClicked".
      * In a `main` method, create a `Button` and register an event handler using an anonymous class that prints the event name. Then, simulate a click.

-----

## Topic 5: Understanding Polymorphism

### Summary 🎭

Polymorphism ("many forms") allows an object to be treated as an instance of its own class, any superclass, or any implemented interface. Behavior, especially of overridden methods, is determined by the object's actual type at runtime.

**Key Concepts:**

  * **Object Type vs. Reference Type:**
      * **Object's Actual Type:** Defined by `new ClassName()`. Never changes. Determines actual fields/methods in memory.
      * **Reference Variable's Type:** Declared type of the variable. Determines which members are *accessible* without a cast.
  * **Casting:**
      * **Upcasting (Implicit):** Subtype to supertype/interface. Safe, no explicit cast needed.
      * **Downcasting (Explicit):** Supertype to subtype. Requires `(Subtype)reference`. Risks `ClassCastException` at runtime if object isn't an instance of the target type.
      * Compiler disallows casts between unrelated types.
  * **`instanceof` Operator:** Checks if an object *is a* particular type. Used with pattern matching (`if (obj instanceof MyType varName)`) to safely cast and assign.
  * **Polymorphism with Method Overriding (Dynamic Method Dispatch):**
      * When an overridden instance method is called, the version executed is based on the **object's actual type at runtime**, not the reference type.
  * **Overriding vs. Hiding:**
      * **Instance Methods:** Overridden (polymorphic).
      * **Static Methods:** Hidden (version called depends on reference type).
      * **Variables (Fields):** Hidden (variable accessed depends on reference type).
      * Hiding is generally discouraged.

### Conceptual Questions

1.  Explain the difference between an object's actual type and the type of the reference variable pointing to it. Which determines accessible members? Which determines behavior of overridden instance methods?
2.  What is "upcasting"? Is an explicit cast operator required? Why is it generally safe?
3.  What is "downcasting"? Why is an explicit cast operator required, and what runtime risk is associated?
4.  Under what circumstances might the compiler disallow a cast between two class types?
5.  What is the primary purpose of the `instanceof` operator when dealing with object references and casting?
6.  How does pattern matching with `instanceof` (e.g., `if (obj instanceof MyType variableName)`) enhance its usability?
7.  Describe "dynamic method dispatch" in the context of overridden instance methods. How does Java decide which version of an overridden method to execute?
8.  If a superclass reference variable points to a subclass object, and you call an instance method defined in the superclass and overridden in the subclass, which version runs?
9.  How does the polymorphic behavior of overridden *instance methods* differ from the behavior of hidden *static methods* when called using a superclass reference pointing to a subclass object?
10. How does accessing an instance *variable* hidden in a subclass differ from calling an overridden instance *method*, regarding which version is used via a superclass reference?
11. Why is hiding members (static methods and variables) generally considered poor practice?

### Scenario-Based Questions

1.  Given `Animal animal = new Dog();` where `Dog extends Animal`. If `Animal` has a method `eat()` and `Dog` overrides `eat()`, which `eat()` method is executed when `animal.eat()` is called?
2.  If `Object obj = "Hello";`, is `String str = (String) obj;` a valid cast? What if `obj` actually held an `Integer`?
3.  If `Shape s = getSomeShape();` and you want to call a method `getRadius()` that only `Circle` shapes have, what should you do before attempting `((Circle)s).getRadius()` to avoid a runtime error?
4.  Consider:
    ```java
    class Parent { public static String info() { return "Parent"; } }
    class Child extends Parent { public static String info() { return "Child"; } }
    // In main:
    // Parent p = new Child();
    // System.out.println(p.info()); // What is printed?
    ```
    What is printed by `p.info()` and why?

### Coding Problems

1.  **Zoo Simulator:**
      * Create an abstract class `Animal` with an abstract method `makeSound()` and a concrete method `eat(String food)` that prints "[Animal Name] is eating [food]".
      * Create two subclasses, `Lion` and `Parrot`, extending `Animal`.
      * Implement `makeSound()` in each subclass to print an appropriate sound.
      * Override `eat(String food)` in `Parrot` to print "Parrot is delicately eating [food]".
      * In a `main` method, create an array of `Animal`. Add a `Lion` and a `Parrot` to it.
      * Iterate through the array and call `makeSound()` and `eat("seeds")` on each animal. Observe polymorphic behavior.
2.  **Shape Casting:**
      * Create an interface `Drawable` with a method `draw()`.
      * Create a class `Circle implements Drawable` with a method `getRadius()`.
      * Create a class `Rectangle implements Drawable` with a method `getArea()`.
      * In `main`, create a `List<Drawable>`. Add a `Circle` and a `Rectangle` to it.
      * Iterate through the list. For each element, call `draw()`.
      * Also, safely check if the element is a `Circle` and if so, cast it and print its radius. Similarly, check for `Rectangle` and print its area. Use `instanceof` and pattern matching if comfortable.

-----