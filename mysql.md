> [!NOTE]
> # Python + MySQL — Foundation + Implementation
> 
> ## PART 1 — What Can Python Do With MySQL?
> 
> Python can communicate with a MySQL database to:
> 
> ```text
> Python Program
>       ↓
> MySQL Connector / Driver
>       ↓
> MySQL Database
> ```
> 
> Python can perform the main database operations:
> 
> ```text
> CREATE  → create database/table/records
> READ    → SELECT data
> UPDATE  → modify existing data
> DELETE  → remove data
> ```
> 
> These are commonly called **CRUD operations**:
> 
> ```text
> C → Create / INSERT
> R → Read   / SELECT
> U → Update / UPDATE
> D → Delete / DELETE
> ```
> 
> Python can also:
> 
> - Connect/disconnect from MySQL
>     
> - Execute SQL queries
>     
> - Pass values safely into queries
>     
> - Fetch query results
>     
> - Insert multiple records
>     
> - Commit changes
>     
> - Roll back changes
>     
> - Call stored procedures
>     
> - Handle database errors
>     
> - Check how many rows were affected
>     
> - Build menu-driven database applications
>     
> 
> ---
> 
> # PART 2 — Basic Architecture
> 
> The basic interaction looks like:
> 
> ```text
>               Python Program
>                     │
>                     ▼
>              MySQL Driver
>               (PyMySQL)
>                     │
>                     ▼
>              Connection
>                     │
>                     ▼
>                Cursor
>                     │
>              execute SQL
>                     │
>                     ▼
>              MySQL Server
>                     │
>                     ▼
>                 Database
> ```
> 
> Two important objects:
> 
> ### `connection`
> 
> Represents the connection between Python and MySQL.
> 
> ```python
> conn = zz.connect(...)
> ```
> 
> Used for:
> 
> ```text
> commit()
> rollback()
> close()
> ```
> 
> ### `cursor`
> 
> Used to execute SQL and retrieve results.
> 
> ```python
> cursor = conn.cursor()
> ```
> 
> Used for:
> 
> ```text
> execute()
> executemany()
> fetchone()
> fetchall()
> callproc()
> rowcount
> ```
> 
> ### Easy way to remember
> 
> ```text
> Connection → controls the database connection/transaction
> 
> Cursor     → executes SQL and handles query results
> ```
> 
> ---
> 
> # PART 3 — Connecting Python to MySQL
> 
> Your code uses:
> 
> ```python
> import pymysql as zz
> ```
> 
> `pymysql` is the Python driver that allows Python to communicate with MySQL.
> 
> `as zz` is simply an alias.
> 
> Therefore:
> 
> ```python
> zz.connect()
> ```
> 
> means:
> 
> ```python
> pymysql.connect()
> ```
> 
> ---
> 
> ## Connection
> 
> Your code:
> 
> ```python
> conn = zz.connect(
>     host="localhost",
>     user="root_user",
>     password="root123",
>     database="actsaug26"
> )
> ```
> 
> Meaning:
> 
> ```text
> host     → MySQL server location
> user     → MySQL username
> password → MySQL password
> database → database to use
> ```
> 
> If successful:
> 
> ```python
> conn
> ```
> 
> contains the database connection object.
> 
> Then:
> 
> ```python
> cursor = conn.cursor()
> ```
> 
> creates a cursor.
> 
> ---
> 
> # PART 4 — Executing SQL
> 
> SQL is sent to MySQL through the cursor.
> 
> ```python
> sql = "SELECT * FROM student"
> 
> cursor.execute(sql)
> ```
> 
> Think:
> 
> ```text
> Python
>   │
>   │ execute(SQL)
>   ▼
> Cursor
>   │
>   ▼
> MySQL
>   │
>   ▼
> Execute query
> ```
> 
> The cursor does **not mean that the result is simply "stored in RAM."**
> 
> For a `SELECT`, the database driver/cursor manages the result set, and methods such as `fetchone()` / `fetchall()` retrieve rows from it.
> 
> ---
> 
> # PART 5 — Reading Data
> 
> Your function:
> 
> ```python
> def displayAllStudents():
>     sql = "select * from student;"
>     cursor.execute(sql)
> 
>     for row in cursor.fetchall():
>         print(row)
> ```
> 
> The flow is:
> 
> ```text
> SELECT
>   ↓
> cursor.execute()
>   ↓
> MySQL executes query
>   ↓
> fetchall()
>   ↓
> Python receives rows
> ```
> 
> Suppose MySQL contains:
> 
> ```text
> 1   Rohit    9876543210   20
> 2   Sameer   9876543211   21
> ```
> 
> Then:
> 
> ```python
> cursor.fetchall()
> ```
> 
> returns something like:
> 
> ```python
> (
>     (1, "Rohit", "9876543210", 20),
>     (2, "Sameer", "9876543211", 21)
> )
> ```
> 
> Each `row` represents one record.
> 
> ```python
> row[0] → SID
> row[1] → Name
> row[2] → Mobile
> row[3] → Age
> ```
> 
> ---
> 
> # PART 6 — `fetchone()`, `fetchall()`, `fetchmany()`
> 
> ### `fetchone()`
> 
> Gets one row:
> 
> ```python
> student = cursor.fetchone()
> ```
> 
> Your search function uses this:
> 
> ```python
> student = cursor.fetchone()
> ```
> 
> ### `fetchall()`
> 
> Gets all remaining rows:
> 
> ```python
> students = cursor.fetchall()
> ```
> 
> ### `fetchmany(n)`
> 
> Gets up to `n` rows:
> 
> ```python
> students = cursor.fetchmany(5)
> ```
> 
> Remember:
> 
> ```text
> fetchone()   → one row
> fetchmany(n) → n rows
> fetchall()   → all remaining rows
> ```
> 
> ---
> 
> # PART 7 — INSERT
> 
> Your `addnewstudent()` function:
> 
> ```python
> sql = "insert into student values (%s,%s,%s,%s)"
> 
> cursor.execute(
>     sql,
>     (sid, sname, mobile, age)
> )
> 
> conn.commit()
> ```
> 
> This performs:
> 
> ```text
> Python values
>      ↓
> Parameterized SQL
>      ↓
> cursor.execute()
>      ↓
> MySQL INSERT
>      ↓
> commit()
>      ↓
> Changes permanently saved
> ```
> 
> ---
> 
> # PART 8 — Why `%s`?
> 
> This:
> 
> ```python
> sql = "INSERT INTO student VALUES (%s,%s,%s,%s)"
> ```
> 
> does **not** mean Python `%` string formatting.
> 
> These `%s` are placeholders understood by the database driver.
> 
> Values are supplied separately:
> 
> ```python
> cursor.execute(
>     sql,
>     (sid, sname, mobile, age)
> )
> ```
> 
> ### Important
> 
> Prefer:
> 
> ```python
> cursor.execute(
>     "SELECT * FROM student WHERE sid=%s",
>     (sid,)
> )
> ```
> 
> instead of building SQL manually:
> 
> ```python
> # ❌ Don't do this
> sql = "SELECT * FROM student WHERE sid=" + str(sid)
> ```
> 
> Parameterized queries help prevent **SQL injection** and correctly handle values.
> 
> ---
> 
> # PART 9 — Why `commit()`?
> 
> Operations such as:
> 
> ```text
> INSERT
> UPDATE
> DELETE
> ```
> 
> modify the database.
> 
> After executing them:
> 
> ```python
> conn.commit()
> ```
> 
> saves the transaction.
> 
> Think:
> 
> ```text
> execute()
>    ↓
> Change made in transaction
>    ↓
> commit()
>    ↓
> Change saved
> ```
> 
> Without committing, the modification may not be persisted.
> 
> ---
> 
> # PART 10 — Searching by ID
> 
> Your code:
> 
> ```python
> def SearchbyID(sid):
>     sql = "select * from student where sid=%s;"
> 
>     cursor.execute(sql, (sid,))
> 
>     student = cursor.fetchone()
> ```
> 
> The important part is:
> 
> ```python
> cursor.execute(sql, (sid,))
> ```
> 
> The `(sid,)` is a **one-element tuple**.
> 
> Then:
> 
> ```python
> student = cursor.fetchone()
> ```
> 
> gets the matching record.
> 
> If a student exists:
> 
> ```python
> if student:
> ```
> 
> otherwise:
> 
> ```python
> else:
>     print("Error:Student ID not found")
> ```
> 
> Flow:
> 
> ```text
> sid
>  ↓
> SELECT ... WHERE sid=%s
>  ↓
> execute()
>  ↓
> fetchone()
>  ↓
> Student found / not found
> ```
> 
> ---
> 
> # PART 11 — UPDATE
> 
> Your function:
> 
> ```python
> def updatestudent(sid, age, mobile):
> 
>     sql = "update student set mobile=%s,age=%s where sid=%s"
> 
>     cursor.execute(sql, (mobile, age, sid))
> 
>     if cursor.rowcount == 0:
>         print("Error:student ID not found.")
>     else:
>         conn.commit()
>         print("student updated successfully.")
> ```
> 
> The SQL:
> 
> ```sql
> UPDATE student
> SET mobile=%s, age=%s
> WHERE sid=%s
> ```
> 
> means:
> 
> ```text
> Find student
>     ↓
> WHERE sid
>     ↓
> Change mobile and age
> ```
> 
> ---
> 
> # PART 12 — `rowcount`
> 
> After executing:
> 
> ```python
> cursor.execute(sql, ...)
> ```
> 
> you can check:
> 
> ```python
> cursor.rowcount
> ```
> 
> It tells you how many rows were affected/reported by the operation.
> 
> Your code uses:
> 
> ```python
> if cursor.rowcount == 0:
> ```
> 
> to detect that no matching student was updated/deleted.
> 
> This is useful for:
> 
> ```text
> UPDATE
> DELETE
> ```
> 
> ---
> 
> # PART 13 — DELETE
> 
> Your function:
> 
> ```python
> def deletebyID(sid):
> 
>     sql = "delete from student where sid=%s"
> 
>     cursor.execute(sql, (sid,))
> 
>     if cursor.rowcount == 0:
>         print("Error:student ID not found.")
>     else:
>         conn.commit()
>         print("student deleted successfully.")
> ```
> 
> Flow:
> 
> ```text
> sid
>  ↓
> DELETE ... WHERE sid=%s
>  ↓
> execute()
>  ↓
> rowcount
>  ↓
> commit()
> ```
> 
> ---
> 
> # PART 14 — Transactions and `rollback()`
> 
> Suppose multiple database operations must succeed together.
> 
> ```python
> try:
>     cursor.execute(...)
>     cursor.execute(...)
> 
>     conn.commit()
> 
> except:
>     conn.rollback()
> ```
> 
> Conceptually:
> 
> ```text
> Operation 1
>     ↓
> Operation 2
>     ↓
> Everything successful?
>     ↓
>    YES ──→ commit()
>     │
>    NO
>     ↓
> rollback()
> ```
> 
> `rollback()` cancels uncommitted changes in the current transaction.
> 
> ---
> 
> # PART 15 — Calling a Stored Procedure
> 
> Your program contains:
> 
> ```python
> result = cursor.callproc(
>     "getsstudentcount",
>     (age, 0)
> )
> ```
> 
> This calls a MySQL **stored procedure**.
> 
> The procedure appears to accept:
> 
> ```text
> age
> output parameter
> ```
> 
> and returns the updated parameter values through `result`.
> 
> Conceptually:
> 
> ```text
> Python
>   ↓
> callproc()
>   ↓
> MySQL stored procedure
>   ↓
> result
> ```
> 
> For example:
> 
> ```python
> result = cursor.callproc(
>     "getsstudentcount",
>     (age, 0)
> )
> 
> print(result)
> ```
> 
> If the second parameter is an OUT parameter, its returned value can be read from the returned sequence according to the driver/procedure setup.
> 
> Your original line:
> 
> ```python
> print("number of students abive {age}", result[1])
> ```
> 
> would **not interpolate `age`** because it isn't an f-string.
> 
> Use:
> 
> ```python
> print(f"number of students above {age}: {result[1]}")
> ```
> 
> ---
> 
> # PART 16 — Exception Handling
> 
> Your program handles database errors:
> 
> ```python
> try:
>     conn = zz.connect(...)
> 
> except zz.MySQLError as e:
>     print("Database connection failed:", e)
> ```
> 
> `MySQLError` is the general PyMySQL database exception family.
> 
> Your INSERT also handles:
> 
> ```python
> except zz.IntegrityError as e:
> ```
> 
> This can occur when a database integrity constraint is violated, such as a duplicate value for a unique/primary-key column.
> 
> Your code checks:
> 
> ```python
> if e.args[0] == 1062:
> ```
> 
> which corresponds to MySQL's duplicate-entry error.
> 
> ---
> 
> # PART 17 — The Complete Program Architecture
> 
> Your mam's program is structured like this:
> 
> ```text
>                     Student Management System
>                               │
>                     ┌─────────┴─────────┐
>                     │                   │
>               MySQL Connection       Menu
>                     │                   │
>                  Cursor                │
>                     │                   │
>         ┌───────────┼────────────┐      │
>         ↓           ↓            ↓      ↓
>       SELECT      INSERT       UPDATE   DELETE
>         │           │            │       │
>      fetch()     commit()     commit() commit()
> ```
> 
> The menu decides which database function to call.
> 
> ```python
> match choice:
> 
>     case 1:
>         addnewstudent()
> 
>     case 2:
>         displayAllStudents()
> 
>     case 3:
>         SearchbyID(sid)
> 
>     case 5:
>         updatestudent(sid, age, mobile)
> 
>     case 6:
>         deletebyID(sid)
> ```
> 
> So the menu is simply the **application layer**, while the functions perform the database operations.
> 
> ---
> 
> # PART 18 — Mapping Your Code to CRUD
> 
> |Function|SQL|Operation|
> |---|---|---|
> |`addnewstudent()`|`INSERT`|Create|
> |`displayAllStudents()`|`SELECT`|Read|
> |`SearchbyID()`|`SELECT ... WHERE`|Read|
> |`updatestudent()`|`UPDATE`|Update|
> |`deletebyID()`|`DELETE`|Delete|
> |`getStudentCountByAge()`|Stored Procedure|Database operation|
> 
> Therefore the whole application is essentially:
> 
> ```text
> Student Management
>        │
>        ├── CREATE → addnewstudent()
>        ├── READ   → displayAllStudents()
>        ├── READ   → SearchbyID()
>        ├── UPDATE → updatestudent()
>        ├── DELETE → deletebyID()
>        └── PROC   → getStudentCountByAge()
> ```
> 
> ---
> 
> # PART 19 — Complete Database Operation Pattern
> 
> Almost every Python + MySQL program follows this basic pattern:
> 
> ```python
> import pymysql
> 
> # 1. Connect
> conn = pymysql.connect(
>     host="localhost",
>     user="root",
>     password="password",
>     database="school"
> )
> 
> # 2. Create cursor
> cursor = conn.cursor()
> 
> # 3. Prepare SQL
> sql = "SELECT * FROM student WHERE sid=%s"
> 
> # 4. Execute
> cursor.execute(sql, (1,))
> 
> # 5. Fetch result
> student = cursor.fetchone()
> 
> print(student)
> 
> # 6. Close
> cursor.close()
> conn.close()
> ```
> 
> For modification:
> 
> ```python
> cursor.execute(
>     "UPDATE student SET age=%s WHERE sid=%s",
>     (21, 1)
> )
> 
> conn.commit()
> ```
> 
> ---
> 
> # ⭐ ONE-GLANCE FOUNDATION
> 
> ```text
> PyMySQL
>    ↓
> connect()
>    ↓
> connection
>    ↓
> cursor()
>    ↓
> cursor
>    ↓
> execute(SQL, values)
>    ↓
>  ┌──────────────┐
>  │ SELECT       │ → fetchone/fetchall
>  │ INSERT       │ → commit
>  │ UPDATE       │ → commit
>  │ DELETE       │ → commit
>  └──────────────┘
>    ↓
> rollback() if transaction fails
>    ↓
> close()
> ```
> 
> ### Essential Methods
> 
> ```text
> connect()       → connect to MySQL
> cursor()        → create cursor
> execute()       → execute SQL
> executemany()   → execute for multiple values
> fetchone()      → get one row
> fetchmany()     → get multiple rows
> fetchall()      → get all rows
> rowcount        → affected/reported rows
> callproc()      → call stored procedure
> commit()        → save transaction
> rollback()      → undo uncommitted changes
> close()         → close cursor/connection
> ```
> 
> ### Core Mental Model
> 
> ```text
> CONNECTION
> → "Am I connected to MySQL?"
> 
> CURSOR
> → "Execute SQL and handle its results."
> 
> SQL
> → "What do I want MySQL to do?"
> 
> FETCH
> → "Give me the SELECT result."
> 
> COMMIT
> → "Save my INSERT/UPDATE/DELETE."
> 
> ROLLBACK
> → "Undo uncommitted changes."
> 
> CLOSE
> → "I'm finished with the database."
> ```
>
> # Database Connection
>
> # mysql connection 

