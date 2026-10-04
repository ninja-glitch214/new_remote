> [!Tip] Notes
> # Python Generators & Decorators
> 
> ## 🔹 Generators
> 
> **Generate values one at a time using `yield` → lazy & memory-efficient.**
> 
> ```python
> def nums():
>     yield 1
>     yield 2
> 
> g = nums()
> next(g)       # 1
> next(g)       # 2
> ```
> 
> ### Key Points
> 
> ```text
> yield        → pauses + remembers state
> next(g)      → resumes generator
> yield from   → yield another iterable/generator
> (x*x for x in data) → generator expression
> ```
> 
> **Use for:** large data, files, streams, pipelines, infinite sequences.
> 
> ```text
> return → ends function
> yield  → pauses function
> ```
> 
> ---
> 
> ## 🔹 Decorators
> 
> **Modify/extend a function without changing its original code.**
> 
> ```python
> def logger(func):
>     def wrapper(*args, **kwargs):
>         print("Start")
>         result = func(*args, **kwargs)
>         print("End")
>         return result
>     return wrapper
> 
> @logger
> def add(a, b):
>     return a + b
> ```
> 
> ```text
> @decorator
>     ↓
> func = decorator(func)
> ```
> 
> ### Key Points
> 
> ```text
> func              → original function
> wrapper()         → adds extra behavior
> *args, **kwargs   → supports any arguments
> return result     → preserves output
> @wraps(func)      → preserves function metadata
> ```
> 
> **Common uses:** logging, timing, validation, authentication, caching, retry logic.
> 
> ### 🧠 Memory Trick
> 
> ```text
> Generator → "Give me the next value when I need it."
> 
> Decorator → "Add behavior around my function."
> ```
