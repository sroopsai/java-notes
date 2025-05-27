# Chapter Answers &amp; Solutions: Lambdas and Functional Interfaces

This document provides answers and solutions to the conceptual questions, scenario-based questions, and coding problems for the chapter "Lambdas and Functional Interfaces."

-----

## Section 1: Writing Simple Lambdas - Answers & Solutions

#### Answers to Conceptual Questions

1.  **What is a lambda expression, and what is its primary purpose in Java?**
    A lambda expression is an unnamed block of code (like an anonymous method) with parameters and a body. Its primary purpose is to provide a concise way to implement a method of a functional interface (an interface with a single abstract method), allowing code to be treated as data and passed around, often for deferred execution.

2.  **What is meant by "deferred execution" in the context of lambda expressions?**
    Deferred execution means that the code within a lambda expression is not executed when the lambda is defined or passed as an argument. Instead, it's executed at a later time, typically when the single abstract method of the functional interface (which the lambda implements) is actually called.

3.  **Explain how Java uses "context" to understand and type-check a lambda expression.**
    Java uses the target type, which must be a functional interface, to understand a lambda. The compiler looks at the signature of the single abstract method in that functional interface to:

      * Infer the types of the lambda's parameters if they are not explicitly stated.
      * Determine the expected return type of the lambda's body.
      * Ensure the lambda's structure (parameters and body) is compatible with that abstract method.

4.  **What are the conditions under which parentheses `()` around the parameter list of a lambda can be omitted?**
    Parentheses can be omitted if and only if there is exactly one parameter in the lambda expression, AND its type is being inferred by the compiler (not explicitly declared). For zero parameters or multiple parameters, or if the type of a single parameter is explicitly declared, parentheses are required.

5.  **When can the curly braces `{}` for the body of a lambda, along with the `return` keyword and semicolon, be omitted?**
    Curly braces, the `return` keyword, and the semicolon can be omitted if the lambda's body consists of only a single expression. The result of this expression will then be implicitly returned by the lambda (if the functional interface method has a non-void return type). If the body has multiple statements or an explicit `return` statement, curly braces are required.

6.  **Why does an assignment like `var myLambda = x -> x.length() > 0;` typically fail to compile?**
    This fails because both `var` and the lambda expression rely on context for type determination. `var` infers its type from the expression on the right-hand side. A lambda expression, however, infers its own types (parameter types, return type) from its target type (a functional interface) on the left-hand side. When both are present like this, neither has enough information to establish a concrete functional interface type, leading to a compilation error due to ambiguity.

#### Answers to Scenario-Based Questions

1.  **Valid lambdas for `Converter` (method `String format(Integer i)`):**

      * `num -> "Value: " + num`: **Valid.** `num` would be inferred as `Integer`. The expression returns a `String`.
      * `(Integer n) -> { return "Number: " + n.toString(); }`: **Valid.** Explicit type, block body with return.
      * `() -> "Default"`: **Invalid.** The interface method expects one `Integer` parameter.
      * `(int x, int y) -> String.valueOf(x + y)`: **Invalid.** The interface method expects one `Integer` parameter, not two `int` parameters.

2.  **Lambda for `process(Runnable r)` (method `void run()`):**
    Yes, you could use a lambda. Conceptually, it would look like:
    `() -> System.out.println("Processing...")`
    This lambda takes no arguments and returns `void`, matching `Runnable`'s `run()` method.

3.  **Inferring interface from `(String s1, String s2) -> s1.concat(s2)`:**
    The functional interface method it implements would:

      * Take **two parameters**.
      * The parameters would likely be of type **`String`** (or a supertype like `CharSequence`).
      * The return type would be **`String`** (since `concat` returns a `String`).
        This matches `BiFunction<String, String, String>` or a custom functional interface with a similar signature.

#### Solutions to Coding Problems

