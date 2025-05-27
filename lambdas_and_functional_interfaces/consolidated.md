# Chapter Study Guide: Lambdas and Functional Interfaces

This document provides summaries, conceptual questions, scenario-based questions, and coding problems for the chapter "Lambdas and Functional Interfaces" to aid your review. Answers and solutions are provided in a separate document.

-----

## Section 1: Writing Simple Lambdas

### Summary 🐑➡️

This section introduces lambda expressions (or "lambdas") as a core feature of functional programming in Java. It explains their purpose, benefits over traditional approaches, and the basic syntax.

1.  **What are Lambda Expressions?**
    * An unnamed block of code (like an anonymous method) that can be passed around and executed later.
    * Used to implement methods of **functional interfaces** (interfaces with exactly one abstract method).
2.  **Benefits Illustrated (Traditional vs. Lambda):**
    * Lambdas allow defining specific behavior inline, directly where needed, reducing boilerplate code.
    * **Deferred Execution:** Code specified now but executed later.
3.  **Lambda Syntax:**
    * Basic structure: `parameters -> body`.
    * **Parameters:**
        * Parentheses optional for a single inferred-type parameter (e.g., `a -> ...`). Required for zero/multiple parameters or if type is specified.
        * Types can be explicit (e.g., `(Animal a) -> ...`) or inferred.
    * **Arrow Operator `->`:** Separates parameters from the body.
    * **Body:**
        * **Single Expression:** Curly braces `{}` optional; result of expression is implicitly returned. No `return` keyword or semicolon.
        * **Block Body:** Multiple statements must be in `{}`; explicit `return` and semicolons needed if returning a value.
4.  **Context and Type Inference:**
    * Java infers lambda types from the target functional interface.
5.  **`var` and Lambdas:**
    * Cannot directly assign a lambda to `var` if the functional interface type isn't clear from context, as both rely on context for type inference.

### Conceptual Questions (for "Writing Simple Lambdas")

1.  In your own words, what is a lambda expression, and what is its primary purpose in Java?
2.  What is meant by "deferred execution" in the context of lambda expressions?
3.  Explain how Java uses "context" to understand and type-check a lambda expression.
4.  What are the conditions under which parentheses `()` around the parameter list of a lambda can be omitted?
5.  When can the curly braces `{}` for the body of a lambda, along with the `return` keyword and semicolon, be omitted?
6.  Why does an assignment like `var myLambda = x -> x.length() > 0;` typically fail to compile?

### Scenario-Based Questions (for "Writing Simple Lambdas")

1.  If a functional interface `Converter` has a method `String format(Integer i)`, which of these lambda syntaxes would be valid to assign to a `Converter` variable?
    * `num -> "Value: " + num`
    * `(Integer n) -> { return "Number: " + n.toString(); }`
    * `() -> "Default"`
    * `(int x, int y) -> String.valueOf(x + y)`
2.  Imagine you have a method `process(Runnable r)` where `Runnable` is a functional interface with a single method `void run()`. If you want to pass a behavior that simply prints "Processing...", could you use a lambda? Conceptually, what would it look like?
3.  If a lambda is `(String s1, String s2) -> s1.concat(s2)`, what can you infer about the functional interface method it is intended to implement (in terms of number of parameters, their likely types, and return type)?

### Coding Problems (for "Writing Simple Lambdas")

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

## Section 2: Coding Functional Interfaces

### Summary 🎯

This section defines functional interfaces, explains the `@FunctionalInterface` annotation, and details the rules for determining if an interface qualifies as functional, including how methods inherited from `java.lang.Object` are treated.

1.  **Definition of a Functional Interface:**
    * An interface that contains **exactly one abstract method** (SAM rule).
2.  **The `@FunctionalInterface` Annotation:**
    * Informs the compiler of the intent.
    * Compiler generates an error if an annotated interface doesn't meet functional interface criteria.
    * Optional but good practice.
3.  **Identifying Functional Interfaces:**
    * **Interface Extension:** Extending a functional interface without adding new abstract methods results in a functional interface. Adding new abstract methods makes it non-functional.
    * **`default`, `static`, `private` methods:** Do not count towards the single abstract method limit.
