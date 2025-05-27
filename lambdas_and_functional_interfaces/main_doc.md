# Writing Simple Lambdas

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Writing Simple Lambdas" section.

-----

## Summary 🐑➡️

This section introduces lambda expressions (or "lambdas") as a core feature of functional programming in Java. It explains their purpose, benefits over traditional approaches, and the basic syntax.

1.  **What are Lambda Expressions?**

    * An unnamed block of code (like an anonymous method) that can be passed around and executed later.
    * Used to implement methods of **functional interfaces** (interfaces with exactly one abstract method).

2.  **Benefits Illustrated (Traditional vs. Lambda):**

    * Lambdas allow defining specific behavior inline, directly where needed, reducing boilerplate code (compared to creating separate implementing classes for simple, one-off logic).
    * **Deferred Execution:** Code specified now but executed later, typically when the functional interface's method is invoked.

3.  **Lambda Syntax:**

    * Basic structure: `parameters -> body`.
    * **Parameters:**
        * If there's one parameter and its type is inferred, parentheses are optional: `a -> ...`
        * For zero or multiple parameters, or if a single parameter's type is explicit, parentheses are required: `() -> ...`, `(x, y) -> ...`, `(Animal a) -> ...`
    * **Arrow Operator `->`:** Separates parameters from the body.
    * **Body:**
        * **Single Expression Body:** If the body is a single expression, curly braces `{}` are optional. The result is implicitly returned (if the functional interface method expects a return). No `return` keyword or semicolon is needed.
            * Example: `a -> a.canHop()`
        * **Block Body:** For multiple statements, curly braces `{}` are required. An explicit `return` statement and semicolons are needed if a value is to be returned.
            * Example: `(Animal a) -> { return a.canHop(); }`

4.  **Context and Type Inference:**

    * Java infers the types of lambda parameters and its return type from the **context** in which it's used—specifically, the signature of the single abstract method of the **target functional interface**.

5.  **`var` and Lambdas:**

    * Directly assigning a lambda to a variable declared with `var` (e.g., `var myLambda = a -> a.canHop();`) usually **does not compile**. This is because `var` needs a clear type from the right-hand side for inference, and the lambda itself needs a target functional interface type from the left-hand side for its own type inference, creating a circular dependency.

-----

## Conceptual Questions (for "Writing Simple Lambdas")

1.  In your own words, what is a lambda expression, and what is its primary purpose in Java?
2.  What is meant by "deferred execution" in the context of lambda expressions?
3.  Explain how Java uses "context" to understand and type-check a lambda expression.
4.  What are the conditions under which parentheses `()` around the parameter list of a lambda can be omitted?
5.  When can the curly braces `{}` for the body of a lambda, along with the `return` keyword and semicolon, be omitted?
6.  Why does an assignment like `var myLambda = x -> x.length() > 0;` typically fail to compile?

-----

## Scenario-Based Questions (for "Writing Simple Lambdas")

1.  If a functional interface `Converter` has a method `String format(Integer i)`, which of these lambda syntaxes would be valid to assign to a `Converter` variable?
    * `num -> "Value: " + num`
    * `(Integer n) -> { return "Number: " + n.toString(); }`
    * `() -> "Default"`
    * `(int x, int y) -> String.valueOf(x + y)`
2.  Imagine you have a method `process(Runnable r)` where `Runnable` is a functional interface with a single method `void run()`. If you want to pass a behavior that simply prints "Processing...", could you use a lambda? Conceptually, what would it look like?
3.  If a lambda is `(String s1, String s2) -> s1.concat(s2)`, what can you infer about the functional interface method it is intended to implement (in terms of number of parameters, their likely types, and return type)?

-----

## Coding Problems (for "Writing Simple Lambdas")

1.  **Simple String Operations:**
    * Define a functional interface `StringOperator` with a single abstract method `String apply(String s)`.
    * In a `main` method, create lambda expressions for the following operations and assign them to `StringOperator` variables:
        * Convert a string to uppercase.
        * Append "\!\!\!" to a string.
    * Test your lambdas by calling their `apply` method with sample strings.