1.  **Simple String Operations:**

    ```java
    import java.util.function.Function; // Using a built-in one, or can define StringOperator

    @FunctionalInterface
    interface StringOperator {
        String apply(String s);
    }

    public class StringLambdaTest {
        public static void main(String[] args) {
            StringOperator toUpper = s -> s.toUpperCase();
            StringOperator addExclamations = s -> s + "!!!";

            System.out.println(toUpper.apply("hello")); // Output: HELLO
            System.out.println(addExclamations.apply("world")); // Output: world!!!

            // Using built-in Function<String, String> which is equivalent to UnaryOperator<String>
            Function<String, String> toUpperFunc = s -> s.toUpperCase();
            System.out.println(toUpperFunc.apply("functional")); // Output: FUNCTIONAL
        }
    }
    ```

2.  **Basic Calculator:**

    ```java
    @FunctionalInterface
    interface IntCalculator {
        int calculate(int a, int b);
    }

    public class CalculatorLambdaTest {
        public static void main(String[] args) {
            IntCalculator add = (a, b) -> a + b;
            IntCalculator subtract = (a, b) -> a - b;
            IntCalculator multiply = (a, b) -> a * b;
            // IntCalculator divide = (a, b) -> (b == 0) ? 0 : a / b; // Example for division

            System.out.println("5 + 3 = " + add.calculate(5, 3));       // Output: 8
            System.out.println("5 - 3 = " + subtract.calculate(5, 3));   // Output: 2
            System.out.println("5 * 3 = " + multiply.calculate(5, 3));   // Output: 15
        }
    }
    ```

3.  **List Filter Lambdas:**
    (Assuming `Animal` record and `CheckTrait` interface as defined in the text)

      * Animals that can swim: `animal -> animal.canSwim()`
      * Animals that *cannot* hop: `animal -> !animal.canHop()`
      * Animals whose species name is "fish": `animal -> "fish".equals(animal.species())`

-----

## Section 2: Coding Functional Interfaces - Answers & Solutions

#### Answers to Conceptual Questions

1.  **Defining characteristic of a functional interface:** It has **exactly one abstract method** (SAM - Single Abstract Method rule).
2.  **`@FunctionalInterface` annotation:**
      * **Purpose:** It indicates to the compiler that the interface is intended to be a functional interface.
      * **Mandatory?:** No, it's not mandatory. An interface with a single abstract method is functional regardless of the annotation. However, using it is good practice because the compiler will then generate an error if the interface doesn't meet the criteria (e.g., if more abstract methods are added accidentally).
3.  **Interface extension and functional status:**
      * If interface `B` extends functional interface `A` and `B` does not declare any *new* abstract methods, then `B` **is also a functional interface** (it inherits the single abstract method from `A`).
      * If `B` declares one new abstract method, then `B` is **not a functional interface** (it would have two abstract methods: its own and the one inherited from `A`).
4.  **`default`, `static`, `private` methods and SAM rule:** No, `default` methods, `static` methods, and `private` methods (which all have implementations or are not abstract in the traditional sense) **do not count** towards the single abstract method limit. An interface can have many of these and still be functional if it has only one true abstract method.
5.  **Abstract methods overriding `Object` methods:** If an interface declares an abstract method with the same signature as a `public` method in `java.lang.Object` (e.g., `toString()`, `equals(Object)`, `hashCode()`), these methods **do not count** towards the single abstract method requirement. This is because any class implementing the interface will already inherit these methods from `Object`.
6.  **`equals(MyType o)` vs. `Object.equals(Object o)`:** An interface declaring `abstract boolean equals(MyType o);` (where `MyType` is not `Object`) would have this method counted towards its abstract method tally because its signature is *different* from the `public boolean equals(Object o)` method in the `Object` class. The parameter type differs, making it a distinct abstract method, not an override of an `Object` method for the purpose of the SAM rule.

#### Answers to Scenario-Based Questions

1.  `interface Processor { void process(String data); }`.
      * Yes, this **is a functional interface** because it has exactly one abstract method (`process(String data)`).
      * If you add `@FunctionalInterface` above it, it **would compile** without errors.
2.  `interface Logger { default void log(String msg) { ... } String getFormat(); }`.
      * Yes, `Logger` **is a functional interface**. The `log(String msg)` method is a `default` method (has an implementation). The only abstract method is `String getFormat()`.
3.  If `interface I1 { void m1(); }` and `interface I2 extends I1 { void m2(); }`.
      * `I2` is **not a functional interface**. It inherits `m1()` from `I1` and declares its own abstract method `m2()`, so it has two abstract methods.
