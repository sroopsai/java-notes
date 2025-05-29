# Section Review: Using the Map Interface

This document contains the summary, conceptual questions, scenario-based questions, and coding problems for the "Using the Map Interface" section.

-----

## Summary 🗺️

This section introduces the `java.util.Map` interface, used for storing key-value pairs. It covers its main characteristics, common implementations (`HashMap`, `LinkedHashMap`, `TreeMap`), factory methods for creating maps, and important `Map` interface methods.

1.  **`Map<K, V>` Interface Overview:**

    * A **`Map`** is an object that maps keys to values.
    * **Keys must be unique** within a map.
    * Each key can map to at most one value.
    * Values can be duplicates.
    * `Map` is part of the Java Collections Framework but **does not extend the `Collection` interface** because its structure and methods are fundamentally different (key-value based vs. single element based).

2.  **Common `Map` Implementations:**

    * **`HashMap<K, V>`:**
        * Stores entries in a hash table based on the `hashCode()` of the keys.
        * **Order:** Does **not** guarantee any specific iteration order for keys or values; order can appear arbitrary.
        * **Performance:** Offers average time complexity of O(1) for `put`, `get`, and `remove` operations, assuming a good hash function.
        * Allows one `null` key and multiple `null` values.
    * **`LinkedHashMap<K, V>`:**
        * Extends `HashMap`.
        * Maintains entries in **insertion order** (or optionally, access order) using an internal doubly linked list.
        * Provides the same no-duplicate-keys guarantee and average O(1) performance as `HashMap` for basic operations, with slight overhead for maintaining order.
        * The text hints at Java 21 capabilities (like `SequencedMap`) which `LinkedHashMap` would leverage.
    * **`TreeMap<K, V>`:**
        * Stores entries in a **sorted tree structure** based on the keys.
        * Keys are **always kept in sorted order** according to their natural ordering (if keys implement `Comparable`) or by a `Comparator` provided at `TreeMap` creation.
        * **Performance:** Offers O(log n) time complexity for `put`, `get`, and `remove`.
        * Keys must be mutually comparable. Does not allow a `null` key if using natural ordering (unless the comparator handles `null`). Allows `null` values.

3.  **Creating Maps with Factory Methods (Immutable):**

    * **`Map.of(K k1, V v1, K k2, V v2, ...)` (Java 9+):**
        * Creates an immutable map with up to 10 key-value pairs.
        * Duplicate keys will cause an `IllegalArgumentException`.
    * **`Map.ofEntries(Map.Entry<K,V>... entries)` (Java 9+):**
        * Creates an immutable map from `Map.Entry` objects (created using `Map.entry(k,v)`). More readable for many entries.
        * Duplicate keys in entries will cause an `IllegalArgumentException`.
    * **`Map.copyOf(Map<? extends K, ? extends V> map)` (Java 10+):**
        * Returns an immutable map containing a copy of the entries from the given map.

4.  **Common `Map<K, V>` Interface Methods (Table 9.6):**

    * **`V put(K key, V value)`:** Associates `value` with `key`. Returns the previous value for `key`, or `null` if `key` was new.
    * **`V get(Object key)`:** Returns the value for `key`, or `null` if `key` is not found.
    * **`V remove(Object key)`:** Removes the mapping for `key`. Returns the previous value, or `null` if `key` was not found.
    * **`boolean containsKey(Object key)`:** Returns `true` if the map contains a mapping for `key`.
    * **`boolean containsValue(Object value)`:** Returns `true` if the map maps one or more keys to `value` (can be less efficient).
    * **`int size()`:** Returns the number of key-value mappings.
    * **`boolean isEmpty()`:** Returns `true` if the map is empty.
    * **`void clear()`:** Removes all mappings.
    * **`Set<K> keySet()`:** Returns a `Set` view of all keys. Changes to the set can affect the map (and vice-versa) if the map is mutable.
    * **`Collection<V> values()`:** Returns a `Collection` view of all values. Changes to the collection can affect the map (and vice-versa) if the map is mutable.
    * **`Set<Map.Entry<K, V>> entrySet()`:** Returns a `Set` view of all entries (`Map.Entry` objects). `Map.Entry` has `getKey()` and `getValue()` (and `setValue()` if the map entry is modifiable).
    * **`void forEach(BiConsumer<? super K, ? super V> action)`:** Performs an action for each key-value pair.
    * **`V getOrDefault(Object key, V defaultValue)`:** Returns value for `key`, or `defaultValue` if `key` not found.
    * **`V putIfAbsent(K key, V value)`:** If `key` is not already associated with a value (or is mapped to `null`), associates it with `value` and returns `null`. Otherwise, returns the current value.
    * **`V replace(K key, V value)`:** Replaces the entry for `key` only if it is currently mapped to some value. Returns the previous value, or `null`.
    * **`void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)`:** Replaces each entry's value with the result of applying the function to the key and current value.
    * **`V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)`:**
        * If key is absent or associated value is `null`: associates key with the given non-null `value`.
        * If key is present with non-null value: `remappingFunction` is applied to current value and given `value`. If result is `null`, key is removed. Otherwise, key is updated with result.
        * Returns the new value associated with key, or `null` if no value (key removed).

