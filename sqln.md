# MySQL Advanced Development Reference Guide

A comprehensive quick-reference guide for advanced MySQL features including Window Functions, Stored Procedures, Stored Functions, Cursors, Error & Transaction Handling, and Triggers (MySQL 5.0+ and 8.0+).

---

## 1. MySQL Window Functions (MySQL 8.0+)

### Core Syntax Overview
```sql
function_name([expr]) OVER (
    [PARTITION BY partition_column]
    [ORDER BY sort_column [ASC|DESC]]
    [frame_clause]
)
```
* **PARTITION BY**: Divides the result set into groups (resets the window per group).
* **ORDER BY**: Defines row order within each partition.
* **frame_clause**: Restricts rows within the partition relative to the current row (e.g., `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW`).

---

### Function Reference
| Function Category | Function | Description |
| :--- | :--- | :--- |
| **Ranking** | `ROW_NUMBER()` | Assigns unique sequential integers (1, 2, 3, 4). |
| | `RANK()` | Assigns rank with gaps for ties (1, 2, 2, 4). |
| | `DENSE_RANK()` | Assigns rank without gaps for ties (1, 2, 2, 3). |
| | `NTILE(N)` | Divides partition into N equal groups. |
| **Value / Analytic** | `LAG(col, offset, default)` | Accesses data from a row *before* the current row. |
| | `LEAD(col, offset, default)` | Accesses data from a row *after* the current row. |
| | `FIRST_VALUE(col)` | Returns first value in the window. |
| | `LAST_VALUE(col)` | Returns last value in the window. |
| | `NTH_VALUE(col, N)` | Returns N-th value in the window. |
| **Aggregate** | `SUM()`, `AVG()`, `COUNT()`, `MIN()`, `MAX()` | Computes standard aggregates over a window without collapsing rows. |

---

### Key Practical Examples

**Sample Table (`employees`):**
| id | name | department | salary |
| :--- | :--- | :--- | :--- |
| 1 | Alice | IT | 9000 |
| 2 | Bob | IT | 9000 |
| 3 | Charlie | IT | 7000 |
| 4 | David | HR | 6000 |
| 5 | Eve | HR | 5000 |

#### 1. Difference Between Ranking Functions (`ROW_NUMBER`, `RANK`, `DENSE_RANK`)
```sql
SELECT 
    name, 
    department, 
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
    RANK()       OVER (PARTITION BY department ORDER BY salary DESC) AS rnk,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rnk
FROM employees;
```
**Output for IT Department:**
* Alice ($9000): `row_num = 1`, `rnk = 1`, `dense_rnk = 1`
* Bob ($9000): `row_num = 2`, `rnk = 1`, `dense_rnk = 1`
* Charlie ($7000): `row_num = 3`, `rnk = 3`, `dense_rnk = 2`

#### 2. Accessing Previous Row Data (`LAG`)
Calculate the salary difference from the previously highest-paid employee in the department:
```sql
SELECT 
    name, 
    department, 
    salary,
    LAG(salary, 1, 0) OVER (PARTITION BY department ORDER BY salary DESC) AS prev_salary,
    salary - LAG(salary, 1, salary) OVER (PARTITION BY department ORDER BY salary DESC) AS salary_diff
FROM employees;
```

#### 3. Running Total (Cumulative Sum)
Compute a cumulative sum of salaries within each department:
```sql
SELECT 
    id, 
    name, 
    department, 
    salary,
    SUM(salary) OVER (
        PARTITION BY department 
        ORDER BY id
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM employees;
```

#### 4. Top-N Rows Per Group Pattern (Using CTE)
Get the highest-paid employee per department:
```sql
WITH RankedEmployees AS (
    SELECT 
        name, 
        department, 
        salary,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT department, name, salary
FROM RankedEmployees
WHERE rn = 1;
```

#### 5. Reusing Named Windows (`WINDOW` Clause)
Avoid repetition when multiple functions share the same window spec:
```sql
SELECT 
    name, 
    department, 
    salary,
    AVG(salary) OVER w AS dept_avg,
    MAX(salary) OVER w AS dept_max
FROM employees
WINDOW w AS (PARTITION BY department);
```

---

