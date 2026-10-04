# Matplotlib — Rapid Revision

**Matplotlib** is a Python library used for **data visualization** — creating graphs and charts.

```python
import matplotlib.pyplot as plt
```

---

## 1. Basic Plot

```python
x = [1, 2, 3, 4]
y = [10, 20, 15, 25]

plt.plot(x, y)
plt.show()
```

```text
plt.plot() → creates the plot
plt.show() → displays the plot
```

---

# 2. Common Plot Types

### Line Plot

```python
plt.plot(x, y)
```

Used for **trends / continuous data**.

### Scatter Plot

```python
plt.scatter(x, y)
```

Used to show **relationships between data points**.

### Bar Chart

```python
plt.bar(names, marks)
```

Used to **compare categories**.

### Histogram

```python
plt.hist(data)
```

Used to show **data distribution/frequency**.

### Pie Chart

```python
plt.pie(values, labels=labels)
```

Used to show **parts of a whole**.

---

# 3. Labels & Title

```python
plt.plot(x, y)

plt.title("Sales")
plt.xlabel("Month")
plt.ylabel("Revenue")

plt.show()
```

---

# 4. Common Customization Options

### Line Style

```python
plt.plot(x, y, linestyle="--")
```

Common values:

```text
"-"   → solid
"--"  → dashed
":"   → dotted
"-."  → dash-dot
```

### Line Width

```python
plt.plot(x, y, linewidth=2)
```

### Marker

```python
plt.plot(x, y, marker="o")
```

Common markers:

```text
"o" → circle
"s" → square
"^" → triangle
"*" → star
"x" → x
```

### Color

```python
plt.plot(x, y, color="red")
```

Can also use:

```python
color="blue"
color="#3498db"
```

### Transparency

```python
plt.plot(x, y, alpha=0.5)
```

`alpha` ranges from:

```text
0 → transparent
1 → fully opaque
```

---

# 5. Grid

```python
plt.grid()
```

Customize it:

```python
plt.grid(
    axis="y",
    linestyle="--",
    alpha=0.5
)
```

Useful options:

```text
axis="x" / "y" / "both"
linestyle
linewidth
alpha
```

---

# 6. Legend

```python
plt.plot(x, y, label="Sales")

plt.legend()
```

Customize:

```python
plt.legend(
    loc="upper left",
    fontsize=10
)
```

Common locations:

```text
"upper right"
"upper left"
"lower right"
"lower left"
"center"
```

---

# 7. Axis Limits

Control the visible range:

```python
plt.xlim(0, 10)
plt.ylim(0, 100)
```

Useful for focusing on a particular region.

---

# 8. Ticks

Customize tick positions:

```python
plt.xticks([1, 2, 3, 4])
plt.yticks([0, 10, 20, 30])
```

You can also change their labels:

```python
plt.xticks(
    [1, 2, 3],
    ["Jan", "Feb", "Mar"]
)
```

Rotate labels:

```python
plt.xticks(rotation=45)
```

---

# 9. Figure Size

```python
plt.figure(figsize=(8, 5))
```

`figsize` is:

```text
(width, height)
```

measured in inches.

---

# 10. Multiple Lines

```python
plt.plot(x, sales, label="Sales")
plt.plot(x, profit, label="Profit")

plt.legend()
plt.show()
```

Each plot can have its own customization:

```python
plt.plot(
    x, sales,
    marker="o",
    linestyle="-",
    linewidth=2,
    label="Sales"
)
```

---

# 11. Subplots

Create multiple plots in one figure:

```python
plt.subplot(1, 2, 1)
plt.plot(x, y)

plt.subplot(1, 2, 2)
plt.bar(x, y)

plt.show()
```

```text
subplot(rows, columns, position)
```

For more complex plots, prefer:

```python
fig, axes = plt.subplots(1, 2)
```

---

# 12. Object-Oriented Style

```python
fig, ax = plt.subplots()

ax.plot(x, y)

ax.set_title("Sales")
ax.set_xlabel("Month")
ax.set_ylabel("Revenue")

ax.grid()
plt.show()
```

```text
fig → entire figure
ax  → individual axes/plot
```

Useful for **multiple plots and larger visualization code**.

---

# 13. Save Figure

```python
plt.savefig("graph.png")
```

Common options:

```python
plt.savefig(
    "graph.png",
    dpi=300,
    bbox_inches="tight"
)
```

```text
dpi           → image resolution
bbox_inches   → control extra whitespace
```

---

# ⭐ Common Customization Cheat Sheet

```text
color="red"          → line/bar color
linestyle="--"       → line style
linewidth=2          → line thickness
marker="o"           → data point marker
markersize=8         → marker size
alpha=0.5            → transparency

title()              → title
xlabel()             → X-axis label
ylabel()             → Y-axis label

xlim() / ylim()      → axis range
xticks() / yticks()  → tick positions/labels
grid()               → grid lines
legend()             → legend

figsize=(8,5)        → figure size
dpi=300               → output resolution
bbox_inches="tight"  → remove extra whitespace
```

### ⭐ Most commonly used in practice

```python
plt.plot(
    x, y,
    color="blue",
    linestyle="--",
    linewidth=2,
    marker="o",
    markersize=6,
    alpha=0.8,
    label="Sales"
)

plt.title("Monthly Sales")
plt.xlabel("Month")
plt.ylabel("Revenue")
plt.grid()
plt.legend()
plt.xlim(0, 12)
plt.show()
```

**Remember:**
**Plot → Label → Customize → Grid/Legend → Show/Save**