4.  `interface Validator { boolean isValid(String s); String toString(); }`.
      * Yes, `Validator` **could be a functional interface**. The `String toString();` method, if it has the exact same signature as `public String toString()` from `java.lang.Object`, does not count towards the abstract method limit. Therefore, `isValid(String s)` would be the single qualifying abstract method.
5.  `interface Configurable { static final int MAX_RETRIES = 3; static String getDefaultName() { return "default"; } }`.
      * No, `Configurable` is **not a functional interface**. It has zero abstract methods. Constants and static methods do not count.

#### Solutions to Coding Problems

1.  **Define `IntOperation`:**
    ```java
    @FunctionalInterface
    public interface IntOperation {
        int apply(int a, int b); // Renamed from calculate for consistency with common FIs
    }
    ```
2.  **Define `StringChecker`:**
    ```java
    @FunctionalInterface
    public interface StringChecker {
        boolean check(String s);

        default boolean isNotNullOrEmpty(String s) {
            return s != null && !s.isEmpty();
        }
    }
    ```
3.  **Identify Functional Interfaces (Conceptual Exercise):**
      * `interface A { void run(); void stop(); }`
          * **Not functional.** Contains two abstract methods (`run` and `stop`).
      * `interface B { void execute(); default void setup() {} static void cleanup() {} }`
          * **Functional.** Contains one abstract method (`execute`). Default and static methods don't count.
      * `interface C { boolean equals(Object obj); }`
          * **Not functional.** The method `equals(Object obj)` matches a public method in `java.lang.Object`, so it doesn't count towards the SAM rule. This interface has zero qualifying abstract methods.
      * `interface D extends B { void anotherAction(); }` (assuming `B` from above is functional with `execute()`)
          * **Not functional.** `D` inherits `execute()` from `B` and adds `anotherAction()`, resulting in two abstract methods.
      * `interface E { int compareTo(Object o); }` (Hint: `java.lang.Comparable` declares `int compareTo(T o)`)
          * **Functional.** `java.lang.Comparable<T>` (which `E` resembles if `T` is `Object`) is a functional interface. `compareTo(Object o)` is its single abstract method. It doesn't override a method from `Object` in a way that disqualifies it.

-----

## Section 3: Using Method References - Answers & Solutions

#### Answers to Conceptual Questions

1.  **Method Reference Purpose:** A method reference is a shorthand syntax for a lambda expression that executes only one existing method. Its primary purpose is to improve code readability and conciseness by directly referring to an existing method by name instead of providing an explicit lambda implementation for that call.
2.  **Operator:** The `::` (double colon) operator.
3.  **Deferred Execution:** It means the method referred to is not invoked immediately when the method reference is created. Instead, the reference is typically passed to another method or assigned to a functional interface variable, and the actual method invocation happens later when the functional interface's abstract method is called.
4.  **Four Types:**
    1.  Reference to a `static` method (`ClassName::staticMethodName`).
    2.  Reference to an instance method of a particular (existing) object (`instanceReference::instanceMethodName`).
    3.  Reference to an instance method of an arbitrary object of a particular type (on a parameter) (`ClassName::instanceMethodName`).
    4.  Reference to a constructor (`ClassName::new`).
5.  **Overload Resolution:** Java infers which specific overloaded method is being referred to from the **context**, specifically the signature (parameter types and return type) of the abstract method of the functional interface to which the method reference is being assigned or passed.
6.  **`myString::length` vs. `String::length`:**
      * `myString::length`: This is a reference to an instance method (`length()`) on a *particular, existing* object (`myString`). The functional interface for this would likely take no arguments (if `length()` takes no arguments) and return an `int`. E.g., `Supplier<Integer>` or a custom `interface StringLengthProvider { int get(); }`.
      * `String::length`: This is a reference to an instance method (`length()`) on an *arbitrary object of type `String`* that will be supplied as the first parameter to the functional interface's method. The functional interface would take a `String` as a parameter and return an `int`. E.g., `Function<String, Integer>` or a custom `interface StringLengthExtractor { int extract(String s); }`.
