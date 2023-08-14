# A first look at Mojo

| Status | Version | Last updated  | Description                                       |
|--------|---------|---------------|---------------------------------------------------|
| Draft  | 0.1     | 2023-08-14    | Early draft of the project README.                |


## What is Mojo?

Mojo is a programming language that is designed to be a superset of Python, meaning that any valid Python code is also valid Mojo code. However, Mojo also introduces new syntax and features that allow you to write faster, safer, and more expressive code than Python. Some of these features are inspired by Rust, another modern language that focuses on performance and memory safety.

Mojo is not interpreted like Python, but compiled ahead-of-time to native machine code using the LLVM toolchain. This means that Mojo programs can run much faster than Python programs, especially when using Mojo-specific features such as manual memory management, vectorization, parallelization, and tiling.

Mojo is still in the very early stages of development, so it is not yet available for download on your own system. However, you can try it out online using the Modular Playground, a web-based Jupyter Notebook environment that runs on Modular's servers. Modular is the company that created Mojo and provides early access to it.

## How to use the Modular Playground

The Modular Playground is a web-based interactive environment where you can write and run Mojo code. It looks like a Jupyter Notebook, which is a popular tool for data science and machine learning in Python. You can create cells that contain either code or text (using Markdown), and execute them one by one or all at once.

The Playground comes with a few example notebooks that demonstrate some of the features and use cases of Mojo. You can open them from the File menu or create your own notebook from scratch. You can also save your notebooks to your Google Drive account or download them as .ipynb files.

To run a cell, you can either click the Run button on the toolbar or press Shift+Enter on your keyboard. The output of the cell will appear below it. You can also use the keyboard shortcuts Ctrl+Enter to run a cell and stay in it, or Alt+Enter to run a cell and insert a new one below it.

To use existing Python modules in your Mojo code, you need to import them using the pyimport keyword instead of the regular import keyword. This tells Mojo to use the Python runtime to load and execute the module. For example, to use numpy and matplotlib in your Mojo code, you would write:

```mojo
pyimport numpy as np
pyimport matplotlib.pyplot as plt
```

Note that using pyimport has a performance cost, since it involves switching between the Mojo compiler and the Python interpreter. Therefore, you should only use it when you need to access functionality that is not available in Mojo or when you want to compare the performance of Mojo and Python code.

## How to write Mojo code

As mentioned before, any valid Python code is also valid Mojo code. However, if you want to take advantage of Mojo's speed and safety features, you need to use some new syntax and keywords that are specific to Mojo. In this section, we will introduce some of these features and show you how they differ from Python.

### Variable declarations

In Python, you can assign any value to any variable without declaring its type or mutability. For example:

```python
x = 42 # x is an integer
x = "Hello" # x is now a string
x = [1, 2, 3] # x is now a list
x[0] = 4 # x is mutable
```

In Mojo, you can still do this if you want to write Python-like code. However, if you want to write more efficient and safe code, you can use the keywords let and var to declare variables that are immutable or mutable respectively. You can also optionally specify the type of the variable using a colon after its name. For example:

```mojo
let x: int = 42 # x is an immutable integer
var y: str = "Hello" # y is a mutable string
let z = [1, 2, 3] # z is an immutable list with inferred type
y = "World" # OK, y is mutable
z[0] = 4 # Error, z is immutable
```

Using let and var has several benefits over using plain assignment statements:

- It makes your code more readable and explicit by indicating the intention and expectation of the variable.
- It allows the compiler to check for errors such as assigning incompatible types or mutating immutable references at compile time rather than at run time.
- It allows the compiler to optimize your code better by eliminating unnecessary checks and conversions.

### Function definitions

In Python, you can define functions using the def keyword without specifying the types or mutability of the parameters or the return value. For example:

```python
def add(x, y):
    return x + y
```

In Mojo, you can still do this if you want to write Python-like code. However, if you want to write more efficient and safe code, you can use the fn keyword to define functions that are Mojo-specific. You need to specify the types and mutability of the parameters and the return value using colons and let or var keywords. For example:

```mojo
fn add(let x: int, let y: int) -> int:
    return x + y
```

