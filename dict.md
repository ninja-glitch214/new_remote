> [!TIP]
> # Python Dictionary — Rapid Revision
> 
> A **dictionary** is used when data needs to be stored in **`key: value`** format.
> 
> ```python
> d = {"name": "Ash", "age": 25}
> ```
> 
> ## Key Properties
> 
> ```text
> ✓ Represented using {}
> ✓ Mutable
> ✓ Ordered (Python 3.7+)
> ✓ Keys are unique
> ✓ Values can be duplicate
> ✓ Keys must be hashable
> ✓ Access is done using keys
> ✓ Random/key-based lookup is possible
> ```
> 
> ### Dictionary Key Restrictions 🔑
> 
> Keys must be **hashable**.
> 
> Common valid key types:
> 
> ```python
> d = {
>     "name": "Ash",       # string
>     10: "number",        # integer
>     3.14: "float",       # float
>     (1, 2): "tuple",     # tuple
>     frozenset({1, 2}): "set"
> }
> ```
> 
> Mutable/unhashable objects cannot be keys:
> 
> ```python
> # d = {[1, 2]: "list"}       # ❌
> # d = {{1, 2}: "set"}        # ❌
> # d = {{"a": 1}: "dict"}     # ❌
> ```
> 
> A tuple is valid as a key only when **all its elements are hashable**:
> 
> ```python
> d = {(1, 2): "valid"}       # ✓
> # d = {(1, [2, 3]): "invalid"}  # ❌
> ```
> 
> ### Duplicate Keys
> 
> Keys must be unique. If the same key occurs again, the **last value overwrites the previous value**.
> 
> ```python
> d = {"name": "Ash", "name": "Bob"}
> 
> # {"name": "Bob"}
> ```
> 
> ---
> 
> # 1. Creating Dictionaries
> 
> ```python
> d = {}
> d = {"a": 10, "b": 20}
> 
> d = dict(name="Ash", age=25)
> 
> d = dict([("a", 10), ("b", 20)])
> ```
> 
> ### Dictionary Comprehension
> 
> ```python
> squares = {x: x*x for x in range(5)}
> ```
> 
> ---
> 
> # 2. Accessing Values
> 
> ```python
> d["name"]
> ```
> 
> If the key doesn't exist, `KeyError` occurs.
> 
> ### `get()`
> 
> ```python
> d.get("name")
> d.get("salary", 0)
> ```
> 
> ```text
> d["x"]       → KeyError if x doesn't exist
> d.get("x")   → None if x doesn't exist
> d.get("x", 0) → 0 if x doesn't exist
> ```
> 
> ---
> 
> # 3. Adding / Updating
> 
> ```python
> d["aaa"] = 34
> ```
> 
> If the key **doesn't exist** → adds a new key-value pair.
> 
> If the key **already exists** → overwrites its value.
> 
> ```python
> d = {"a": 10}
> 
> d["a"] = 100
> d["b"] = 200
> 
> # {"a": 100, "b": 200}
> ```
> 
> ### `update()`
> 
> Adds/updates multiple key-value pairs:
> 
> ```python
> d = {"a": 10, "b": 100}
> 
> d1 = {"b": 200, "c": 345, "d": 456}
> 
> d.update(d1)
> ```
> 
> Result:
> 
> ```python
> {"a": 10, "b": 200, "c": 345, "d": 456}
> ```
> 
> Existing keys are overwritten.
> 
> ---
> 
> # 4. Merging Dictionaries
> 
> ### Dictionary Unpacking
> 
> ```python
> d3 = {**d, **d1}
> ```
> 
> Example:
> 
> ```python
> d = {"a": 10, "b": 100}
> d1 = {"b": 200, "c": 345, "d": 456}
> 
> d3 = {**d, **d1}
> ```
> 
> Result:
> 
> ```python
> {"a": 10, "b": 200, "c": 345, "d": 456}
> ```
> 
> If the same key exists, the **rightmost value wins**.
> 
> ### `|` Operator — Python 3.9+
> 
> ```python
> d3 = d | d1
> ```
> 
> Creates a new dictionary.
> 
> ```python
> d |= d1
> ```
> 
> Updates `d` in-place.
> 
> ---
> 
> # 5. Removing Items
> 
> ### `del`
> 
> ```python
> del d["age"]
> ```
> 
> Deletes the key-value pair.
> 
> ### `pop()`
> 
> ```python
> value = d.pop("age")
> ```
> 
> Removes the key and **returns its value**.
> 
> If key doesn't exist:
> 
> ```python
> d.pop("age")       # ❌ KeyError
> d.pop("age", 0)    # returns 0
> ```
> 
> ### `popitem()`
> 
> Removes and returns the **last inserted key-value pair**:
> 
> ```python
> key, value = d.popitem()
> ```
> 
> Returns:
> 
> ```python
> (key, value)
> ```
> 
> ### `clear()`
> 
> Removes all key-value pairs:
> 
> ```python
> d.clear()
> ```
> 
> Dictionary remains empty:
> 
> ```python
> {}
> ```
> 
> ---
> 
> # 6. `get()`
> 
> Returns the value associated with a key.
> 
> ```python
> d.get("name")
> ```
> 
> If key doesn't exist:
> 
> ```python
> d.get("name")           # None
> d.get("name", "Unknown") # "Unknown"
> ```
> 
> Unlike `d["name"]`, `get()` does **not raise `KeyError`** for a missing key.
> 
> ---
> 
> # 7. `setdefault()`
> 
> ```python
> d.setdefault(key, default_value)
> ```
> 
> If key exists → returns its existing value.
> 
> If key doesn't exist → adds `key: default_value` and returns the default value.
> 
> ```python
> d = {"a": 10}
> 
> x = d.setdefault("a", 100)
> # x = 10
> # d = {"a": 10}
> 
> x = d.setdefault("b", 200)
> # x = 200
> # d = {"a": 10, "b": 200}
> ```
> 
> Useful for grouping:
> 
> ```python
> groups = {}
> 
> for name, group in data:
>     groups.setdefault(group, []).append(name)
> ```
> 
> ---
> 
> # 8. `keys()`, `values()`, `items()`
> 
> ### `keys()`
> 
> Returns a **view of all keys**:
> 
> ```python
> d.keys()
> ```
> 
> ### `values()`
> 
> Returns a **view of all values**:
> 
> ```python
> d.values()
> ```
> 
> ### `items()`
> 
> Returns a **view of key-value pairs** as tuples:
> 
> ```python
> d.items()
> ```
> 
> Example:
> 
> ```python
> for key, value in d.items():
>     print(key, value)
> ```
> 
> The returned objects are views, **not lists**:
> 
> ```python
> list(d.keys())
> list(d.values())
> list(d.items())
> ```
> 
> ---
> 
> # 9. Checking Keys
> 
> ```python
> "name" in d
> "name" not in d
> ```
> 
> `in` checks **keys by default**:
> 
> ```python
> "name" in d
> ```
> 
> To check values:
> 
> ```python
> "Ash" in d.values()
> ```
> 
> ---
> 
> # 10. Length
> 
> ```python
> len(d)
> ```
> 
> Returns the number of key-value pairs.
> 
> ---
> 
> # 11. Copying
> 
> ### Shallow Copy
> 
> ```python
> d2 = d.copy()
> ```
> 
> or:
> 
> ```python
> d2 = dict(d)
> ```
> 
> The dictionary itself is copied, but nested mutable objects may still be shared.
> 
> ### Deep Copy
> 
> ```python
> import copy
> 
> d2 = copy.deepcopy(d)
> ```
> 
> Creates independent copies of nested objects.
> 
> ---
> 
> # 12. `fromkeys()`
> 
> Creates a dictionary using a sequence as keys and gives every key the same value.
> 
> ```python
> lst = [1, 2, 3]
> 
> d = dict.fromkeys(lst, 100)
> ```
> 
> Result:
> 
> ```python
> {1: 100, 2: 100, 3: 100}
> ```
> 
> ⚠️ Be careful with mutable default values:
> 
> ```python
> d = dict.fromkeys(["a", "b"], [])
> ```
> 
> Both keys reference the **same list**.
> 
> ---
> 
> # 13. Looping Through Dictionary
> 
> Keys:
> 
> ```python
> for key in d:
>     print(key)
> ```
> 
> Values:
> 
> ```python
> for value in d.values():
>     print(value)
> ```
> 
> Key + value:
> 
> ```python
> for key, value in d.items():
>     print(key, value)
> ```
> 
> ---
> 
> # 14. Dictionary Comprehension
> 
> Basic:
> 
> ```python
> squares = {x: x*x for x in range(5)}
> ```
> 
> With condition:
> 
> ```python
> even = {
>     x: x*x
>     for x in range(10)
>     if x % 2 == 0
> }
> ```
> 
> Transform an existing dictionary:
> 
> ```python
> prices = {"apple": 100, "banana": 50}
> 
> discounted = {
>     item: price * 0.9
>     for item, price in prices.items()
> }
> ```
> 
> ---
> 
> # 15. Nested Dictionaries
> 
> A dictionary can contain another dictionary:
> 
> ```python
> users = {
>     "user1": {
>         "name": "Ash",
>         "age": 25
>     },
>     "user2": {
>         "name": "Bob",
>         "age": 30
>     }
> }
> 
> print(users["user1"]["name"])
> ```
> 
> ---
> 
> # 16. Sorting Dictionaries
> 
> Sort keys:
> 
> ```python
> sorted(d)
> ```
> 
> Sort key-value pairs by key:
> 
> ```python
> sorted(d.items())
> ```
> 
> Sort by values:
> 
> ```python
> sorted(d.items(), key=lambda item: item[1])
> ```
> 
> Create a dictionary sorted by values:
> 
> ```python
> sorted_d = dict(
>     sorted(d.items(), key=lambda item: item[1])
> )
> ```
> 
> ---
> 
> # 17. Useful Built-in Functions
> 
> ```python
> len(d)          # Number of key-value pairs
> min(d)          # Minimum key
> max(d)          # Maximum key
> sorted(d)       # Sorted keys
> ```
> 
> For values:
> 
> ```python
> sum(d.values())
> min(d.values())
> max(d.values())
> ```
> 
> ---
> 
> # 18. Dictionary Unpacking
> 
> Unpack keys:
> 
> ```python
> d = {"name": "Ash", "age": 25}
> 
> print(*d)
> ```
> 
> For function arguments:
> 
> ```python
> def user(name, age):
>     print(name, age)
> 
> data = {"name": "Ash", "age": 25}
> 
> user(**data)
> ```
> 
> `**data` converts dictionary entries into keyword arguments.
> 
> ---
> 
> # 19. Dictionary as Lookup Table
> 
> A dictionary can replace long `if/elif` chains in some situations:
> 
> ```python
> operations = {
>     "add": lambda a, b: a + b,
>     "sub": lambda a, b: a - b,
>     "mul": lambda a, b: a * b
> }
> 
> print(operations["add"](10, 5))
> ```
> 
> ---
> 
> # Dictionary Methods — Quick Revision
> 
> ```text
> get()          → safely retrieve value
> keys()         → view of keys
> values()       → view of values
> items()        → view of (key, value) pairs
> update()       → add/update multiple pairs
> pop()          → remove key + return value
> popitem()      → remove last pair + return (key, value)
> clear()        → remove all pairs
> copy()         → shallow copy
> setdefault()   → get/create key
> fromkeys()     → create dict from keys
> ```
> 
> # Important Operations
> 
> ```text
> d[key]             → access / add / update
> key in d           → check key
> key not in d       → check key absence
> len(d)             → number of pairs
> 
> d1 | d2            → merge into new dictionary
> d1 |= d2           → merge in-place
> 
> {**d1, **d2}       → dictionary unpacking/merge
> **d               → unpack as keyword arguments
> ```
> 
> # ⭐ Must Remember
> 
> ```text
> Dictionary       → key : value
> Keys             → unique + hashable
> Values           → can be duplicate + any type
> Access           → d[key] / get()
> Add/Update       → d[key] = value / update()
> Remove           → del / pop() / popitem()
> Iterate          → keys() / values() / items()
> Copy             → copy()
> Merge            → | / {**d1, **d2}
> Advanced         → setdefault() / fromkeys()
> Comprehension    → {key: value for ...}
> ```
> 
> **Core idea:**  
> A dictionary is mainly used when you need to **associate a unique key with a value and retrieve/update that value using the key**.