### Important Rules & Gotchas
1. **Query Execution Order:** Window functions run in the `SELECT` phase, which occurs **after** `WHERE`, `GROUP BY`, and `HAVING` clauses.
2. **No Direct Filtering:** You **cannot** use window functions directly in a `WHERE` clause (e.g., `WHERE ROW_NUMBER() = 1` is invalid). Use a Common Table Expression (CTE) or subquery instead.
3. **Default Frame:** When `ORDER BY` is specified inside `OVER()`, the default frame is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`.

---

## 2. Stored Procedures, Functions, and Cursors

### Core Distinctions: Procedure vs. Function
| Feature | Stored Procedure (`PROCEDURE`) | Stored Function (`FUNCTION`) |
| :--- | :--- | :--- |
| **Primary Purpose** | Execute business logic, dynamic actions, updates. | Calculate and return a single scalar value. |
| **Invocation** | Called using `CALL procedure_name()`. | Called inline within SQL statements (e.g., `SELECT fn()`). |
| **Return Value** | No direct `RETURN`. Uses `INOUT` / `OUT` parameters. | **Must** return a single value via `RETURN datatype`. |
| **SQL Statements** | Allows `INSERT`, `UPDATE`, `DELETE`, `SELECT`, DDL, etc. | Strictly restricted from modifying tables or running DDL. |

---

### Stored Procedures
Procedures accept parameters (`IN`, `OUT`, `INOUT`) and perform multi-step database operations.

#### Syntax Template
```sql
DELIMITER //

CREATE PROCEDURE procedure_name(
    IN p_input_var DATATYPE,
    OUT p_output_var DATATYPE,
    INOUT p_inout_var DATATYPE
)
BEGIN
    -- Declare local variables first
    DECLARE v_local_var DATATYPE DEFAULT initial_value;

    -- Body logic
    IF p_input_var > 100 THEN
        SET p_output_var = p_input_var * 2;
    ELSE
        SET p_output_var = p_input_var;
    END IF;
END //

DELIMITER ;
```

#### Complete Practical Example
Update an employee's salary and return the updated value:
```sql
DELIMITER //

CREATE PROCEDURE ApplyRaise(
    IN p_emp_id INT,
    IN p_percent DECIMAL(5,2),
    OUT p_new_salary DECIMAL(10,2)
)
BEGIN
    UPDATE employees 
    SET salary = salary + (salary * (p_percent / 100))
    WHERE id = p_emp_id;

    SELECT salary INTO p_new_salary
    FROM employees
    WHERE id = p_emp_id;
END //

DELIMITER ;

-- Execution:
CALL ApplyRaise(1, 10.00, @updated_sal);
SELECT @updated_sal AS NewSalary;
```

---

### Stored Functions
Functions accept input parameters and **must** return a scalar value.

#### Syntax Template
```sql
DELIMITER //

CREATE FUNCTION function_name(
    p_param1 DATATYPE
)
RETURNS DATATYPE
DETERMINISTIC -- Options: DETERMINISTIC | NOT DETERMINISTIC | READS SQL DATA
BEGIN
    DECLARE v_result DATATYPE;
    
    SET v_result = ...;
    
    RETURN v_result;
END //

DELIMITER ;
```

#### Complete Practical Example
Categorize salary tiers:
```sql
DELIMITER //

CREATE FUNCTION GetSalaryBand(p_salary DECIMAL(10,2))
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
    DECLARE v_band VARCHAR(20);

    IF p_salary >= 10000.00 THEN
        SET v_band = 'Executive';
    ELSEIF p_salary >= 6000.00 THEN
        SET v_band = 'Senior';
    ELSE
        SET v_band = 'Standard';
    END IF;

    RETURN v_band;
END //

DELIMITER ;

-- Execution:
SELECT name, salary, GetSalaryBand(salary) AS band 
FROM employees;
```

---

### Cursors
Iterate through a result set row-by-row inside a procedure.

#### Strict Declaration Order inside `BEGIN...END`
1. Declare local variables (`DECLARE v_name ...`)
2. Declare cursors (`DECLARE cur CURSOR FOR ...`)
3. Declare handlers (`DECLARE CONTINUE HANDLER FOR NOT FOUND ...`)

#### Complete Practical Example
```sql
DELIMITER //

CREATE PROCEDURE ProcessEmployeesInDept(IN p_dept VARCHAR(50))
BEGIN
    DECLARE v_id INT;
    DECLARE v_salary DECIMAL(10,2);
    DECLARE v_done INT DEFAULT FALSE;

    DECLARE emp_cursor CURSOR FOR 
        SELECT id, salary FROM employees WHERE department = p_dept;

    DECLARE CONTINUE HANDLER FOR NOT FOUND SET v_done = TRUE;

    OPEN emp_cursor;

    read_loop: LOOP
        FETCH emp_cursor INTO v_id, v_salary;

        IF v_done THEN
            LEAVE read_loop;
        END IF;

        IF v_salary < 6000 THEN
            UPDATE employees SET salary = salary + 500 WHERE id = v_id;
        END IF;

    END LOOP;

    CLOSE emp_cursor;
END //

DELIMITER ;

-- Execution:
CALL ProcessEmployeesInDept('IT');
```

---

## 3. Error Handling & Transactions

### Custom Condition Handling & Manual Validation
```sql
DELIMITER //