4.  **Abstract Methods Overriding `java.lang.Object` Methods:**
    * If an interface declares an abstract method with the same signature as a `public` method in `java.lang.Object` (e.g., `toString()`, `equals(Object)`, `hashCode()`), these **do not count** towards the SAM limit.
    * An interface cannot declare a method incompatible with an `Object` method (e.g., `int toString()`).
    * An abstract method like `equals(MyType o)` (not `equals(Object o)`) *does* count as an abstract method.

### Conceptual Questions (for "Coding Functional Interfaces")

1.  What is the defining characteristic of a functional interface in Java (often referred to by an acronym)?
2.  What is the purpose of the `@FunctionalInterface` annotation? Is it mandatory for an interface to be functional?
3.  If an interface `B` extends a functional interface `A` but does not declare any new methods, is interface `B` also a functional interface? What if `B` declares one new abstract method?
4.  Do `default` methods, `static` methods, or `private` methods within an interface count towards the single abstract method limit when determining if an interface is functional?
5.  Explain the special rule regarding abstract methods in an interface that have the same signature as `public` methods in the `java.lang.Object` class (like `toString()` or `equals(Object)`). How do these affect an interface's status as functional?
6.  Why would an interface declaring `abstract boolean equals(MyType o);` (where `MyType` is not `Object`) potentially have this `equals` method counted towards its abstract method tally, even though `Object` has an `equals` method?

### Scenario-Based Questions (for "Coding Functional Interfaces")

1.  Consider an interface `Processor { void process(String data); }`. Is this a functional interface? If you add `@FunctionalInterface` above it, would it compile?
2.  Imagine an interface `Logger { default void log(String msg) { System.out.println(msg); } String getFormat(); }`. Is `Logger` a functional interface? Why or why not?
3.  If interface `I1` has `void m1();` and interface `I2 extends I1` has `void m2();`, is `I2` a functional interface?
4.  An interface `Validator` declares `boolean isValid(String s);` and also `String toString();`. Could this `Validator` interface be a functional interface? Explain.
5.  If an interface `Configurable` only contains `static final int MAX_RETRIES = 3;` and `static String getDefaultName() { return "default"; }`, is it a functional interface?

### Coding Problems (for "Coding Functional Interfaces")

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

## Section 3: Using Method References

### Summary 🎯::

This section introduces method references as a shorthand syntax for certain lambda expressions, referring to existing methods by name to improve code readability for deferred execution via functional interfaces.

1.  **What are Method References?**
    * Compact way to write a lambda that only calls an existing method. Uses the `::` operator.
2.  **Equivalence to Lambdas:** Behave the same at runtime.
3.  **Four Types of Method References:**
    * **1. Reference to a `static` Method:**
        * Syntax: `ClassName::staticMethodName`
        * Lambda: `(params) -> ClassName.staticMethodName(params)`
        * Overload resolution based on functional interface context.
    * **2. Reference to an Instance Method of a Particular (Existing) Object:**
        * Syntax: `instanceReference::instanceMethodName`
        * Lambda: `(params) -> instanceReference.instanceMethodName(params)`
    * **3. Reference to an Instance Method of an Arbitrary Object of a Particular Type (via a Parameter):**
        * Syntax: `ClassName::instanceMethodName`
        * Lambda (1 param FI): `(obj) -> obj.instanceMethodName()`
        * Lambda (multi-param FI): `(obj, p1, ...) -> obj.instanceMethodName(p1, ...)`
    * **4. Reference to a Constructor:**
        * Syntax: `ClassName::new`
        * Lambda (no-arg): `() -> new ClassName()`
        * Lambda (with args): `(params) -> new ClassName(params)`
4.  **Context is Key:** The functional interface's abstract method signature determines which specific method or constructor is referenced, especially with overloading.

### Conceptual Questions (for "Using Method References")

