# Code
```
#numpy is useful for handling n dimenssinal 
#array, arrays are homogeneous


import numpy as np
#it has a class ndarray
#create 1D array
a=np.array([12,13,23.89,45])   #[12,13,14,56,"xxxx",[5,6,7]]
print(a)
#the type of a
print("array type ",type(a))
#the type of data in the array
print("data type",a.dtype)
print("Dimensions: ",a.ndim) #find dimenssions
#to find number of rows and columns
print("shape :",a.shape)   

 


a=np.array([["xxx","yyy"],["zzz","pppp"]])
print(a)
print("array type ",type(a))
print("data type",a.dtype)
print("Dimensions: ",a.ndim) #find dimenssions
print("shape :",a.shape)

#create 2d array
a=np.array([[12,13,23,45,5],[45,22,45,55,10]])
print(a)
print("array type ",type(a))
print("data type",a.dtype)
print("Dimensions: ",a.ndim) #find dimenssions
print("shape :",a.shape)

#create 3D array
s=np.array([[[1,2,3],[10,20,30]],[[5,6,7],[8,9,10]]])
print(s)
print("array type ",type(s))
print("data type",s.dtype)
print("Dimensions: ",s.ndim) #find dimenssions
print("shape :",s.shape)


#add 0 to 11 in array b
b=np.arange(12)    #0..11
print(b)
c=np.arange(1,32,2).reshape(4,4)   #1,3,5,7,......31
print(c)
print(c)
print(c[1:3,2:4])  #to display portion of error and index starts from 0
print(c[1,-1])
print(c[:,-1]) #last column
print(c[-1,:]) #last row


#to arrange data columnwise
c=np.arange(1,32,2).reshape(4,4,order='f')
print(c)

c[2,1]=231
print(c)


a=np.zeros((3,3),dtype='int32')
b=np.ones((3,3))
c=np.empty((3,3)) # will have garbage value so random numbers will be printed
print(a)
print(b)
print(c)


#memberwise addition  
#FOR ADDTION dimensions of both matrix should be same
a=a+10
print(a)
a=a+b
print(a)  

b=b+2
print(b)
#memberwise multiplication
print(a*b)
#matrix multiplication
print(a.dot(b))




#identity matrix
a=np.identity(3)  #(3,3)
print(a)
b=np.eye(3,4)
print(b)
b=np.eye(4,5,k=-1)  
#-ve values of k  will give left side  diagonal to the main diagonal
#+ve  values of k  will give right side  diagonal to the main diagonal
print(b)



#filter the array

a=np.array([[12,13,14,15],[3,24,13,6],[10,18,1,2]])
#to search the value
print(a[a%6==0])
print(a[a>5])

#to search the position
print(np.where(a%2==0))
#all row values
lst=list(np.where(a%2==0)[0].data)
print(lst)
#all column values
lst1=list(np.where(a%2==0)[1].data)
print(lst1)

for pos in zip(lst,lst1):
    print(pos)




#to find sum of all values
print(np.sum(a,axis=1)) # row wise sum
print(np.sum(a,axis=0)) # column wise sum
print(np.sum(a)) # sum of all values

#to find mean
print(np.mean(a)) #find avg of entire array
print(np.mean(a,axis=1))#find avg of each row of the array
print(np.mean(a,axis=0))#find avg of each column of the array


#hstack and vstack
x=np.array([10,20,30])
y=np.array([11,12,13])
z=np.array([21,22,23])

#to arrange data horizontally
print(np.hstack((x,y,z)))

d=np.vstack((x,y,z))
print(np.vstack((x,y,z)))
print(type(d))


#revesrse columns and rows both 
#works like transpose
print(np.flip(d)) 
#flips the rows, the 1st row will be the last row
#last row will be the first row
print(np.flip(d,axis=1)) 
print(np.flip(d,axis=0))

#endpoint=False will exclude 30
x=np.linspace(10,30,7,endpoint=True)
print(x)

student=np.array([[40,50,48],[30,35,38],[44,38,48]])
print(student)

avmks=np.mean(student,axis=1) #[45,67,56]
print(avmks)
names=['student1','student2','student3']

import matplotlib.pyplot as plt
plt.pie(avmks,labels=names,colors=['r','y','b'],explode=(0.3,0,0),startangle=90)
#plt.pie(avmks,labels=names,colors=['r','y','b'])
plt.bar(names,avmks,color='cyan')

```

