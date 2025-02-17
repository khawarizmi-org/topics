# Recursion

Recursion is a fundamental concept in programming where a function calls itself to solve a problem.

#### What happens when we call a function?

- The current function will stop, and a new function will start execution.
- Once the new function finishes execution, the initial function will continue from the same place it stopped. 

#### What happens when the function we called calls another function?

We can think of the execution of our program as a stack of functions. When we start our program, it pushes the main function into this stack, and with each call, it stops at some point and pushes the new function to the stack.

The function’s parameters and local variables are stored in a new stack frame on the call stack.

Once a function finishes execution, it will be popped from the stack, and the execution will continue from where it was called.


# **Example: Execution Trace and Stack Call Representation**

## **Code:**

```cpp
#include <iostream>

using namespace std;

int B(int a, int b) {
    return a + b;
}

int A(int a, int b, int c) {
    return B(a, b) + c;
}

int main() {
    cout << A(1, 2, 3) << endl;
}

```

----------

## **Step-by-Step Execution**

### **1. `main()` is called**

-   The program starts execution from `main()`.
-   Inside `main()`, the function `A(1, 2, 3)` is called.

### **2. `A(1, 2, 3)` is called**

-   Control jumps to `A()`, and it starts execution.
-   Inside `A()`, it calls `B(1, 2)`.

### **3. `B(1, 2)` is called**

-   Control jumps to `B()`, and it starts execution.
-   `B(1, 2)` computes `1 + 2 = 3` and returns `3` to `A()`.

### **4. `A(1, 2, 3)` resumes**

-   `A(1, 2, 3)` now computes `B(1, 2) + c = 3 + 3 = 6` and returns `6` to `main()`.

### **5. `main()` resumes**

-   `main()` receives `6` from `A(1, 2, 3)`.
-   It prints `"6"` to the console.
-   `main()` finishes execution and the program exits.

----------

## **Call Stack Representation**

### **Step 1: `main()` is called**

```
Stack:
------------------
| main()         |
------------------
```

### **Step 2: `A(1, 2, 3)` is called**

```
Stack:
------------------
| A(1, 2, 3)     |
------------------
| main()         |
------------------
```

### **Step 3: `B(1, 2)` is called from `A(1, 2, 3)`**

```
Stack:
------------------
| B(1, 2)        |
------------------
| A(1, 2, 3)     |
------------------
| main()         |
------------------
```

### **Step 4: `B(1, 2)` Returns `3` and is Popped**

```
Stack:
------------------
| A(1, 2, 3)     |
------------------
| main()         |
------------------
```

### **Step 5: `A(1, 2, 3)` Returns `6` and is Popped**

```
Stack:
------------------
| main()         |
------------------

```

### **Step 6: `main()` Prints `6` and Exits (Stack is Empty)**

```
(Stack is now empty)
```
----------

## **Final Output:**

```
6
```
----------



#### What happens when the function calls it self?

When a function calls itself, it pushes a new instance of the function onto the stack. Each instance of the function is treated as a separate execution, and the function does not repeat itself.

#### How we can use recursion to solve problems? 

Recursion allows us to break down a complex problem into smaller subproblems. We solve the smaller problem first and then use its result to solve the larger problem. To use recursion effectively, we need to reduce the current problem into a simpler version that we can solve.

For example, let's try to find the value of $n!$.

We know what:
$n!$ = $(n - 1) \times (n - 2) \times (n - 3) \times ... \times 2 \times 1$.

We need to find a way to express this as a recurrence relation.

The relation is:
$n!$ = $n \times (n - 1)!$.

So, if we know the value of $(n - 1)!$, we can compute $n!$ by multiplying $(n - 1)!$ by $n$.

If we want to find $5!$, for example, we first need to find $4!$, and to find $4!$, we need $3!$, and so on.

Without recursion, we would need to write separate functions to compute each factorial, which would be inefficient, especially when $n$ is large.

Here, recursion allows us to write just one function to calculate $n!$.

Let's look at the following function:

```c++
int factorial(int n){
	return factorial(n - 1) * n;
}
```

What this function does is compute the value of $(n - 1)!$ first and multiply it by $n$ to get $n!$.

However, the problem is that when we call `factorial(n)`, it will call `factorial(n - 1)`, then `factorial(n - 2)`, and so on, creating an infinite loop because there's no stopping condition.

To fix this, we need to define a base case that terminates the recursion.

We know that $0! = 1$, so we can use this as the base case where the recursion stops.

The recurrence relation should be:

$n! = \begin{cases} 1 & \text{if } n = 0 \\ n \times (n - 1)! & \text{otherwise} \end{cases}$

Now we can rewrite the factorial function to include this base case:

```c++
int factorial(int n){
	if(n == 0)
		return 1;
	return factorial(n - 1) * n;
}
```

Now what will happen when we call ```cout << factorial(3) << endl```



----------

## **Step-by-Step Execution**

###  **1. `factorial(3)` is called**

### **3. `factorial(3)` will call `factorial(2)`**

### **3. `factorial(2)` will call `factorial(1)`**

### **4. `factorial(1)` will call `factorial(0)`**

### **5. `factorial(0)` will return $1$**

### **6. `factorial(1)` will return $1 \times 1 = 1$**

### **7. `factorial(2)` will return $1 \times 2 = 2$**

### **8. `factorial(3)` will return $2 \times 3 = 6$**

### **9. `cout << factorial(3) << endl`  will print $6$**

----------

## **Call Stack Representation**

### **Step 1: `factorial(3)` is called**

```
Stack:
------------------
| factorial(3)   |
------------------
```

### **Step 2: `factorial(2)` is called from `factorial(3)`**

```
Stack:
------------------
| factorial(2)   |
------------------
| factorial(3)   |
------------------
```

### **Step 3: `factorial(1)` is called from `factorial(2)`**

```
Stack:
------------------
| factorial(1)   |
------------------
| factorial(2)   |
------------------
| factorial(3)   |
------------------
```

### **Step 4: `factorial(0)` is called from `factorial(1)`**

```
Stack:
------------------
| factorial(0)   |
------------------
| factorial(1)   |
------------------
| factorial(2)   |
------------------
| factorial(3)   |
------------------
```

### **Step 5: `factorial(0)` Returns `1` and is Popped**

```
Stack:
------------------
| factorial(1)   |
------------------
| factorial(2)   |
------------------
| factorial(3)   |
------------------
```

### **Step 6: `factorial(1)` Returns `1` and is Popped**

```
Stack:
------------------
| factorial(2)   |
------------------
| factorial(3)   |
------------------
```

### **Step 7: `factorial(2)` Returns `2` and is Popped**

```
Stack:
------------------
| factorial(3)   |
------------------
```

### **Step 8: `factorial(3)` Returns `6` and is Popped**

```
Stack:
(Stack is now empty)
```

## **Final Output:**

```
6
```
----------
