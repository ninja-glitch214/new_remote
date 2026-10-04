> [!TIP] Note
> # Python Regex (`re`) — Rapid Revision
> 
> **Regex (Regular Expression)** is a pattern-matching system used to **search, match, extract, validate, split, and replace text**.
> 
> ```python
> import re
> ```
> 
> ## 1. Basic Pattern Symbols
> 
> ```text
> .       → Any one character
> 
> [a-zA-Z] → Any one alphabet character
> [0-9]    → Any one digit
> [abc]    → a, b, or c
> [^abc]   → Anything except a, b, or c
> [a-z]    → Character range
> 
> ^       → Start of string
> $       → End of string
> 
> *       → 0 or more occurrences
> +       → 1 or more occurrences
> ?       → 0 or 1 occurrence
> {n}     → Exactly n occurrences
> {m,n}   → Minimum m, maximum n occurrences
> {m,}    → Minimum m occurrences
> 
> \d      → Digit [0-9]
> \D      → Non-digit
> \w      → Word character [A-Za-z0-9_]
> \W      → Non-word character
> \s      → Whitespace
> \S      → Non-whitespace
> \b      → Word boundary
> \B      → Non-word-boundary
> ```
> 
> ### `\b` and `\B`
> 
> `\b` and `\B` represent **positions**, not actual characters.
> 
> ```python
> text = "cat scatter"
> 
> re.findall(r"\bcat\b", text)
> # ['cat']
> ```
> 
> `\bcat\b` matches `cat` as a **complete word**, but not the `cat` inside `scatter`.
> 
> ---
> 
> ## 2. Character Sets and OR
> 
> Character sets can match one character from a specified set:
> 
> ```python
> re.findall(r"[aeiou]", "Hello World")
> ```
> 
> ```text
> [Oo]r → matches "Or" or "or"
> ```
> 
> You can also use `|` for alternatives:
> 
> ```python
> pattern = r"cat|dog"
> 
> print(re.findall(pattern, "cat dog bird"))
> # ['cat', 'dog']
> ```
> 
> ---
> 
> ## 3. Useful Pattern Examples
> 
> ```text
> [Oo]r       → "Or" or "or" anywhere
> 
> ^[Oo]r      → "Or" or "or" at beginning
> 
> [Oo]r$      → "Or" or "or" at end
> 
> \b[Oo]r\b   → complete word "Or" or "or"
> ```
> 
> For example:
> 
> ```text
> This is origami       → [Oo]r matches
> This is normal        → no match
> There is a tailor     → no match
> this or that          → matches "or"
> Or                    → matches "Or"
> There is a cat        → no match
> Origami is good       → matches "Or"
> ```
> 
> ---
> 
> ## 4. Greedy vs Non-Greedy Matching
> 
> `*`, `+`, and similar quantifiers are generally **greedy** — they try to consume as much as possible.
> 
> ```python
> text = "Something is there somewhere"
> 
> re.search(r"[Ss].*e", text)
> ```
> 
> `.*` → **greedy**, so it consumes as much as possible before the final possible `e`.
> 
> Use `?` after a quantifier for **non-greedy/lazy** matching:
> 
> ```python
> re.search(r"[Ss].*?e", text)
> ```
> 
> `.*?` → consumes as little as possible.
> 
> ```text
> .*  → greedy     → as much as possible
> .*? → non-greedy → as little as possible
> ```
> 
> ---
> 
> ## 5. Combining Patterns
> 
> Patterns can be combined to describe more complex structures.
> 
> ```python
> text = "Something is there somewhere"
> 
> re.findall(r"\w+\s\w+\s\w+", text)
> ```
> 
> ```text
> \w+ → one or more word characters
> \s  → whitespace
> ```
> 
> So:
> 
> ```text
> \w+\s\w+\s\w+
> ```
> 
> matches:
> 
> ```text
> Something is there
> ```
> 
> Another example:
> 
> ```python
> re.fullmatch(r"^\w+\s\w+\s\w+$", "This is line")
> ```
> 
> ---
> 
> ## 6. `re.search()`
> 
> Finds the **first match anywhere** in the string.
> 
> ```python
> text = "My age is 25"
> 
> match = re.search(r"\d+", text)
> 
> print(match.group())
> # 25
> ```
> 
> Returns a **match object** or `None`.
> 
> ---
> 
> ## 7. `re.match()`
> 
> Checks for a match **only at the beginning**.
> 
> ```python
> text = "Python is great"
> 
> match = re.match(r"Python", text)
> 
> print(match.group())
> # Python
> ```
> 
> Unlike `search()`, it does not search through the entire string.
> 
> ---
> 
> ## 8. `re.fullmatch()`
> 
> Requires the **entire string** to match the pattern.
> 
> ```python
> value = "12345"
> 
> if re.fullmatch(r"\d+", value):
>     print("Only digits")
> ```
> 
> Useful for **validation**.
> 
> ---
> 
> ## 9. `re.findall()`
> 
> Returns **all matches** as a list of strings.
> 
> ```python
> text = "I have 10 apples and 20 oranges"
> 
> numbers = re.findall(r"\d+", text)
> 
> print(numbers)
> # ['10', '20']
> ```
> 
> Very useful for **extracting data**.
> 
> ---
> 
> ## 10. `re.finditer()`
> 
> Returns an **iterator of match objects**.
> 
> ```python
> text = "Age 25, Score 90"
> 
> for match in re.finditer(r"\d+", text):
>     print(match.group(), match.start())
> ```
> 
> Useful when you need:
> 
> ```text
> match.group() → matched text
> match.start() → starting position
> match.end()   → ending position
> match.span()  → (start, end)
> ```
> 
> ---
> 
> ## 11. `re.sub()` — Replace
> 
> Replace matching text.
> 
> ```python
> text = "My phone is 123-456"
> 
> result = re.sub(r"\d", "X", text)
> 
> print(result)
> # My phone is XXX-XXX
> ```
> 
> ### `count`
> 
> ```python
> re.sub(pattern, replacement, string, count=0)
> ```
> 
> ```text
> count=0 → Replace ALL matches
> count=2 → Replace first 2 matches
> ```
> 
> Example:
> 
> ```python
> text = "cat cat cat"
> 
> result = re.sub(r"cat", "dog", text, count=2)
> 
> print(result)
> # dog dog cat
> ```
> 
> A replacement function can also be used:
> 
> ```python
> text = "10 20 30"
> 
> result = re.sub(
>     r"\d+",
>     lambda m: str(int(m.group()) * 2),
>     text
> )
> 
> print(result)
> # 20 40 60
> ```
> 
> ---
> 
> ## 12. `re.split()` — Split Using a Pattern
> 
> ```python
> text = "apple,banana;orange"
> 
> result = re.split(r"[,;]", text)
> 
> print(result)
> # ['apple', 'banana', 'orange']
> ```
> 
> Useful when multiple delimiters are possible.
> 
> ---
> 
> ## 13. Capturing Groups `()`
> 
> Groups allow you to **capture parts of a match**.
> 
> ```python
> text = "John - 25"
> 
> match = re.search(r"(\w+) - (\d+)", text)
> 
> print(match.group(1))
> # John
> 
> print(match.group(2))
> # 25
> ```
> 
> ```text
> group(0) → Entire match
> group(1) → First captured group
> group(2) → Second captured group
> ```
> 
> ---
> 
> ## 14. Named Groups
> 
> Give captured groups names:
> 
> ```python
> text = "John - 25"
> 
> match = re.search(
>     r"(?P<name>\w+) - (?P<age>\d+)",
>     text
> )
> 
> print(match.group("name"))
> print(match.group("age"))
> ```
> 
> Useful when patterns contain many groups.
> 
> ---
> 
> ## 15. Non-Capturing Groups `(?:...)`
> 
> Group without storing the match:
> 
> ```python
> pattern = r"(?:Mr|Mrs|Ms)\. \w+"
> ```
> 
> Useful when grouping is needed but you don't need the group's value.
> 
> ---
> 
> ## 16. Word Boundary `\b`
> 
> Matches a boundary between word/non-word characters.
> 
> ```python
> text = "cat scatter category"
> 
> print(re.findall(r"\bcat\b", text))
> # ['cat']
> ```
> 
> Useful when you need to match a **complete word**, not part of another word.
> 
> ---
> 
> ## 17. Regex Flags
> 
> Flags modify how regex behaves.
> 
> ### `re.IGNORECASE`
> 
> Ignores uppercase/lowercase differences:
> 
> ```python
> re.findall(
>     r"python",
>     "Python PYTHON python",
>     re.IGNORECASE
> )
> ```
> 
> ### `re.MULTILINE`
> 
> Makes `^` and `$` work with **each line**:
> 
> ```python
> re.findall(r"^Error", text, re.MULTILINE)
> ```
> 
> ### `re.DOTALL`
> 
> Makes `.` also match **newlines**:
> 
> ```python
> re.search(r"start.*end", text, re.DOTALL)
> ```
> 
> ### Combine flags
> 
> ```python
> re.search(
>     r"hello.*world",
>     text,
>     re.IGNORECASE | re.DOTALL
> )
> ```
> 
> ---
> 
> ## 18. `re.escape()`
> 
> Escapes special regex characters so text can be treated **literally**.
> 
> ```python
> text = "price: $10"
> 
> pattern = re.escape("$10")
> 
> print(re.search(pattern, text))
> ```
> 
> Useful when constructing regex patterns from **user-provided or dynamic text**.
> 
> ---
> 
> ## 19. Compile a Regex
> 
> For a pattern used repeatedly:
> 
> ```python
> pattern = re.compile(r"\d+")
> 
> print(pattern.findall("10 apples, 20 oranges"))
> print(pattern.findall("30 bananas"))
> ```
> 
> Useful when the **same regex is reused multiple times**.
> 
> ---
> 
> # Common Practical Uses
> 
> Regex is commonly used for:
> 
> ```text
> ✓ Find numbers
> ✓ Extract emails
> ✓ Extract URLs
> ✓ Validate formats
> ✓ Find specific words
> ✓ Search text patterns
> ✓ Replace text
> ✓ Remove unwanted characters
> ✓ Split text using complex delimiters
> ✓ Extract dates
> ✓ Extract phone numbers
> ✓ Extract IDs/codes
> ✓ Parse semi-structured text
> ✓ Clean text
> ✓ Find repeated patterns
> ✓ Validate usernames/password formats
> ```
> 
> ### Example — Extract Emails
> 
> ```python
> text = "Contact a@test.com or b@example.com"
> 
> emails = re.findall(
>     r"[\w.-]+@[\w.-]+\.\w+",
>     text
> )
> 
> print(emails)
> ```
> 
> ### Example — Extract Dates
> 
> ```python
> text = "Meeting: 27-09-2026"
> 
> date = re.search(
>     r"\d{2}-\d{2}-\d{4}",
>     text
> )
> 
> print(date.group())
> ```
> 
> ### Example — Remove Non-Digits
> 
> ```python
> phone = "987-654-3210"
> 
> clean = re.sub(r"\D", "", phone)
> 
> print(clean)
> # 9876543210
> ```
> 
> ---
> 
> # Quick Revision
> 
> ```text
> re.search()      → First match anywhere
> re.match()       → Match at beginning
> re.fullmatch()   → Entire string must match
> re.findall()     → All matches as list
> re.finditer()    → All matches as match objects
> re.sub()         → Replace matches
> re.split()       → Split using pattern
> re.compile()     → Reusable compiled pattern
> re.escape()      → Escape literal text
> ```
> 
> ### Match Object
> 
> ```text
> .group()         → Matched text
> .start()         → Start position
> .end()           → End position
> .span()          → (start, end)
> .group(n)        → Captured group
> .group("name")   → Named captured group
> ```
> 
> ### ⭐ Most Important Patterns
> 
> ```text
> .      → Any one character
> \d     → Digit
> \D     → Non-digit
> \w     → Word character
> \W     → Non-word character
> \s     → Whitespace
> \S     → Non-whitespace
> \b     → Word boundary
> \B     → Non-word-boundary
> 
> +      → One or more
> *      → Zero or more
> ?      → Zero or one
> {n}    → Exactly n
> {m,n}  → m to n
> {m,}   → Minimum m
> 
> []     → Character set
> [^]    → Exclude characters
> ()     → Capturing group
> (?:)   → Non-capturing group
> |      → OR
> ^      → Start
> $      → End
> ```
> 
> **Core idea:**
> 
> ```text
> Pattern → Search/Match → Extract / Replace / Split
> ```
> 
> Regex is especially useful when the text has **patterns**, rather than when you're simply looking for an exact string.