> [!Question] Note
> 
> # NumPy — Rapid Revision
> 
> **NumPy (Numerical Python)** is a Python library for **fast numerical/scientific computing** and handling **N-dimensional arrays**.
> 
> ```python
> import numpy as np
> ```
> 
> Its main array object is:
> 
> ```python
> np.ndarray
> ```
> 
> ### Key Properties
> 
> ```text
> ✓ N-dimensional arrays
> ✓ Usually homogeneous → elements have a common dtype
> ✓ Fast numerical operations
> ✓ Vectorized operations
> ✓ Broadcasting
> ✓ Matrix / linear algebra operations
> ```
> 
> ---
> 
> # 1. Creating Arrays
> 
> ### 1D Array
> 
> ```python
> a = np.array([12, 13, 23, 45])
> ```
> 
> ### 2D Array
> 
> ```python
> a = np.array([
>     [12, 13, 23],
>     [45, 22, 10]
> ])
> ```
> 
> ### 3D Array
> 
> ```python
> a = np.array([
>     [[1, 2, 3], [10, 20, 30]],
>     [[5, 6, 7], [8, 9, 10]]
> ])
> ```
> 
> Check array information:
> 
> ```python
> type(a)      # numpy.ndarray
> a.dtype      # data type
> a.ndim       # number of dimensions
> a.shape      # dimensions / shape
> a.size       # total number of elements
> a.itemsize   # bytes per element
> ```
> 
> Example:
> 
> ```python
> a = np.array([[1, 2, 3],
>               [4, 5, 6]])
> 
> a.shape       # (2, 3)
> a.ndim        # 2
> a.size        # 6
> ```
> 
> ---
> 
> # 2. Creating Arrays with Built-in Functions
> 
> ### `arange()`
> 
> Creates evenly spaced values using a **step**.
> 
> ```python
> np.arange(12)
> # 0 ... 11
> 
> np.arange(1, 10, 2)
> # 1 3 5 7 9
> ```
> 
> ### `zeros()`
> 
> ```python
> np.zeros((3, 3))
> ```
> 
> ### `ones()`
> 
> ```python
> np.ones((3, 3))
> ```
> 
> Specify data type:
> 
> ```python
> np.zeros((3, 3), dtype="int32")
> ```
> 
> ### `empty()`
> 
> Creates an array **without initializing its elements**.
> 
> ```python
> np.empty((3, 3))
> ```
> 
> The values should **not be relied upon** because they are whatever values happen to exist in the allocated memory.
> 
> ---
> 
> # 3. `linspace()`
> 
> Creates a specified number of **evenly spaced values** between two endpoints.
> 
> ```python
> np.linspace(10, 30, 5)
> # [10. 15. 20. 25. 30.]
> ```
> 
> ### `endpoint`
> 
> ```python
> np.linspace(10, 30, 5, endpoint=True)
> ```
> 
> Includes `30`.
> 
> ```python
> np.linspace(10, 30, 5, endpoint=False)
> ```
> 
> Excludes `30`.
> 
> ```text
> arange()   → control by step
> linspace() → control by number of values
> ```
> 
> ---
> 
> # 4. Indexing & Slicing
> 
> For a 2D array:
> 
> ```python
> a[row, column]
> ```
> 
> Example:
> 
> ```python
> a = np.array([
>     [10, 20, 30],
>     [40, 50, 60],
>     [70, 80, 90]
> ])
> 
> a[1, 2]      # 60
> ```
> 
> ### Slicing
> 
> ```python
> a[1:3, 1:3]
> ```
> 
> ### Entire column
> 
> ```python
> a[:, -1]
> ```
> 
> ### Entire row
> 
> ```python
> a[-1, :]
> ```
> 
> ```text
> :   → all values
> -1  → last index
> ```
> 
> ---
> 
> # 5. Reshaping
> 
> Change the shape without changing the data.
> 
> ```python
> a = np.arange(12)
> 
> b = a.reshape(3, 4)
> ```
> 
> ### Column-major / Fortran order
> 
> ```python
> a = np.arange(1, 17).reshape(4, 4, order="F")
> ```
> 
> ```text
> order="C" → row-major order (default)
> order="F" → column-major / Fortran order
> ```
> 
> Other useful operations:
> 
> ```python
> a.flatten()    # 1D copy
> a.ravel()      # 1D view when possible
> a.T            # transpose
> ```
> 
> ---
> 
> # 6. Element-wise Operations
> 
> Operations are normally performed **element by element**.
> 
> ```python
> a = np.array([1, 2, 3])
> b = np.array([10, 20, 30])
> 
> a + b
> # [11 22 33]
> 
> a * b
> # [10 40 90]
> 
> a ** 2
> # [1 4 9]
> ```
> 
> ### Broadcasting
> 
> A scalar can be applied to every element:
> 
> ```python
> a + 10
> a * 2
> ```
> 
> ```text
> [1, 2, 3] + 10
>       ↓
> [11, 12, 13]
> ```
> 
> Arrays can also broadcast against compatible shapes.
> 
> ---
> 
> # 7. Element-wise vs Matrix Multiplication
> 
> ```python
> A * B
> ```
> 
> → **Element-wise multiplication**
> 
> ```python
> A @ B
> ```
> 
> → **Matrix multiplication**
> 
> Also:
> 
> ```python
> A.dot(B)
> ```
> 
> performs matrix multiplication when dimensions are compatible.
> 
> ```text
> *       → element-wise multiplication
> @       → matrix multiplication
> .dot()  → matrix multiplication
> ```
> 
> ---
> 
> # 8. Identity & Diagonal Matrices
> 
> ### Identity Matrix
> 
> ```python
> np.identity(3)
> ```
> 
> Produces:
> 
> ```text
> 1 0 0
> 0 1 0
> 0 0 1
> ```
> 
> ### `eye()`
> 
> ```python
> np.eye(3, 4)
> ```
> 
> Creates a matrix with `1`s on the main diagonal.
> 
> ### `k`
> 
> Controls which diagonal is filled.
> 
> ```python
> np.eye(4, 5, k=-1)
> ```
> 
> ```text
> k = 0  → main diagonal
> k > 0  → diagonal above main diagonal
> k < 0  → diagonal below main diagonal
> ```
> 
> ---
> 
> # 9. Boolean Filtering
> 
> Filter elements using conditions.
> 
> ```python
> a = np.array([10, 15, 20, 25, 30])
> 
> a[a > 20]
> # [25 30]
> ```
> 
> Multiple conditions:
> 
> ```python
> a[(a > 10) & (a < 30)]
> ```
> 
> ```text
> &  → AND
> |  → OR
> ~  → NOT
> ```
> 
> Use parentheses around individual conditions.
> 
> Example:
> 
> ```python
> a[a % 6 == 0]
> ```
> 
> → values divisible by `6`.
> 
> ---
> 
> # 10. Finding Positions — `np.where()`
> 
> `np.where()` can find the **indices/positions** where a condition is true.
> 
> ```python
> a = np.array([
>     [12, 13, 14],
>     [3, 24, 13],
>     [10, 18, 1]
> ])
> 
> np.where(a % 2 == 0)
> ```
> 
> For a 2D array, it returns:
> 
> ```text
> (row_indices, column_indices)
> ```
> 
> Example concept:
> 
> ```python
> rows, cols = np.where(a % 2 == 0)
> ```
> 
> You can combine them:
> 
> ```python
> for position in zip(rows, cols):
>     print(position)
> ```
> 
> This gives the coordinates of matching elements.
> 
> ---
> 
> # 11. Aggregation Functions
> 
> ### Sum
> 
> ```python
> np.sum(a)
> ```
> 
> ### Row-wise
> 
> ```python
> np.sum(a, axis=1)
> ```
> 
> ### Column-wise
> 
> ```python
> np.sum(a, axis=0)
> ```
> 
> ### Mean
> 
> ```python
> np.mean(a)
> np.mean(a, axis=1)
> np.mean(a, axis=0)
> ```
> 
> Other useful functions:
> 
> ```python
> np.min(a)
> np.max(a)
> np.std(a)
> np.median(a)
> ```
> 
> ```text
> axis=0 → operate down rows → column-wise result
> 
> axis=1 → operate across columns → row-wise result
> ```
> 
> ---
> 
> # 12. Combining Arrays
> 
> ### `hstack()`
> 
> Horizontal combination:
> 
> ```python
> x = np.array([10, 20, 30])
> y = np.array([11, 12, 13])
> 
> np.hstack((x, y))
> ```
> 
> ### `vstack()`
> 
> Vertical combination:
> 
> ```python
> np.vstack((x, y))
> ```
> 
> Example:
> 
> ```text
> hstack:
> [10 20 30 11 12 13]
> 
> vstack:
> [10 20 30]
> [11 12 13]
> ```
> 
> Other useful operations:
> 
> ```python
> np.concatenate([a, b])
> ```
> 
> ---
> 
> # 13. Flipping Arrays
> 
> ### Flip everything
> 
> ```python
> np.flip(a)
> ```
> 
> Reverses the elements along all axes.
> 
> ### Flip rows
> 
> ```python
> np.flip(a, axis=0)
> ```
> 
> First row becomes last row.
> 
> ### Flip columns
> 
> ```python
> np.flip(a, axis=1)
> ```
> 
> Columns are reversed.
> 
> ```text
> axis=0 → flip rows
> axis=1 → flip columns
> ```
> 
> ---
> 
> # 14. Useful Array Functions
> 
> ```python
> np.unique(a)       # unique values
> np.sort(a)         # sorted values
> np.argmax(a)       # index of maximum
> np.argmin(a)       # index of minimum
> ```
> 
> ---
> 
> # 15. Random Numbers
> 
> Modern NumPy style:
> 
> ```python
> rng = np.random.default_rng()
> 
> rng.random(5)
> rng.integers(1, 10, size=5)
> rng.normal(size=5)
> ```
> 
> Useful for:
> 
> - Testing
>     
> - Simulations
>     
> - Sampling
>     
> - Machine learning
>     
> 
> ---
> 
> # 16. Linear Algebra
> 
> ```python
> A = np.array([
>     [1, 2],
>     [3, 4]
> ])
> ```
> 
> Useful operations:
> 
> ```python
> A @ B
> np.linalg.det(A)    # determinant
> np.linalg.inv(A)    # inverse
> ```
> 
> NumPy's `np.linalg` module provides many linear-algebra operations.
> 
> ---
> 
> # ⭐ NumPy Quick Revision
> 
> ```text
> NUMPY
> │
> ├── np.array()       → Create ndarray
> ├── ndarray          → NumPy array class
> ├── dtype             → Data type
> ├── ndim              → Number of dimensions
> ├── shape             → Dimensions
> ├── size              → Number of elements
> │
> ├── arange()          → Values using step
> ├── linspace()        → Evenly spaced values
> ├── zeros()           → Zero-filled array
> ├── ones()            → One-filled array
> ├── empty()           → Uninitialized array
> │
> ├── reshape()         → Change shape
> ├── flatten()         → Flatten to 1D
> ├── ravel()           → Flatten/view when possible
> ├── .T                → Transpose
> │
> ├── Boolean indexing  → Filter
> ├── where()           → Find positions
> │
> ├── sum()             → Sum
> ├── mean()            → Average
> ├── min()/max()       → Min/max
> ├── std()             → Standard deviation
> │
> ├── hstack()          → Horizontal stacking
> ├── vstack()          → Vertical stacking
> ├── flip()            → Reverse
> ├── unique()          → Unique values
> ├── sort()            → Sort
> │
> ├── identity()        → Identity matrix
> ├── eye()             → Diagonal matrix
> │
> ├── *                 → Element-wise multiplication
> ├── @                 → Matrix multiplication
> ├── dot()             → Matrix multiplication
> │
> ├── np.random         → Random numbers
> └── np.linalg         → Linear algebra
> ```
> 
> ## 🧠 Most Important Distinctions
> 
> ```text
> arange()       → step-based
> linspace()     → count-based
> 
> shape          → dimensions
> ndim           → number of dimensions
> size           → total elements
> 
> axis=0         → column-wise operation
> axis=1         → row-wise operation
> 
> *              → element-wise multiplication
> @              → matrix multiplication
> 
> a[a > 5]       → filter values
> np.where(...)  → find positions
> 
> hstack()       → horizontal
> vstack()       → vertical
> 
> zeros()        → initialized with 0
> ones()         → initialized with 1
> empty()        → not initialized
> 
> identity()     → square identity matrix
> eye()          → flexible diagonal matrix
> ```
> 
> ### Core idea
> 
> **NumPy = `ndarray` + vectorized operations + broadcasting + multidimensional indexing + numerical/linear-algebra operations.
>
> 
