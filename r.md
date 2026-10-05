# R — Rapid Revision Shortnote

## 1. Reading / Getting Data into R

### From keyboard

```r
x <- scan()
# Enter values: 10 20 30 40
```

```r
df <- data.frame(
  name = c("A", "B", "C"),
  age = c(20, 21, 19)
)
```

### Read CSV

```r
df <- read.csv("students.csv")
```

### Read text file

```r
data <- read.table("data.txt", header = TRUE)
```

### Read Excel

```r
install.packages("readxl")
library(readxl)

df <- read_excel("students.xlsx")
```

---

# 2. Exporting Data from R

### CSV

```r
write.csv(df, "students.csv", row.names = FALSE)
```

### Text file

```r
write.table(df, "students.txt", row.names = FALSE)
```

### R object

```r
save(df, file = "students.RData")
load("students.RData")
```

---

# 3. Data Objects in R

| Object     | Example                | Use               |
| ---------- | ---------------------- | ----------------- |
| Vector     | `c(10,20,30)`          | 1D values         |
| Matrix     | `matrix(1:6, 2, 3)`    | 2D same-type data |
| Array      | `array(1:8, c(2,2,2))` | Multi-dimensional |
| List       | `list(10,"A",TRUE)`    | Different types   |
| Factor     | `factor(c("M","F"))`   | Categorical data  |
| Data Frame | `data.frame(name,age)` | Tabular data      |

### Check object

```r
class(x)
typeof(x)
length(x)
str(x)
```

---

# 4. Constructing Data Objects

### Vector

```r
x <- c(10, 20, 30)
x <- 1:5
x <- seq(1, 10, by = 2)
```

### Matrix

```r
m <- matrix(1:6, nrow = 2, ncol = 3)
```

### List

```r
L <- list(name = "John", age = 20, marks = c(80,90))
```

### Factor

```r
gender <- factor(c("Male", "Female", "Male"))
```

### Data Frame

```r
df <- data.frame(
  Name = c("A", "B", "C"),
  Age = c(20, 21, 19),
  Marks = c(80, 90, 75)
)
```

---

# 5. Working with Objects

```r
ls()          # list objects
rm(x)         # remove object
rm(list=ls()) # remove all objects

class(df)
names(df)
dim(df)
length(df)
str(df)
summary(df)
```

### Viewing objects within objects

For a list:

```r
L <- list(
  name = "John",
  marks = c(80, 90, 85)
)

L$name
L[["marks"]]
L[[2]]
```

For nested objects:

```r
L$marks[1]
```

---

# 6. Data Frame Operations

## Create

```r
df <- data.frame(
  Name = c("A", "B", "C"),
  Age = c(20, 22, 19),
  Marks = c(80, 90, 75)
)
```

## Access

```r
df$Name
df[1, ]       # first row
df[, 2]       # second column
df[1, 2]      # row 1, column 2
df["Name"]
df[, c("Name", "Marks")]
```

### Add column

```r
df$Grade <- c("B", "A", "C")
```

### Add row

```r
df <- rbind(df, c("D", 21, 88, "B"))
```

### Remove column

```r
df$Grade <- NULL
```

---

# 7. Extracting Data

### Using conditions

```r
df[df$Marks > 80, ]
```

```r
df[df$Age >= 20 & df$Marks > 75, ]
```

### Extract selected columns

```r
df[, c("Name", "Marks")]
```

### `subset()`

```r
subset(df, Marks > 80)
subset(df, Marks > 80, select = c(Name, Marks))
```

---

# 8. Sorting Data Frames

### `order()`

```r
df[order(df$Marks), ]      # ascending
df[order(-df$Marks), ]     # descending
```

Multiple columns:

```r
df[order(df$Age, -df$Marks), ]
```

### `sort()`

```r
sort(df$Marks)
sort(df$Marks, decreasing = TRUE)
```

---

# 9. Combining Data

### Combine vectors

```r
c(1, 2, 3, 4)
```

### Combine rows

```r
rbind(df1, df2)
```

### Combine columns

