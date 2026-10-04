> [!TIP] Note
> # Python Tuple — Rapid Revision
> 
> A **tuple** is an **ordered, immutable collection** that can store multiple values of different types.
> 
> ```python
> t = (10, 20, 30)
> ```
> 
> ## Creating Tuples
> 
> ```python
> t = (1, 2, 3)
> empty = ()
> single = (10,)       # comma is required
> t = 1, 2, 3          # tuple packing
> ```
> 
> ## Accessing Elements
> 
> ```python
> t[0]       # first element
> t[-1]      # last element
> t[1:3]     # slicing
> t[::-1]    # reversed copy
> ```
> 
> ## Tuple Unpacking
> 
> ```python
> a, b, c = (10, 20, 30)
> ```
> 
> Extended unpacking:
> 
> ```python
> first, *middle, last = (1, 2, 3, 4, 5)
> 
> # first  → 1
> # middle → [2, 3, 4]
> # last   → 5
> ```
> 
> Swap values:
> 
> ```python
> a, b = b, a
> ```
> 
> ---
> 
> ## Tuple Methods
> 
> Tuples have only **two methods**:
> 
> ```python
> t.count(10)    # number of occurrences
> t.index(20)    # index of first occurrence
> ```
> 
> ---
> 
> ## Useful Built-in Functions
> 
> ```python
> len(t)       # number of elements
> min(t)       # smallest
> max(t)       # largest
> sum(t)       # sum of elements
> sorted(t)    # returns a LIST
> any(t)       # True if any element is truthy
> all(t)       # True if all elements are truthy
> ```
> 
> ⚠️ `sorted()` does **not** modify the tuple. It creates and returns a **new list**:
> 
> ```python
> t = (3, 1, 2)
> 
> result = sorted(t)
> 
> print(result)   # [1, 2, 3]
> print(t)        # (3, 1, 2)
> ```
> 
> If you need a sorted tuple:
> 
> ```python
> t = tuple(sorted(t))
> # (1, 2, 3)
> ```
> 
> ---
> 
> # Tuple Restrictions & Important Edge Cases ⚠️
> 
> ## 1. Is the Order of Elements Changeable?
> 
> **No.** A tuple is ordered, but its order **cannot be changed in-place**.
> 
> ```python
> t = (3, 1, 2)
> 
> # t.sort()      ❌
> # t[0] = 100    ❌
> ```
> 
> Unlike a list, tuples do **not** have `.sort()` or `.reverse()` methods.
> 
> You can create a **new tuple** with a different order:
> 
> ```python
> t = (3, 1, 2)
> 
> t = tuple(sorted(t))
> 
> print(t)
> # (1, 2, 3)
> ```
> 
> Or reverse it:
> 
> ```python
> t = t[::-1]
> ```
> 
> So remember:
> 
> ```text
> Tuple order → cannot be modified
> New tuple  → can be created with a different order
> ```
> 
> ---
> 
> ## 2. Can a Tuple Contain Mutable Data?
> 
> **Yes.** A tuple can contain mutable objects such as lists, dictionaries, or sets.
> 
> ```python
> t = ([1, 2], {"name": "Ash"})
> 
> t[0].append(3)
> 
> print(t)
> # ([1, 2, 3], {'name': 'Ash'})
> ```
> 
> The **tuple itself hasn't changed** — the object stored inside it changed.
> 
> Think of it as:
> 
> ```text
> Tuple
>  ├── reference → List [1, 2]
>  └── reference → Dictionary {...}
> ```
> 
> The tuple cannot change which objects those positions refer to, but the mutable objects themselves can change.
> 
> ---
> 
> ## 3. Why Can This Be Bad Practice?
> 
> Putting mutable objects inside tuples isn't automatically wrong, but it can cause **unexpected behavior** when the tuple is intended to represent fixed/unchanging data.
> 
> Example:
> 
> ```python
> person = ("Ash", ["Python", "C++"])
> 
> data = {person: "Student"}   # ❌
> ```
> 
> This fails because the tuple contains a **list**, and therefore the tuple is not hashable.
> 
> A tuple is hashable only when **all of its elements are hashable**.
> 
> ```python
> good = ("Ash", "Python")        # ✓ hashable
> bad = ("Ash", ["Python"])      # ❌ not hashable
> ```
> 
> This matters when using tuples as:
> 
> - Dictionary keys
>     
> - Set elements
>     
> - Cached values
>     
> - Immutable identifiers
>     
> 
> ### Better approach
> 
> If the data should truly be fixed, use immutable objects inside the tuple:
> 
> ```python
> person = ("Ash", ("Python", "C++"))
> ```
> 
> ---
> 
> ## 4. Tuple Hashability
> 
> A tuple is **not automatically hashable just because it is immutable**.
> 
> Its elements must also be hashable.
> 
> ```python
> a = (1, 2, 3)          # ✓ hashable
> b = ("A", "B")         # ✓ hashable
> c = (1, [2, 3])        # ❌ not hashable
> d = (1, {2, 3})        # ❌ not hashable
> ```
> 
> Therefore:
> 
> ```python
> locations = {
>     (10, 20): "Point A"
> }
> ```
> 
> works because integers are hashable.
> 
> But:
> 
> ```python
> locations = {
>     ([10], [20]): "Point A"
> }
> ```
> 
> doesn't work because lists are unhashable.
> 
> ---
> 
> ## 5. Can You Add or Remove Elements?
> 
> No.
> 
> ```python
> t = (1, 2, 3)
> 
> # t.append(4)     ❌
> # t.remove(2)     ❌
> # del t[0]        ❌
> ```
> 
> To "modify" a tuple, create a new tuple:
> 
> ```python
> t = (1, 2, 3)
> 
> t = t + (4,)
> 
> print(t)
> # (1, 2, 3, 4)
> ```
> 
> ---
> 
> ## 6. Immutability vs Mutability Inside a Tuple
> 
> This distinction is important:
> 
> ```python
> t = ([1, 2], 10)
> ```
> 
> You **cannot** do:
> 
> ```python
> t[0] = [3, 4]       # ❌
> ```
> 
> But you **can** do:
> 
> ```python
> t[0].append(3)      # ✓
> ```
> 
> Because:
> 
> ```text
> Changing tuple's element/reference → ❌
> Changing mutable object stored inside → ✓
> ```
> 
> ---
> 
> ## Concatenation
> 
> ```python
> a = (1, 2)
> b = (3, 4)
> 
> c = a + b
> 
> # (1, 2, 3, 4)
> ```
> 
> ## Repetition
> 
> ```python
> t = (1, 2)
> 
> print(t * 3)
> 
> # (1, 2, 1, 2, 1, 2)
> ```
> 
> ## Membership
> 
> ```python
> 10 in t
> 50 not in t
> ```
> 
> ## Convert Between List and Tuple
> 
> ```python
> list(t)        # tuple → list
> tuple(my_list) # list → tuple
> ```
> 
> ---
> 
> ## Tuple as Dictionary Key
> 
> A tuple containing only **hashable elements** can be used as a dictionary key:
> 
> ```python
> locations = {
>     (10, 20): "Point A",
>     (30, 40): "Point B"
> }
> ```
> 
> This is commonly useful for:
> 
> - Coordinates
>     
> - Grid positions
>     
> - Composite identifiers
>     
> 
> ---
> 
> ## Returning Multiple Values
> 
> Functions commonly return multiple values using a tuple:
> 
> ```python
> def calculate(a, b):
>     return a + b, a * b
> 
> sum_, product = calculate(5, 3)
> 
> # sum_     → 8
> # product  → 15
> ```
> 
> ---
> 
> ## Named Tuple
> 
> For lightweight structured data:
> 
> ```python
> from collections import namedtuple
> 
> Point = namedtuple("Point", ["x", "y"])
> 
> p = Point(10, 20)
> 
> print(p.x)
> print(p.y)
> ```
> 
> ---
> 
> ## Tuple Comprehension? ⚠️
> 
> There is **no tuple comprehension**.
> 
> This:
> 
> ```python
> (x * 2 for x in range(5))
> ```
> 
> creates a **generator**, not a tuple.
> 
> To create a tuple:
> 
> ```python
> tuple(x * 2 for x in range(5))
> ```
> 
> ---
> 
> # Quick Revision
> 
> ```text
> ()                    → tuple
> (10,)                 → single-element tuple
> t[i]                  → indexing
> t[start:end]          → slicing
> t[::-1]               → reversed copy
> 
> count()               → count occurrences
> index()               → find first index
> 
> len()                 → length
> min(), max(), sum()   → calculations
> sorted()              → returns LIST
> any(), all()          → boolean checks
> 
> +                     → concatenate
> *                     → repeat
> in / not in           → membership
> 
> tuple()               → convert to tuple
> list()                → convert tuple to list
> 
> a, b = t              → unpacking
> a, *b, c = t          → extended unpacking
> 
> Immutable             → cannot change tuple elements
> Ordered               → maintains element order
> No .sort()            → tuple has no sort method
> No .append()          → cannot add elements
> No .remove()          → cannot remove elements
> 
> Mutable objects       → can exist inside tuple
> Hashable tuple        → only if ALL elements are hashable
> ```
> 
> ### ⭐ Key Restrictions
> 
> **Tuple is ordered → but its order cannot be changed in-place.**
> 
> **Tuple is immutable → elements cannot be added, removed, or replaced.**
> 
> **Tuple can contain mutable objects → but this can make the tuple unhashable.**
> 
> **A tuple is hashable only when every element inside it is hashable.**
> 
> **`sorted(tuple)` → returns a list; it does not modify the tuple.
