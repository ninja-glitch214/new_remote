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
>
> =========================================================================
>
```
def mynumbers():
    yield 1
    yield 2
    yield 3
    yield 4
    yield 5
    
def mynumbers1():
    for i in range(1,6):
        yield i
        
        
        
        
g=mynumbers()
print(type(g))




print(next(g))


for i in mynumbers():
    print(i)
    
    
    
    
    
print(mynumbers1())



def myf1():
    with open ("employeeinfo.txt") as fh:
        for ln in fh:
            yield ln
            
e=myf1()            
print(e)            







#%%


#decorator


def logging():
    print("in loggingunction")
    
def validateusr():
    print("in validateuser")
    

def mydecorator(f):
    def innerfunction(*t,**kwarg):
        logging()
        validateusr()
        print("in mydecorator")
        print("-"*80)
        z=f(*t,**kwarg)
        print("exiting from mydecorator")
        return z
    return innerfunction
    
    

@mydecorator     #mydecorator(f1)  #innerfunction(*t,**kwarg)
def f1(x,y,**kw):
    print("in f1()",x,y,kw)
    return x+10

@mydecorator #mydecorator(f2)
def f2():
    print("in f2()")
    

print(f1(10,20,a=34,b=35))
f2()


```
