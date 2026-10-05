# R — Manipulating Data Frames & Operations on Data Frames

A **data frame** is a 2-dimensional tabular data structure in R where **each column can have a different data type**, but all values within a column generally have the same type.

```r
students <- data.frame(
  ID = c(101, 102, 103, 104),
  Name = c("Amit", "Riya", "John", "Sara"),
  Age = c(20, 21, 19, 22),
  Marks = c(85, 92, 76, 88)
)
```

---

## 1. Creating Data Frames

### Using `data.frame()`

```r
df <- data.frame(
  Name = c("A", "B", "C"),
  Age = c(20, 21, 19),
  Marks = c(80, 90, 75)
)
```

### Creating from vectors

```r
name <- c("A", "B", "C")
age <- c(20, 21, 19)
marks <- c(80, 90, 75)

df <- data.frame(name, age, marks)
```

### Check the data frame

```r
class(df)       # "data.frame"
str(df)         # structure
dim(df)         # rows and columns
nrow(df)        # number of rows
ncol(df)        # number of columns
names(df)       # column names
summary(df)     # summary
```

---

# 2. Viewing Data Frames

### View complete data

```r
df
```

### First rows

```r
head(df)
head(df, 3)
```

### Last rows

```r
tail(df)
tail(df, 3)
```

### Spreadsheet-style view

```r
View(df)
```

### Structure

```r
str(df)
```

Example:

```text
'data.frame': 4 obs. of 4 variables:
 $ ID   : num
 $ Name : chr
 $ Age  : num
 $ Marks: num
```

---

# 3. Accessing Data Frame Elements

Suppose:

```r
df <- data.frame(
  Name = c("A", "B", "C"),
  Age = c(20, 21, 19),
  Marks = c(80, 90, 75)
)
```

## Access a column using `$`

```r
df$Name
df$Marks
```

## Access using `[row, column]`

```r
df[1, 2]       # row 1, column 2
df[2, 3]       # row 2, column 3
```

## Access a complete row

```r
df[1, ]
```

## Access a complete column

```r
df[, 2]
```

## Multiple rows

```r
df[1:2, ]
```

## Multiple columns

```r
df[, c("Name", "Marks")]
```

## Using column numbers

```r
df[, c(1, 3)]
```

---

# 4. Adding Columns

### Direct assignment

```r
df$Grade <- c("B", "A", "C")
```

Now:

```text
Name   Age   Marks   Grade
A      20     80      B
B      21     90      A
C      19     75      C
```

### Calculate a new column

```r
df$Result <- ifelse(df$Marks >= 40, "Pass", "Fail")
```

### Add column using existing columns

```r
df$Bonus <- df$Marks + 5
```

---

# 5. Modifying Columns

Change values:

```r
df$Age <- df$Age + 1
```

Change a particular value:

```r
df$Marks[1] <- 95
```

Change multiple values:

```r
df$Marks[df$Marks < 80] <- 80
```

Change column name:

```r
names(df)[2] <- "StudentAge"
```

Or:

```r
names(df) <- c("Name", "Age", "Marks")
```

---

# 6. Removing Columns

### Assign `NULL`

```r
df$Grade <- NULL
```

### Using column index

```r
df <- df[, -2]
```

Removes column 2.

### Remove multiple columns

```r
df <- df[, -c(2, 4)]
```

---

# 7. Adding Rows

Use `rbind()` (**row bind**).

```r
new_student <- data.frame(
  Name = "D",
  Age = 23,
  Marks = 82
)

df <- rbind(df, new_student)
```

Multiple rows:

```r
new <- data.frame(
  Name = c("D", "E"),
  Age = c(23, 24),
  Marks = c(82, 91)
)

df <- rbind(df, new)
```

> Column names and compatible data types should match.

---

# 8. Removing Rows

### By row number

```r
df <- df[-2, ]
```

Removes row 2.

Multiple rows:

```r
df <- df[-c(2, 4), ]
```

### Based on condition

```r
df <- df[df$Marks >= 40, ]
```

Keeps only students who passed.

---

# 9. Filtering / Extracting Rows

One of the most important data-frame operations.

```r
df[df$Marks > 80, ]
```

Returns students with marks greater than 80.

### Multiple conditions

```r
df[df$Age > 20 & df$Marks > 80, ]
```

### OR condition

```r
df[df$Age > 20 | df$Marks > 90, ]
```

### NOT condition

```r
df[!(df$Marks < 40), ]
```