Using fn has several benefits over using def:

- It makes your code more readable and explicit by indicating the types and mutability of the parameters and the return value.
- It allows the compiler to check for errors such as passing incompatible types or mutating immutable parameters at compile time rather than at run time.
- It allows the compiler to optimize your code better by eliminating unnecessary checks and conversions.

Note that in Mojo, parameters are immutable by default, unless you use the var keyword to indicate that they are mutable. This is different from Python, where parameters are mutable by default, unless you use the global keyword to indicate that they are global. For example:

```python
def increment(x):
    x += 1 # OK, x is mutable
    global y
    y += 1 # OK, y is global

x = 10
y = 20
increment(x)
print(x) # 10, x is unchanged
print(y) # 21, y is incremented
```

```mojo
fn increment(var x: int):
    x += 1 # OK, x is mutable
    y += 1 # Error, y is not in scope

let x: int = 10
let y: int = 20
increment(x)
print(x) # 11, x is incremented
print(y) # 20, y is unchanged
```

### Struct definitions

In Python, you can define classes using the class keyword without specifying the types or mutability of the attributes. For example:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(1, 2)
p.x = 3 # OK, p.x is mutable
```

In Mojo, you can still do this if you want to write Python-like code. However, if you want to write more efficient and safe code, you can use the struct keyword to define structs that are Mojo-specific. You need to specify the types and mutability of the attributes using colons and let or var keywords. For example:

```mojo
struct Point {
    let x: int
    let y: int
}

let p: Point = Point {x: 1, y: 2}
p.x = 3 # Error, p.x is immutable
```

Using struct has several benefits over using class:

- It makes your code more readable and explicit by indicating the types and mutability of the attributes.
- It allows the compiler to check for errors such as assigning incompatible types or mutating immutable attributes at compile time rather than at run time.
- It allows the compiler to optimize your code better by using fixed layouts and avoiding dynamic allocation.

Note that in Mojo, structs are value types, meaning that they are copied by value rather than by reference when assigned or passed as parameters. This is different from Python, where classes are reference types, meaning that they are copied by reference when assigned or passed as parameters. For example:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p1 = Point(1, 2)
p2 = p1 # p2 is a reference to p1
p2.x = 3 # p1.x is also changed

print(p1.x) # 3
print(p2.x) # 3
```

```mojo
struct Point {
    let x: int
    let y: int
}

let p1: Point = Point {x: 1, y: 2}
let p2: Point = p1 # p2 is a copy of p1
p2.x = 3 # p1.x is unchanged

print(p1.x) # 1
print(p2.x) # 3
```

## How to write fast Mojo code

One of the main goals of Mojo is to enable you to write fast code without sacrificing readability or safety. In this section, we will introduce some of the techniques and features that allow you to achieve high performance with Mojo.

### Type annotations

One of the simplest ways to improve the performance of your Mojo code is to add type annotations to your variables and functions. Type annotations tell the compiler what kind of values you expect to use in your code, which allows it to generate more efficient machine code and avoid unnecessary checks and conversions.

For example, consider the following Python function that computes the sum of a list of numbers:

```python
def sum_list(lst):
    total = 0
    for x in lst:
        total += x
    return total
```

This function works for any kind of list, but it is not very fast, because the compiler has to infer the types of the variables and perform dynamic dispatch for the addition operation. If we know that the list contains only integers, we can add type annotations to the function using the fn keyword and the colon syntax:

```mojo
fn sum_list(let lst: [int]) -> int:
    let total: int = 0
    for let x: int in lst:
        total += x
    return total
```

This function is much faster, because the compiler knows that the variables are integers and can use a single instruction for the addition operation. It also avoids creating temporary objects or performing type conversions.

You can use type annotations for any type that Mojo supports, such as bool, float, str, [T], (T1, T2), {T}, etc. You can also define your own types using struct or enum keywords, which we will cover later.

Type annotations are optional in Mojo, but they are highly recommended if you want to write fast and safe code. They also make your code more readable and explicit by indicating your intention and expectation.

### Memory management