1.  What is a method reference in Java, and what is its primary purpose compared to a full lambda expression?
2.  What is the special operator used to denote a method reference?
3.  Method references, like lambdas, are used for "deferred execution." What does this mean?
4.  List the four main types/formats of method references.
5.  When using a method reference for an overloaded method (e.g., `Math::round`), how does Java determine which specific version of the overloaded method is being referred to?
6.  Explain the difference between a method reference like `myString::length` (instance method on a particular object) and `String::length` (instance method on an arbitrary object of a particular type). What would the functional interface signatures look like for each to be valid?
7.  Can all lambda expressions be converted into method references? If not, describe a scenario where a lambda cannot be easily replaced by a method reference.
8.  How does Java determine which constructor is being referred to when you use a constructor reference like `ArrayList::new` if `ArrayList` has multiple constructors?

### Scenario-Based Questions (for "Using Method References")

1.  If you have a `List<String> names` and you want to print each name using `forEach`, could you use a method reference for `System.out.println`? If so, what would it look like?
2.  Consider a functional interface `Predicate<T>` with a method `boolean test(T t)`. If you have a class `Person` with an instance method `boolean isAdult()`, and an instance `Person person = new Person();`:
    * How would you write a method reference that could be assigned to a `Supplier<Boolean>` using `person.isAdult()`?
    * How would you write a method reference that could be assigned to `Predicate<Person>` which refers to the `isAdult` method to be called on *any* `Person` object passed to the predicate?
3.  Suppose you have a functional interface `Supplier<T>` with a method `T get()`. If you want to create new `StringBuilder` objects using this, what would the constructor reference look like?
4.  If a functional interface `MyFunction` has the method `int convert(String s)`, and the `Integer` class has a static method `parseInt(String s)`, what would be the method reference to implement `MyFunction` using `Integer.parseInt`?

### Coding Problems (for "Using Method References")

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

## Section 4: Working with Built-in Functional Interfaces

### Summary 🛠️⚙️

Java provides many general-purpose functional interfaces in `java.util.function` to avoid custom ones for common patterns. Generics (`<T>`, `<U>`, `<R>`) specify data types.

1.  **Core Generic Functional Interfaces (Table 8.4):**
    * **`Supplier<T>`**: `T get()` - Produces a `T`.
    * **`Consumer<T>`**: `void accept(T t)` - Consumes a `T`.
    * **`BiConsumer<T, U>`**: `void accept(T t, U u)` - Consumes `T` and `U`.
    * **`Predicate<T>`**: `boolean test(T t)` - Tests a `T`, returns `boolean`.
    * **`BiPredicate<T, U>`**: `boolean test(T t, U u)` - Tests `T` and `U`, returns `boolean`.
    * **`Function<T, R>`**: `R apply(T t)` - Converts `T` to `R`.
    * **`BiFunction<T, U, R>`**: `R apply(T t, U u)` - Converts `T` and `U` to `R`.
    * **`UnaryOperator<T>`** (extends `Function<T, T>`): `T apply(T t)` - `T` to `T` transformation.
    * **`BinaryOperator<T>`** (extends `BiFunction<T, T, T>`): `T apply(T t1, T t2)` - Two `T`s to one `T`.
2.  **Convenience Default Methods (Table 8.5):**
    * **`Predicate`**: `and()`, `or()`, `negate()`.
    * **`Consumer`**: `andThen()`.
    * **`Function`**: `andThen()` (applies this function, then the argument function), `compose()` (applies argument function, then this function).
3.  **Functional Interfaces for Primitives (`double`, `int`, `long`):**
    * **Purpose:** Avoid boxing/unboxing overhead.
    * **`BooleanSupplier`**: `boolean getAsBoolean()`.
    * **Common Primitive Interfaces (Table 8.6):** Analogues like `IntSupplier` (`getAsInt()`), `IntConsumer` (`accept(int)`), `IntPredicate` (`test(int)`), `IntFunction<R>` (`apply(int)` returns `R`), `IntUnaryOperator` (`applyAsInt(int)` returns `int`), `IntBinaryOperator` (`applyAsInt(int, int)` returns `int`). Method names often specialized.
    * **Primitive-Specific Interfaces (Table 8.7):**
        * `ToDouble/Int/LongFunction<T>`: `T` to primitive.
        * `ToDouble/Int/LongBiFunction<T, U>`: `T,U` to primitive.
        * Primitive-to-primitive: `DoubleToIntFunction`, etc.
        * `ObjDouble/Int/LongConsumer<T>`: `T` and a primitive, returns void.