7.  **All Lambdas to Method References?:** No. A lambda can only be replaced by a method reference if the lambda's body consists of doing nothing more than calling a single existing method, and the parameters match up appropriately. If the lambda involves more complex logic, multiple method calls, or manipulation of parameters beyond directly passing them, it cannot be directly converted to a simple method reference.
      * Scenario: `(x, y) -> { int sum = x + y; System.out.println(sum); return sum > 10; }`. This has multiple operations and cannot be a single method reference.
8.  **Constructor Reference Resolution:** Java determines which constructor is being referred to by `ArrayList::new` based on the **context** provided by the functional interface's abstract method. If the functional interface method takes no arguments (e.g., `Supplier<ArrayList>`), it refers to the no-arg constructor. If it takes arguments (e.g., `Function<Integer, ArrayList>`), it refers to a constructor matching those argument types.

#### Answers to Scenario-Based Questions

1.  **`forEach` with `System.out.println`:** Yes, you could use a method reference. It would look like:
    `names.forEach(System.out::println);`
    This is a reference to an instance method (`println`) on a particular object (`System.out`). The `forEach` method expects a `Consumer<String>`, whose `accept(String s)` method matches the signature of `PrintStream.println(String s)`.
2.  **`Predicate<Person>` for `isAdult()`:**
      * Using `person.isAdult()` (instance method on a particular object): `Predicate<Person> p = ignoredPerson -> person.isAdult();` (The lambda argument is ignored, not ideal for method ref here directly as `isAdult` is on `person` instance). A direct method reference isn't straightforward if `Predicate` needs to take a `Person` and `person.isAdult()` takes no argument.
        A better fit for `person::isAdult` would be a `Supplier<Boolean>`: `Supplier<Boolean> s = person::isAdult;`.
      * Referring to `isAdult` on *any* `Person` passed to the predicate: `Predicate<Person> p = Person::isAdult;` (This is a reference to an instance method on an arbitrary object of a particular type).
3.  **`Supplier<StringBuilder>` with constructor reference:**
    `Supplier<StringBuilder> sbSupplier = StringBuilder::new;`
4.  **`MyFunction` (`int convert(String s)`) with `Integer.parseInt`:**
    `MyFunction mf = Integer::parseInt;` (Reference to a static method).

#### Solutions to Coding Problems

