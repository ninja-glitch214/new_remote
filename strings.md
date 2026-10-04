> [!NOTE]
> # Python Strings — Rapid Revision & Complete Reference
> 
>   
> 
> A **string** is an immutable sequence of characters.
> 
>   
> 
> ```python
> 
> s = "Hello Python"
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ## Part 1: Core Fundamentals & Rapid Revision
> 
>   
> 
> ### 1. Creating Strings
> 
> Strings can be defined using single quotes, double quotes, or triple quotes for multi-line strings.
> 
>   
> 
> ```python
> 
> s1 = "Hello"
> 
> s2 = 'Hello'
> 
> s3 = """Multi
> 
> line
> 
> string"""
> 
> ```
> 
>   
> 
> #### Raw Strings
> 
> Prefixing a string with `r` treats backslashes literally (useful for regex patterns and filesystem paths):
> 
>   
> 
> ```python
> 
> path = r"C:\Users\Ash\Documents"
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 2. Indexing & Slicing
> 
> Indices allow character extraction from the left (0-indexed) or right (negative indices).
> 
>   
> 
> ```python
> 
> s = "Python"
> 
>   
> 
> s[0]       # 'P'
> 
> s[-1]      # 'n'
> 
> s[1:4]     # 'yth'
> 
> s[:3]      # 'Pyt'
> 
> s[3:]      # 'hon'
> 
> s[::2]     # 'Pto'
> 
> s[::-1]    # 'nohtyP' (reverse string)
> 
> ```
> 
>   
> 
> **Slicing Formula:**
> 
> ```text
> 
> [start : stop : step]
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 3. Common String Methods
> 
>   
> 
> #### Case Conversion
> 
> ```python
> 
> s.upper()        # CONVERT TO UPPERCASE
> 
> s.lower()        # convert to lowercase
> 
> s.title()        # Title Case Every Word
> 
> s.capitalize()   # Capitalize first character only
> 
> s.swapcase()     # Swap upper to lower and lower to upper
> 
> s.casefold()     # Aggressive lowercase for Unicode comparisons
> 
> ```
> 
>   
> 
> #### Searching & Counting
> 
> ```python
> 
> s.find("Py")        # Returns first index of substring, or -1 if absent
> 
> s.rfind("a")        # Searches from the right; returns index or -1
> 
> s.index("Py")       # Returns first index; raises ValueError if absent
> 
> s.rindex("a")       # Searches from right; raises ValueError if absent
> 
> s.count("a")        # Returns number of non-overlapping occurrences
> 
> ```
> 
>   
> 
> #### Checking Content
> 
> These methods test character properties and return `True` or `False`:
> 
>   
> 
> ```python
> 
> s.startswith("Py")  # True if starts with substring
> 
> s.endswith("on")    # True if ends with substring
> 
>   
> 
> s.isalpha()         # True if all characters are alphabetic
> 
> s.isdigit()         # True if all characters are digits
> 
> s.isalnum()         # True if all characters are alphanumeric
> 
> s.isdecimal()       # True if strictly decimal characters
> 
> s.isnumeric()       # True if numeric characters
> 
> s.isidentifier()    # True if valid Python identifier
> 
> s.islower()         # True if all cased characters are lowercase
> 
> s.isupper()         # True if all cased characters are uppercase
> 
> s.isspace()         # True if all characters are whitespace
> 
> s.istitle()         # True if title-cased
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 4. Removing & Replacing
> 
>   
> 
> #### Stripping Characters & Whitespace
> 
> ```python
> 
> s.strip()           # Remove whitespace from both ends
> 
> s.lstrip()          # Remove whitespace from left side
> 
> s.rstrip()          # Remove whitespace from right side
> 
>   
> 
> s.strip(".,!")      # Remove specified characters from both ends
> 
> ```
> 
>   
> 
> #### Substring Replacement
> 
> ```python
> 
> s.replace("Python", "Java")     # Replace all occurrences
> 
>   
> 
> # Limit the number of replacements using count parameter (default count=-1 replaces all)
> 
> s.replace("a", "A", 2)          # Replace first 2 occurrences
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 5. Splitting Strings
> 
> Converts a string into a list of substrings:
> 
>   
> 
> ```python
> 
> "apple,banana,mango".split(",")
> 
> # Result: ['apple', 'banana', 'mango']
> 
>   
> 
> # Split from the right with a maxsplit limit:
> 
> s.rsplit(",", 1)
> 
>   
> 
> # Split by line breaks:
> 
> text.splitlines()
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 6. Joining Strings
> 
> Combines an iterable of strings into a single string using a specified separator:
> 
>   
> 
> ```python
> 
> names = ["Ash", "Bob", "John"]
> 
> result = ", ".join(names)
> 
> # Result: "Ash, Bob, John"
> 
> ```
> 
>   
> 
> **Syntax:**
> 
> ```python
> 
> separator.join(iterable)
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 7. String Formatting
> 
>   
> 
> #### f-Strings (Formatted String Literals) ⭐
> 
> ```python
> 
> name = "Ash"
> 
> age = 25
> 
> print(f"{name} is {age} years old")
> 
>   
> 
> # Expressions inside f-strings:
> 
> print(f"Total = {10 * 5}")
> 
>   
> 
> # Formatting floating-point numbers:
> 
> price = 1234.5678
> 
> print(f"{price:.2f}")  # '1234.57'
> 
> ```
> 
>   
> 
> **Useful Format Specifiers:**
> 
> | Specifier | Description | Example Output |
> 
> | :--- | :--- | :--- |
> 
> | `:.2f` | Round to 2 decimal places | `1234.57` |
> 
> | `:,` | Thousands separator | `1,234.57` |
> 
> | `:>10` | Right align (width 10) | `'      Text'` |
> 
> | `:<10` | Left align (width 10) | `'Text      '` |
> 
> | `:^10` | Center align (width 10) | `'   Text   '` |
> 
> | `:05` | Zero padding to 5 digits | `'00042'` |
> 
>   
> 
> ```python
> 
> print(f"{42:05}")  # Output: '00042'
> 
> ```
> 
>   
> 
> #### Old `.format()` Method
> 
> ```python
> 
> name = "Ash"
> 
> print("Hello, {}".format(name))
> 
>   
> 
> # Positional / Indexed:
> 
> print("{1} {0}".format("World", "Hello"))
> 
>   
> 
> # Named Parameters:
> 
> print("{name} is {age}".format(name="Ash", age=25))
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 8. Useful String Operations
> 
>   
> 
> #### Concatenation (`+`) & Repetition (`*`)
> 
> ```python
> 
> a = "Hello"
> 
> b = "World"
> 
> c = a + " " + b     # 'Hello World'
> 
>   
> 
> "ha" * 3            # 'hahaha'
> 
> ```
> 
>   
> 
> #### Membership (`in` / `not in`)
> 
> ```python
> 
> "Py" in "Python"     # True
> 
> "x" not in "Python"  # True
> 
> ```
> 
>   
> 
> #### Lexicographical Comparison
> 
> Strings are compared alphabetically character by character based on Unicode code points:
> 
>   
> 
> ```python
> 
> "apple" == "apple"   # True
> 
> "apple" < "banana"   # True
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 9. Useful Built-in Functions
> 
> Functions that work directly with strings:
> 
>   
> 
> ```python
> 
> len(s)       # Returns string length
> 
> str(123)     # Converts object to string
> 
> ord("A")     # Character → Unicode integer (65)
> 
> chr(65)      # Unicode integer → Character ('A')
> 
> sorted(s)    # Returns sorted list of characters
> 
> reversed(s)  # Returns reverse iterator
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 10. Character Translation & Mapping
> 
> Use `str.maketrans()` and `translate()` for multi-character replacements or character deletions in a single pass:
> 
>   
> 
> ```python
> 
> # Replace 'a'->'1', 'b'->'2', 'c'->'3'
> 
> table = str.maketrans("abc", "123")
> 
> print("abc cab".translate(table))  # Output: '123 312'
> 
>   
> 
> # Remove specified characters (3rd argument):
> 
> table = str.maketrans("", "", "aeiou")
> 
> print("hello".translate(table))    # Output: 'hll'
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 11. Padding & Alignment Methods
> 
> ```python
> 
> "42".zfill(5)          # '00042' (Left pad with zeros)
> 
>   
> 
> "hello".center(10)     # '  hello   ' (Centered)
> 
> "hello".ljust(10)      # 'hello     ' (Left justified)
> 
> "hello".rjust(10)      # '     hello' (Right justified)
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 12. Partitioning
> 
> Splits a string on the first or last delimiter into a **3-tuple**: `(before, separator, after)`. Unlike `split()`, `partition()` always preserves the separator and returns exactly three elements.
> 
>   
> 
> ```python
> 
> text = "name=Ash"
> 
> text.partition("=")     # ('name', '=', 'Ash')
> 
>   
> 
> # Partition from the right:
> 
> "path/to/file.txt".rpartition("/")  # ('path/to', '/', 'file.txt')
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 13. Prefix & Suffix Removal (Python 3.9+)
> 
> Directly removes an exact matching prefix or suffix:
> 
>   
> 
> ```python
> 
> filename = "file.py"
> 
> filename.removeprefix("file")  # '.py'
> 
> filename.removesuffix(".py")   # 'file'
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 14. Immutability ⚠️
> 
> Strings **cannot** be modified in-place. Attempting index assignment raises a `TypeError`:
> 
>   
> 
> ```python
> 
> s = "hello"
> 
> # s[0] = "H"  # TypeError: 'str' object does not support item assignment
> 
>   
> 
> # Correct approach: Create a new string
> 
> s = "H" + s[1:]
> 
> # or
> 
> s = s.replace("h", "H")
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ## Part 2: Additional Methods & Detailed Distinctions
> 
>   
> 
> ### 1. Extended Character-Checking Methods
> 
> ```python
> 
> s.isascii()       # True if all characters are ASCII (0-127)
> 
> s.isprintable()   # True if all characters are printable (False for '\n', '\t')
> 
> s.isdecimal()     # True if strict decimal digits
> 
> s.isdigit()       # True if digits or superscripts
> 
> s.isnumeric()     # True if numeric characters (including fractions, Roman numerals)
> 
> ```
> 
>   
> 
> #### Comparison of Numeric Methods
> 
> | Method | Description | Base 10 (`'0123'`) | Fractions/Superscripts (`'2/3'`, `'2²'`) | Roman Numerals (`'ↁ'`) |
> 
> | :--- | :--- | :---: | :---: | :---: |
> 
> | `isdecimal()` | Strict decimal digits (0-9) | **True** | False | False |
> 
> | `isdigit()` | Digits + Superscripts/Subscripts | **True** | **True** | False |
> 
> | `isnumeric()` | Widest numeric check | **True** | **True** | **True** |
> 
>   
> 
> ---
> 
>   
> 
> ### 2. Method Variations & Advanced Parameters
> 
>   
> 
> #### `split()` and `rsplit()` with `maxsplit`
> 
> ```python
> 
> s = "a-b-c-d"
> 
> s.split("-", 2)   # ['a', 'b', 'c-d']
> 
> s.rsplit("-", 1)  # ['a-b-c', 'd']
> 
> ```
> 
>   
> 
> #### `splitlines(keepends=True)`
> 
> ```python
> 
> text = "one\n\ntwo"
> 
> text.splitlines()                 # ['one', '', 'two']
> 
> text.splitlines(keepends=True)   # ['one\n', '\n', 'two']
> 
> ```
> 
>   
> 
> #### `expandtabs(tabsize)`
> 
> Replaces tab characters (`\t`) with spaces:
> 
> ```python
> 
> s = "Name:\tAsh"
> 
> print(s.expandtabs(4))  # 'Name:   Ash'
> 
> ```
> 
>   
> 
> #### Searching with Slice Range (`start`, `end`)
> 
> ```python
> 
> s = "banana"
> 
> s.count("a", 0, 4)     # 2 (counts 'a' in "bana")
> 
> s.find("a", 2, 5)      # Index of 'a' in slice [2:5]
> 
> ```
> 
>   
> 
> #### `startswith()` and `endswith()` with Multiple Options
> 
> Pass a **tuple** of choices to check multiple extensions or prefixes at once:
> 
> ```python
> 
> filename = "image.png"
> 
> filename.endswith((".jpg", ".png", ".gif"))  # True
> 
> ```
> 
>   
> 
> #### Case-Insensitive Comparison with `casefold()`
> 
> `casefold()` is stronger than `lower()` and designed for Unicode-aware comparisons:
> 
> ```python
> 
> a = "HELLO"
> 
> b = "hello"
> 
> a.casefold() == b.casefold()  # True
> 
> ```
> 
>   
> 
> #### Encoding & Decoding (`str` ↔ `bytes`)
> 
> Converts strings to bytes for network streams, files, and APIs:
> 
> ```python
> 
> text = "Hello"
> 
> data = text.encode("utf-8")  # b'Hello'
> 
> decoded_text = data.decode("utf-8")  # 'Hello'
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### 3. Critical Method Comparisons & Pitfalls
> 
>   
> 
> #### `strip()` vs `replace()` ⚠️
> 
> * `strip()` removes target characters **only from the outer ends**:
> 
>   ```python
> 
>   "---hello---".strip("-")  # 'hello'
> 
>   "hello-world".strip("-")  # 'hello-world' (middle '-' is untouched)
> 
>   ```
> 
> * `replace()` removes or changes occurrences **everywhere** in the string:
> 
>   ```python
> 
>   "hello world".replace(" ", "")  # 'helloworld'
> 
>   ```
> 
>   
> 
> #### `removeprefix()` vs `lstrip()` ⚠️
> 
> * `removeprefix("www.")` removes the **exact prefix substring**:
> 
>   ```python
> 
>   "www.example.com".removeprefix("www.")  # 'example.com'
> 
>   ```
> 
> * `lstrip("w.")` treats characters as a **set** and removes any combination of 'w' or '.' from the left:
> 
>   ```python
> 
>   "www.example.com".lstrip("w.")  # 'example.com'
> 
>   ```
> 
>   
> 
> ---
> 
>   
> 
> ## Part 3: Dictionary Mapping & Complete Checklist
> 
>   
> 
> ### Formatting with `format_map()`
> 
> Allows string formatting directly using a dictionary object:
> 
>   
> 
> ```python
> 
> data = {"name": "Ash", "age": 25}
> 
> print("{name} is {age}".format_map(data))  # 'Ash is 25'
> 
> ```
> 
>   
> 
> ---
> 
>   
> 
> ### Master Method Checklist & Quick Reference
> 
>   
> 
> ```python
> 
> Case Conversion:   upper(), lower(), title(), capitalize(), swapcase(), casefold()
> 
> Searching:         find(), rfind(), index(), rindex(), count()
> 
> Checking Content:  startswith(), endswith(), isalpha(), isdigit(), isalnum(),
> 
>                    isdecimal(), isnumeric(), isidentifier(), islower(), isupper(),
> 
>                    isspace(), istitle(), isascii(), isprintable()
> 
> Modifying/Trimming:replace(), strip(), lstrip(), rstrip(), removeprefix(), removesuffix()
> 
> Splitting/Joining: split(), rsplit(), splitlines(), join(), partition(), rpartition()
> 
> Padding/Aligning:  zfill(), center(), ljust(), rjust(), expandtabs()
> 
> Formatting:        f"{val}", format(), format_map()
> 
> Translation:       maketrans(), translate()
> 
> Encoding/Decoding: encode(), decode()
> 
> Built-in Funcs:    len(), str(), ord(), chr(), sorted(), reversed()
> 
> ```
