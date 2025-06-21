# Chapter Summary: Methods

This summary covers the key concepts for declaring and using methods in Java, including variable scope, access control, static members, data passing, and overloading.

## 1. Declaring Local and Instance Variables

* **Local Variables**
    * **Definition:** Declared within a method, constructor, or a block of code.
    * **Scope:** Limited to the block in which they are declared. They are destroyed once the block execution is complete.
    * **Initialization:** Must be explicitly initialized before they are used.
    * **Modifiers:** The only applicable modifier is `final`.
    * **`final` Local Variables:** Can be assigned a value only once. The `final` keyword makes a primitive's value immutable and an object's reference immutable.
    * **`effectively final`:** A local variable whose value is never changed after its initial assignment. This is a crucial concept for lambda expressions and inner classes.

* **Instance Variables**
    * **Definition:** Declared inside a class but outside any method. Also known as member variables.
    * **Scope:** Tied to the object instance; each object gets its own copy.
    * **Initialization:** Receive a default value if not explicitly initialized (e.g., `0`, `false`, `null`). This does **not** apply to `final` instance variables.
    * **Modifiers:** Can use access modifiers (`public`, `protected`, etc.) and other specifiers like `final`, `volatile`, and `transient`.
    * **`final` Instance Variables:** Must be initialized exactly once, either at declaration, in an instance initializer block, or within every constructor.

## 2. Working with Varargs

* **Definition:** Varargs (variable arguments) allow a method to accept zero or more arguments of a specified type, which are treated as an array inside the method.
* **Declaration Rules:**
    1.  A method can have at most **one** varargs parameter.
    2.  The varargs parameter must be the **last parameter** in the method signature.
* **Calling a Varargs Method:** Can be done by passing elements individually, passing a pre-made array, or passing zero arguments (which creates a zero-length array).
* **Handling `null`:** Passing `null` to a varargs parameter is possible but will cause a `NullPointerException` if the method body tries to access its length or elements.

## 3. Applying Access Modifiers

This summary is ordered from most restrictive to least restrictive.

* **`private`**: Accessible **only** within the same class.
* **Package-Private (Default)**: No modifier. Accessible to any class within the **same package**.
* **`protected`**: Includes package-private access **plus** access for subclasses in different packages.
    * **The "Gotcha" Rule:** A subclass in a different package can only access a `protected` member through a reference of its own type (or a subtype), not through a superclass reference.
* **`public`**: Accessible from **any class in any package**.

| Access Modifier | Same Class | Same Package | Subclass (Different Package) | Different Package (Unrelated) |
| :--- | :---: | :---: | :---: | :---: |
| `private` | **Yes** | No | No | No |
| `(package)` | **Yes** | **Yes** | No | No |
| `protected` | **Yes** | **Yes** | **Yes** | No |
| `public` | **Yes** | **Yes** | **Yes** | **Yes** |

## 4. Accessing Static Data

* **Core Concept:** The `static` keyword means a member (variable or method) belongs to the **class itself**, not to any specific instance.
* **Static vs. Instance Interaction:**
    * A `static` method **cannot** directly access an `instance` member without an explicit object reference.
    * An `instance` method **can** directly access `static` members.
* **Accessing Static Members:** The standard way is via the class name (`ClassName.staticMember`). Accessing through a `null` reference **does not** throw a `NullPointerException`.
* **`static final` Constants:** Variables that never change. They must be initialized exactly once, either at declaration or in a `static initializer` block.
* **Static Initializers:** A `static { ... }` block that is executed only once when the class is first loaded into memory.
* **Static Imports:** Used to import `static` members directly to be used without the class name prefix (e.g., `import static java.util.Arrays.asList;`).

## 5. Passing Data and Autoboxing

* **Java is Pass-by-Value:** A method receives a **copy** of the variable's value.
    * **Primitives:** Changing the copy inside the method does not affect the original.
    * **Object References:** The reference is copied, not the object. Changing the object's *state* through the copied reference affects the original object. *Reassigning* the reference inside the method does not.
* **Ignoring Return Values:** If a method returns a new value (common with immutable types like `String`) and the caller does not assign it to a variable, the original variable remains unchanged.
* **Autoboxing and Unboxing:**
    * **Autoboxing:** Automatic conversion from a primitive to its wrapper class (e.g., `int` to `Integer`).
    * **Unboxing:** Automatic conversion from a wrapper class to its primitive (e.g., `Integer` to `int`).
    * **`NullPointerException`:** Attempting to unbox a `null` wrapper object will cause an NPE.
    * **Key Limitation:** Java will not perform autoboxing and a widening conversion in a single step (e.g., you cannot assign an `int` directly to a `Long` reference).

## 6. Overloading Methods

* **Definition:** Declaring multiple methods in the same class with the **same name** but **different parameter lists** (differing by type, number, or order of parameters).
* **Invalid Overloading:** You cannot overload by only changing the return type, access modifier, or `static` keyword. A method cannot be overloaded with an array (`int[]`) and a vararg (`int...`) of the same type.
* **Method Resolution Order:** When an overloaded method is called, the compiler chooses which one to use based on this strict order of preference:
    1.  **Exact Match** (e.g., `int` argument matches `method(int)`).
    2.  **Larger Primitive Type (Widening)** (e.g., `int` argument matches `method(long)`).
    3.  **Autoboxed Type** (e.g., `int` argument matches `method(Integer)`). This can be followed by a widening reference conversion (e.g., to `Object`).
    4.  **Varargs** (e.g., `int` argument matches `method(int...)`).