1.  **List Transformation:**
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.stream.Collectors;

    public class ListTransform {
        public static void main(String[] args) {
            List<String> stringNumbers = Arrays.asList("10", "25", "133");

            List<Integer> integers = stringNumbers.stream()
                                                .map(Integer::parseInt) // Method reference
                                                .collect(Collectors.toList());

            System.out.println(integers); // Output: [10, 25, 133]
        }
    }
    ```
2.  **Sorting Objects:**
    ```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Comparator;
    import java.util.List;

    class Person {
        String name;
        int age;

        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() { return name; }
        public int getAge() { return age; }

        @Override
        public String toString() {
            return name + " (" + age + ")";
        }
    }

    public class SortPerson {
        public static void main(String[] args) {
            List<Person> people = new ArrayList<>();
            people.add(new Person("Alice", 30));
            people.add(new Person("Bob", 25));
            people.add(new Person("Charlie", 35));
            people.add(new Person("Zoey", 25));

            // Sort by name using a method reference to an instance method
            // of an arbitrary object of a particular type (String::compareToIgnoreCase)
            // after extracting the name via a lambda.
            // A more direct method reference if Person had a method that returns a Comparator,
            // or if using Comparator.comparing directly with method references.
            System.out.println("Sorted by name:");
            people.sort(Comparator.comparing(Person::getName, String::compareToIgnoreCase));
            // Or Collections.sort(people, Comparator.comparing(Person::getName));
            people.forEach(System.out::println);

            // Sort by age using a method reference to a static method (Integer.compare)
            System.out.println("\nSorted by age:");
            people.sort(Comparator.comparingInt(Person::getAge));
            // Or Collections.sort(people, Comparator.comparingInt(Person::getAge));
            // Or using Integer.compare:
            // people.sort((p1, p2) -> Integer.compare(p1.getAge(), p2.getAge()));
            // which can be: people.sort(Comparator.comparing(Person::getAge, Integer::compare));
            people.forEach(System.out::println);
        }
    }
    ```
3.  **Object Creation:**
    ```java
    @FunctionalInterface
    interface ObjectFactory<T, P> {
        T create(P param);
    }

    class Message {
        private String content;

        public Message(String content) {
            this.content = content;
            System.out.println("Message created with content: " + content);
        }

        public String getContent() {
            return content;
        }
    }

    public class FactoryTest {
        public static void main(String[] args) {
            ObjectFactory<Message, String> messageFactory = Message::new; // Constructor reference

            Message myMessage = messageFactory.create("Hello, Method References!");
            System.out.println("Retrieved content: " + myMessage.getContent());
        }
    }
    ```

-----

## Section 4: Working with Built-in Functional Interfaces - Answers & Solutions

#### Answers to Conceptual Questions

1.  **Why Built-in FIs:** To provide a standard set of commonly needed functional interface shapes, promoting code reuse, interoperability (e.g., with the Stream API), and avoiding the need for developers to define many simple, repetitive functional interfaces.
2.  **`Function<T, R>` vs. `UnaryOperator<T>`:**
      * In `Function<T, R>`, `T` is the type of the input argument, and `R` is the type of the result. `T` and `R` can be different.
      * `UnaryOperator<T>` is a special case of `Function<T, R>` where `R` is the same as `T` (i.e., it's `Function<T, T>`). It takes an argument of type `T` and returns a result also of type `T`.
3.  **`Predicate<T>` vs. `Consumer<T>`:**
      * `Predicate<T>` has the method `boolean test(T t)`, which takes an argument of type `T` and returns a `boolean` primitive (for testing a condition).
      * `Consumer<T>` has the method `void accept(T t)`, which takes an argument of type `T` and returns `void` (for performing an action with the argument).
4.  **`BiConsumer<T, U>`:** Its purpose is to perform an operation that accepts two input arguments, one of type `T` and one of type `U`, without returning any result. Its abstract method `accept(T t, U u)` takes two arguments.
5.  **`BinaryOperator<T>` vs. `BiFunction<T, U, R>`:**
      * `BinaryOperator<T>` is a special case of `BiFunction<T, U, R>`.
      * `BinaryOperator<T>` extends `BiFunction<T, T, T>`. This means it specifically requires that both input arguments and the result must all be of the same type `T`.
6.  **Primitive vs. Generic FIs:** You'd choose a primitive functional interface (e.g., `IntSupplier`, `DoubleConsumer`) over its generic counterpart (`Supplier<Integer>`, `Consumer<Double>`) primarily to **avoid the performance overhead associated with autoboxing and unboxing** when dealing with primitive values. This is especially relevant in performance-sensitive code or when processing large numbers of primitives (e.g., in streams).
7.  **`function1.andThen(function2)` vs. `function1.compose(function2)`:**
      * `f1.andThen(f2)`: `f1` is applied first, and its result becomes the input to `f2`. Execution order: `f1` then `f2`. (e.g., `f2(f1(input))`)
      * `f1.compose(f2)`: `f2` (the argument function) is applied first, and its result becomes the input to `f1`. Execution order: `f2` then `f1`. (e.g., `f1(f2(input))`)
8.  **`BooleanSupplier`:** The method name is `getAsBoolean()`, and it returns a `boolean` primitive.
9.  **String to `int` length:**
      * Generic: `Function<String, Integer>` (e.g., `s -> s.length()`).
      * Primitive-specialized: `ToIntFunction<String>` (method `applyAsInt(String s)`), as it takes a `String` (an object `T`) and returns an `int`.
10. **Object and `int` to void:** `ObjIntConsumer<Person>` (method `accept(Person p, int i)`).

#### Answers to Scenario-Based Questions

1.  **Generate `UUID`:** `Supplier<UUID>`. Its abstract method is `get()`. Example: `UUID::randomUUID`.
2.  **Check if `Integer` is even:**
      * Generic: `Predicate<Integer>` (e.g., `num -> num % 2 == 0`).
      * Primitive-specialized: `IntPredicate` (e.g., `i -> i % 2 == 0`). The `IntPredicate` would be more efficient if dealing with many `int` values.
3.  **Lambda `(String s1, String s2) -> s1.length() + s2.length()`:**
      * If result is `Integer`: `BiFunction<String, String, Integer>`.
      * If result is `int` primitive: `ToIntBiFunction<String, String>`.
4.  **Combining `Predicate<String>`:**
    `Predicate<String> combinedPredicate = startsWithA.and(isLongerThan5);`
    Or for starts with "A" OR is longer than 5: `Predicate<String> combinedPredicate = startsWithA.or(isLongerThan5);`
5.  **`stringToInt.andThen(intToString).apply("123")`:**
    1.  `stringToInt.apply("123")` is called first. `Integer.parseInt("123")` executes, returning the `int` value `123`, which is then autoboxed to an `Integer` object.
    2.  The result (`Integer` object with value 123) is passed to `intToString.apply(...)`. `Object.toString()` is called on this `Integer` object, returning the `String` "123".
        The final result is the string "123".

#### Solutions to Coding Problems

1.  **Using `Supplier`, `Consumer`, `Predicate`, `Function`:**
    ```java
    import java.time.LocalDateTime;
    import java.util.function.Consumer;
    import java.util.function.Function;
    import java.util.function.Predicate;
    import java.util.function.Supplier;

    public class BasicFunctionalInterfaces {
        public static void main(String[] args) {
            // Supplier
            Supplier<LocalDateTime> currentTimeSupplier = LocalDateTime::now;
            LocalDateTime now = currentTimeSupplier.get();
            System.out.println("Current time: " + now);

            // Consumer
            Consumer<String> printUpperCase = s -> System.out.println(s.toUpperCase());
            printUpperCase.accept("hello consumer");

            // Predicate
            Predicate<Integer> isPositive = num -> num > 0;
            System.out.println("Is 10 positive? " + isPositive.test(10));
            System.out.println("Is -5 positive? " + isPositive.test(-5));

            // Function
            Function<String, Integer> stringLengthFunction = String::length;
            System.out.println("Length of 'java': " + stringLengthFunction.apply("java"));
        }
    }
    ```
2.  **Chaining Predicates:**
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.function.Predicate;
    import java.util.stream.Collectors;

    public class ChainingPredicates {
        public static void main(String[] args) {
            List<String> names = Arrays.asList("John", "Jane", "Jack", "Joe", "Sarah", "James", null, "Jeremy");

            Predicate<String> startsWithJ = s -> s != null && s.startsWith("J");
            Predicate<String> hasLength4 = s -> s != null && s.length() == 4;
            Predicate<String> isNotNull = s -> s != null; // Or use Objects::nonNull

            System.out.println("Names starting with J AND length 4:");
            names.stream()
                 .filter(isNotNull.and(startsWithJ).and(hasLength4)) // Ensure not null before other checks
                 .forEach(System.out::println); // Expected: John, Jack, Jane, Joe (if we consider isNotNull)
                                                // Correcting: Only John, Jack for startsWithJ.and(hasLength4)

            // Let's re-evaluate carefully with isNotNull
            Predicate<String> jAndLength4 = isNotNull.and(startsWithJ).and(hasLength4);
            names.stream().filter(jAndLength4).forEach(name -> System.out.print(name + " ")); // John Jack Jane Joe
            System.out.println("\n---");


            System.out.println("Names NOT starting with J OR have length 4 (and are not null):");
            // (NOT startsWithJ) OR hasLength4
            Predicate<String> notJOrLength4 = isNotNull.and(startsWithJ.negate().or(hasLength4));
            names.stream()
                 .filter(notJOrLength4)
                 .forEach(System.out::println);
        }
    }
    ```
