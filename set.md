> [!Question] Note
> # Python Sets — Rapid Revision
> 
> A **set** is a **mutable, unordered collection of unique elements**.
> 
> ```python
> s = {1, 2, 3, 4}
> ```
> 
> ## Key Properties
> 
> ```text
> ✓ Mutable
> ✓ Unordered
> ✓ Stores only unique values
> ✓ No indexing / slicing
> ✓ Elements must be hashable
> ✓ Can contain different hashable data types
> ```
> 
> ---
> 
> ## Creating Sets
> 
> ```python
> s = {1, 2, 3}
> 
> empty = set()        # Correct
> empty = {}           # Creates an empty dictionary, NOT a set
> ```
> 
> Using `set()` with an iterable:
> 
> ```python
> s = set([1, 2, 2, 3])
> # {1, 2, 3}
> ```
> 
> ### Remove Duplicate Characters from a String
> 
> ```python
> text = "this is string"
> 
> unique_chars = set(text)
> print(unique_chars)
> ```
> 
> `set()` removes duplicate characters.
> 
> ---
> 
> # Hashable Element Restriction ⭐
> 
> Set elements **must be hashable** because Python uses hashing to store and quickly locate set elements.
> 
> ### ✅ Hashable
> 
> ```python
> s = {
>     10,
>     3.14,
>     "hello",
>     True,
>     (1, 2),
>     frozenset({3, 4})
> }
> ```
> 
> Common hashable types:
> 
> ```text
> int
> float
> str
> bool
> tuple* 
> frozenset
> None
> ```
> 
> `tuple` is hashable **only when all its elements are hashable**:
> 
> ```python
> {(1, 2, 3)}          # ✓
> {(1, [2, 3])}        # ✗
> ```
> 
> ### ❌ Unhashable
> 
> These cannot be set elements:
> 
> ```python
> {[1, 2, 3]}          # ✗ list
> {{1, 2, 3}}          # ✗ set
> {{"a": 1}}            # ✗ dictionary
> ```
> 
> ```text
> list       → mutable → unhashable
> set        → mutable → unhashable
> dict       → mutable → unhashable
> ```
> 
> ### Remember
> 
> > **Set elements must be hashable.**
> 
> Do not simply memorize "immutable = hashable"; **hashability is the actual requirement**.
> 
> ---
> 
> # Adding Elements
> 
> ### `add()`
> 
> Adds **one** element.
> 
> ```python
> s.add(5)
> ```
> 
> If the value already exists, nothing changes:
> 
> ```python
> s.add(5)
> s.add(5)       # Still only one 5
> ```
> 
> ### `update()`
> 
> Adds multiple elements from an iterable:
> 
> ```python
> s.update([6, 7, 8])
> ```
> 
> It can accept different iterables:
> 
> ```python
> s.update((9, 10))
> s.update("abc")
> ```
> 
> ---
> 
> # Removing Elements
> 
> ### `remove()`
> 
> Removes a specific value.
> 
> ```python
> s.remove(5)
> ```
> 
> If the value does not exist → **`KeyError`**.
> 
> ### `discard()`
> 
> Removes a specific value.
> 
> ```python
> s.discard(5)
> ```
> 
> If the value does not exist → **no error**.
> 
> ### `pop()`
> 
> Removes and returns an **arbitrary element**.
> 
> ```python
> value = s.pop()
> print(value)
> ```
> 
> Do **not** rely on which element gets removed.
> 
> ### `clear()`
> 
> Removes everything:
> 
> ```python
> s.clear()
> ```
> 
> ---
> 
> # Set Operations ⭐
> 
> Given:
> 
> ```python
> A = {1, 2, 3, 4}
> B = {3, 4, 5, 6}
> ```
> 
> ## 1. Union — `|`
> 
> All unique elements from both sets.
> 
> ```python
> A | B
> # {1, 2, 3, 4, 5, 6}
> ```
> 
> ```python
> A.union(B)
> ```
> 
> ---
> 
> ## 2. Intersection — `&`
> 
> Elements common to both sets.
> 
> ```python
> A & B
> # {3, 4}
> ```
> 
> ```python
> A.intersection(B)
> ```
> 
> ---
> 
> ## 3. Difference — `-`
> 
> Elements present in `A` but **not** in `B`.
> 
> ```python
> A - B
> # {1, 2}
> ```
> 
> ```python
> A.difference(B)
> ```
> 
> Note:
> 
> ```python
> B - A
> # {5, 6}
> ```
> 
> Direction matters.
> 
> ---
> 
> ## 4. Symmetric Difference — `^`
> 
> Elements present in **either set, but not both**.
> 
> ```python
> A ^ B
> # {1, 2, 5, 6}
> ```
> 
> ```python
> A.symmetric_difference(B)
> ```   
> 
> #### Also, `union()`, `intersection()`, etc. can accept **iterables**, not only sets. For example:  
> ```
> {1, 2}.union([2, 3])
> {1, 2, 3}
> ```  
> 
> ### Quick Memory
> 
> ```text
> A | B  → UNION
> A & B  → INTERSECTION
> A - B  → DIFFERENCE
> A ^ B  → SYMMETRIC DIFFERENCE
> ```
> 
> ---
> 
> # Set Relationship Checks
> 
> ## `issubset()`
> 
> Checks whether all elements of one set exist in another.
> 
> ```python
> A = {1, 2}
> B = {1, 2, 3}
> 
> A.issubset(B)
> # True
> ```
> 
> Operators:
> 
> ```python
> A <= B       # subset (allows equality)
> A < B        # proper/strict subset
> ```
> 
> ---
> 
> ## `issuperset()`
> 
> Checks whether a set contains all elements of another set.
> 
> ```python
> B.issuperset(A)
> # True
> ```
> 
> Operators:
> 
> ```python
> B >= A       # superset (allows equality)
> B > A        # proper/strict superset
> ```
> 
> ---
> 
> ## `isdisjoint()`
> 
> Checks whether two sets have **no common elements**.
> 
> ```python
> A = {1, 2}
> B = {3, 4}
> 
> A.isdisjoint(B)
> # True
> ```
> 
> If they have even one common element:
> 
> ```python
> A = {1, 2}
> B = {2, 3}
> 
> A.isdisjoint(B)
> # False
> ```
> 
> ---
> 
> # Updating Sets In-Place
> 
> Normal operations return a **new set**:
> 
> ```python
> C = A | B
> ```
> 
> Update methods modify the existing set.
> 
> ```python
> A.update(B)
> # A |= B
> ```
> 
> ```python
> A.intersection_update(B)
> # A &= B
> ```
> 
> ```python
> A.difference_update(B)
> # A -= B
> ```
> 
> ```python
> A.symmetric_difference_update(B)
> # A ^= B
> ```
> 
> ---
> 
> # Copying
> 
> ```python
> B = A.copy()
> ```
> 
> Creates a **shallow copy** of the set.
> 
> Changes to `B` do not change the set structure of `A`.
> 
> ---
> 
> # Membership Testing ⭐
> 
> Sets are particularly useful for checking whether something exists.
> 
> ```python
> users = {"admin", "user", "guest"}
> 
> "admin" in users
> # True
> 
> "manager" not in users
> # True
> ```
> 
> Common development use:
> 
> ```python
> if username in allowed_users:
>     print("Access granted")
> ```
> 
> ---
> 
> # Set Comprehension
> 
> Create a set using comprehension:
> 
> ```python
> squares = {x * x for x in range(5)}
> ```
> 
> With filtering:
> 
> ```python
> evens = {x for x in numbers if x % 2 == 0}
> ```
> 
> Duplicates are automatically removed.
> 
> ---
> 
> # Removing Duplicates ⭐
> 
> One of the most common uses of sets:
> 
> ```python
> numbers = [1, 2, 2, 3, 3, 4]
> 
> unique = set(numbers)
> 
> print(unique)
> # {1, 2, 3, 4}
> ```
> 
> Convert back to a list:
> 
> ```python
> unique = list(set(numbers))
> ```
> 
> ⚠️ **Order is not guaranteed.**
> 
> If preserving original order is important, don't blindly use `list(set(...))`.
> Use 
> 
> ```python
> unique = list(dict.fromkeys(numbers))
> ```
> 
> ---
> 
> # Length
> 
> ```python
> len(s)
> ```
> 
> Returns the number of **unique elements**.
> 
> ---
> 
> # Iterating
> 
> ```python
> for item in s:
>     print(item)
> ```
> 
> Because a normal set is unordered, **do not rely on iteration order**.
> 
> ---
> 
> # No Indexing or Slicing
> 
> You cannot do:
> 
> ```python
> s[0]       # ✗
> s[1:3]     # ✗
> ```
> 
> If you need indexed access:
> 
> ```python
> list(s)[0]
> ```
> 
> But remember that the resulting order is not guaranteed.
> 
> ---
> 
> # `frozenset`
> 
> `frozenset` is an **immutable version of a set**.
> 
> ```python
> fs = frozenset([1, 2, 3])
> ```
> 
> It cannot be modified:
> 
> ```python
> fs.add(4)       # ✗
> ```
> 
> Because it is hashable, a `frozenset` can itself be:
> 
> - A dictionary key
>     
> - An element of another set
>     
> 
> Example:
> 
> ```python
> s = {frozenset({1, 2}), frozenset({3, 4})}
> ```
> 
> ---
> 
> # Useful Set Methods
> 
> ```text
> add(x)                         → Add one element
> update(iterable)               → Add multiple elements
> 
> remove(x)                      → Remove; error if missing
> discard(x)                     → Remove; ignore if missing
> pop()                          → Remove/return arbitrary element
> clear()                        → Remove all elements
> copy()                         → Shallow copy
> 
> union()                        → Union
> intersection()                 → Intersection
> difference()                   → Difference
> symmetric_difference()         → Symmetric difference
> 
> intersection_update()          → Intersection in-place
> difference_update()            → Difference in-place
> symmetric_difference_update()  → Symmetric difference in-place
> 
> issubset()                     → Check subset
> issuperset()                   → Check superset
> isdisjoint()                   → Check no common elements
> ```
> 
> ---
> 
> # Operators Quick Revision
> 
> ```text
> A | B       → Union
> A & B       → Intersection
> A - B       → Difference
> A ^ B       → Symmetric difference
> 
> A <= B      → Subset or equal
> A < B       → Proper subset
> 
> A >= B      → Superset or equal
> A > B       → Proper superset
> 
> x in A      → Membership
> x not in A  → Not a member
> ```
> 
> # ⭐ Final Remember List
> 
> ```text
> Set              → Mutable + unordered + unique
> {}               → Empty dictionary
> set()            → Empty set
> 
> add()            → One element
> update()         → Multiple elements
> remove()         → Remove + error if missing
> discard()        → Remove + no error if missing
> pop()            → Remove arbitrary element
> 
> |                → Union
> &                → Intersection
> -                → Difference
> ^                → Symmetric difference
> 
> in               → Fast membership checking
> set(iterable)    → Remove duplicates
> 
> Elements         → Must be hashable
> list/set/dict    → Cannot be set elements
> tuple/frozenset  → Can be elements if hashable
> 
> frozenset        → Immutable set
> ```
> 
> **Main use cases:** removing duplicates, fast membership testing, comparing collections, finding common/different elements, and performing mathematical set operations.