import pymysql as zz

def displayAllStudents():
    sql = "select * from student;"
    cursor.execute(sql)
    print("\n students details")
    print("-" * 61)
    print(f"{'SID':<10}{'NAME':<20}{'Mobile':<15}{'AGE':<10}")
    print("-" * 61)
    for row in cursor.fetchall():
        print(f"{row[0]:<10}{row[1]:<20}{row[2]:<15}{row[3]:<10}")

def addnewstudent():
    try:
        sid = int(input("enter sid "))
        sname = input("enter name ")
        mobile = input("enter mobile ")
        age = int(input("enter age "))
        sql = "insert into student values (%s,%s,%s,%s)"
        cursor.execute(sql, (sid, sname, mobile, age))
        conn.commit()
        print("Student added successfully!")
        return True
    except ValueError as e:
        print("id and age has to be numeric", e)
    except zz.IntegrityError as e:
        if e.args[0] == 1062:
            print("Error: Student ID or Mobile Number is same")
        else:
            print("Database integrity error:", e)

def SearchbyID(sid):
    sql = "select * from student where sid=%s;"
    cursor.execute(sql, (sid,))
    student = cursor.fetchone()
    if student:    
        print("\n Student found")
        print("SID :", student[0])
        print("Name :", student[1])
        print("Mobile :", student[2])
        print("Age :", student[3])
    else:
        print("Error: Student ID not found")

