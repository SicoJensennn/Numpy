# Chapter 2: More Python and Numpy

## Python

- Patterns
    1. Regular expressions. 
    
    `(r"[Aa]pple|[Bb]anana", str)`. In this example we saw a new special character `|` that denotes an alternative. On either side of the bar character we have a *subpattern*.
    
    ```python
    r"[A-Za-z_][A-Za-z_0-9]*\Z"
    ```
    
    - `A-Z` means all the characters from `A` to `Z`.
    - `*` means that we allow any number (0,1,2, . . . ) of the **previous subpattern**. For example the pattern `r"ba*"` allows strings `"b"`, `"ba"`, `"baa"`, `"baaa"`, and so on.
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled.png)
        
        ```python
        # a few examples
        `r"(ab)+"` 
        # “ab”,”abab”,”ababab”…
        
        `r"([a-z]{3,}) \1 \1"` 
        # "aca aca aca", but not the strings "aca aba aca" or "ac ac ac". 
        # `\number`：It looking for a repeated "word". 所以会有3遍aca
        ```
        
    - The special syntax `\Z`denotes the end of the string.
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%201.png)
        
        The pattern `\b` matches at the start or end of a word. The pattern `\B` does the reverse.
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%202.png)
        
    - To sum up, the regular expression is telling the computer to find the first character starts with letter or _, and then all the other characters start with letter, _, or number.
    
    The problem is that the pattern `.*` tries to match as many characters as possible. This is called *greedy matching*. 
    
    The hat character (`ˆ`) in the beginning of a character class means the complement character class. 
    
    - Function in re
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%203.png)
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%204.png)
        
- File process
    
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%205.png)
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%206.png)
    
    The attribute `sys.path`is the list of folders that Python uses to look for imported modules.
    
    The list `sys.argv` contains the so called *command line parameters*.
    
    The function `sys.exit` can be used to exit immediately your program. Usually the return value 0 means that the program ran successfully, and non-zero integer means that an error occurred.
    
- Sequences, iterables, generators: revisited
    
    In our class there needs to be a special method `__iter__` that returns an *iterator* for the container. An iterator is an object that has method `__next__`, which returns the next element from the container.
    
    ```python
    def mydate(day=1, month=1):   # Generates dates starting from the given date
        lengths=(31,28,31,30,31,30,31,31,30,31,30,31)   # How many days in a month
        first_day=day
        for m in range(month, 13):
            for d in range(first_day, lengths[m-1] + 1):
                yield (d, m)
            first_day=1
    # Create the generator by calling the function:        
    gen = mydate(26, 2)   # Start from 26th of February
    for i, (day, month) in enumerate(gen):   
        if i == 5: break                 # Print only the first five dates from the generator
        print(f"Index {i}, day {day}, month {month}")
    ```
    
    ```python
    def fab(max): 
        n, a, b = 0, 0, 1 
        while n < max: 
            yield b      # 使用 yield
            # print b 
            a, b = b, a + b 
            n = n + 1
     
    for n in fab(5): 
        print (n)
    ```
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%207.png)
    
- Class and Inheritance
    
    ```python
    class B(object):
        def f(self):
            print("Executing B.f")
        def g(self):
            print("Executing B.g")
        
    class C(B):
        def g(self):
            print("Executing C.g")
            
    x=C()
    x.f() # inherited from B
    x.g() # overridden by C
    ```
    
    就近原则
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%208.png)
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%209.png)
    
    `__eq__`, `__ge__`, `__gt__`, `__le__`, `__lt__`, and `__ne__` get called when the corresponding operators `x==y`, `x>=y`, `x>y`, `x<=y`, `x<y`, and `x!=y` are used.
    
- Try/Except
    
    ```python
    try:
        # here are the statements that can cause exceptions
    except (Exceptionname1, Exceptionname2, ...):
        # here we handle the exceptions
    else:
        # this gets executed if try-block caused no exceptions
    finally:
        # this is always executed, clean-up code
    ```
    
    ```python
    L=[1,2,3]
    try:
        print(L[3])
    except IndexError:
        print("Index does not exist")
    ```
    
- Iterate
    
    In our class there needs to be a special method `__iter__` that returns an *iterator* for the container. An iterator is an object that has method `__next__`, which returns the next element from the container.
    
    ```python
    class WeekdayIterator(object):
        """Iterator over the weekdays."""
        def __init__(self):
            self.i=0           # Start from Monday
            self.weekdays = ("Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday")
        def __iter__(self):    # If this object were a container, then this method would return the iterator over the 
                               # elements of the container.
            return self        # However, this object is already an iterator, hence we return self.
        def __next__(self):    # Returns the next weekday
            if self.i == 7:    
                raise StopIteration # Signal that all weekdays were already iterated over
            else:
                weekday = self.weekdays[self.i]
                self.i += 1
                return weekday
            
    for w in WeekdayIterator():
        print(w)
    ```
    

---

## Numpy

