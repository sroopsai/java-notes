# Section Review: Writing Simple Lambdas

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