### Conceptual Questions (for "Working with Built-in Functional Interfaces")

1.  What is the primary reason Java provides a set of built-in functional interfaces in the `java.util.function` package?
2.  For `Function<T, R>`, what do `T` and `R` represent? How does this differ from `UnaryOperator<T>`?
3.  What is the key difference in the abstract method signature between a `Predicate<T>` and a `Consumer<T>`?
4.  What is the purpose of `BiConsumer<T, U>`? How many arguments does its abstract method take?
5.  How does `BinaryOperator<T>` relate to `BiFunction<T, U, R>`? What constraint does `BinaryOperator` place on the types?
6.  When would you choose to use a primitive functional interface (e.g., `IntSupplier`) over its generic counterpart (e.g., `Supplier<Integer>`)?
7.  Explain the difference in execution order between `function1.andThen(function2)` and `function1.compose(function2)`.
8.  What is the method name in `BooleanSupplier`, and what type does it return?
9.  What kind of functional interface would you use if you need to take an object of type `String` and return its `int` length? (Name the specific primitive-specialized interface if applicable).
10. What kind of functional interface would you use if you need to take an object (e.g., a `Person`) and an `int` (e.g., an age adjustment) and perform an action without returning anything?

### Scenario-Based Questions (for "Working with Built-in Functional Interfaces")

1.  If you need a lambda expression that generates a new `UUID` object each time it's called (without any input), which built-in functional interface would be most appropriate? What is its abstract method called?
2.  You want to write a lambda to check if a given `Integer` is even. Which functional interface fits best? (Consider both generic and primitive-specialized options).
3.  If you have a lambda `(String s1, String s2) -> s1.length() + s2.length()`, which `Bi`-prefixed functional interface would be suitable for this lambda, assuming the result should be an `Integer`? What if the result should be an `int` primitive?
4.  You have two `Predicate<String>` objects: `startsWithA` and `isLongerThan5`. How would you combine them using convenience methods to create a new predicate that checks if a string starts with "A" AND is longer than 5 characters?
5.  Consider a `Function<String, Integer> stringToInt = Integer::parseInt;` and a `Function<Integer, String> intToString = Object::toString;`. If you use `stringToInt.andThen(intToString).apply("123")`, what is the sequence of operations?

### Coding Problems (for "Working with Built-in Functional Interfaces")

1.  **Using `Supplier`, `Consumer`, `Predicate`, `Function`:**
    * Create a `Supplier<LocalDateTime>` that returns the current date and time.
    * Create a `Consumer<String>` that prints a string to the console in uppercase.
    * Create a `Predicate<Integer>` that tests if a number is positive.
    * Create a `Function<String, Integer>` that returns the length of a given string.
    * In a `main` method, demonstrate the use of each of these functional interfaces with appropriate lambda expressions or method references.
2.  **Chaining Predicates:**
    * Create a `List<String>` with various names.
    * Write three `Predicate<String>` lambdas:
        * `startsWithJ`: checks if a string starts with "J".
        * `hasLength4`: checks if a string has a length of 4.
        * `isNotNull`: checks if a string is not null.
    * Use `Stream` API's `filter()` method and the `and()` / `or()` / `negate()` convenience methods on your predicates to filter the list and print:
        * Names that start with "J" AND have a length of 4.
        * Names that do NOT start with "J" OR have a length of 4.
3.  **Primitive Functional Interfaces:**
    * Define an `IntBinaryOperator` that calculates the sum of two integers.
    * Define a `ToDoubleFunction<Integer>` that converts an `Integer` to a `double` and then multiplies it by 2.5.
    * Define an `ObjIntConsumer<String>` that takes a `String` and an `int`, and prints them in the format: "String: [string\_value], Int: [int\_value]".
    * Demonstrate their usage.

-----

## Section 5: Working with Variables in Lambdas

### Summary 📝

This section details how variables are handled in lambda expressions, covering lambda parameters, local variables defined within a lambda's body, and variables from the enclosing scope that are accessed by the lambda.

