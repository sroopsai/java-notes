# Section Review: Using the Queue and Deque Interfaces

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using the Queue and Deque Interfaces" section.

-----

## Summary  FIFO LIFO

This section explores the `java.util.Queue` and `java.util.Deque` interfaces, their common implementations (`LinkedList` and `ArrayDeque`), and their methods for adding, removing, and inspecting elements, including their use as FIFO queues and LIFO stacks.

1.  **`Queue<E>` Interface:**

    * **Concept:** A collection designed for holding elements prior to processing, typically ordering elements in a FIFO (First-In, First-Out) manner.
    * **Analogy:** A line of people waiting.
    * **Core Methods (Table 9.3):** Each operation typically has two forms: one that throws an exception on failure and one that returns a special value (like `false` or `null`).
        * **Adding to back (tail):**
            * `boolean add(E e)`: Throws `IllegalStateException` if no space is available (for capacity-restricted queues).
            * `boolean offer(E e)`: Returns `false` if the element cannot be added (e.g., queue is full).
        * **Retrieving from front (head) without removing:**
            * `E element()`: Throws `NoSuchElementException` if the queue is empty.
            * `E peek()`: Returns `null` if the queue is empty.
        * **Retrieving and removing from front (head):**
            * `E remove()`: Throws `NoSuchElementException` if the queue is empty.
            * `E poll()`: Returns `null` if the queue is empty.

2.  **`Deque<E>` Interface (Double-Ended Queue):**

    * **Concept:** Extends `Queue`. Allows element insertion and removal at **both ends** (front/head and back/tail). Pronounced "deck."
    * **Common Implementations:**
        * **`LinkedList<E>`:** Implements both `List` and `Deque`. Versatile but might be less efficient for pure queue/deque operations compared to `ArrayDeque`.
        * **`ArrayDeque<E>`:** A resizable array implementation of `Deque`. Generally more efficient than `LinkedList` for queue and stack operations if `List` functionalities are not needed.
    * **`Deque` Methods (Table 9.4 - extending `Queue` methods for both ends):**
        * **Add to Front (Head):** `void addFirst(E e)` (throws exception on failure), `boolean offerFirst(E e)` (returns special value).
        * **Add to Back (Tail):** `void addLast(E e)` (throws exception), `boolean offerLast(E e)` (returns special value). *Note: `addLast` is equivalent to `Queue.add`, and `offerLast` to `Queue.offer`.*
        * **Read from Front (Head):** `E getFirst()` (throws exception), `E peekFirst()` (returns special value). *Note: `getFirst` is equivalent to `Queue.element`, and `peekFirst` to `Queue.peek`.*
        * **Read from Back (Tail):** `E getLast()` (throws exception), `E peekLast()` (returns special value).
        * **Get and Remove from Front (Head):** `E removeFirst()` (throws exception), `E pollFirst()` (returns special value). *Note: `removeFirst` is equivalent to `Queue.remove`, and `pollFirst` to `Queue.poll`.*
        * **Get and Remove from Back (Tail):** `E removeLast()` (throws exception), `E pollLast()` (returns special value).
    * **`Deque` as a Stack (LIFO - Last-In, First-Out) (Table 9.5):**
        * `Deque` can be used to implement stack behavior.
        * **Add to Top (Front):** `void push(E e)` (equivalent to `addFirst`).
        * **Remove from Top (Front):** `E pop()` (equivalent to `removeFirst`, throws exception if empty).
        * **Peek at Top (Front):** `E peek()` (equivalent to `peekFirst`, returns `null` if empty).

Understanding whether a `Deque` is being used as a FIFO queue, LIFO stack, or a general double-ended queue is key to interpreting its method calls.

-----

## Conceptual Questions

1.  What is the primary ordering principle for a standard `Queue` (e.g., when using `add`/`offer` and `remove`/`poll`)? What does FIFO stand for?
2.  The `Queue` interface methods for adding, retrieving (without removing), and removing elements each come in two forms. What is the main difference in behavior between these two forms (e.g., `add` vs. `offer`, `element` vs. `peek`, `remove` vs. `poll`)?
3.  What core capability does the `Deque` interface add to the `Queue` interface?
4.  Name the two common implementations of the `Deque` interface mentioned in the text. What is a key difference or trade-off between them?
5.  If you want to use a `Deque` as a LIFO stack, which methods would you typically use for push, pop, and peek operations? To which `Deque` methods (like `addFirst`, `removeFirst`, etc.) do these stack methods correspond?
6.  When would you choose `ArrayDeque` over `LinkedList` if you need a `Deque`?

-----

## Scenario-Based Questions

1.  You have a `Queue<String> taskQueue = new LinkedList<>();`. If the queue is empty and you call `taskQueue.element()`, what happens? What if you call `taskQueue.peek()` instead?
2.  If `Deque<Integer> deque = new ArrayDeque<>(); deque.offerFirst(10); deque.offerLast(20); deque.offerFirst(5);`, what element would `deque.pollFirst()` return? What would `deque.pollLast()` return after that?
3.  You're implementing a "undo" feature using a LIFO behavior. Which `Deque` methods would be most idiomatic for adding an action to the undo history and retrieving the last action to undo?
4.  If `Queue<String> q = new LinkedList<>(); q.add("A"); q.add("B");`, what is the state of `q` after `q.poll()` and then `q.offer("C")`?
5.  A `Deque<String> d = new ArrayDeque<>();` is used as a stack. After `d.push("X"); d.push("Y"); d.push("Z");`, what will `d.peek()` return? What will `d.pop()` return? What will `d.peek()` return after the `pop()`?

-----

## Coding Problems

1.  **Ticket Counter Simulation:**
    * Create a `Queue<String>` using `LinkedList` to represent people waiting in line for tickets.
    * Simulate adding 5 people to the queue using `offer()`.
    * Simulate serving 3 people from the queue using `poll()`, printing the name of each person served.
    * Print the remaining people in the queue.
    * Use `peek()` to see who is next without removing them.
2.  **Browser History (Stack Behavior):**
    * Create a `Deque<String>` using `ArrayDeque` to represent a simplified browser history (LIFO).
    * Simulate visiting several URLs by `push`ing them onto the deque.
    * Implement a "back button" functionality: `pop` the last visited URL and print it. Repeat this a couple of times.
    * Use `peek()` to show the current page without going back.
3.  **Palindrome Checker using Deque:**
    * Write a method `boolean isPalindrome(String text)` that uses a `Deque<Character>` to check if the given string is a palindrome (reads the same forwards and backwards, ignoring case and non-alphanumeric characters).
    * Add all relevant characters from the input string to one end of the deque.
    * Then, repeatedly remove characters from both ends and compare them until the deque has 0 or 1 characters left.

-----