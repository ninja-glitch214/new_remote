> [!TIP] Note
> # Python File Handling — Rapid Revision
> 
> ## 1. Open a File
> 
> ```python
> f = open("data.txt", "r")
> ```
> 
> ```python
> open(file, mode, encoding=None)
> ```
> 
> ### Common Modes
> 
> ```text
> "r"  → Read                 → file must exist
> "w"  → Write                → creates / overwrites
> "a"  → Append               → creates / adds at end
> "x"  → Create               → error if exists
> 
> "b"  → Binary
> "t"  → Text (default)
> "+"  → Read + Write
> ```
> 
> ```text
> "rb" → read binary
> "wb" → write binary
> "r+" → read + write
> "w+" → write + read (overwrites)
> "a+" → append + read
> ```
> 
> ---
> 
> # 2. Read Entire File
> 
> ```python
> with open("data.txt", "r", encoding="utf-8") as f:
>     data = f.read()
> 
> print(data)
> ```
> 
> ---
> 
> # 3. Read First N Characters
> 
> ```python
> with open("data.txt", encoding="utf-8") as f:
>     data = f.read(10)
> 
> print(data)
> ```
> 
> ---
> 
> # 4. Read One Line
> 
> ```python
> with open("data.txt", encoding="utf-8") as f:
>     line = f.readline()
> 
> print(line)
> ```
> 
> ---
> 
> # 5. Read All Lines
> 
> Returns a **list of lines**:
> 
> ```python
> with open("data.txt", encoding="utf-8") as f:
>     lines = f.readlines()
> 
> print(lines)
> ```
> 
> ---
> 
> # 6. Read File Line-by-Line ⭐
> 
> Memory-efficient and commonly used:
> 
> ```python
> with open("data.txt", encoding="utf-8") as f:
>     for line in f:
>         print(line.strip())
> ```
> 
> ```text
> strip() → removes leading/trailing whitespace,
>           including the newline
> ```
> 
> ---
> 
> # 7. Write to a File
> 
> `"w"` creates the file if it doesn't exist and **overwrites existing content**.
> 
> ```python
> with open("data.txt", "w", encoding="utf-8") as f:
>     f.write("Hello World")
> ```
> 
> ---
> 
> # 8. Write Multiple Lines
> 
> ```python
> lines = ["Apple\n", "Banana\n", "Mango\n"]
> 
> with open("fruits.txt", "w", encoding="utf-8") as f:
>     f.writelines(lines)
> ```
> 
> ⚠️ `writelines()` does **not automatically add `\n`**.
> 
> ---
> 
> # 9. Append to a File ⭐
> 
> Adds content to the end without deleting existing content.
> 
> ```python
> with open("data.txt", "a", encoding="utf-8") as f:
>     f.write("\nNew line")
> ```
> 
> ---
> 
> # 10. Read + Write
> 
> Using `"r+"`:
> 
> ```python
> with open("data.txt", "r+", encoding="utf-8") as f:
>     data = f.read()
>     f.write("\nNew content")
> ```
> 
> ```text
> r+ → file must already exist
> ```
> 
> ---
> 
> # 11. Binary Files
> 
> Useful for images, PDFs, audio, etc.
> 
> ### Read binary
> 
> ```python
> with open("image.jpg", "rb") as f:
>     data = f.read()
> ```
> 
> ### Write binary
> 
> ```python
> with open("copy.jpg", "wb") as f:
>     f.write(data)
> ```
> 
> ---
> 
> # 12. Copy a File
> 
> Simple binary copy:
> 
> ```python
> with open("source.jpg", "rb") as src:
>     data = src.read()
> 
> with open("copy.jpg", "wb") as dest:
>     dest.write(data)
> ```
> 
> For large files, process in chunks instead of loading everything into memory:
> 
> ```python
> with open("source.jpg", "rb") as src, \
>      open("copy.jpg", "wb") as dest:
> 
>     while chunk := src.read(8192):
>         dest.write(chunk)
> ```
> 
> ---
> 
> # 13. File Pointer — `tell()` / `seek()`
> 
> ```python
> with open("data.txt", encoding="utf-8") as f:
>     print(f.tell())     # current position
> 
>     print(f.read(5))
> 
>     f.seek(0)           # move to beginning
> 
>     print(f.read(5))
> ```
> 
> ```text
> tell() → current position
> seek() → move position
> ```
> 
> ---
> 
> # 14. Check if File Exists
> 
> Modern approach:
> 
> ```python
> from pathlib import Path
> 
> path = Path("data.txt")
> 
> if path.exists():
>     print("File exists")
> else:
>     print("File not found")
> ```
> 
> ---
> 
> # 15. Delete a File
> 
> ```python
> from pathlib import Path
> 
> path = Path("data.txt")
> 
> if path.exists():
>     path.unlink()
> ```
> 
> ---
> 
> # 16. Get File Information
> 
> ```python
> from pathlib import Path
> 
> path = Path("data.txt")
> 
> print(path.name)       # filename
> print(path.suffix)     # extension
> print(path.parent)     # parent directory
> print(path.exists())   # exists?
> print(path.stat().st_size)  # size in bytes
> ```
> 
> ---
> 
> # 17. Handle Common Errors
> 
> ```python
> try:
>     with open("data.txt", encoding="utf-8") as f:
>         data = f.read()
> 
> except FileNotFoundError:
>     print("File not found")
> 
> except PermissionError:
>     print("Permission denied")
> ```
> 
> ### Common Errors
> 
> ```text
> FileNotFoundError   → file/path doesn't exist
> PermissionError     → insufficient permission
> FileExistsError     → file already exists ("x")
> IsADirectoryError   → tried to open directory as file
> UnicodeDecodeError  → decoding problem
> UnicodeEncodeError  → encoding problem
> ValueError          → operation on closed file
> ```
> 
> ---
> 
> # 18. `pathlib` — Modern File Operations
> 
> ```python
> from pathlib import Path
> 
> path = Path("data.txt")
> 
> # Write
> path.write_text("Hello", encoding="utf-8")
> 
> # Read
> text = path.read_text(encoding="utf-8")
> 
> print(text)
> ```
> 
> Useful:
> 
> ```text
> exists()       → check existence
> read_text()    → read text
> write_text()   → write text
> read_bytes()   → read binary
> write_bytes()  → write binary
> unlink()       → delete file
> ```
> 
> ---
> 
> # ⭐ Most Common Snippets
> 
> ### Read
> 
> ```python
> with open("file.txt", encoding="utf-8") as f:
>     data = f.read()
> ```
> 
> ### Write
> 
> ```python
> with open("file.txt", "w", encoding="utf-8") as f:
>     f.write("Hello")
> ```
> 
> ### Append
> 
> ```python
> with open("file.txt", "a", encoding="utf-8") as f:
>     f.write("\nNew data")
> ```
> 
> ### Line-by-line
> 
> ```python
> with open("file.txt", encoding="utf-8") as f:
>     for line in f:
>         print(line.strip())
> ```
> 
> ### Binary
> 
> ```python
> with open("image.jpg", "rb") as f:
>     data = f.read()
> ```
> 
> ### Check existence
> 
> ```python
> from pathlib import Path
> 
> if Path("file.txt").exists():
>     print("Exists")
> ```
> 
> ### Delete
> 
> ```python
> Path("file.txt").unlink()
> ```
> 
> ---
> 
> # 🧠 One-Glance Memory
> 
> ```text
> with open() → safest/common way
> 
> r → read
> w → write/overwrite
> a → append
> x → create only
> b → binary
> + → read + write
> 
> read()       → entire content
> readline()   → one line
> readlines()  → list of lines
> write()      → write string
> writelines() → write multiple strings
> 
> tell()       → current pointer
> seek()       → move pointer
> 
> Path.exists() → check
> Path.unlink() → delete
> 
> with → automatically closes file
> ```
> 
> **Best practice:**
> 
> ```python
> with open("file.txt", "r", encoding="utf-8") as f:
>     data = f.read()
> ```
>
> ======================================================================================
>
 ```
> # file handling in



fh=open("employeeinfo.txt")
fh1=open("employeedata.txt","w")
for line in fh:
    print(line)
    lst=line.split(",")
    print(lst[0],lst[1])
    ln=":".join(lst)
    fh1.write(ln)
fh.close()
fh1.close()

try: 
    fh=open("employeeinfo111.txt")
    fh1=open("employeedata.txt","w")
    for line in fh:
        print(line)
        lst=line.split(",")
        print(lst[0],lst[1])
        if lst[3]=='Admin':
            ln=":".join(lst)
            fh1.write(ln)
    
except FileNotFoundError as e:
    print(e)
finally:
    fh.close()
    fh1.close()
    
    
with open("employeeinfo.txt") as fh:
    with open("empcopy.txt","w") as fh1:
        for line in fh:
            print(line)
            fh1.write(line)
    
    


```