- Creation of arrays
    
    ```python
    import numpy as np
    np.array([1,2,3])  # one dimension
    np.array([[1,2,3], [4,5,6]])  # 2 dimension
    ```
    
    Some helper functions to create common types of arrays.
    
    ```python
    np.zeros((3,4))  # 3 rows 4 columns, value = 0
    np.array([[1,2,3], [4,5,6]])  
    np.zeros((3,4), dtype=int)  # specify the datatype
    np.ones((2,3))  # value = 1
    np.full((2,3), fill_value=7)  # value = 7
    np.empty((2,4))
    np.eye(5, dtype=int)  # 对角线矩阵 5 row 5 column
    np.arange(0,10,2)  # array([0, 2, 4, 6, 8])
    
    #For non-integer ranges it is better to use linspace:
    np.linspace(0, np.pi, 5)  # array([0, 0.78539816, 1.57079633, 2.35619449, 3.14159265])
    
    ```
    
- Random elements
    
    ```python
    np.random.random((3,4))
    np.random.normal(0, 1, (3,4))    
    # Elements are normally distributed with mean 0 and standard deviation 1
    np.random.randint(-2, 10, (3,4))
    ```
    
    1. seed
        
        to debug our program it needs to behave deterministically. If we always start from the same starting point. This starting point is usually an integer, and we call it a *seed*.
        
        ```python
        np.random.seed(0)
        print(np.random.randint(0, 100, 10))
        
        # If run the above cell multiple times, always give the same numbers
        ```
        
- Array types and attributes
    
    An array has several attributes: `ndim` tells the number of dimensions, `shape` tells the size in each dimension, `size` tells the number of elements, and `dtype` tells the element type.
    
    ```python
    # a = [[1 2 3]
    # [4 5 6]]
    print(f"{a.ndim}, shape {a.shape}, size {a.size}, and dtype {a.dtype}:")
    # 2, shape (2, 3), size 6, and dtype int64:
    ```
    
- Index, slicing, reshape
    
    Just like list.
    
    ```python
    b=np.array([[1,2,3], [4,5,6]])
    print(b[1,2])    # row index 1, column index 2: 6
    ```
    
    Modification through indexing:
    
    ```python
    b[0,0] = 10
    ```
    
    ```python
    print(b)
    # [[10 2 3]
    # [ 4 5 6]]
    print(b[:,0])  # 逗号前面是行，后面是列。第一列的
    # [10 4]
    print(b[0,:])  # 第一行的
    # [10 2 3]
    print(b[:,1:])  # 第2列及以后的列
    # [[2 3]
    # [5 6]]
    
    ```
    
    Reshape
    
    ```python
    a=np.arange(9)  # [0 1 2 3 4 5 6 7 8]
    anew=a.reshape(3,3)
    # [[0 1 2]
    # [3 4 5]
    # [6 7 8]]
    ```
    
    `np.newaxis` keyword could create column or row vectors.
    
    ```python
    info("drow", d[:, np.newaxis])  # 行变为列
    info("drow", d[np.newaxis, :])  # array变为行，2D
    ```
    
    ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2010.png)
    
- Concatenation, splitting and stacking
    1. concatenate
        
        ```python
        np.concatenate((a,b))
        ```
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2011.png)
        
        ```python
        print("New row:")
        print(np.concatenate((c,a.reshape(1,2))))
        print("New column:")
        print(np.concatenate((c,a.reshape(2,1)), axis=1))
        ```
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2012.png)
        
    2. stack
        
        Use `stack` to create higher dimensional arrays from lower dimensional arrays.
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2013.png)
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2014.png)
        
    3. split
        
        Inverse operation of `concatenate` is `split`. Its argument specifies either the number of equal parts the array is divided into, or it specifies explicitly the break points.
        
        ```python
        d=np.arange(12).reshape(2, 6)
        d1,d2 = np.split(d, 2)  # 3 rows, 3 rows
        
        # If indices_or_sections is a 1-D array of sorted integers
        # the entries indicate where along axis the array is split.
        parts=np.split(d, (2,3,5), axis=1)  # 2,3,5 index分割线
        for i, p in enumerate(parts):
            print("part %i:" % i)
            print(p)
        ```
        
        ![Untitled](Chapter%202%20More%20Python%20and%20Numpy%20c1d0b0034614494f87872bfd992d46b9/Untitled%2015.png)
        
    4. 
- Broadcasting
    
    Add 4 to all elements of an array with the following expression.
    
    ```python
    np.arange(3) + np.array([4])
    # array([4, 5, 6])
    np.arange(3) + 4
    ```
    
    NumPy tries to stretch the arrays to have the same shape.
    
    ```python
    a=np.full((3,3), 5)
    b=np.arange(3)
    print("a:", a, sep="\n")
    print("b:", b)
    print("a+b:", a+b, sep="\n")
    ```
    
    ```
    5 5 5     0 1 2       5 6 7
    5 5 5  +  0 1 2   =   5 6 7
    5 5 5     0 1 2       5 6 7
    ```
    
    规则：(m, n) ， 一维的必须是(m,) / (, n).