2.  **Basic Calculator:**
    * Define a functional interface `IntCalculator` with a method `int calculate(int a, int b)`.
    * Implement addition, subtraction, and multiplication operations using lambda expressions assigned to `IntCalculator` variables.
    * Test them.
3.  **List Filter (Conceptual from Text):**
    * You have the `Animal` record: `public record Animal(String species, boolean canHop, boolean canSwim) {}`
    * You have the `CheckTrait` interface: `public interface CheckTrait { boolean test(Animal a); }`
    * You have the `print` method: `private static void print(List<Animal> animals, CheckTrait checker) { ... }`
    * Write down *only* the lambda expressions (as they would appear in the call to `print`) for the following criteria:
        * Animals that can swim.
        * Animals that *cannot* hop.
        * Animals whose species name is "fish".

-----

# Coding Functional Interfaces

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Coding Functional Interfaces" section.

-----

## Summary 🎯

This section defines functional interfaces, explains the `@FunctionalInterface` annotation, and details the rules for determining if an interface qualifies as functional, including how methods inherited from `java.lang.Object` are treated.

1.  **Definition of a Functional Interface:**

    * An interface that contains **exactly one abstract method**. This is often referred to as the SAM (Single Abstract Method) rule.
    * These interfaces are the target types for lambda expressions and method references.

2.  **The `@FunctionalInterface` Annotation:**

    * **Purpose:** This annotation is used to indicate to the compiler that the annotated interface is intended to be a functional interface.
    * **Compiler Check:** If an interface is annotated with `@FunctionalInterface` but does not satisfy the criteria of a functional interface (e.g., it has zero or more than one abstract method), the compiler will generate an error.
    * **Optional but Recommended:** An interface can be functional without this annotation if it meets the SAM rule. However, using the annotation is good practice as it makes the intent clear and ensures future compatibility if the interface is modified.

3.  **Identifying Functional Interfaces:**

    * **Basic Case:** An interface with one declared abstract method is functional.
    * **Interface Extension:**
        * If an interface `Dash` extends a functional interface `Sprint` and `Dash` does not declare any new abstract methods, `Dash` is also a functional interface (it inherits the single abstract method from `Sprint`).
        * If an interface `Skip` extends `Sprint` but also declares its own new abstract method `skip()`, then `Skip` is *not* a functional interface because it now has two abstract methods.
    * **Impact of `default`, `static`, and `private` methods:** These types of methods (which have implementations or are not abstract in the traditional sense) **do not count** towards the single abstract method limit. An interface can have many default, static, or private methods and still be a functional interface if it possesses only one abstract method.

4.  **Abstract Methods Overriding `java.lang.Object` Methods:**

    * **Special Rule:** If an interface declares an abstract method that has the **same signature as a `public` method in `java.lang.Object`** (such as `toString()`, `equals(Object)`, or `hashCode()`), these methods **do not count** towards the single abstract method limit for determining if an interface is functional.
    * **Rationale:** Any class that implements such an interface will already inherit these methods from `Object` (since all classes extend `Object`).
    * **Incompatibility with `Object` methods:** An interface cannot declare an abstract method that is incompatible with a public `Object` method (e.g., `int toString()` would not compile because `Object.toString()` returns `String`).
    * **Signature Mismatch:** An abstract method like `boolean equals(Hibernate o)` (where `Hibernate` is not `Object`) *does* count as an abstract method because its signature is different from `Object`'s `public boolean equals(Object o)`.

-----

## Conceptual Questions (for "Coding Functional Interfaces")