Another way to improve the performance of your Mojo code is to use manual memory management instead of relying on automatic garbage collection. Garbage collection is a feature that frees memory automatically when it is no longer needed by your program. However, it also has some drawbacks, such as unpredictable pauses, memory fragmentation, and overhead.

Mojo allows you to control how memory is allocated and deallocated using the keywords new and delete. These keywords allow you to create and destroy objects on the heap, which is a region of memory that can grow and shrink dynamically. For example:

```mojo
let p: Point = new Point {x: 1, y: 2} # create a new Point object on the heap
print(p.x) # 1
delete p # destroy the Point object and free its memory
print(p.x) # Error, p is invalid
```

Using new and delete has several benefits over relying on garbage collection:

- It gives you more control over when and how memory is used by your program.
- It reduces memory usage and fragmentation by avoiding unnecessary allocations and deallocations.
- It eliminates garbage collection pauses and overhead by avoiding runtime checks and tracing.

However, using new and delete also has some risks, such as memory leaks, double frees, dangling pointers, etc. These are errors that occur when you forget to free memory that is no longer needed, free memory that is still needed, or access memory that has been freed. These errors can cause your program to crash or behave unpredictably.

To avoid these errors, you need to follow some rules when using new and delete:

- Always pair new with delete for every object that you create on the heap.
- Never delete an object that was not created with new or that has already been deleted.
- Never use a pointer that points to an object that has been deleted or that is out of scope.

Mojo helps you follow these rules by using a feature called ownership. Ownership is a system that tracks which variable owns an object on the heap and ensures that only one variable can own an object at a time. When a variable goes out of scope or is reassigned, it automatically deletes its owned object if it has one. For example:

```mojo
fn main():
    let p1: Point = new Point {x: 1, y: 2} # p1 owns a new Point object
    let p2: Point = p1 # p2 takes ownership of the Point object from p1
    print(p1.x) # Error, p1 no longer owns the Point object
    print(p2.x) # 1
    # p2 goes out of scope and deletes its owned Point object

main()
```

Ownership helps you avoid memory leaks and double frees by ensuring that every object on the heap has exactly one owner at any time. However, it also limits what you can do with pointers, such as passing them as parameters or returning them from functions. To overcome this limitation, you can use another feature called borrowing. Borrowing is a system that allows you to temporarily use an object without taking ownership of it. You can borrow an object using the & operator, which creates a reference to it. For example:

```mojo
fn print_point(let p: &Point):
    print(p.x, p.y)

fn main():
    let p: Point = new Point {x: 1, y: 2} # p owns a new Point object
    print_point(&p) # borrow p and pass it as a reference
    print(p.x) # 1, p still owns the Point object
    # p goes out of scope and deletes its owned Point object

main()
```

Borrowing helps you avoid dangling pointers and invalid accesses by ensuring that you can only use an object as long as its owner is alive. However, it also imposes some restrictions on how you can borrow an object, such as:

- You can only borrow an object that is owned by a variable or a constant.
- You can only borrow an object that is immutable or that is mutable and not already borrowed.
- You can only borrow an object for a limited scope or lifetime.

Mojo enforces these restrictions using a feature called lifetimes. Lifetimes are annotations that tell the compiler how long a reference is valid for. You can specify lifetimes using apostrophes and names, such as 'a, 'b, 'c, etc. For example:

```mojo
fn longest<'a>(let x: &'a str, let y: &'a str) -> &'a str:
    if x.len() > y.len() {
        return x
    } else {
        return y
    }

fn main():
    let s1: str = "Hello"
    let s2: str = "World"
    let s3: &str = longest(&s1, &s2) # s3 borrows from s1 or s2
    print(s3) # Hello

main()
```

Lifetimes help you avoid lifetime errors by ensuring that you can only return or store a reference that lives as long as its owner. However, they also require you to annotate your functions and structs with lifetimes when they involve references. This can make your code more verbose and complex.

Mojo helps you avoid this complexity by using a feature called lifetime elision. Lifetime elision is a system that allows you to omit lifetimes in some cases where the compiler can infer them automatically. For example, the previous function can be written without lifetimes as:

```mojo
fn longest(let x: &str, let y: &str) -> &str:
    if x.len() > y.len() {
        return x
    } else {
        return y
    }
```