-----

## Conceptual Questions

1.  What is the fundamental structural difference between a `Map` and a `Collection` (like `List` or `Set`)?
2.  Why does the `Map` interface not extend the `Collection` interface in Java?
3.  Compare `HashMap`, `LinkedHashMap`, and `TreeMap` in terms of:
    * Order of elements (keys/entries).
    * Whether they allow `null` keys or `null` values.
    * Typical performance for `put`, `get`, and `remove`.
4.  What is the primary risk if you use keys in a `HashMap` whose `hashCode()` method is poorly implemented (e.g., always returns the same value)?
5.  Explain the difference between `Map.of()` and `Map.ofEntries()` for creating maps. What is a key restriction on the keys provided to these methods?
6.  What do the `keySet()`, `values()`, and `entrySet()` methods return, and what is their relationship to the original map (especially for mutable maps)?
7.  Describe the behavior of `put(K key, V value)` versus `putIfAbsent(K key, V value)`. When would you prefer one over the other?
8.  Explain the purpose and arguments of the `merge(K key, V value, BiFunction remappingFunction)` method. Under what conditions is the `remappingFunction` NOT called? What happens if the `remappingFunction` returns `null`?
9.  What does `map.getOrDefault(key, defaultValue)` do differently from `map.get(key)`?
10. If you iterate over a `HashMap` multiple times (assuming no modifications between iterations), are you guaranteed to get the keys/entries in the same order each time?

-----

## Scenario-Based Questions

1.  You need to store a dictionary of words (keys) and their definitions (values). You want to be able to look up definitions quickly, and the order of words doesn't matter. Which `Map` implementation would be most suitable?
2.  You are building a cache where you want to store recently accessed items. When iterating through the cache, you want to see the items in the order they were last accessed (or inserted). Which `Map` implementation might be a good choice for this? (Hint: `LinkedHashMap` can be configured for access order).
3.  If you try to create `Map<String, Integer> map = Map.of("one", 1, "two", 2, "one", 10);`, what will happen at runtime?
4.  Given `Map<String, Integer> scores = new HashMap<>(); scores.put("Alice", 90); scores.put("Bob", 80);`. What does `scores.put("Alice", 95);` return? What is the final content of `scores`?
5.  You have `Map<String, String> config = new HashMap<>(); config.put("url", "http://example.com");`. How would you use `config.merge("url", "/api", (oldVal, newVal) -> oldVal + newVal);` to update the URL? What would be the new value for "url"?
6.  If `Map<Integer, String> myMap = new HashMap<>(); myMap.put(1, "A");`, what is the difference in output between printing `myMap.keySet()` and `myMap.values()`?

-----

## Coding Problems

1.  **Word Frequency Counter:**
    * Write a program that takes a sentence (a `String`) as input.
    * Use a `HashMap<String, Integer>` to count the frequency of each word in the sentence. Ignore case for words (e.g., "The" and "the" are the same word).
    * Print each word and its frequency.
2.  **Configuration Manager:**
    * Create a class `ConfigManager`.
    * Internally, use a `Map<String, String>` (you can choose the implementation, e.g., `HashMap`) to store configuration key-value pairs.
    * Implement the following methods:
        * `void setProperty(String key, String value)`
        * `String getProperty(String key)`
        * `String getPropertyOrDefault(String key, String defaultValue)`
        * `void printAllProperties()` (iterating using `forEach` or `entrySet`).
    * Demonstrate its usage in a `main` method.
3.  **User Preferences with `merge()`:**
    * Imagine you are storing user preferences in a `Map<String, String>`.
    * A user might update a preference, or set it if it doesn't exist. If they try to set a preference with an empty value, it should perhaps remove the preference or set a default.
    * Create a `Map<String, String> userPrefs = new HashMap<>();`.
    * Add an initial preference: `userPrefs.put("theme", "dark");`.
    * Use the `merge()` method to:
        * Update "theme" to "light". The remapping function should just take the new value.
        * Add a new preference "fontSize" = "12px".
        * Try to "update" "notifications" to `null` (or an empty string) using `merge` such that if the remapping function returns `null`, the key is removed.
    * Print the map after each merge operation.

-----