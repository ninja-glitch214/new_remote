# Seaborn — Rapid Revision

**Seaborn** is a Python library used for creating **statistical and attractive data visualizations**. It is built on top of **Matplotlib** and works especially well with **Pandas DataFrames**.

```python
import seaborn as sns
import matplotlib.pyplot as plt
```

## Common Plots

```python
sns.scatterplot(data=df, x="age", y="salary")
sns.lineplot(data=df, x="year", y="sales")
sns.barplot(data=df, x="category", y="sales")
sns.countplot(data=df, x="category")
sns.histplot(data=df, x="age")
sns.boxplot(data=df, x="category", y="salary")
sns.violinplot(data=df, x="category", y="salary")
sns.heatmap(df.corr(numeric_only=True), annot=True)
```

Display:

```python
plt.show()
```

## Important Functions

| Function        | Used for                                 |
| --------------- | ---------------------------------------- |
| `scatterplot()` | Relationship between two variables       |
| `lineplot()`    | Trends over a continuous variable        |
| `barplot()`     | Compare aggregated values                |
| `countplot()`   | Count observations by category           |
| `histplot()`    | Distribution of a variable               |
| `boxplot()`     | Distribution + outliers                  |
| `violinplot()`  | Distribution + density                   |
| `heatmap()`     | Matrix/correlation visualization         |
| `pairplot()`    | Relationships between multiple variables |

## `hue` — Very Useful

Use `hue` to split/color data based on another column:

```python
sns.scatterplot(
    data=df,
    x="age",
    y="salary",
    hue="gender"
)
```

## Styling

```python
sns.set_theme()
```

Seaborn provides sensible statistical plotting defaults, while **Matplotlib** can still be used for further customization.

---

# Complete Runnable Example

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Create sample data
data = {
    "Name": ["A", "B", "C", "D", "E", "F", "G", "H"],
    "Age": [22, 25, 28, 24, 30, 27, 23, 29],
    "Salary": [30000, 40000, 50000, 35000, 60000, 52000, 32000, 58000],
    "Department": [
        "IT", "HR", "IT", "HR",
        "IT", "Finance", "Finance", "IT"
    ]
}

df = pd.DataFrame(data)

# Apply Seaborn theme
sns.set_theme()

# Create a scatter plot
sns.scatterplot(
    data=df,
    x="Age",
    y="Salary",
    hue="Department",
    s=100
)

# Add labels and title
plt.title("Age vs Salary")
plt.xlabel("Age")
plt.ylabel("Salary")

# Display the graph
plt.show()
```

### What happens here?

```text
Dictionary
    ↓
Pandas DataFrame
    ↓
Seaborn reads DataFrame
    ↓
scatterplot()
    ↓
Matplotlib displays the figure
```

The important pattern to remember is:

```python
sns.plot_function(
    data=df,
    x="column1",
    y="column2",
    hue="column3"
)

plt.show()
```

## Quick Revision

```text
Seaborn
   ↓
Statistical visualization
   ↓
Built on Matplotlib
   ↓
Works well with Pandas
```

**Remember:**

```text
scatterplot → relationship
lineplot    → trend
barplot     → comparison
countplot   → category counts
histplot    → distribution
boxplot     → distribution + outliers
heatmap     → matrix/correlation
pairplot    → many variable relationships
hue         → split/color by category
```