1.  What is the defining characteristic of a functional interface in Java (often referred to by an acronym)?
2.  What is the purpose of the `@FunctionalInterface` annotation? Is it mandatory for an interface to be functional?
3.  If an interface `B` extends a functional interface `A` but does not declare any new methods, is interface `B` also a functional interface? What if `B` declares one new abstract method?
4.  Do `default` methods, `static` methods, or `private` methods within an interface count towards the single abstract method limit when determining if an interface is functional?
5.  Explain the special rule regarding abstract methods in an interface that have the same signature as `public` methods in the `java.lang.Object` class (like `toString()` or `equals(Object)`). How do these affect an interface's status as functional?
6.  Why would an interface declaring `abstract boolean equals(MyType o);` (where `MyType` is not `Object`) potentially have this `equals` method counted towards its abstract method tally, even though `Object` has an `equals` method?

-----

## Scenario-Based Questions (for "Coding Functional Interfaces")

1.  Consider an interface `Processor { void process(String data); }`. Is this a functional interface? If you add `@FunctionalInterface` above it, would it compile?
2.  Imagine an interface `Logger { default void log(String msg) { System.out.println(msg); } String getFormat(); }`. Is `Logger` a functional interface? Why or why not?
3.  If interface `I1` has `void m1();` and interface `I2 extends I1` has `void m2();`, is `I2` a functional interface?
4.  An interface `Validator` declares `boolean isValid(String s);` and also `String toString();`. Could this `Validator` interface be a functional interface? Explain.
5.  If an interface `Configurable` only contains `static final int MAX_RETRIES = 3;` and `static String getDefaultName() { return "default"; }`, is it a functional interface?

-----

## Coding Problems (for "Coding Functional Interfaces")

1.  **Define `IntOperation`:**
    * Create a functional interface named `IntOperation` that has a single abstract method capable of taking two `int` arguments and returning an `int` result.
    * Annotate it with `@FunctionalInterface`.
2.  **Define `StringChecker`:**
    * Create a functional interface named `StringChecker`.
    * Its single abstract method should take a `String` and return a `boolean`.
    * Add a `default` method to this interface called `isNotNullOrEmpty(String s)` that returns `true` if the given string is not null and not empty, `false` otherwise.
3.  **Identify Functional Interfaces (Conceptual Exercise):**
    Analyze whether the following conceptual interface definitions would qualify as functional interfaces. Explain your reasoning for each.
    * `interface A { void run(); void stop(); }`
    * `interface B { void execute(); default void setup() {} static void cleanup() {} }`
    * `interface C { boolean equals(Object obj); }`
    * `interface D extends B { void anotherAction(); }` (assuming `B` from above)
    * `interface E { int compareTo(Object o); }`

-----

# Using Method References

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using Method References" section.

-----

## Summary 🎯::

This section introduces method references as a shorthand syntax for certain lambda expressions, referring to existing methods by name to improve code readability for deferred execution via functional interfaces.

1.  **What are Method References?**

    * A method reference is a compact, readable shorthand for a lambda expression whose sole purpose is to call an existing method.
    * It uses the `::` (double colon) operator.
    * It refers to a method without invoking it immediately; invocation happens later.

2.  **Equivalence to Lambdas:**

    * Method references and their equivalent lambdas behave the same way at runtime.
    * They are used as implementations for functional interfaces.

