> [!Summary] Note
> # Python Comprehensions — Rapid Revision
> 
> Comprehensions provide a compact way to create **lists, sets, and dictionaries** using loops and optional conditions.
> 
> ---
> 
> ## 1. List Comprehension
> 
> ### Basic
> 
> ```python
> [expression for item in iterable]
> ```
> 
> ```python
> squares = [x * x for x in range(5)]
> # [0, 1, 4, 9, 16]
> ```
> 
> ### With Condition — Filtering
> 
> ```python
> evens = [x for x in range(10) if x % 2 == 0]
> ```
> 
> ### With `if-else` — Transformation
> 
> ```python
> result = ["Even" if x % 2 == 0 else "Odd" for x in numbers]
> ```
> 
> **Remember the order:**
> 
> ```python
> [expression for item in iterable if condition]
> ```
> 
> ```python
> [expression_if_true if condition else expression_if_false
>  for item in iterable]
> ```
> 
> ---
> 
> ## 2. Set Comprehension
> 
> Uses `{}` and creates a **set**, so duplicate values are automatically removed.
> 
> ### Syntax
> 
> ```python
> {expression for item in iterable}
> ```
> 
> Example:
> 
> ```python
> squares = {x * x for x in range(5)}
> # {0, 1, 4, 9, 16}
> ```
> 
> Duplicates:
> 
> ```python
> values = [1, 2, 2, 3, 3, 3]
> 
> unique = {x for x in values}
> # {1, 2, 3}
> ```
> 
> With condition:
> 
> ```python
> even_squares = {x*x for x in numbers if x % 2 == 0}
> ```
> 
> ---
> 
> ## 3. Dictionary Comprehension
> 
> Creates a dictionary using **key : value** pairs.
> 
> ### Syntax
> 
> ```python
> {key: value for item in iterable}
> ```
> 
> Example:
> 
> ```python
> squares = {x: x*x for x in range(5)}
> ```
> 
> Result:
> 
> ```python
> {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
> ```
> 
> ### With Condition
> 
> ```python
> even_squares = {
>     x: x*x
>     for x in range(10)
>     if x % 2 == 0
> }
> ```
> 
> ### Transform an Existing Dictionary
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
> ## 4. Nested Comprehensions
> 
> Multiple `for` clauses can be used.
> 
> ```python
> pairs = [(x, y) for x in [1, 2] for y in [3, 4]]
> ```
> 
> Result:
> 
> ```text
> [(1, 3), (1, 4), (2, 3), (2, 4)]
> ```
> 
> Can also be used with sets/dictionaries.
> 
> ---
> 
> ### Useful Examples
> 
> ```python
> # Convert strings to uppercase
> names = [name.upper() for name in names]
> 
> # Filter + transform
> squares = [x*x for x in numbers if x > 0]
> 
> # Flatten a nested list
> flat = [x for row in matrix for x in row]
> ```
> 
> ### Remember
> 
> ```text
> [expression for item in iterable]          → Create
> [expression for item in iterable if cond]  → Filter
> [A if cond else B for item in iterable]   → Transform
> ```
> 
> **Rule of thumb:** Use comprehensions for **simple, readable transformations**. If the logic becomes complicated or deeply nested, use a normal `for` loop.**
> 