The compiler will infer the lifetimes of the parameters and the return value based on some rules, such as:

- Each reference parameter gets its own lifetime.
- If there is exactly one reference parameter, its lifetime is assigned to the return value.
- If there are multiple reference parameters, but one of them is self or &self, its lifetime is assigned to the return value.

Lifetime elision helps you write concise and clear code by avoiding unnecessary lifetimes in most cases. However, it does not work in all cases, so you still need to understand how lifetimes work and how to use them when needed.

### Vectorization

Another way to improve the performance of your Mojo code is to use vectorization instead of looping over arrays or lists. Vectorization is a feature that allows you to apply an operation to multiple elements at once using a single instruction. For example:

```mojo
let x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
let z: [int] = x + y # vectorized addition
print(z) # [5, 7, 9]
```

This code is much faster than using a loop to add each element individually, because it uses a single instruction that operates on multiple elements in parallel. It also avoids creating temporary objects or performing type conversions.

You can use vectorization for any arithmetic or logical operation that Mojo supports, such as +, -, *, /, %, &, |, ^, etc. You can also use vectorization for comparison operations, such as ==, !=, <, >, <=, >= etc. For example:

```mojo
let x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
let z: [bool] = x < y # vectorized comparison
print(z) # [true, true, true]
```

You can also use vectorization for assignment operations, such as +=, -=, *= etc. For example:

```mojo
var x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
x += y # vectorized assignment
print(x) # [5, 7 ,9]
```

Vectorization works for any type that Mojo supports for arrays or lists. However

OK, let's continue with the tutorial on how to write fast Mojo code.

### Vectorization

Another way to improve the performance of your Mojo code is to use vectorization instead of looping over arrays or lists. Vectorization is a feature that allows you to apply an operation to multiple elements at once using a single instruction. For example:

```mojo
let x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
let z: [int] = x + y # vectorized addition
print(z) # [5, 7, 9]
```

This code is much faster than using a loop to add each element individually, because it uses a single instruction that operates on multiple elements in parallel. It also avoids creating temporary objects or performing type conversions.

You can use vectorization for any arithmetic or logical operation that Mojo supports, such as +, -, *, /, %, &, |, ^, etc. You can also use vectorization for comparison operations, such as ==, !=, <, >, <=, >= etc. For example:

```mojo
let x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
let z: [bool] = x < y # vectorized comparison
print(z) # [true, true, true]
```

You can also use vectorization for assignment operations, such as +=, -=, *= etc. For example:

```mojo
var x: [int] = [1, 2, 3]
let y: [int] = [4, 5, 6]
x += y # vectorized assignment
print(x) # [5, 7 ,9]
```

Vectorization works for any type that Mojo supports for arrays or lists. However, it works best for numeric types such as int and float. For other types such as str and bool, vectorization may not be faster or even possible.

To use vectorization effectively in your Mojo code, you need to follow some rules and tips:

- Use arrays instead of lists whenever possible. Arrays are fixed-length and homogeneous collections of values that are stored contiguously in memory. Lists are variable-length and heterogeneous collections of values that are stored as linked nodes in memory. Arrays are more suitable for vectorization because they allow faster and easier access to the elements.
- Use the same type for the operands and the result of the operation. If the types are different, the compiler may need to perform type conversions or promotions that can slow down the execution.
- Use the same length for the operands and the result of the operation. If the lengths are different, the compiler may need to perform padding or truncation that can waste memory or cause errors.
- Use simple operations that can be performed in parallel without dependencies or side effects. If the operations are complex or sequential, the compiler may not be able to vectorize them or may need to use extra instructions that can reduce the performance.

### Parallelization

Another way to improve the performance of your Mojo code is to use parallelization instead of running your code on a single thread. Parallelization is a feature that allows you to run your code on multiple threads or cores simultaneously. For example:

```mojo
fn main():
    let x: int = 10
    let y: int = 20
    spawn fn():
        print(x + y) # run on a new thread
    print(x * y) # run on the main thread

main()
```

This code is faster than running both operations on the main thread sequentially, because it uses two threads that can execute independently and concurrently. It also avoids blocking the main thread by waiting for the result of the other thread.