3.  **Four Types of Method References:**

    * **1. Reference to a `static` Method:**
        * **Syntax:** `ClassName::staticMethodName`
        * **Lambda Equivalent:** `(parameters) -> ClassName.staticMethodName(parameters)`
        * **Example:** `Math::round` is like `x -> Math.round(x)`.
        * Java infers which overloaded static method to use based on the functional interface's method signature.
    * **2. Reference to an Instance Method of a Particular (Existing) Object:**
        * **Syntax:** `instanceReference::instanceMethodName`
        * **Lambda Equivalent:** `(parameters) -> instanceReference.instanceMethodName(parameters)`
        * **Example:** If `String str = "Zoo";`, then `str::startsWith` is like `s -> str.startsWith(s)`. For no-parameter methods: `str::isEmpty` is like `() -> str.isEmpty()`.
        * **Limitation:** Not all lambdas using an instance variable can be converted if the lambda does more than just call a method on that specific instance or passes additional arguments not covered by the functional interface's parameters.
    * **3. Reference to an Instance Method of an Arbitrary Object of a Particular Type (on a parameter):**
        * **Syntax:** `ClassName::instanceMethodName`
        * **Lambda Equivalent (if FI method takes one parameter of `ClassName`):** `(obj) -> obj.instanceMethodName()`
        * **Lambda Equivalent (if FI method takes `ClassName` as first param, then others):** `(obj, p1, p2) -> obj.instanceMethodName(p1, p2)`
        * **Example 1 (no additional parameters for instance method):** `String::isEmpty` is like `s -> s.isEmpty()`. The lambda parameter becomes the instance.
        * **Example 2 (with additional parameters for instance method):** `String::startsWith` (for a two-parameter FI) is like `(s, p) -> s.startsWith(p)`. The first lambda parameter is the instance, the second is the argument.
    * **4. Reference to a Constructor:**
        * **Syntax:** `ClassName::new`
        * **Lambda Equivalent (no-arg constructor):** `() -> new ClassName()`
        * **Lambda Equivalent (constructor with arguments):** `(params) -> new ClassName(params)`
        * **Example:** `String::new` can be like `() -> new String()` or `x -> new String(x)`, depending on the functional interface.

4.  **Context is Key:**

    * For all types, especially with overloaded methods or multiple constructors, Java uses the **context** of the functional interface's abstract method signature (parameter types, return type) to determine which specific method or constructor is being referenced.

-----

## Conceptual Questions (for "Using Method References")

1.  What is a method reference in Java, and what is its primary purpose compared to a full lambda expression?
2.  What is the special operator used to denote a method reference?
3.  Method references, like lambdas, are used for "deferred execution." What does this mean?
4.  List the four main types/formats of method references.
5.  When using a method reference for an overloaded method (e.g., `Math::round`), how does Java determine which specific version of the overloaded method is being referred to?
6.  Explain the difference between a method reference like `myString::length` (instance method on a particular object) and `String::length` (instance method on an arbitrary object of a particular type). What would the functional interface signatures look like for each to be valid?
7.  Can all lambda expressions be converted into method references? If not, describe a scenario where a lambda cannot be easily replaced by a method reference.
8.  How does Java determine which constructor is being referred to when you use a constructor reference like `ArrayList::new` if `ArrayList` has multiple constructors?

-----

## Scenario-Based Questions (for "Using Method References")

1.  If you have a `List<String> names` and you want to print each name using `forEach`, could you use a method reference for `System.out.println`? If so, what would it look like?
2.  Consider a functional interface `Predicate<T>` with a method `boolean test(T t)`. If you have a class `Person` with an instance method `boolean isAdult()`, and an instance `Person person = new Person();`:
    * How would you write a method reference that could be assigned to a `Supplier<Boolean>` using `person.isAdult()`?
    * How would you write a method reference that could be assigned to `Predicate<Person>` which refers to the `isAdult` method to be called on *any* `Person` object passed to the predicate?
3.  Suppose you have a functional interface `Supplier<T>` with a method `T get()`. If you want to create new `StringBuilder` objects using this, what would the constructor reference look like?
4.  If a functional interface `MyFunction` has the method `int convert(String s)`, and the `Integer` class has a static method `parseInt(String s)`, what would be the method reference to implement `MyFunction` using `Integer.parseInt`?

-----

## Coding Problems (for "Using Method References")

1.  **List Transformation:**
    * Given a `List<String>` containing numbers as strings (e.g., "1", "2", "3").
    * Use the `Stream` API's `map` method and a method reference to convert this list into a `List<Integer>`.
    * Print the resulting list of integers.
2.  **Sorting Objects:**
    * Create a simple `Person` class with `String name` and `int age` fields.
    * Create a list of `Person` objects.
    * Sort the list by name using `Collections.sort()` or `List.sort()` and a method reference.
    * Then, sort the list by age using a method reference (Hint: `Integer.compare`).