CREATE PROCEDURE SafeWithdrawal(
    IN p_account_id INT,
    IN p_amount DECIMAL(10,2)
)
BEGIN
    DECLARE v_current_balance DECIMAL(10,2);
    
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SELECT 'Transaction failed due to a database error.' AS Message;
    END;

    START TRANSACTION;

        SELECT balance INTO v_current_balance 
        FROM accounts 
        WHERE account_id = p_account_id 
        FOR UPDATE;

        IF v_current_balance < p_amount THEN
            ROLLBACK;
            SIGNAL SQLSTATE '45000'
                SET MESSAGE_TEXT = 'Insufficient funds for withdrawal.';
        ELSE
            UPDATE accounts 
            SET balance = balance - p_amount 
            WHERE account_id = p_account_id;
            
            COMMIT;
            SELECT 'Withdrawal successful.' AS Message;
        END IF;

END //

DELIMITER ;
```

---

### Log Errors to an Audit Table using `CONTINUE HANDLER`
```sql
DELIMITER //

CREATE PROCEDURE BatchInsertUsers()
BEGIN
    DECLARE v_duplicate_key INT DEFAULT 0;

    DECLARE CONTINUE HANDLER FOR 1062 
    BEGIN
        SET v_duplicate_key = 1;
    END;

    INSERT INTO users (id, email) VALUES (1, 'alice@example.com');
    INSERT INTO users (id, email) VALUES (1, 'bob@example.com');

    IF v_duplicate_key = 1 THEN
        INSERT INTO error_logs (log_time, message) 
        VALUES (NOW(), 'Failed to insert user due to duplicate key.');
    END IF;

END //

DELIMITER ;
```

---

### Declaration Order & Transaction Rules
1. **Strict Ordering:** `DECLARE` variables $\rightarrow$ `DECLARE` conditions $\rightarrow$ `DECLARE` cursors $\rightarrow$ `DECLARE` handlers.
2. **Row Locking (`FOR UPDATE`):** Locks target rows during a transaction to prevent concurrent race conditions.
3. **`SIGNAL SQLSTATE '45000'`:** Standard SQLSTATE code for user-defined unhandled exceptions.

---

## 4. MySQL Triggers (MySQL 5.0+)

### Core Concepts & Pseudo-Rows
A **Trigger** automatically executes on DML events (`INSERT`, `UPDATE`, `DELETE`) either `BEFORE` or `AFTER`.

| Event | `OLD` Keyword | `NEW` Keyword |
| :--- | :--- | :--- |
| `INSERT` | Not available | Contains new values being inserted |
| `UPDATE` | Contains values *before* update | Contains values *after* update |
| `DELETE` | Contains values *being deleted* | Not available |

*Note: `NEW.column_name` can only be modified in `BEFORE` triggers.*

---

### Practical Trigger Examples

#### 1. Data Validation & Formatting (`BEFORE INSERT`)
```sql
DELIMITER //

CREATE TRIGGER before_users_insert
BEFORE INSERT ON users
FOR EACH ROW
BEGIN
    SET NEW.email = LOWER(NEW.email);

    IF NEW.age < 18 THEN
        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Validation Error: User must be at least 18 years old.';
    END IF;
END //

DELIMITER ;
```

#### 2. Audit Logging (`AFTER UPDATE`)
```sql
CREATE TABLE salary_audit (
    audit_id INT AUTO_INCREMENT PRIMARY KEY,
    emp_id INT,
    old_salary DECIMAL(10,2),
    new_salary DECIMAL(10,2),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER //

CREATE TRIGGER after_employee_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF OLD.salary <> NEW.salary THEN
        INSERT INTO salary_audit (emp_id, old_salary, new_salary)
        VALUES (NEW.id, OLD.salary, NEW.salary);
    END IF;
END //

DELIMITER ;
```

#### 3. Maintaining Aggregates (`AFTER DELETE`)
```sql
DELIMITER //

CREATE TRIGGER after_order_item_delete
AFTER DELETE ON order_items
FOR EACH ROW
BEGIN
    UPDATE orders
    SET total_amount = total_amount - (OLD.price * OLD.quantity)
    WHERE order_id = OLD.order_id;
END //

DELIMITER ;
```

---

### Managing & Rules for Triggers
```sql
SHOW TRIGGERS;
SHOW CREATE TRIGGER trigger_name;
DROP TRIGGER IF EXISTS trigger_name;
```

1. **No Direct TCL:** `START TRANSACTION`, `COMMIT`, and `ROLLBACK` are not allowed directly inside triggers.
2. **No Self-Modification:** A trigger cannot run `INSERT`/`UPDATE`/`DELETE` on the same table that fired it. Use `SET NEW.column = value;` in a `BEFORE` trigger instead.
3. **`FOR EACH ROW` Execution:** Executes row-by-row for every modified record.