```r
cbind(df1, new_column)
```

### Combine data frames by common column

```r
merge(df1, df2, by = "ID")
```

Example:

```r
students <- data.frame(ID=c(1,2), Name=c("A","B"))
marks <- data.frame(ID=c(1,2), Marks=c(80,90))

merge(students, marks, by="ID")
```

---

# 10. Reshaping Data Frames

### Wide → Long

Using `reshape2`:

```r
library(reshape2)

long <- melt(df, id.vars = "Name")
```

### Long → Wide

```r
wide <- dcast(long, Name ~ variable)
```

Modern approach using `tidyr`:

```r
library(tidyr)

long <- pivot_longer(df, cols = c(Age, Marks))
wide <- pivot_wider(long, names_from = variable,
                    values_from = value)
```

---

# 11. Control Structures

## `if`

```r
if (x > 10) {
    print("Large")
}
```

## `if ... else`

```r
if (x >= 50)
    print("Pass")
else
    print("Fail")
```

## `for`

```r
for (i in 1:5) {
    print(i)
}
```

## `while`

```r
i <- 1
while (i <= 5) {
    print(i)
    i <- i + 1
}
```

## `repeat`

```r
i <- 1

repeat {
    print(i)
    i <- i + 1

    if (i > 5)
        break
}
```

### Loop control

```r
break      # exit loop
next       # skip current iteration
```

---

# 12. Functions in R

### Basic structure

```r
function_name <- function(arguments) {
    statements
    return(value)
}
```

Example:

```r
add <- function(a, b) {
    return(a + b)
}

add(10, 20)
```

---

## Numeric Functions

```r
abs(-10)       # 10
sqrt(25)       # 5
round(3.567,2) # 3.57
ceiling(3.2)   # 4
floor(3.8)     # 3
signif(3.567,2)
```

```r
sum(c(1,2,3))
prod(c(1,2,3))
min(c(5,2,8))
max(c(5,2,8))
```

---

## Character Functions

```r
x <- "Hello R"

nchar(x)             # number of characters
toupper(x)           # HELLO R
tolower(x)           # hello r
substr(x, 1, 5)      # Hello
paste("Hello", "R")   # Hello R
paste0("Hello", "R")  # HelloR
```

```r
strsplit("A,B,C", ",")
```

---

## Statistical Functions

```r
x <- c(10,20,30,40,50)

mean(x)
median(x)
sd(x)
var(x)
min(x)
max(x)
range(x)
quantile(x)
```

```r
summary(x)
```

---

# 13. Packages

Packages provide additional functions, datasets and tools.

### Install

```r
install.packages("ggplot2")
```

### Load

```r
library(ggplot2)
```

### Check installed packages

```r
installed.packages()
```

### Get help

```r
help(package = "ggplot2")
?mean
```

**Common packages:**

```text
dplyr     → data manipulation
tidyr     → reshaping
ggplot2   → visualization
readxl    → Excel files
stringr   → string manipulation
```

---

# 14. Useful Data Frame Functions — One Glance

```r
head(df)        # first rows
tail(df)        # last rows
nrow(df)        # number of rows
ncol(df)        # number of columns
dim(df)         # rows × columns
names(df)       # column names
str(df)         # structure
summary(df)     # statistical summary
View(df)        # spreadsheet-like view
```

---

## ⭐ Exam Quick Revision

```text
READ DATA
read.csv()       read.table()       read_excel()

WRITE DATA
write.csv()      write.table()      save()

OBJECTS
vector → matrix → array → list → factor → data.frame

DATA FRAME
df[row, col]
df$column
subset()
order()
rbind() / cbind()
merge()
melt() / dcast()
pivot_longer() / pivot_wider()

CONTROL
if / else
for
while
repeat
break / next

FUNCTIONS
function(x) { ... }

NUMERIC
abs sqrt round ceiling floor sum min max

CHARACTER
nchar toupper tolower substr paste paste0 strsplit

STATISTICS
mean median sd var quantile summary

PACKAGES
install.packages()
library()
```