3.  **Object Creation:**
    * Define a functional interface `ObjectFactory<T, P>` with a method `T create(P param)`.
    * Use this interface and a constructor reference to create instances of a class `Message` which has a constructor `Message(String content)`.
    * Demonstrate creating a `Message` object using your factory.

-----

# Working with Built-in Functional Interfaces

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Working with Built-in Functional Interfaces" section.

-----

## Summary 🛠️⚙️

Java provides a rich set of general-purpose functional interfaces in the `java.util.function` package, eliminating the need to create custom functional interfaces for many common use cases. These interfaces use generics (`<T>`, `<U>` for input types, `<R>` for return type) to provide flexibility.

1.  **Core Generic Functional Interfaces (from Table 8.4 - must memorize):**

    * **`Supplier<T>`**:
        * Method: `T get()`
        * Purpose: Supplies/produces a value of type `T` without taking any input. Used for generating values or constructing objects (e.g., `LocalDate::now`, `StringBuilder::new`).
    * **`Consumer<T>`**:
        * Method: `void accept(T t)`
        * Purpose: Consumes/accepts an input of type `T` and performs an operation without returning a result (e.g., printing: `System.out::println`).
    * **`BiConsumer<T, U>`**:
        * Method: `void accept(T t, U u)`
        * Purpose: Consumes two inputs (types `T` and `U`, which can be different) and performs an operation (e.g., `map::put`).
    * **`Predicate<T>`**:
        * Method: `boolean test(T t)`
        * Purpose: Evaluates a condition on an input of type `T` and returns a `boolean` primitive. Used for filtering/matching (e.g., `String::isEmpty`).
    * **`BiPredicate<T, U>`**:
        * Method: `boolean test(T t, U u)`
        * Purpose: Evaluates a condition on two inputs. Example: `String::startsWith`.
    * **`Function<T, R>`**:
        * Method: `R apply(T t)`
        * Purpose: Accepts an argument of type `T` and returns a result of type `R`. Used for transformations (e.g., `String::length` returns `Integer`). `T` and `R` can be the same or different.
    * **`BiFunction<T, U, R>`**:
        * Method: `R apply(T t, U u)`
        * Purpose: Accepts two arguments (types `T` and `U`) and returns a result of type `R`. Example: `String::concat`.
    * **`UnaryOperator<T>`**:
        * Extends: `Function<T, T>`
        * Method: `T apply(T t)` (inherited).
        * Purpose: A specialized `Function` where the input and output types are the same. Represents a unary operation (e.g., `String::toUpperCase`).
    * **`BinaryOperator<T>`**:
        * Extends: `BiFunction<T, T, T>`
        * Method: `T apply(T t1, T t2)` (inherited).
        * Purpose: A special `BiFunction` where both inputs and the output are of the same type. Represents a binary operation (e.g., `String::concat`, or `(a,b) -> a+b` for numbers).

2.  **Notes on Usage:**

    * Often, implementations (lambdas/method references) are passed directly to methods expecting these interfaces, without assigning to an intermediate variable.
    * The `toString()` representation of a lambda is implementation-specific and not typically human-readable.
    * Distinguish between returning `boolean` (for `Predicate`) and `Boolean` (for `Function<T, Boolean>`).

3.  **Convenience Default Methods (from Table 8.5):**

    * These methods allow for composing or modifying functional interfaces.
    * **`Predicate<T>`**: `and(Predicate other)`, `or(Predicate other)`, `negate()`.
    * **`Consumer<T>`**: `andThen(Consumer after)`.
    * **`Function<T, R>`**: `andThen(Function after)` (applies this function, then `after`), `compose(Function before)` (applies `before`, then this function).

