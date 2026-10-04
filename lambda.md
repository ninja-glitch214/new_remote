> [!TIP] Note
> # Python Lambda Functions — Rapid Revision
> 
> A **lambda** is a small **anonymous function** written in a single expression.
> 
> ### Syntax
> 
> ```python
> lambda arguments: expression
> ```
> 
> Example:
> 
> ```python
> square = lambda x: x * x
> 
> print(square(5))   # 25
> ```
> 
> Equivalent to:
> 
> ```python
> def square(x):
>     return x * x
> ```
> 
> ---
> 
> ## 1. Multiple Arguments
> 
> ```python
> add = lambda a, b: a + b
> 
> print(add(10, 20))
> ```
> 
> ---
> 
> ## 2. Conditional Logic
> 
> ```python
> check = lambda x: "Even" if x % 2 == 0 else "Odd"
> 
> print(check(5))
> ```
> 
> ---
> 
> ## 3. `sorted()` — Custom Sorting ⭐
> 
> One of the most common uses.
> 
> ```python
> students = [
>     ("Ash", 80),
>     ("Bob", 95),
>     ("John", 70)
> ]
> 
> students.sort(key=lambda x: x[1])
> ```
> 
> Sort in descending order:
> 
> ```python
> students.sort(key=lambda x: x[1], reverse=True)
> ```
> 
> ---
> 
> ## 4. `map()` — Transform Values
> 
> Apply a function to every item:
> 
> ```python
> numbers = [1, 2, 3, 4]
> 
> squares = list(map(lambda x: x * x, numbers))
> ```
> 
> Result:
> 
> ```text
> [1, 4, 9, 16]
> ```
> 
> ---
> 
> ## 5. `filter()` — Filter Values
> 
> Keep items that satisfy a condition:
> 
> ```python
> numbers = [1, 2, 3, 4, 5, 6]
> 
> evens = list(filter(lambda x: x % 2 == 0, numbers))
> ```
> 
> Result:
> 
> ```text
> [2, 4, 6]
> ```
> 
> ---
> 
> ## 6. `reduce()` — Repeatedly Combine Values
> 
> `reduce()` is available in `functools`.
> 
> ```python
> from functools import reduce
> 
> numbers = [1, 2, 3, 4]
> 
> total = reduce(lambda a, b: a + b, numbers)
> 
> print(total)
> # 10
> ```
> 
> Other operations:
> 
> ```python
> product = reduce(lambda a, b: a * b, numbers)
> ```
> 
> ---
> 
> ## 7. Custom Key for Dictionaries
> 
> Sort dictionary items by their values:
> 
> ```python
> data = {
>     "A": 30,
>     "B": 10,
>     "C": 20
> }
> 
> result = sorted(data.items(), key=lambda x: x[1])
> ```
> 
> Result:
> 
> ```text
> [('B', 10), ('C', 20), ('A', 30)]
> ```
> 
> ---
> 
> ## 8. Sorting Objects
> 
> ```python
> class Student:
>     def __init__(self, name, marks):
>         self.name = name
>         self.marks = marks
> 
> students = [
>     Student("Ash", 80),
>     Student("Bob", 95),
>     Student("John", 70)
> ]
> 
> students.sort(key=lambda student: student.marks)
> ```
> 
> ---
> 
> ## 9. Custom `max()` / `min()`
> 
> Find an object based on a particular property:
> 
> ```python
> students = [
>     ("Ash", 80),
>     ("Bob", 95),
>     ("John", 70)
> ]
> 
> highest = max(students, key=lambda x: x[1])
> lowest = min(students, key=lambda x: x[1])
> ```
> 
> ---
> 
> ## 10. Passing Lambda as a Function
> 
> Lambda functions can be passed as arguments:
> 
> ```python
> def apply(func, value):
>     return func(value)
> 
> result = apply(lambda x: x * 10, 5)
> 
> print(result)
> # 50
> ```
> 
> This is useful when you need a **small function only once**.
> 
> ---
> 
> ## 11. Returning a Lambda
> 
> A function can create and return another function:
> 
> ```python
> def multiplier(n):
>     return lambda x: x * n
> 
> double = multiplier(2)
> triple = multiplier(3)
> 
> print(double(5))
> print(triple(5))
> ```
> 
> Output:
> 
> ```text
> 10
> 15
> ```
> 
> This is related to **closures**.
> 
> ---
> 
> ## 12. Lambda with `if-else` in Sorting/Processing
> 
> ```python
> numbers = [10, 5, 20, 3]
> 
> result = sorted(
>     numbers,
>     key=lambda x: "even" if x % 2 == 0 else "odd"
> )
> ```
> 
> Lambda expressions can contain conditional expressions, but only **expressions**, not statements.
> 
> ---
> 
> ## 13. Immediately Execute a Lambda
> 
> A lambda can be created and called immediately:
> 
> ```python
> result = (lambda x: x * 2)(10)
> 
> print(result)
> # 20
> ```
> 
> This is possible but uncommon; usually a normal expression or `def` is clearer.
> 
> ---
> 
> ## 14. Lambda with `*args`
> 
> Lambda can accept variable positional arguments:
> 
> ```python
> total = lambda *args: sum(args)
> 
> print(total(10, 20, 30))
> # 60
> ```
> 
> And keyword arguments:
> 
> ```python
> show = lambda **kwargs: kwargs
> 
> print(show(name="Ash", age=25))
> ```
> 
> ---
> 
> ## Important Limitations
> 
> Lambda functions are intentionally limited:
> 
> ```text
> ✓ One expression
> ✓ Can have multiple arguments
> ✓ Can use conditions/ternary expressions
> ✓ Can call other functions
> ✓ Can use *args / **kwargs
> ✗ Cannot contain normal statements
> ✗ Cannot contain multiple statements
> ✗ Usually unsuitable for complex logic
> ```
> 
> For example, this is **not valid**:
> 
> ```python
> # ❌ Not valid lambda syntax
> lambda x:
>     y = x * 2
>     return y
> ```
> 
> Use `def` instead.
> 
> ---
> 
> # Common Lambda Use Cases
> 
> ```text
> sorted() / sort() → custom sorting key       ⭐⭐⭐
> map()             → transform values         ⭐⭐
> filter()          → filter values            ⭐⭐
> max() / min()     → custom selection         ⭐⭐
> reduce()          → combine values           ⭐
> callbacks         → pass small functions     ⭐⭐
> closures          → return customized funcs  ⭐
> *args / **kwargs  → flexible lambda args
> ```
> 
> ### Remember
> 
> ```python
> lambda x: x * 2
> ```
> 
> means:
> 
> > **"Create a small function that takes `x` and returns `x * 2`."**
> 
> **Best rule:** Use `lambda` for **small, one-expression functions**, especially when a function is needed temporarily as an argument. Use `def` when the logic deserves a name or becomes complicated.
