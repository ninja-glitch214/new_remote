> [!Question] Note
> # Python List — Rapid Revision
> 
> A **list** is an **ordered, mutable collection** that can store multiple values of different types.
> 
> ```python
> numbers = [10, 20, 30, 40]
> mixed = [10, "Python", 3.14, True]
> ```
> 
> ## 1. Accessing Elements
> 
> ```python
> numbers[0]      # 10
> numbers[-1]     # 40
> numbers[1:3]    # [20, 30]
> numbers[::-1]   # Reverse
> ```
> 
> ### Modify
> 
> ```python
> numbers[0] = 100
> numbers[1:3] = [200, 300]
> ```
> 
> ---
> 
> ## 2. Adding Elements
> 
> ```python
> lst.append(10)          # Add one item at end
> lst.extend([20, 30])    # Add multiple items
> lst.insert(1, 99)       # Insert at index
> ```
> 
> ### Difference
> 
> ```python
> append([1, 2])   # Adds the list as ONE item
> extend([1, 2])   # Adds 1 and 2 separately
> ```
> 
> ---
> 
> ## 3. Removing Elements
> 
> ```python
> lst.remove(10)    # Remove first matching value
> lst.pop()         # Remove & return last item
> lst.pop(2)        # Remove & return item at index 2
> del lst[1]        # Delete by index
> lst.clear()       # Remove everything
> ```
> 
> ### Important
> 
> ```python
> lst.remove(10)    # value
> lst.pop(2)        # index
> del lst[2]        # index
> ```
> 
> ---
> 
> ## 4. Searching / Checking
> 
> ```python
> 10 in lst             # True/False
> 10 not in lst         # True/False
> 
> lst.index(10)         # First index of value
> lst.count(10)         # Number of occurrences
> ```
> 
> ---
> 
> ## 5. Length
> 
> ```python
> len(lst)
> ```
> 
> Returns the number of elements.
> 
> ---
> 
> ## 6. Sorting
> 
> ```python
> lst.sort()                 # Ascending, modifies list
> lst.sort(reverse=True)     # Descending
> ```
> 
> ### Using `key`
> 
> ```python
> words = ["apple", "kiwi", "banana"]
> 
> words.sort(key=len)
> ```
> 
> Sort by length.
> 
> ### `sorted()`
> 
> Unlike `.sort()`, `sorted()` **returns a new list**:
> 
> ```python
> new_list = sorted(lst)
> ```
> 
> Original list remains unchanged.
> 
> ---
> 
> ## 7. Reverse
> 
> ```python
> lst.reverse()
> ```
> 
> Reverses the list **in place**.
> 
> Or:
> 
> ```python
> reversed_list = lst[::-1]
> ```
> 
> Or:
> 
> ```python
> reversed_list = list(reversed(lst))
> ```
> 
> ---
> 
> ## 8. Copying Lists ⚠️
> 
> ```python
> b = a
> ```
> 
> Does **not** create a new list. Both variables reference the same list.
> 
> ### Copying Lists — Shallow vs Deep Copy
> 
> #### Shallow Copy
> 
> Creates a **new outer list**, but nested mutable objects are still shared.
> 
> ```python
> a = [[1, 2], [3, 4]]
> 
> b = a.copy()
> # or: b = a[:]
> # or: b = list(a)
> ```
> 
> ```python
> b[0].append(99)
> 
> print(a)
> # [[1, 2, 99], [3, 4]]
> ```
> 
> ⚠️ Changes to nested objects affect both lists.
> 
> #### Deep Copy
> 
> Creates a completely independent copy, including nested objects.
> 
> ```python
> from copy import deepcopy
> 
> a = [[1, 2], [3, 4]]
> 
> b = deepcopy(a)
> 
> b[0].append(99)
> 
> print(a)
> # [[1, 2], [3, 4]]
> 
> print(b)
> # [[1, 2, 99], [3, 4]]
> ```
> 
> ### Remember
> 
> ```text
> Shallow → new outer list, shared nested objects
> Deep    → new outer list + copied nested objects
> 
> .copy() / [:] / list() → Shallow copy
> deepcopy()              → Deep copy
> ```
> 
> **Use `deepcopy()` when nested mutable objects must be completely independent.**
> 
> ---
> 
> ## 9. Concatenation & Repetition
> 
> ```python
> a + b       # Combine lists
> a * 3       # Repeat list
> ```
> 
> Example:
> 
> ```python
> [1, 2] + [3, 4]
> # [1, 2, 3, 4]
> 
> [1, 2] * 3
> # [1, 2, 1, 2, 1, 2]
> ```
> 
> ---
> 
> ## 10. Unpacking
> 
> ```python
> numbers = [10, 20, 30]
> 
> a, b, c = numbers
> ```
> 
> Using `*`:
> 
> ```python
> first, *middle, last = [1, 2, 3, 4, 5]
> ```
> 
> Result:
> 
> ```python
> first   # 1
> middle  # [2, 3, 4]
> last    # 5
> ```
> 
> ---
> 
> ## 11. Looping
> 
> ```python
> for item in lst:
>     print(item)
> ```
> 
> With index:
> 
> ```python
> for index, value in enumerate(lst):
>     print(index, value)
> ```
> 
> ---
> 
> ## 12. List Comprehension
> 
> Create/filter lists compactly:
> 
> ```python
> squares = [x*x for x in range(10)]
> ```
> 
> With condition:
> 
> ```python
> evens = [x for x in numbers if x % 2 == 0]
> ```
> 
> With `if-else`:
> 
> ```python
> result = ["Even" if x % 2 == 0 else "Odd" for x in numbers]
> ```
> 
> ---
> 
> ## 13. Nested Lists
> 
> Lists can contain other lists:
> 
> ```python
> matrix = [
>     [1, 2, 3],
>     [4, 5, 6]
> ]
> 
> matrix[0][1]    # 2
> ```
> 
> Flatten a simple nested list:
> 
> ```python
> flat = [x for row in matrix for x in row]
> ```
> 
> ---
> 
> ## 14. Useful Built-in Functions
> 
> ```python
> len(lst)       # Number of elements
> min(lst)       # Smallest
> max(lst)       # Largest
> sum(lst)       # Total
> sorted(lst)    # Sorted copy
> reversed(lst)  # Reverse iterator
> any(lst)       # At least one truthy value
> all(lst)       # All values truthy
> ```
> 
> Example:
> 
> ```python
> numbers = [10, 20, 30]
> 
> sum(numbers)   # 60
> min(numbers)   # 10
> max(numbers)   # 30
> ```
> 
> ---
> 
> ## 15. `any()` and `all()`
> 
> ```python
> any([False, False, True])
> # True
> ```
> 
> ```python
> all([True, True, True])
> # True
> ```
> 
> Useful with conditions:
> 
> ```python
> any(x > 100 for x in numbers)
> all(x > 0 for x in numbers)
> ```
> 
> ---
> 
> ## 16. Combining Lists with `zip()`
> 
> ```python
> names = ["A", "B", "C"]
> scores = [90, 80, 70]
> 
> pairs = list(zip(names, scores))
> ```
> 
> Result:
> 
> ```python
> [("A", 90), ("B", 80), ("C", 70)]
> ```
> 
> ---
> 
> ## 17. Filtering
> 
> Using list comprehension:
> 
> ```python
> positive = [x for x in numbers if x > 0]
> ```
> 
> Using `filter()`:
> 
> ```python
> positive = list(filter(lambda x: x > 0, numbers))
> ```
> 
> ---
> 
> ## 18. Mapping
> 
> Using comprehension:
> 
> ```python
> squares = [x*x for x in numbers]
> ```
> 
> Using `map()`:
> 
> ```python
> squares = list(map(lambda x: x*x, numbers))
> ```
> 
> ---
> 
> ## 19. Important List Methods
> 
> ```text
> append(x)       → Add item at end
> extend(iterable)→ Add multiple items
> insert(i, x)    → Insert at index
> remove(x)       → Remove first matching value
> pop([i])        → Remove & return item
> clear()         → Remove all items
> 
> index(x)        → Find first index
> count(x)        → Count occurrences
> 
> sort()          → Sort in place
> reverse()       → Reverse in place
> copy()          → Shallow copy
> ```
> 
> ## Quick Revision
> 
> ```text
> []                  → Create list
> lst[i]              → Access
> lst[i] = value      → Modify
> lst[a:b]            → Slice
> 
> append()            → Add one
> extend()            → Add many
> insert()            → Add at position
> 
> remove()            → Remove by value
> pop()               → Remove by index / end
> del                 → Delete
> clear()             → Empty list
> 
> index()             → Find position
> count()             → Count value
> 
> sort()              → Sort in place
> sorted()            → New sorted list
> reverse()           → Reverse in place
> 
> copy()              → Shallow copy
> deepcopy()          → Deep copy
> 
> +                   → Concatenate
> *                   → Repeat
> in / not in         → Membership
> 
> len()               → Length
> sum()               → Total
> min() / max()       → Minimum / Maximum
> any() / all()       → Boolean checks
> 
> enumerate()         → Index + value
> zip()               → Combine iterables
> ```
> 
> ### ⭐ Most Important Distinctions
> 
> ```text
> append()  → adds ONE object
> extend()  → adds elements from an iterable
> 
> remove()  → by VALUE
> pop()     → by INDEX and returns the item
> del       → deletes by INDEX/slice
> 
> sort()    → changes original list
> sorted()  → returns a new sorted list
> 
> reverse() → changes original list
> [::-1]    → creates a reversed copy
> 
> copy()    → shallow copy
> deepcopy()→ independent nested copy
> ```
> 
> ## 0. List Creation
> 
> ### Direct Creation
> 
> ```python
> a = [1, 2, 3]
> empty = []
> ```
> 
> ### Using `list()`
> 
> Convert an iterable into a list:
> 
> ```python
> list("abc")
> # ['a', 'b', 'c']
> 
> list(range(5))
> # [0, 1, 2, 3, 4]
> ```
> 
> ⚠️ `list()` expects an **iterable**:
> 
> ```python
> list(123)      # TypeError
> ```
> 
> ### Repeating a List
> 
> ```python
> [0] * 5
> # [0, 0, 0, 0, 0]
> ```
> 
> Useful for creating a list of repeated immutable values.
> 
> ---
> 
> ## ⚠️ List Creation & Repetition Pitfalls
> 
> ### 1. `[[]] * n` — Shared References
> 
> This is a common trap:
> 
> ```python
> matrix = [[]] * 3
> 
> matrix[0].append(10)
> 
> print(matrix)
> # [[10], [10], [10]]
> ```
> 
> All three positions refer to the **same inner list**.
> 
> ✅ Use a comprehension:
> 
> ```python
> matrix = [[] for _ in range(3)]
> 
> matrix[0].append(10)
> 
> print(matrix)
> # [[10], [], []]
> ```
> 
> ### 2. Same Problem with Nested Lists
> 
> ❌ Avoid:
> 
> ```python
> matrix = [[0] * 3] * 3
> ```
> 
> Changing one row changes all rows:
> 
> ```python
> matrix[0][0] = 99
> 
> # [[99, 0, 0],
> #  [99, 0, 0],
> #  [99, 0, 0]]
> ```
> 
> ✅ Correct:
> 
> ```python
> matrix = [[0] * 3 for _ in range(3)]
> 
> matrix[0][0] = 99
> ```
> 
> Now only the first row changes.
> 
> ### 3. `*` Repeats References, Not Objects
> 
> List repetition creates references to the **same objects**; it does not independently copy mutable objects.
> 
> ```python
> x = [[1, 2]] * 3
> 
> x[0].append(3)
> 
> print(x)
> # [[1, 2, 3], [1, 2, 3], [1, 2, 3]]
> ```
> 
> ### Remember
> 
> ```text
> [0] * 5          → Safe for immutable values
> [[]] * 5         → ⚠️ Same inner list
> [[0]*3] * 3      → ⚠️ Same inner lists
> [[] for _ in range(5)] → Separate lists
> ```
> 
> **Golden rule:**  
> When creating repeated **mutable objects** (`list`, `dict`, etc.), prefer a **comprehension** over `*`.