---

# 10. `subset()` — Easier Filtering

Instead of:

```r
df[df$Marks > 80, ]
```

Use:

```r
subset(df, Marks > 80)
```

Multiple conditions:

```r
subset(df, Age > 20 & Marks > 80)
```

Select particular columns:

```r
subset(df, Marks > 80, select = c(Name, Marks))
```

---

# 11. Sorting Data Frames

## `order()`

### Ascending

```r
df[order(df$Marks), ]
```

### Descending

```r
df[order(-df$Marks), ]
```

or:

```r
df[order(df$Marks, decreasing = TRUE), ]
```

### Sort by multiple columns

```r
df[order(df$Age, df$Marks), ]
```

First sorts by `Age`, then `Marks`.

### Descending by Marks

```r
df[order(df$Age, -df$Marks), ]
```

---

# 12. `sort()` vs `order()`

This is important for exams.

### `sort()`

Sorts the **values themselves**.

```r
sort(df$Marks)
```

Output:

```text
75 80 90
```

### `order()`

Returns the **positions/indexes** that would sort the data.

```r
order(df$Marks)
```

Then:

```r
df[order(df$Marks), ]
```

sorts the **whole data frame** according to Marks.

```text
sort()  → sorts values
order() → gives positions → useful for sorting rows
```

---

# 13. Renaming Columns

### `names()`

```r
names(df)
```

Rename one:

```r
names(df)[1] <- "StudentName"
```

Rename all:

```r
names(df) <- c("StudentName", "Age", "Marks")
```

### `colnames()`

```r
colnames(df)[2] <- "StudentAge"
```

---

# 14. Combining Data Frames

There are two main ways to combine data frames:

```text
rbind() → combine rows
cbind() → combine columns
merge() → combine based on matching columns
```

---

## 14.1 `rbind()` — Row-wise Combination

```r
df1 <- data.frame(
  Name = c("A", "B"),
  Marks = c(80, 90)
)

df2 <- data.frame(
  Name = c("C", "D"),
  Marks = c(75, 85)
)

result <- rbind(df1, df2)
```

Result:

```text
Name   Marks
A       80
B       90
C       75
D       85
```

---

## 14.2 `cbind()` — Column-wise Combination

```r
name <- c("A", "B", "C")
marks <- c(80, 90, 75)

df <- cbind(name, marks)
```

Result:

```text
name   marks
A       80
B       90
C       75
```

The objects should have compatible numbers of rows.

---

# 15. Merging Data Frames

`merge()` combines data frames based on a **common column/key**.

### Example

```r
students <- data.frame(
  ID = c(1, 2, 3),
  Name = c("A", "B", "C")
)

marks <- data.frame(
  ID = c(1, 2, 3),
  Marks = c(80, 90, 75)
)
```

Merge:

```r
result <- merge(students, marks, by = "ID")
```

Result:

```text
ID   Name   Marks
1     A      80
2     B      90
3     C      75
```

### Important

```r
merge(df1, df2, by = "ID")
```

means:

> Match rows where `ID` is the same.

---

# 16. Different Types of Merge

Suppose some IDs don't exist in both data frames.

### Inner join

Only matching rows:

```r
merge(df1, df2, by = "ID")
```

### Left join

Keep all rows from `df1`:

```r
merge(df1, df2, by = "ID", all.x = TRUE)
```

### Right join

Keep all rows from `df2`:

```r
merge(df1, df2, by = "ID", all.y = TRUE)
```

### Full join

Keep all rows from both:

```r
merge(df1, df2, by = "ID", all = TRUE)
```

```text
all.x = TRUE  → LEFT JOIN
all.y = TRUE  → RIGHT JOIN
all = TRUE    → FULL JOIN
default       → INNER JOIN
```

---

# 17. Reshaping Data Frames

Reshaping means changing the **structure/layout** of data without changing its underlying information.

Common transformations:

```text
Wide format  ↔  Long format
```

Example wide data:

```text
Name   Math   Science
A       80      90
B       70      85
```

Long format:

```text
Name   Subject   Marks
A      Math       80
A      Science    90
B      Math       70
B      Science    85
```

### Using `tidyr`

```r
library(tidyr)
```

### Wide → Long

```r
long <- pivot_longer(
  df,
  cols = c(Math, Science),
  names_to = "Subject",
  values_to = "Marks"
)
```

### Long → Wide