3.  **Primitive Functional Interfaces:**
    ```java
    import java.util.function.IntBinaryOperator;
    import java.util.function.ObjIntConsumer;
    import java.util.function.ToDoubleFunction;

    public class PrimitiveFunctionalInterfacesDemo {
        public static void main(String[] args) {
            // IntBinaryOperator
            IntBinaryOperator sum = (a, b) -> a + b;
            System.out.println("Sum of 10 and 20: " + sum.applyAsInt(10, 20));

            // ToDoubleFunction<Integer>
            ToDoubleFunction<Integer> intToDoubleAndMultiply = i -> i.doubleValue() * 2.5;
            System.out.println("Integer 5 to double * 2.5: " + intToDoubleAndMultiply.applyAsDouble(5));

            // ObjIntConsumer<String>
            ObjIntConsumer<String> printStringAndInt =
                (str, val) -> System.out.println("String: " + str + ", Int: " + val);
            printStringAndInt.accept("Value is", 100);
        }
    }
    ```

-----

## Section 5: Working with Variables in Lambdas - Answers & Solutions

#### Answers to Conceptual Questions

1.  **Lambda Parameter Formats & Consistency:**

    1.  **No explicit types:** `x -> ...` (parentheses optional for single param like this). e.g., `(x,y) -> ...`
    2.  **Using `var`:** `(var x) -> ...` (parentheses required). e.g., `(var x, var y) -> ...`
    3.  **Explicit types:** `(String x) -> ...` (parentheses required). e.g., `(String x, int y) -> ...`
        The consistency rule is that all parameters in a single lambda's list must use the same format (either all `var`, all explicit types, or all without explicit types if syntax permits). You cannot mix them, e.g., `(var x, String y)` is invalid.

