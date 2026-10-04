```
import pandas as pd


#DataFrame    DataSeries

df=pd.read_table('http://bit.ly/chiporders')

print(df.shape)
s=df['item_name'] #print column in Series format
print(type(s))
print(s)



s=df[['item_name']] #print item_name column in DataFrame format
print(type(s))
print(s)
print(df.columns)  #print all column names

print(df.head())  #prints first 5 lines
print(df.tail())  #prints last 5 lines

print(df.head(12))  #prints first 12 lines
print(df.tail(7))  #prints last 7 lines

print(df.iloc[0:4,1:3])  #to retrieve data by integer position

#it will exclude 7th row
#display rows from 0-6 and columns 1 onward all
print(df.iloc[0:7,1:])

#store all columns except the last column
df1=df.iloc[:,:-1]

#It will not exclude 6 th row
print(df.loc[2:6,['order_id','item_name']]) #this is by location it will include 6 th row also

print(df.info())

print(df[['order_id','quantity']].mean())  #to calculate columnwise mean
print(df.iloc[:,:2].mean())  #to calculate columnwise mean
print(df[['order_id','quantity']].std())
print(df[['order_id','quantity']].median())
print(df[['order_id','quantity']].max())

print(df.describe()) #all statistical measures for all integer columns
print(df.info())  #how many not null values and data type of all columns

df['item_price1']=df['item_price'].map(lambda x:x.replace('$','0')) #to replace $ with 0

df['item_price2']=df['item_price1'].astype('float') 
 #to convert from object data type to int
 
print(df.info())


df['discounted_price']=df['item_price2']*0.85  #add new column

df.pop('choice_description') # to delete the column
print(df[df['discounted_price']>3])



index_nm=df[df['discounted_price']>3].index
print(index_nm,len(index_nm))
df.drop(index_nm,inplace=True)  #drop all rows with discounted_price > 3 and ovewrite the original frame
df.drop([2,3,4],axis=0,inplace=True) 

pd.unique(df['item_price'])
df1=df['item_price'].value_counts() #frequency of each distinct value
df1.shape
import matplotlib.pyplot as plt
plt.pie(df1,labels=df1.index,shadow=False,startangle=90,rotatelabels = 270)


```

===================================================================================================

# Pandas — Rapid Revision

**Pandas** is a Python library used for **data manipulation, cleaning, analysis, and tabular data processing**.

```python
import pandas as pd
```

## 1. Core Data Structures

```text
Series      → 1D labeled data
DataFrame   → 2D labeled table
```

### Series

```python
s = pd.Series([10, 20, 30])
```

### DataFrame

```python
df = pd.DataFrame({
    "Name": ["Ash", "John"],
    "Age": [20, 25]
})
```

```text
       Name  Age
0       Ash   20
1      John   25
```

---

## 2. Creating / Reading Data

```python
pd.DataFrame(data)
pd.Series(data)

pd.read_csv("data.csv")
pd.read_excel("data.xlsx")
pd.read_json("data.json")
```

Write data:

```python
df.to_csv("output.csv", index=False)
df.to_excel("output.xlsx", index=False)
```

---

## 3. Inspecting Data

```python
df.head()        # first 5 rows
df.tail()        # last 5 rows
df.shape         # (rows, columns)
df.columns       # column names
df.index         # row index
df.dtypes        # data types
df.info()        # structure + non-null values
df.describe()    # statistical summary
```

---

## 4. Selecting Data

### Column

```python
df["Name"]
```

Multiple columns:

```python
df[["Name", "Age"]]
```

### `loc` — label-based

```python
df.loc[0, "Name"]
df.loc[0:2, ["Name", "Age"]]
```

### `iloc` — position-based

```python
df.iloc[0, 1]
df.iloc[0:3, 0:2]
```

```text
loc  → labels
iloc → integer positions
```

---

## 5. Filtering

```python
df[df["Age"] > 20]
```

Multiple conditions:

```python
df[(df["Age"] > 20) & (df["Age"] < 30)]
```

```text
& → AND
| → OR
~ → NOT
```

---

## 6. Adding / Modifying Columns

```python
df["Salary"] = [30000, 40000]
```

Using existing columns:

```python
df["Bonus"] = df["Salary"] * 0.10
```

Modify values:

```python
df["Age"] = df["Age"] + 1
```

---

## 7. Adding / Removing Rows & Columns

```python
df.drop(columns=["Age"])
df.drop(index=0)
```

To modify the original:

```python
df.drop(columns=["Age"], inplace=True)
```

Add row:

```python
df.loc[len(df)] = ["Sam", 22]
```

---

## 8. Missing Values

Check:

```python
df.isna()
df.isna().sum()
```

Remove:

```python
df.dropna()
```

Fill:

```python
df.fillna(0)
```

Fill with column mean:

```python
df["Age"] = df["Age"].fillna(df["Age"].mean())
```

---

## 9. Sorting

```python
df.sort_values("Age")
```

Descending:

```python
df.sort_values("Age", ascending=False)
```

---

## 10. Useful Data Operations

### Unique values

```python
df["Name"].unique()
```

### Number of unique values

```python
df["Name"].nunique()
```

### Value frequency

```python
df["City"].value_counts()
```

### Aggregation

```python
df["Salary"].sum()
df["Salary"].mean()
df["Salary"].min()
df["Salary"].max()
```

---

## 11. GroupBy

Used to perform calculations **group-wise**.

```python
df.groupby("Department")["Salary"].mean()
```

Multiple aggregations:

```python
df.groupby("Department")["Salary"].agg(
    ["mean", "min", "max"]
)
```

```text
Group → Calculate
```

---

## 12. Apply Functions

Apply a function to values:

```python
df["Age"].apply(lambda x: x + 1)
```

Example:

```python
df["Name"] = df["Name"].apply(str.upper)
```

---

## 13. Combining Data

### Concatenate

```python
pd.concat([df1, df2])
```

### Merge

Similar to SQL JOIN:

```python
pd.merge(df1, df2, on="id")
```

Common joins:

```text
inner
left
right
outer
```

---

## 14. Index

Set a column as index:

```python
df.set_index("id")
```

Reset it:

```python
df.reset_index()
```

Access index:

```python
df.index
```

---

# ⭐ Rapid Revision

```text
import pandas as pd

Series       → 1D labeled data
DataFrame    → 2D table

read_csv()   → read CSV
to_csv()     → write CSV

head()       → first rows
tail()       → last rows
info()       → structure
describe()   → statistics
shape        → rows × columns

df["col"]    → column
loc          → label-based selection
iloc         → position-based selection

df[condition] → filtering

drop()       → remove
fillna()     → fill missing values
dropna()     → remove missing values

sort_values() → sorting
unique()      → unique values
value_counts() → frequency

groupby()     → group-wise analysis
apply()       → apply function
concat()      → combine tables
merge()       → join tables
```

### Core idea

```text
Pandas
  ↓
Load Data
  ↓
Inspect
  ↓
Select / Filter
  ↓
Clean
  ↓
Transform
  ↓
Group / Aggregate
  ↓
Analyze
  ↓
Export
```