4.  **Functional Interfaces for Primitives (`double`, `int`, `long`):**

    * **Purpose:** To avoid the overhead of autoboxing and unboxing when working with primitive types in functional contexts (like streams).
    * **`BooleanSupplier`**:
        * Method: `boolean getAsBoolean()`
    * **Common Primitive Functional Interfaces (from Table 8.6):**
        * Direct analogues to generic versions, with specialized names and method names (e.g., `IntSupplier` has `getAsInt()`, `IntConsumer` has `accept(int)`, `IntPredicate` has `test(int)`, `IntFunction<R>` has `apply(int)`, `IntUnaryOperator` has `applyAsInt(int)`, `IntBinaryOperator` has `applyAsInt(int, int)`).
        * Generics are often removed or reduced.
    * **Primitive-Specific Functional Interfaces (from Table 8.7):**
        * Object-to-Primitive Functions: `ToDoubleFunction<T>`, `ToIntFunction<T>`, `ToLongFunction<T>`.
        * TwoObjects-to-Primitive Functions: `ToDoubleBiFunction<T,U>`, `ToIntBiFunction<T,U>`, `ToLongBiFunction<T,U>`.
        * Primitive-to-Primitive Functions: e.g., `DoubleToIntFunction`, `IntToLongFunction`.
        * Object-and-Primitive Consumers: `ObjDoubleConsumer<T>`, `ObjIntConsumer<T>`, `ObjLongConsumer<T>`.

-----

## Conceptual Questions (for "Working with Built-in Functional Interfaces")

1.  What is the primary reason Java provides a set of built-in functional interfaces in the `java.util.function` package?
2.  For `Function<T, R>`, what do `T` and `R` represent in terms of the function's input and output? How does this differ from `UnaryOperator<T>`?
3.  What is the key difference in the abstract method signature (parameters and return type) between a `Predicate<T>` and a `Consumer<T>`?
4.  What is the purpose of `BiConsumer<T, U>`? How many arguments does its abstract method take, and what does it return?
5.  How does `BinaryOperator<T>` relate to `BiFunction<T, U, R>`? What specific constraint does `BinaryOperator<T>` place on the types `T`, `U`, and `R`?
6.  When would you generally prefer to use a primitive functional interface (e.g., `IntSupplier`) over its generic counterpart (e.g., `Supplier<Integer>`)?
7.  Explain the difference in the order of execution when combining two `Function` instances, `f1` and `f2`, using `f1.andThen(f2)` versus `f1.compose(f2)`.
8.  What is the name of the abstract method in `BooleanSupplier`, and what primitive type does it return?
9.  If you need a functional interface that takes an object of type `String` and returns its length as a primitive `int`, which specific primitive-specialized functional interface from `java.util.function` would be most suitable?
10. If you need a functional interface to perform an action with a `User` object and an `int` representing a score, without returning any value, which type of functional interface (possibly involving `Obj` and a primitive type) would be appropriate?

-----

## Scenario-Based Questions (for "Working with Built-in Functional Interfaces")

1.  You need a lambda expression that, when called, always returns a new `java.awt.Point` object initialized to (0,0). Which built-in functional interface is most fitting for this lambda?
2.  If you want to define a lambda that takes a `List<String>` and checks if it's empty, returning `true` if empty and `false` otherwise, which functional interface would you use?
3.  Consider a lambda `(Employee e, Double bonus) -> e.setSalary(e.getSalary() + bonus);`. Which `Bi`-prefixed functional interface (that returns `void`) would this lambda be compatible with?
4.  You have `Predicate<String> isEmpty = String::isEmpty;`. How would you create a new `Predicate<String>` called `isNotEmpty` using a convenience method on `isEmpty`?
5.  If `Function<Integer, String> intToStr = i -> "#" + i;` and `Function<String, String> strToLoudStr = s -> s.toUpperCase() + "!!!";`, what would be the result of `intToStr.andThen(strToLoudStr).apply(5)`?

-----

## Coding Problems (for "Working with Built-in Functional Interfaces")

1.  **Basic Operations with Standard Interfaces:**
    * Instantiate and demonstrate the use of the following functional interfaces using lambda expressions or method references:
        * A `Supplier<String>` that returns your name.
        * A `Consumer<Integer>` that prints whether an integer is even or odd.
        * A `Predicate<String>` that checks if a string has more than 5 characters.
        * A `Function<Integer, String>` that converts an integer to its hexadecimal representation.
        * A `UnaryOperator<Double>` that squares a double.
        * A `BinaryOperator<Integer>` that finds the maximum of two integers.