def SearchbyName(name):
    sql = "select * from student where name LIKE %s;"
    cursor.execute(sql, (f"%{name}%",))
    students = cursor.fetchall()
    if students:
        print("\n Student(s) found")
        print("-" * 61)
        print(f"{'SID':<10}{'NAME':<20}{'Mobile':<15}{'AGE':<10}")
        print("-" * 61)
        for row in students:
            print(f"{row[0]:<10}{row[1]:<20}{row[2]:<15}{row[3]:<10}")
    else:
        print("Error: No student found with that name")

def updatestudent(sid, age, mobile):
    sql = "update student set mobile=%s, age=%s where sid=%s"
    cursor.execute(sql, (mobile, age, sid))
   
    if cursor.rowcount == 0:
        print("Error: student ID not found.")
    else:
        conn.commit()
        print("student updated successfully.")

def deletebyID(sid):
    sql = "delete from student where sid=%s"
    cursor.execute(sql, (sid,))
    
    if cursor.rowcount == 0:
        print("Error: student ID not found.")
    else:
        conn.commit()
        print("student deleted successfully.")

# Fixed missing colon in function definition
def getStudentCountByAge(age):
    result = cursor.callproc("getsstudentcount", (age, 0))
    # Fixed f-string missing 'f' prefix
    print(f"number of students above {age}:", result[1])