You can use parallelization for any code that can be executed independently and concurrently without interfering with each other. You can also use parallelization for code that needs to communicate or synchronize with each other using channels or locks.

To use parallelization effectively in your Mojo code, you need to follow some rules and tips:

- Use spawn instead of thread.start whenever possible. Spawn is a keyword that creates a new thread and runs a function on it. Thread.start is a function that creates a new thread object and returns it. Spawn is more convenient and efficient than thread.start because it does not require you to create or manage thread objects.
- Use let instead of var for variables that are shared between threads. Let is a keyword that declares immutable variables that cannot be changed after initialization. Var is a keyword that declares mutable variables that can be changed after initialization. Let is more suitable for parallelization because it avoids race conditions and data corruption that can occur when multiple threads try to access or modify the same variable.
- Use channels instead of locks whenever possible. Channels are data structures that allow threads to send and receive messages asynchronously. Locks are data structures that allow threads to access shared resources exclusively. Channels are more suitable for parallelization because they avoid deadlocks and contention that can occur when multiple threads try to acquire or release the same lock.

### Tiling

Another way to improve the performance of your Mojo code is to use tiling instead of processing large data sets at once. Tiling is a feature that allows you to divide a large data set into smaller chunks and process them separately or in parallel. For example:

```mojo
let x: [int] = [1, 2, 3, 4, 5, 6, 7, 8]
let y: [int] = [9, 10, 11, 12, 13, 14, 15, 16]
let z: [int] = x + y # process the whole arrays at once
print(z) # [10, 12, 14, 16, 18, 20, 22, 24]

let x: [int] = [1, 2, 3, 4, 5, 6, 7, 8]
let y: [int] = [9, 10, 11, 12, 13, 14, 15, 16]
let z: [int] = []
for let i: int in range(0, len(x), 4): # divide the arrays into chunks of size 4
    let x_chunk: [int] = x[i:i+4] # get a chunk of x
    let y_chunk: [int] = y[i:i+4] # get a chunk of y
    let z_chunk: [int] = x_chunk + y_chunk # process the chunks separately
    z += z_chunk # append the result to z
print(z) # [10, 12, 14, 16, 18, 20 ,22 ,24]
```

This code is faster than processing the whole arrays at once, because it reduces the memory usage and bandwidth by processing smaller chunks. It also allows parallelization by processing different chunks on different threads or cores.

You can use tiling for any data set that can be divided into smaller chunks and processed separately or in parallel. You can also use tiling for data sets that are too large to fit in memory or cache.

To use tiling effectively in your Mojo code, you need to follow some rules and tips:

- Use slicing instead of indexing whenever possible. Slicing is a feature that allows you to get a subarray or sublist from an array or list using the colon syntax. Indexing is a feature that allows you to get a single element from an array or list using the bracket syntax. Slicing is more suitable for tiling because it avoids copying the elements and creating temporary objects.
- Use a suitable chunk size for your data set and your hardware. The chunk size is the number of elements that you process in each iteration. The chunk size should be large enough to avoid overhead and small enough to fit in memory or cache. The optimal chunk size may vary depending on your data set and your hardware.
- Use parallelization if possible and appropriate. Parallelization is a feature that allows you to run your code on multiple threads or cores simultaneously. Parallelization can improve the performance of tiling by processing different chunks concurrently. However, parallelization may not be possible or appropriate if your code has dependencies or side effects between chunks.

## Conclusion

In this tutorial, we have learned some of the main features and differences of Mojo compared to Python. We have also learned some of the techniques and features that allow us to write fast and safe code with Mojo. We hope that you have enjoyed this tutorial and that you are interested in learning more about Mojo.

If you want to try out Mojo online using the Modular Playground, you can visit this link. If you want to learn more about Mojo's syntax and features, you can visit this link. If you want to see some examples and use cases of Mojo code, you can visit this link.

Thank you for your attention and happy coding! 😊

[Modular Playground](https://playground.modular.com/mojo)
[Mojo Documentation](https://docs.modular.com/moj)
[Examples of Mojo programs](https://examples.modular.com/mojo)