2.  **Combining Predicates for Filtering:**
    * Create a `List<Integer>` with values from -5 to 5.
    * Define two `Predicate<Integer>`: `isPositive` and `isEven`.
    * Using these predicates and their convenience methods (`and`, `or`, `negate`), filter the list using streams to print:
        * Positive even numbers.
        * Numbers that are negative OR odd.
3.  **Using Primitive Functional Interfaces:**
    * Define an `IntSupplier` that returns a random integer between 1 and 100.
    * Define a `LongConsumer` that prints a long value multiplied by 2.
    * Define a `DoublePredicate` that checks if a double is greater than 0 but less than 1.
    * Define a `ToIntFunction<String>` that parses a string to an int, or returns 0 if parsing fails (use a try-catch block within the lambda).
    * Demonstrate each with a sample call.

-----

# Section Review: Working with Variables in Lambdas

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Working with Variables in Lambdas" section.

-----

## Summary 📝

This section details how variables are handled in lambda expressions, covering lambda parameters, local variables defined within a lambda's body, and variables from the enclosing scope that are accessed by the lambda.

1.  **Lambda Parameter Lists:**

    * **Type Inference:** The types of lambda parameters are often inferred by the compiler from the context (i.e., the functional interface they are implementing).
    * **Syntax Options for Parameters:**
        1.  **No explicit types:** Parentheses `()` can be omitted if there's exactly one parameter with an inferred type (e.g., `x -> true`). Parentheses are required for zero or multiple parameters (e.g., `() -> true`, `(x, y) -> true`).
        2.  **Using `var`:** `var` can be used for parameter types (e.g., `(var x) -> true`, `(var x, var y) -> true`). Parentheses are required.
        3.  **Explicit types:** Parameter types can be explicitly declared (e.g., `(String x) -> true`). Parentheses are required.
    * **Consistency Rule:** Within a single lambda's parameter list, all parameters must follow the same format: either all use `var`, all use explicit types, or all omit types (if syntax allows). Mixing these styles (e.g., `(var x, String y) -> ...`) is not allowed and results in a compile error.
    * **Modifiers:** Lambda parameters can have modifiers like `final` or annotations (e.g., `(final var x, @Deprecated var y) -> ...`).

2.  **Local Variables Declared *Inside* a Lambda Body:**

    * If a lambda has a block body (enclosed in `{}`), it can declare its own local variables.
    * These variables are scoped strictly to the lambda's body.
    * **Shadowing Rule:** A local variable declared inside a lambda body **cannot have the same name** as a lambda parameter or any local variable/parameter in the enclosing scope that the lambda is part of. Attempting to do so results in a compile error (e.g., `(a, b) -> { int a = 0; return 5; }` is invalid if `a` is a lambda parameter).

