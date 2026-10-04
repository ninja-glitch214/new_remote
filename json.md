
```
import json

with open("student1.json", "r") as file:
    data = json.load(file)

students = data["students"]

for student in students:
    print(student["sid"], student["name"], student["marks"])


for student in students:
    if student["marks"] > 80:
        print(student["name"], student["marks"])
        
      
top_student = max(students, key=lambda s: s["marks"])

print("Top Student:", top_student["name"])
print("Marks:", top_student["marks"])

students.append({"sid": 104, "name": "Sujata", "marks": 92})

data1={"students":students}
with open("student2.json", "w") as file:
    json.dump(data1, file, indent=4)
    
    
    

#to convert data from string to python object use loads, and dumpsfor student in students:
   # print(student["sid"], student["name"], student["marks"])
#convert python object to String use dumps


import json

json_string = '{"name": "Amit", "age": 21, "marks": 85}'

student = json.loads(json_string)

print(student["name"])
print(student["marks"])

```

================================================================================================================================

# JSON Handling in Python — Rapid Revision

**JSON (JavaScript Object Notation)** is a lightweight text format commonly used for **APIs, configuration files, and data exchange**.

Python provides the built-in **`json` module**.

```python
import json
```

## Python ↔ JSON

```text
Python                  JSON
--------------------------------
dict                    object
list / tuple             array
str                      string
int / float              number
True / False             true / false
None                     null
```

---

## 1. Python → JSON : `json.dumps()`

Converts a Python object into a **JSON string**.

```python
data = {
    "name": "Ash",
    "age": 25,
    "skills": ["Python", "SQL"]
}

json_data = json.dumps(data)

print(json_data)
```

### Pretty JSON

```python
print(json.dumps(data, indent=4))
```

Useful for readable output/debugging.

---

## 2. JSON → Python : `json.loads()`

Converts a **JSON string** into a Python object.

```python
json_data = '{"name": "Ash", "age": 25}'

data = json.loads(json_data)

print(data["name"])
```

```text
JSON string
    ↓ loads()
Python object
```

---

## 3. Write JSON to a File — `json.dump()`

`dump()` writes a Python object directly into a file.

```python
data = {
    "name": "Ash",
    "age": 25
}

with open("student.json", "w") as file:
    json.dump(data, file, indent=4)
```

```text
Python object
     ↓ dump()
 JSON file
```

---

## 4. Read JSON from a File — `json.load()`

Reads JSON from a file and converts it into a Python object.

```python
with open("student.json", "r") as file:
    data = json.load(file)

print(data["name"])
```

```text
JSON file
   ↓ load()
Python object
```

---

## `load` vs `loads`

```text
load()   → JSON FILE → Python object
loads()  → JSON STRING → Python object

dump()   → Python object → JSON FILE
dumps()  → Python object → JSON STRING
```

### Easy Memory Trick

```text
s = string

loads  → load string
dumps  → dump string

load   → file
dump   → file
```

---

## 5. Common API Usage

JSON is heavily used when working with APIs.

```python
import json

response_text = '{"id": 101, "name": "Ash"}'

data = json.loads(response_text)

print(data["id"])
print(data["name"])
```

Typical flow:

```text
API Response
     ↓
JSON
     ↓
json.loads()
     ↓
Python dict/list
     ↓
Process data
```

---

## 6. Handling JSON Errors

Invalid JSON can raise `JSONDecodeError`.

```python
try:
    data = json.loads('{"name": "Ash"')
except json.JSONDecodeError:
    print("Invalid JSON")
```

---

## ⭐ Rapid Revision

```text
import json

json.dumps(obj)  → Python → JSON string
json.loads(str)  → JSON string → Python

json.dump(obj, file)  → Python → JSON file
json.load(file)       → JSON file → Python

indent=4 → Pretty/readable JSON

JSON commonly used for:
→ APIs
→ Configuration
→ Data exchange
→ Storing structured data
```

**Most important:**
`loads/dumps` → **string**
`load/dump` → **file**

========================================================================
# JSON Handling in Python — Rapid Revision

**JSON (JavaScript Object Notation)** is a lightweight text format commonly used for **APIs, configuration files, and data exchange**.

```python
import json
```

## Python ↔ JSON Type Conversion

| Python           | JSON             |
| ---------------- | ---------------- |
| `dict`           | object           |
| `list` / `tuple` | array            |
| `str`            | string           |
| `int` / `float`  | number           |
| `True` / `False` | `true` / `false` |
| `None`           | `null`           |

### ⚠️ Type Conversion Caveats

JSON supports only a **limited set of data types**.

```python
data = {
    "name": "Ash",
    "age": 25,
    "skills": ("Python", "SQL")
}

print(json.dumps(data))
```

The tuple becomes a JSON array:

```text
Python tuple → JSON array
            → Python list when loaded back
```

So:

```python
json.loads(json.dumps((1, 2, 3)))
# [1, 2, 3]
```

**The original tuple type is not preserved.**

### Unsupported Types

Objects such as these cannot normally be serialized directly:

```python
set
bytes
datetime
custom objects
```

For example:

```python
data = {1, 2, 3}

json.dumps(data)
# TypeError: Object of type set is not JSON serializable
```

You may need to convert them first:

```python
data = list({1, 2, 3})

json.dumps(data)
```

### Dictionary Key Caveat

JSON object keys must be strings.

Python allows:

```python
data = {
    1: "one",
    2: "two"
}
```

But during JSON serialization, numeric keys are converted to strings:

```python
json_data = json.dumps(data)

print(json_data)
# {"1": "one", "2": "two"}
```

After loading:

```python
result = json.loads(json_data)

print(result)
# {'1': 'one', '2': 'two'}
```

So **dictionary key types may not be preserved**.

### Special Floating-Point Values

JSON's standard number representation does not include Python's:

```python
float("nan")
float("inf")
float("-inf")
```

Python's `json` module has special behavior for these by default, but they are **not standard JSON values** and may cause interoperability problems with strict JSON parsers.

---

## ⭐ Type Conversion Reminder

```text
Python              JSON
--------------------------------
dict       ───────→ object
list       ───────→ array
tuple      ───────→ array
str        ───────→ string
int/float  ───────→ number
True       ───────→ true
False      ───────→ false
None       ───────→ null
```

### ⚠️ Remember

**JSON conversion is not always lossless.**

* `tuple` → `list`
* Dictionary non-string keys → usually strings
* `set`, `bytes`, custom objects, etc. → not directly serializable
* Some Python-specific types need custom conversion

For custom objects, use techniques such as `default=` with `json.dumps()` or convert the object to a JSON-compatible `dict` first.