2.  **Local Variables Inside Lambda:** Yes, new local variables can be declared inside a lambda's block body `{}`. The rule is that these local variables cannot shadow (have the same name as) any lambda parameters or any local variables/parameters from the enclosing scope that are accessible to the lambda.

3.  **Effectively Final:** A local variable or method parameter is "effectively final" if its value is assigned only once and never changed after that initial assignment. Even if the `final` keyword is not explicitly used, the compiler treats it as such if it meets this condition.

4.  **Variables Always Accessible from Lambda:** Instance variables and static variables of the enclosing class (or any class, if static and accessible) can always be accessed by a lambda body without restriction (assuming standard access modifiers allow it from the context where the lambda is defined).

5.  **Restriction on Accessing Local/Method Parameters & Why:** A lambda body can only access local variables or method parameters from its enclosing scope if those variables are `final` or effectively `final`. This restriction exists because lambdas can be executed at a later time (deferred execution), possibly after the enclosing method has completed and its stack frame (containing non-final local variables) is gone. If the lambda captured a non-final variable, it might try to access a value that no longer exists or has changed unpredictably. Making them final/effectively final ensures the lambda captures a fixed value.

6.  **Compiler Error Location for Non-Effectively Final:** If a lambda uses a local variable or method parameter that is not effectively final (because it's reassigned after its initial assignment *and also used by the lambda*), the compiler typically reports the error at the point where the lambda **tries to use that variable**. It's the *usage* by the lambda of a variable that *could have changed* which is problematic.

#### Answers to Scenario-Based Questions

1.  **Lambda `(x, y) -> x + y` for `BiFunction<Integer, Integer, Integer>`:**

      * You can infer that `x` and `y` are treated as `Integer` (or `int` which get autoboxed to `Integer`).
      * `(var x, int y) -> x + y`: This would **not be a valid parameter list** due to mixing `var` with an explicit type (`int`). It should be `(var x, var y)` or `(int x, int y)` or `(Integer x, Integer y)`.

2.  **Lambda accessing `count`, `count` later incremented:**

      * The lambda's access to `count` would **not compile**. When `count++` occurs after the lambda definition (but within the same scope where the lambda could access `count`), `count` is no longer effectively final. The lambda attempts to capture a variable that is modified.

3.  **Lambda accessing instance field `this.value`:**

      * Yes, a lambda defined within an instance method can freely read and modify an instance field like `this.value` (assuming `value` itself is not `final` if modification is intended, and `this` is accessible). Instance fields are not subject to the "effectively final" restriction that applies to local variables/method parameters.

4.  **Lambda parameter `input` and local variable `input`:**

      * `input -> { String input = "test"; ... }`: This would **not compile**. You cannot declare a local variable inside the lambda body with the same name as one of its parameters. This is a form of illegal shadowing.

#### Solutions to Coding Problems

1.  **String Formatter:**

    ```java
    @FunctionalInterface
    interface AdvancedStringFormatter {
        String format(String prefix, String content, String suffix);
    }

    public class FormatterLambda {
        public static void main(String[] args) {
            AdvancedStringFormatter formatter = (p, c, s) -> {
                // Local variable inside lambda
                int totalLength = (p != null ? p.length() : 0) +
                                  (c != null ? c.length() : 0) +
                                  (s != null ? s.length() : 0);
                return (p != null ? p : "") +
                       (c != null ? c : "") +
                       (s != null ? s : "") +
                       " (Total Length: " + totalLength + ")";
            };

            String result = formatter.format("<<", "Lambda", ">>");
            System.out.println(result); // Output: <<Lambda>> (Total Length: 12)

            String result2 = formatter.format(null, "Test", null);
            System.out.println(result2); // Output: Test (Total Length: 4)
        }
    }
    ```

2.  **Effectively Final Demonstration:**

    ```java
    import java.util.function.Function;

    public class EffectivelyFinalDemo {

        public void process(int factor) { // factor is a method parameter
            int offset = 10; // offset is a local variable, initialized once

            // Lambda uses 'factor' and 'offset'
            Function<Integer, Integer> multiplier = input -> (input * factor) + offset;

            System.out.println("Initial call (5): " + multiplier.apply(5)); // (5 * factor) + 10

            // If you uncomment any of the following lines, 'factor' or 'offset'
            // would no longer be effectively final, and the lambda definition above
            // would cause a compile error where 'factor' or 'offset' is used.

            // factor = factor + 1; // Makes 'factor' not effectively final
            // offset = offset + 5; // Makes 'offset' not effectively final

            // System.out.println("Call after attempting modification (6): " + multiplier.apply(6));
            // The error would be on the line: input -> (input * factor) + offset;
            // stating "local variables referenced from a lambda expression must be final or effectively final"
            // or "variable factor/offset is accessed from within inner class, needs to be final or effectively final"
        }

        public static void main(String[] args) {
            EffectivelyFinalDemo demo = new EffectivelyFinalDemo();
            demo.process(3); // Output: Initial call (5): 25
        }
    }
    ```

    **Explanation of what would happen if `factor` or `offset` were reassigned:**
    If, for example, `offset = offset + 5;` was added after the lambda definition but before its use (or even just anywhere in the `process` method's scope that makes `offset` non-effectively final), the compiler would generate an error at the line `Function<Integer, Integer> multiplier = input -> (input * factor) + offset;` where `offset` is captured. The error message would indicate that local variables referenced from a lambda expression must be final or effectively final. This is because the lambda captures the *value* of these variables. If they could change after capture but before the lambda executes, it could lead to unpredictable behavior, especially with deferred execution or concurrency.

3.  **Parameter Style Consistency:**
    The lambda parameter list `(var x, String y)` would not compile because it mixes `var` with an explicit type (`String`).

    **Two ways to correct it (assuming `x` should be an `Integer` and `y` a `String` for a lambda like `(x, y) -> x + y.length()` which would target `BiFunction<Integer, String, Integer>`):**

    1.  **Using explicit types for all:**
        `(Integer x, String y) -> x + y.length()`
        *Or, if context allows type inference for the functional interface's method, you could use primitive `int` if appropriate for `x` and it gets autoboxed.*
        `(int x, String y) -> x + y.length()`

    2.  **Using `var` for all:**
        `(var x, var y) -> ((Integer)x) + y.length()`
        *Note: With `(var x, var y)`, `x` and `y` would be inferred based on the target functional interface. If the target is `BiFunction<Integer, String, Integer>`, `x` becomes `Integer` and `y` becomes `String`. The cast `((Integer)x)` would only be needed if `x` was inferred as a different number type that doesn't directly support `+` with `y.length()` in the way intended, or for clarity.* More simply, if the target is `BiFunction<Integer, String, Integer>`, then `(var x, var y) -> x + y.length()` would have `x` inferred as `Integer` and `y` as `String`.

    A simpler consistent form without explicit types (if the target type makes it clear):
    `(x, y) -> x + y.length()`
    *Here, if assigned to `BiFunction<Integer, String, Integer>`, `x` would be inferred as `Integer` and `y` as `String`.*

-----