3.  **Referencing Variables *from* the Lambda Body (Capturing Variables):**

    * Lambdas can access (or "capture") variables from their surrounding (enclosing) scope.
    * **Instance Variables (of the enclosing class):** Can always be accessed and modified by the lambda (if the lambda is part of an instance method or is an inner class's lambda and the variable is not final).
    * **Static Variables (of the enclosing class or any class):** Can always be accessed and modified (if not final).
    * **Local Variables (of the enclosing method/block):** Can be accessed by the lambda **only if they are `final` or effectively final.**
        * **Effectively final:** A local variable is effectively final if its value is not changed after its initial assignment. If a local variable is reassigned *anywhere* in the method after its initialization (and before the lambda might access it, or even in a way that makes its value at the point of lambda execution uncertain from a simple capture), it is not effectively final, and a lambda trying to use it will cause a compile error.
    * **Method Parameters (of the enclosing method):** Behave like local variables; they can be accessed by the lambda **only if they are `final` or effectively final.**
    * **Lambda Parameters:** Are always accessible within the lambda's own body.
    * **Compiler Error Location:** If a lambda attempts to use a local variable or method parameter that is not final or effectively final (because it's reassigned elsewhere), the compile error typically occurs at the point of *use within the lambda*.

-----

## Conceptual Questions (for "Working with Variables in Lambdas")

1.  What are the three different formats you can use for specifying parameters in a lambda expression? What is the consistency rule regarding these formats in a single lambda?
2.  Can you declare new local variables inside the body of a lambda expression? If so, what is an important rule regarding the names of these variables in relation to lambda parameters or variables from the enclosing scope?
3.  What does it mean for a local variable or a method parameter to be "effectively final"?
4.  Which types of variables from an enclosing scope can a lambda body always access without restriction (assuming standard access modifiers allow it from the context where the lambda is defined)?
5.  What is the restriction on a lambda body accessing local variables or method parameters from its enclosing scope? Why does this restriction exist?
6.  If a local variable is reassigned *after* the point where a lambda expression that references it is defined (but in the same method scope), where does the compiler typically report an error if that variable was used by the lambda?

-----

## Scenario-Based Questions (for "Working with Variables in Lambdas")

1.  If a lambda is `(x, y) -> x + y`, and it's assigned to a `BiFunction<Integer, Integer, Integer>`, what can you infer about the types of `x` and `y`? Would `(var x, int y) -> x + y` be a valid parameter list for this lambda according to the consistency rule?
2.  A method has a local variable `int count = 0;`. A lambda defined later in the method tries to print `count`. If, after the lambda definition but before the method ends (and before the lambda necessarily executes), `count` is incremented (`count++`), would the lambda's access to `count` compile? Why or why not?
3.  Consider an instance method of a class `MyDataProcessor` that has an instance field `private String processStatus = "idle";`. If a lambda is defined within this instance method, can it freely read and modify `this.processStatus` (assuming no other concurrency issues)?
4.  If a lambda parameter is named `element`, can you declare a local variable also named `element` inside the lambda's block body (e.g., `element -> { String element = "test_value"; ... }`)?

-----

## Coding Problems (for "Working with Variables in Lambdas")

1.  **String Formatter with Local Variable:**
    * Define a functional interface `AdvancedStringFormatter` with a method `String format(String prefix, String content, String suffix)`.
    * In a `main` method, create a lambda expression for this interface.
    * Inside the lambda body, declare a local variable `int totalLength = prefix.length() + content.length() + suffix.length();`.
    * The lambda should return a formatted string that includes all three parts and also appends `  " (Total Length: " + totalLength + ")" `.
    * Test your lambda.
2.  **Effectively Final Demonstration (Conceptual & Explanation):**
    * Write a method `public void processValue(int initialValue)`
    * Inside this method, declare a local variable `int multiplier = 2;`.
    * Create a `Function<Integer, String> valueProcessor` lambda that takes an `Integer` parameter `val`, and returns a `String` like: `"Input: <val>, Factor: <initialValue>, Offset: <multiplier>, Result: " + (val * initialValue + multiplier)`.
    * Call and print the result for `valueProcessor.apply(10)`.
    * Now, conceptually explain what would happen if you tried to add the line `multiplier = 3;` *after* the lambda definition but before calling `apply`. Then explain what would happen if you tried to add `initialValue++;` after the lambda definition.
3.  **Parameter Style Consistency Check (Conceptual):**
    * Given a functional interface `BiIntOperator` with a method `int operate(int a, int b)`.
    * For each of the following lambda assignments, state if it's valid or invalid according to the parameter style consistency rule, and why:
        * `BiIntOperator op1 = (var x, y) -> x + y;`
        * `BiIntOperator op2 = (int x, var y) -> x - y;`
        * `BiIntOperator op3 = (x, final int y) -> x * y;`
        * `BiIntOperator op4 = (var x, var y) -> x / y;`
        * `BiIntOperator op5 = (int x, int y) -> x % y;`

-----