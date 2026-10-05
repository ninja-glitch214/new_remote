# `ggplot2` — Common Graphs & Customizations

`ggplot2` is based on the idea:

```r
ggplot(data, aes(x = ..., y = ...)) +
    GEOM()
```

Common customization pattern:

```r
ggplot(data, aes(x, y, color = group)) +
  geom_point(size = 3) +
  labs(
    title = "My Graph",
    x = "X Axis",
    y = "Y Axis"
  ) +
  theme_minimal()
```

For the examples below, assume:

```r
library(ggplot2)

df <- data.frame(
  Name = c("A", "B", "C", "D", "E"),
  Age = c(18, 20, 22, 24, 26),
  Marks = c(65, 75, 82, 90, 95),
  Gender = c("M", "F", "M", "F", "M"),
  Subject = c("Math", "Science", "Math", "Science", "Math")
)
```

---

# 1. Scatter Plot

Used to show the **relationship between two numerical variables**.

```r
ggplot(df, aes(x = Age, y = Marks)) +
  geom_point()
```

### Customize points

```r
ggplot(df, aes(Age, Marks)) +
  geom_point(
    color = "blue",
    size = 4,
    shape = 19,
    alpha = 0.7
  )
```

Common `shape` values:

```text
0 → square
1 → circle
2 → triangle
15 → filled square
16 → filled circle
17 → filled triangle
```

### Color by category

```r
ggplot(df, aes(Age, Marks, color = Gender)) +
  geom_point(size = 4)
```

### Add trend line

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_smooth(method = "lm")
```

Without confidence interval:

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_smooth(
    method = "lm",
    se = FALSE
  )
```

---

# 2. Line Graph

Used for **trends and ordered data**, especially time series.

```r
sales <- data.frame(
  Month = c("Jan", "Feb", "Mar", "Apr", "May"),
  Sales = c(100, 120, 115, 150, 180)
)

ggplot(sales, aes(Month, Sales, group = 1)) +
  geom_line()
```

### Customize

```r
ggplot(sales, aes(Month, Sales, group = 1)) +
  geom_line(
    color = "blue",
    linewidth = 1.5,
    linetype = "dashed"
  ) +
  geom_point(size = 3)
```

Useful:

```text
linewidth → line thickness
linetype  → solid/dashed/dotted
```

---

# 3. Bar Chart

Used to compare **categories**.

```r
ggplot(df, aes(x = Name, y = Marks)) +
  geom_col()
```

### `geom_bar()`

When R should **count observations**:

```r
ggplot(df, aes(x = Gender)) +
  geom_bar()
```

### `geom_col()`

When you already have values:

```r
ggplot(df, aes(x = Name, y = Marks)) +
  geom_col()
```

### Customize

```r
ggplot(df, aes(Name, Marks)) +
  geom_col(
    fill = "steelblue",
    color = "black",
    width = 0.7
  )
```

```text
fill  → inside color
color → border color
width → bar width
```

---

# 4. Horizontal Bar Chart

Use `coord_flip()`:

```r
ggplot(df, aes(Name, Marks)) +
  geom_col() +
  coord_flip()
```

Useful when category names are long.

---

# 5. Grouped / Side-by-Side Bar Chart

Suppose:

```r
marks <- data.frame(
  Student = c("A","A","B","B","C","C"),
  Subject = c("Math","Science",
              "Math","Science",
              "Math","Science"),
  Marks = c(80,90,70,85,95,88)
)
```

```r
ggplot(marks, aes(Student, Marks, fill = Subject)) +
  geom_col(position = "dodge")
```

`position = "dodge"` puts bars side-by-side.

---

# 6. Stacked Bar Chart

```r
ggplot(marks, aes(Student, Marks, fill = Subject)) +
  geom_col()
```

Default bars are stacked.

### 100% stacked bar

```r
ggplot(marks, aes(Student, Marks, fill = Subject)) +
  geom_col(position = "fill")
```

Useful for showing **proportions/percentages**.

---

# 7. Histogram

Used to show the **distribution of numerical data**.

```r
ggplot(df, aes(Marks)) +
  geom_histogram()
```

