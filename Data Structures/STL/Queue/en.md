# Queue

## Definition

A queue is an STL used to store and load items using the FIFO System (First in first out), which means items can be added to the end of the queue, and only the items in the front can be accessed (the items that were first added), We can also look at the queue as a line in a shop where the first person arrives is the first person served.

In the queue, the operations are done on both ends of the queue the left end which is usually referred to as "front" and the right end usually referred to as "back".

## Queue Operations

There are some operations used in the queue to add new items, access items in the queue, and remove items:
* Add (Push): insert a new item to the right end of the queue (the back).
* Remove (Pop): delete the first item added to the queue (the item in the front).
* Get (Front): returns the first item added to the queue (the item in the front).

All operations supported by the queue have time complicity of $O(1)$ so the overall complexity of the queue is $O(1)$ the space complicity of the queue is $O(n)$ where n is the number of items.

## Example on Queue
Let's observe how the queue works when we apply the following operations in the same order they were given:

1. Add 2
2. Add 3
3. Get
4. Add 0
5. Remove
6. Add 1
7. Remove
8. Get

<div align="center">
    <img src="images/example_1_1.png">
    <img src="images/example_1_2.png">
    <p><em>(1) In the beginning the queue  is empty, after the first operation 2 will be added to the back of the queue</em></p>
    <p><em>(now the 2 is in the front and also in the back of the queue since it is the only item)</em></p>
</div>

<div align="center">
    <img src="images/example_2_1.png">
    <img src="images/example_2_2.png">
<p><em>(2) After the second operation 3 will be added to the back of the queue</em></p>
    <p><em>(now the 2 is in the front and 3 is in the back)</em></p></div>

<div align="center">
    <img src="images/example_3_1.png">
    <p><em>(3) The third operation is a Get operation and the result is the item in the front (2)</em></p>
</div>

<div align="center">
    <img src="images/example_4_1.png">
    <img src="images/example_4_2.png">
    <p><em>(4) The fourth operation is an Add 0 operation so 0 is added to the back of the queue</em></p>
    <p><em>
    (now the 2 is in the front and 0 is in the back)
    </em></p>
</div>

<div align="center">
    <img src="images/example_5_1.png">
    <img src="images/example_5_2.png">
    <p><em>(5) The fifth operation is a Remove operation so the item at the front of the queue is removed</em></p>
</div>

<div align="center">
    <img src="images/example_6_1.png">
    <img src="images/example_6_2.png">
    <p><em>(6) The Sixth operation is an Add 1 operation so 1 is added to the back of the queue</em></p>
</div>

<div align="center">
    <img src="images/example_7_1.png">
    <img src="images/example_7_2.png">
    <p><em>(7) The seventh operation is a Remove operation so the item at the front of the queue is removed</em></p>
</div>

<div align="center">
    <img src="images/example_8_1.png">
    <p><em>(8) The eighth operation is a Get operation and the result is the item in the front (0)</em></p>
</div>

## Queue Implementation

=== "C++"
```cpp
#include <iostream>
#include <queue>//queue header

using namespace std;

int main(){
    /*
    In C++:
        push: is used to add items to the queue
        pop: is used to remove the first item in the queue
        front: is used to get the item in the front of the queue
    */

    //Create an empty queue
    queue<int> items;

    //Add 2
    items.push(2);
    //2 is added to the end of the queue 
    //items = 2 <- item in the back
        
    //Add 3
    items.push(3);
    //items = 2, 3 <- item in the back

    //Get
    cout << "Item on front is: " << items.front() << endl;
    //Prints the item in the back of the queue (2)

    //Add 0
    items.push(0);
    //items = 2, 3, 0 <- item in the back

    //Remove
    items.pop();
    //Removes the item in the front of the queue (2)
    //items = 3, 0 <- item in the back

    //Add 1
    items.push(1);
    //items = 3, 0, 1 <- item in the back

    //Remove
    items.pop();
    //Removes the item in the front of the queue (3)
    //items = 0, 1 <- item in the back

    //Get
    cout << "Item on front is: " << items.front() << endl;
    //Prints the item in the back of the queue (0)
}
```
=== "Python"
```python
from queue import Queue

# In Python:
# put(): add item to the queue
# get(): remove the front item
# queue[0]: access the front item using queue.queue[0] (not standard, but works for Queue)

# Create an empty queue
items = Queue()

# Add 2
items.put(2)
# items = 2

# Add 3
items.put(3)
# items = 2, 3

# Get front item
print("Item on front is:", items.queue[0])
# Prints 2

# Add 0
items.put(0)
# items = 2, 3, 0

# Remove front item
items.get()
# items = 3, 0

# Add 1
items.put(1)
# items = 3, 0, 1

# Remove front item
items.get()
# items = 0, 1

# Get front item
print("Item on front is:", items.queue[0])
# Prints 0
```

```plaintext
Output:

Item on front is: 2
Item on front is: 0
```