```r
wide <- pivot_wider(
  long,
  names_from = Subject,
  values_from = Marks
)
```

---

# 18. Applying Functions to Data Frames

### `apply()`

For matrix-like operations:

```r
apply(df[, c("Age", "Marks")], 2, mean)
```

`2` means operate column-wise.

```text
1 → rows
2 → columns
```

### `lapply()`

Returns a list:

```r
lapply(df, class)
```

Useful for applying a function to every column.

### `sapply()`

Similar to `lapply()` but tries to simplify the result:

```r
sapply(df, class)
```

---

# 19. Statistical Operations on Data Frames

```r
mean(df$Marks)
median(df$Marks)
sd(df$Marks)
var(df$Marks)
min(df$Marks)
max(df$Marks)
sum(df$Marks)
```

### Summary

```r
summary(df)
```

For a particular column:

```r
summary(df$Marks)
```

---

# 20. Handling Missing Values

R represents missing data using `NA`.

```r
df$Marks <- c(80, 90, NA, 75)
```

### Check missing values

```r
is.na(df$Marks)
```

### Count missing values

```r
sum(is.na(df$Marks))
```

### Remove rows containing NA

```r
na.omit(df)
```

### Ignore NA during calculation

```r
mean(df$Marks, na.rm = TRUE)
```

---

# 21. Selecting Rows and Columns Together

```r
df[df$Marks > 80, c("Name", "Marks")]
```

Meaning:

```text
Rows    → Marks > 80
Columns → Name and Marks
```

This is extremely useful for exam questions.

---

# 22. Duplicate Rows

### Find duplicated rows

```r
duplicated(df)
```

### Remove duplicate rows

```r
df <- df[!duplicated(df), ]
```

### Remove duplicates based on one column

```r
df <- df[!duplicated(df$ID), ]
```

---

# 23. Useful Data Frame Operations — Quick Table

| Operation      | Function / Syntax                 |
| -------------- | --------------------------------- |
| Create         | `data.frame()`                    |
| View           | `View(df)`                        |
| Structure      | `str(df)`                         |
| First rows     | `head(df)`                        |
| Last rows      | `tail(df)`                        |
| Rows           | `nrow(df)`                        |
| Columns        | `ncol(df)`                        |
| Dimensions     | `dim(df)`                         |
| Column names   | `names(df)`                       |
| Access column  | `df$Name`                         |
| Access row     | `df[1, ]`                         |
| Access cell    | `df[1,2]`                         |
| Filter         | `df[df$Marks > 80, ]`             |
| Filter easily  | `subset()`                        |
| Sort rows      | `order()`                         |
| Add row        | `rbind()`                         |
| Add column     | `cbind()`                         |
| Merge          | `merge()`                         |
| Rename         | `names()`                         |
| Remove column  | `df$col <- NULL`                  |
| Remove row     | `df[-1, ]`                        |
| Missing values | `is.na()`                         |
| Remove NA rows | `na.omit()`                       |
| Duplicates     | `duplicated()`                    |
| Reshape        | `pivot_longer()`, `pivot_wider()` |

---

# ⭐ Data Frame Manipulation Flow

```text
                 DATA FRAME
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    CREATE         ACCESS        VIEW
 data.frame()   [, ]  $       head(), str()
       │
       ↓
   MANIPULATE
       │
 ┌─────┼─────────┬──────────┐
 ↓     ↓         ↓          ↓
ADD   REMOVE    FILTER     MODIFY
 │      │         │          │
+col  -col    subset()    assignment
+row  -row    conditions
       │
       ↓
    PROCESS
       │
 ┌─────┼──────────┐
 ↓     ↓          ↓
SORT  COMBINE   RESHAPE
 │      │          │
order  rbind      long
       cbind       ↕
       merge      wide
```

### ⭐ Most important exam syntax

```r
# Access
df$Marks
df[1, ]
df[, 2]
df[1, 2]

# Filter
df[df$Marks > 80, ]
subset(df, Marks > 80)

# Sort
df[order(df$Marks), ]
df[order(-df$Marks), ]

# Add
df$Grade <- c("A", "B", "C")
df <- rbind(df, new_row)

# Remove
df$Grade <- NULL
df <- df[-2, ]

# Combine
rbind(df1, df2)
cbind(df1, x)

# Merge
merge(df1, df2, by = "ID")

# Missing values
is.na(df)
na.omit(df)

# Reshape
pivot_longer(df, ...)
pivot_wider(df, ...)
```