###########################################################################
conn = None
try:    
    conn = zz.connect(host="localhost", user="root_user", password="root123", database="actsaug26")
    
    if conn is not None:
        print("connection is successful")
        cursor = conn.cursor()

        choice = -1

        while choice != 8:
            try:
                choice = int(input("""
                             1.add student
                             2.display student
                             3.search by id
                             4.search by name
                             5.update student 
                             6.delete student 
                             7.find student count
                             8.exit
                             Enter choice: """))
            except ValueError:
                print("Please enter a valid numeric choice.")
                continue

            match choice:
                case 1:
                    addnewstudent()
                case 2:
                    displayAllStudents()
                case 3:
                    sid = int(input("Enter id to search: "))
                    SearchbyID(sid)
                case 4:
                    name = input("Enter name to search: ")
                    SearchbyName(name)
                case 5:
                    sid = int(input("Enter id to update: "))
                    age = int(input("Enter new age: "))
                    mobile = input("Enter mobile: ")
                    updatestudent(sid, age, mobile)    
                case 6:
                    sid = int(input("Enter id to delete: "))
                    deletebyID(sid)
                case 7:
                    age = int(input("Enter the age threshold: "))
                    getStudentCountByAge(age)
                case 8:
                    print("Thank you for visiting..........")
                case _:
                    print("Wrong choice")

except zz.MySQLError as e:
    print("Database connection/query failed:", e)

finally:
    if conn:
        conn.close()


# Procedure

DELIMITER //

CREATE PROCEDURE getsstudentcount(
    IN min_age INT,
    OUT student_count INT
)
BEGIN
    SELECT COUNT(*) 
    INTO student_count
    FROM student
    WHERE age > min_age;
END //

DELIMITER ;
