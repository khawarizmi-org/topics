# Stack
## Definition

Stack is an STL used to store and load items using the LIFO System (last in first out), which means items can be added to the end of the stack only and the last item added is the only item that can be accessed (get, edit or remove).



In the stack, all operations are done on one end, the right end, which is usually referred to as the "top".

## Stack Operations
There are some operations used in stacks to add new items, access items in the stack, and remove items:

* Add: insert a new item to the right end of the stack (top).
* Remove: delete the last item added to the stack (the item at the top)
* Get: returns the last item added to the stack (the item at the top)

All operations supported by the stack have time complicity of $O(1)$ so the overall complexity of the stack is $O(1)$ the space complicity of the stack is $O(n)$ where $n$ is the number of items.

## Example on Stack
Let's observe how the stack works when we apply the following operations in the same order they were given:

1. Add 2
2. Add 3
3. Get
4. Add 0
5. Remove
6. Add 1
7. Remove
8. Get

<div align="center">
    <img src="images/example_1_1.png" alt="Empty stack">
    <img src="images/example_1_2.png" alt="Add 2 to the stack">
    <p><em>(1) In the beginning the stack is empty, after the first operation 2 will be added to the top of the stack</em></p>
</div>

<div align="center">
    <img src="images/example_2_1.png" alt="Stack with 2">
    <img src="images/example_2_2.png" alt="Add 3 to the stack">
    <p><em>(2) After the second operation 3 will be added to the top of the stack</em></p>
</div>

<div align="center">
    <img src="images/example_3_1.png" alt="Stack with 2 and 3">
    <p><em>(3) The third operation is a Get operation and the result is the item in the top (3)</em></p>
</div>

<div align="center">
    <img src="images/example_4_1.png" alt="Stack with 2 and 3">
    <img src="images/example_4_2.png" alt="Add 0 to the stack">
    <p><em>(4) The fourth operation is an Add 0 operation so 0 is added to the top of the stack</em></p>
</div>

<div align="center">
    <img src="images/example_5_1.png" alt="Stack with 2, 3, and 0">
    <img src="images/example_5_2.png" alt="Remove top element (0)">
    <p><em>(5) The fifth operation is a Remove operation so the item at the top of the stack is removed</em></p>
</div>

<div align="center">
    <img src="images/example_6_1.png" alt="Stack with 2 and 3">
    <img src="images/example_6_2.png" alt="Add 1 to the stack">
    <p><em>(6) The Sixth operation is an Add 1 operation so 1 is added to the top of the stack</em></p>
</div>

<div align="center">
    <img src="images/example_7_1.png" alt="Stack with 2, 3, and 1">
    <img src="images/example_7_2.png" alt="Remove top element (1)">
    <p><em>(7) The seventh operation is a Remove operation so the item at the top of the stack is removed</em></p>
</div>

<div align="center">
    <img src="images/example_8_1.png" alt="Stack with 2 and 3">
    <p><em>(8) The eighth operation is a Get operation and the result is the item in the top (3)</em></p>
</div>


## Stack Implementation
```c++
#include 
#include //Stack header

using namespace std;

int main(){
    /*
    In C++:
        push is used to add items to the stack
        pop is used to remove the last item in the stack
        top is used to get the last item in the stack
    */

    //Create an empty stack
    stack<int> items;

    //Add 2
    items.push(2);
    //2 is added to the end of the stack //items = 2 <- item in the top
        
    //Add 3
    items.push(3);
    //items = 2, 3 <- item in the top

    //Get
    cout << "Item on top is: " << items.top() << endl;
    //Prints the item in the top of the stack (3)

    //Add 0
    items.push(0);
    //items = 2, 3, 0 <- item in the top

    //Remove
    items.pop();
    //Removes the item in the top of the stack (0)//items = 2, 3 <- item in the top

    //Add 1
    items.push(1);
    //items = 2, 3, 1 <- item in the top

    //Remove
    items.pop();
    //Removes the item in the top of the stack (1)//items = 2, 3 <- item in the top

    //Get
    cout << "Item on top is: " << items.top() << endl;
    //Prints the item in the top of the stack (3)
}
```
```plaintext
Output:
Item on top is: 3
Item on top is: 3
```