1.  **Lambda Parameter Lists:**
    * **Type Inference:** Types often inferred from context (functional interface).
    * **Syntax Options:**
        1.  No explicit types: `x -> true` (parentheses optional for single param like this). e.g., `(x,y) -> ...`
        2.  Using `var`: `(var x) -> true` (parentheses required). e.g., `(var x, var y) -> ...`
        3.  Explicit types: `(String x) -> true` (parentheses required). e.g., `(String x, int y) -> ...`
    * **Consistency Rule:** All parameters in a list must use the same format (all `var`, all explicit types, or all no types).
    * **Modifiers:** Can have `final` or annotations.
2.  **Local Variables Declared *Inside* a Lambda Body:**
    * Block bodies `{}` can declare local variables, scoped to the lambda.
    * **Shadowing Rule:** Cannot redeclare a variable with the same name as a lambda parameter or an accessible local variable/parameter from the enclosing scope.
3.  **Referencing Variables *from* the Lambda Body (Capturing Variables):**
    * **Instance/Static Variables:** Always accessible (and modifiable if not final).
    * **Local Variables (from enclosing method/scope) & Method Parameters (of enclosing method):** Accessible only if they are `final` or **effectively final** (value not changed after initial assignment).
    * **Lambda Parameters:** Always accessible within the lambda.
    * **Compiler Error Location:** If a non-effectively final local/method parameter is used, error is at the lambda's usage point.

### Conceptual Questions (for "Working with Variables in Lambdas")

1.  What are the three different formats you can use for specifying parameters in a lambda expression? What is the consistency rule regarding these formats in a single lambda?
2.  Can you declare new local variables inside the body of a lambda expression? If so, what is an important rule regarding the names of these variables in relation to lambda parameters or variables from the enclosing scope?
3.  What does it mean for a local variable or a method parameter to be "effectively final"?
4.  Which types of variables from an enclosing scope can a lambda body always access without restriction (assuming standard access modifiers allow it from the context where the lambda is defined)?
5.  What is the restriction on a lambda body accessing local variables or method parameters from its enclosing scope? Why does this restriction exist?
6.  If a local variable is reassigned *after* the point where a lambda expression that references it is defined (but in the same method scope), where does the compiler typically report an error if that variable was used by the lambda?

### Scenario-Based Questions (for "Working with Variables in Lambdas")

1.  If a lambda is `(x, y) -> x + y`, and it's assigned to a `BiFunction<Integer, Integer, Integer>`, what can you infer about the types of `x` and `y`? Would `(var x, int y) -> x + y` be a valid parameter list for this lambda?
2.  A method has a local variable `int count = 0;`. A lambda defined later in the method tries to print `count`. If, after the lambda definition but before the method ends, `count` is incremented (`count++`), would the lambda's access to `count` compile?
3.  Consider an instance method of a class `MyClass` that has an instance field `this.value`. If a lambda is defined within this instance method, can it freely read and modify `this.value` (assuming `value` is not final)?
4.  If a lambda parameter is named `input`, can you declare a local variable also named `input` inside the lambda's block body (e.g., `input -> { String input = "test"; ... }`)?

### Coding Problems (for "Working with Variables in Lambdas")

1.  **String Formatter:**
    * Define a functional interface `AdvancedStringFormatter` with a method `String format(String prefix, String content, String suffix)`.
    * In a `main` method, create a lambda expression for this interface.
    * Inside the lambda body, declare a local variable `int totalLength = prefix.length() + content.length() + suffix.length();`.
    * The lambda should return a formatted string that includes all three parts and also appends `  " (Total Length: " + totalLength + ")" `.
    * Test your lambda.
2.  **Effectively Final Demonstration:**
    * Write a method that takes an `int factor`.
    * Inside this method, define another `int offset = 10;`.
    * Create a `Function<Integer, Integer> multiplier` using a lambda that multiplies its input by `factor` and then adds `offset`.
    * Call this `multiplier` and print the result.
    * Describe what would happen (compiler error and why) if you tried to reassign `factor` or `offset` *after* the lambda definition but *before* it's called (or even after it's called, within the same method scope where the lambda could still access them).
3.  **Parameter Style Consistency:**
    * Conceptually explain why `(var x, String y) -> x + y.length()` would not compile as a lambda parameter list, and show two ways to correct it (assuming `x` should be an `Integer` and `y` a `String`).

-----