### Customize bins

```r
ggplot(df, aes(Marks)) +
  geom_histogram(
    bins = 5,
    fill = "skyblue",
    color = "black"
  )
```

Or:

```r
ggplot(df, aes(Marks)) +
  geom_histogram(binwidth = 10)
```

Important:

```text
bins      → number of intervals
binwidth  → width of each interval
```

---

# 8. Density Plot

Shows a **smoothed distribution**.

```r
ggplot(df, aes(Marks)) +
  geom_density()
```

### Customize

```r
ggplot(df, aes(Marks)) +
  geom_density(
    fill = "lightblue",
    alpha = 0.5,
    linewidth = 1
  )
```

Compare groups:

```r
ggplot(df, aes(Marks, fill = Gender)) +
  geom_density(alpha = 0.4)
```

---

# 9. Box Plot

Shows:

```text
Minimum
   │
Lower whisker
   │
Q1 ────────┐
           │
Median     │
           │
Q3 ────────┘
   │
Upper whisker
   │
Outliers
```

Example:

```r
ggplot(df, aes(Gender, Marks)) +
  geom_boxplot()
```

### Customize

```r
ggplot(df, aes(Gender, Marks)) +
  geom_boxplot(
    fill = "lightblue",
    color = "black",
    width = 0.6
  )
```

---

# 10. Violin Plot

Useful for showing the **distribution shape**.

```r
ggplot(df, aes(Gender, Marks)) +
  geom_violin()
```

Combine with box plot:

```r
ggplot(df, aes(Gender, Marks)) +
  geom_violin(fill = "lightblue") +
  geom_boxplot(width = 0.1)
```

---

# 11. Area Chart

Used to show **trends with filled area**.

```r
ggplot(sales, aes(Month, Sales, group = 1)) +
  geom_area()
```

Customize:

```r
ggplot(sales, aes(Month, Sales, group = 1)) +
  geom_area(
    fill = "skyblue",
    alpha = 0.5
  ) +
  geom_line(linewidth = 1)
```

---

# 12. Pie Chart

`ggplot2` does not have a dedicated `geom_pie()`.

Create it using:

```text
geom_bar()
    +
coord_polar()
```

Example:

```r
gender_count <- data.frame(
  Gender = c("Male", "Female"),
  Count = c(3, 2)
)

ggplot(gender_count, aes(x = "", y = Count, fill = Gender)) +
  geom_col() +
  coord_polar("y")
```

### Add labels

```r
ggplot(gender_count, aes(x = "", y = Count, fill = Gender)) +
  geom_col() +
  coord_polar("y") +
  geom_text(
    aes(label = Count),
    position = position_stack(vjust = 0.5)
  )
```

---

# 13. Dot Plot

Useful for comparing individual observations.

```r
ggplot(df, aes(Marks, Name)) +
  geom_point(size = 4)
```

---

# 14. Jitter Plot

Useful when many points overlap.

```r
ggplot(df, aes(Gender, Marks)) +
  geom_jitter()
```

Customize:

```r
ggplot(df, aes(Gender, Marks)) +
  geom_jitter(
    width = 0.2,
    height = 0,
    size = 3,
    alpha = 0.7
  )
```

---

# 15. Count Plot

Count observations in each category.

```r
ggplot(df, aes(Gender)) +
  geom_bar()
```

Equivalent idea:

```text
Category → Count → Bar
```

Add fill:

```r
ggplot(df, aes(Gender, fill = Gender)) +
  geom_bar()
```

---

# 16. Bubble Chart

A bubble chart is essentially a scatter plot where **size represents another variable**.

```r
ggplot(df, aes(
  x = Age,
  y = Marks,
  size = Marks
)) +
  geom_point(alpha = 0.6)
```

Add color:

```r
ggplot(df, aes(
  Age,
  Marks,
  size = Marks,
  color = Gender
)) +
  geom_point(alpha = 0.7)
```

---

# 17. Heatmap

Useful for showing values using **color intensity**.

Example:

```r
heat <- data.frame(
  X = rep(c("A", "B", "C"), each = 3),
  Y = rep(c("X", "Y", "Z"), 3),
  Value = c(10,20,30,40,50,60,70,80,90)
)

ggplot(heat, aes(X, Y, fill = Value)) +
  geom_tile()
```

### Customize

```r
ggplot(heat, aes(X, Y, fill = Value)) +
  geom_tile(color = "white") +
  geom_text(aes(label = Value))
```

---

# 18. Faceted Graphs

Faceting creates **multiple graphs from one dataset**.

### `facet_wrap()`

```r
ggplot(marks, aes(Student, Marks)) +
  geom_col() +
  facet_wrap(~Subject)
```

Multiple variables:

```r
ggplot(marks, aes(Student, Marks)) +
  geom_col() +
  facet_wrap(~Subject, ncol = 2)
```

### `facet_grid()`

```r
ggplot(marks, aes(Student, Marks)) +
  geom_col() +
  facet_grid(. ~ Subject)
```

or:

```r
facet_grid(Gender ~ Subject)
```

---

# 19. Error Bar / Mean ± SD

Useful for showing **uncertainty or variation**.

Example:

```r
summary_df <- data.frame(
  Group = c("A", "B", "C"),
  Mean = c(70, 80, 90),
  SD = c(5, 7, 4)
)
```

```r
ggplot(summary_df, aes(Group, Mean)) +
  geom_col() +
  geom_errorbar(
    aes(
      ymin = Mean - SD,
      ymax = Mean + SD
    ),
    width = 0.2
  )
```

---

# 20. Customizing Titles and Labels

Use `labs()`.

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  labs(
    title = "Age vs Marks",
    subtitle = "Student performance",
    x = "Student Age",
    y = "Marks",
    caption = "Source: Student Data"
  )
```

Important:

```text
title
subtitle
x
y
caption
```

---

# 21. Customize Colors

### Fixed color

```r
ggplot(df, aes(Age, Marks)) +
  geom_point(color = "red")
```

### Color based on variable

```r
ggplot(df, aes(Age, Marks, color = Gender)) +
  geom_point(size = 4)
```

### Custom color scale

```r
ggplot(df, aes(Age, Marks, color = Gender)) +
  geom_point(size = 4) +
  scale_color_manual(
    values = c(
      "M" = "blue",
      "F" = "red"
    )
  )
```

---

# 22. Customize Fill

```r
ggplot(df, aes(Gender, Marks, fill = Gender)) +
  geom_boxplot()
```

Custom:

```r
ggplot(df, aes(Gender, Marks, fill = Gender)) +
  geom_boxplot() +
  scale_fill_manual(
    values = c(
      "M" = "lightblue",
      "F" = "pink"
    )
  )
```

---

# 23. Customize Axis Limits

### `xlim()` / `ylim()`

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  xlim(15, 30) +
  ylim(0, 100)
```

Better in many cases:

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  coord_cartesian(
    xlim = c(15, 30),
    ylim = c(0, 100)
  )
```

---

# 24. Customize Axis Breaks

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  scale_y_continuous(
    breaks = seq(0, 100, 10)
  )
```

Custom labels:

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  scale_y_continuous(
    breaks = c(0, 50, 100),
    labels = c("Low", "Medium", "High")
  )
```

---

# 25. Rotate X-Axis Labels

```r
ggplot(df, aes(Name, Marks)) +
  geom_col() +
  theme(
    axis.text.x = element_text(
      angle = 45,
      hjust = 1
    )
  )
```

---

# 26. Themes

Themes control the **overall appearance**.

### Minimal

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  theme_minimal()
```

### Classic

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  theme_classic()
```

### Gray

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  theme_gray()
```

### Black & white

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  theme_bw()
```

Common:

```text
theme_gray()
theme_bw()
theme_classic()
theme_minimal()
theme_light()
theme_dark()
theme_void()
```

---

# 27. Customize Text

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  labs(
    title = "Student Performance"
  ) +
  theme(
    plot.title = element_text(
      size = 20,
      face = "bold",
      hjust = 0.5
    ),
    axis.title = element_text(size = 14),
    axis.text = element_text(size = 11)
  )
```

Common:

```text
size
face = "bold"
face = "italic"
hjust = 0    # left
hjust = 0.5  # center
hjust = 1    # right
```

---

# 28. Add Text Labels to Points

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_text(
    aes(label = Name),
    vjust = -0.5
  )
```

### Better label positioning

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_text(
    aes(label = Name),
    nudge_y = 2
  )
```

---

# 29. Add Reference Lines

### Horizontal line

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_hline(
    yintercept = 40,
    linetype = "dashed"
  )
```

### Vertical line

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_vline(
    xintercept = 20,
    linetype = "dashed"
  )
```

### Diagonal line

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_abline(
    slope = 2,
    intercept = 0
  )
```

---

# 30. Add Regression Equation / Trend

Basic regression:

```r
ggplot(df, aes(Age, Marks)) +
  geom_point() +
  geom_smooth(
    method = "lm",
    se = TRUE
  )
```

`se = TRUE` shows the confidence band.

```r
se = FALSE
```

removes it.

---

# 31. Coordinate Customization

### Flip coordinates

```r
ggplot(df, aes(Name, Marks)) +
  geom_col() +
  coord_flip()
```

### Polar coordinates

Used for pie/donut-style graphs:

```r
ggplot(gender_count, aes("", Count, fill = Gender)) +
  geom_col() +
  coord_polar("y")
```

---

# 32. Save a Graph

Use `ggsave()`:

```r
p <- ggplot(df, aes(Age, Marks)) +
  geom_point()

ggsave("graph.png", p)
```

Specify size:

```r
ggsave(
  "graph.png",
  p,
  width = 8,
  height = 6,
  units = "in",
  dpi = 300
)
```

---

# ⭐ Most Important `geom_*()` Functions

```text
geom_point()       → Scatter plot
geom_line()        → Line graph
geom_bar()         → Count bar chart
geom_col()         → Bar chart with supplied values
geom_histogram()   → Histogram
geom_density()     → Density plot
geom_boxplot()     → Box plot
geom_violin()      → Violin plot
geom_area()        → Area chart
geom_tile()        → Heatmap
geom_text()        → Text labels
geom_label()       → Label boxes
geom_errorbar()    → Error bars
geom_smooth()      → Trend/regression
geom_hline()       → Horizontal reference line
geom_vline()       → Vertical reference line
geom_abline()      → Diagonal reference line
geom_jitter()      → Jittered points
```

# ⭐ Customization Cheat Sheet

```r
ggplot(data, aes(x, y)) +
  geom_xxx(
    color = "...",       # outline/line/point color
    fill = "...",        # inside color
    size = ...,          # point/text size
    linewidth = ...,     # line thickness
    alpha = ...,         # transparency
    shape = ...,         # point shape
    width = ...,         # bar/error-bar width
    linetype = "..."     # solid/dashed/dotted
  ) +
  labs(
    title = "...",
    subtitle = "...",
    x = "...",
    y = "...",
    caption = "..."
  ) +
  scale_x_continuous(...) +
  scale_y_continuous(...) +
  scale_color_manual(...) +
  scale_fill_manual(...) +
  theme_minimal()
```

## 🧠 Exam One-Glance Map

```text
NUMERICAL vs NUMERICAL
        ↓
   geom_point()
    Scatter


CATEGORY vs VALUE
        ↓
    geom_col()
     Bar


TIME / ORDER vs VALUE
        ↓
    geom_line()
     Line


DISTRIBUTION
        ↓
 ┌──────┼─────────┐
 ↓      ↓         ↓
Hist.  Density   Boxplot
        ↓
   geom_histogram()
   geom_density()
   geom_boxplot()


CATEGORY + PROPORTION
        ↓
 geom_bar() + coord_polar()
       Pie


2+ VARIABLES / GROUPS
        ↓
   facet_wrap()
   facet_grid()


RELATIONSHIP + TREND
        ↓
 geom_point()
      +
 geom_smooth()
```

**Most important rule:** `ggplot()` defines the **data + aesthetic mapping**, while `geom_*()` decides **what type of graph is drawn**. Then `labs()`, `scale_*()`, `coord_*()`, and `theme()` customize it.
