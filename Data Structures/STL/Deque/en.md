# Deque
## Definition

Deque (double ended queue) is an STL used to store and load items, which supports adding items to both ends (left end and right end), getting items at both ends, and removing items from both ends, we also can look at the deque as a two-way tube, if you look from the left side you can only see the leftmost item and if you look from the right you only see the rightmost item.

In deque, the operations are done on both ends of the queue the left end which is usually referred to as "front" and the right end usually referred to as "back".

## Deque Operations
There are some operations used in deque to add new items, access items in the deque, and remove items:

* Add Right: insert a new item to the right end of the deque (the back).
* Add Left: insert a new item to the left end of the deque (the front).
* Remove Right: delete the item at the right end of the deque (the item in the back).
* Remove Left: delete the item at the left end of the deque (the item in the front).
* Get Right: returns the item at the right end of the deque (the item in the back).
* Get Left: returns the item at the left end of the deque (the item in the front).

All operations supported by the deque have time complexity of $O(1)$ so the overall complexity of adding $n$ elements to the deque is $O(n)$ and the space complexity is $O(n)$.

## Example on Deque
Let's observe how the deque works when we apply the following operations in the same order they were given:

1. Add Left 5
2. Add Left 3
3. Remove Right
4. Add Right 4
5. Get Left
6. Get Right
7. Add Left 8
8. Get Left
9. Add Right 4
10. Remove Right

<div align="center">
    <img src="images/example_1_1.png" alt="Empty deque">
    <img src="images/example_1_2.png" alt="Add 5 to the front">
    <p><em>(1) In the beginning the deque is empty, after the first operation 5 will be added to the front of the deque</em></p>
</div>

<div align="center">
    <img src="images/example_2_1.png" alt="Deque with 5">
    <img src="images/example_2_2.png" alt="Add 3 to the front">
    <p><em>(2) After the second operation 3 will be added to the front of the deque (now the 3 is in the front and 5 is in the back)</em></p>
</div>

<div align="center">
    <img src="images/example_3_1.png" alt="Deque with 3, 5">
    <img src="images/example_3_2.png" alt="Remove 5 from the back">
    <p><em>(3) The third operation is a Remove Right operation so the item in the right is removed (5)</em></p>
</div>

<div align="center">
    <img src="images/example_4_1.png" alt="Deque with 3">
    <img src="images/example_4_2.png" alt="Add 4 to the back">
    <p><em>(4) The fourth operation is an Add Right 4 operation so 4 is added to the back of the deque (now the 3 is in the front and 4 is in the back)</em></p>
</div>

<div align="center">
    <img src="images/example_5_1.png" alt="Deque with 3, 4">
    <img src="images/example_5_2.png" alt="Get Left operation">
    <p><em>(5) The fifth operation is a Get Left operation and the result is 3 (item in the front)</em></p>
</div>

<div align="center">
    <img src="images/example_6_1.png" alt="Deque with 3, 4">
    <img src="images/example_6_2.png" alt="Get Right operation">
    <p><em>(6) The sixth operation is a Get Right operation and the result is 4 (item in the back)</em></p>
</div>

<div align="center">
    <img src="images/example_7_1.png" alt="Deque with 3, 4">
    <img src="images/example_7_2.png" alt="Add 8 to the front">
    <p><em>(7) The seventh operation is an Add Left 8 operation so 8 is added to the front of the deque (now the 8 is in the front and 4 is in the back)</em></p>
</div>

<div align="center">
    <img src="images/example_8_1.png" alt="Deque with 8, 3, 4">
    <p><em>(8) The eighth operation is a Get Left operation and the result is 8 (item in the front)</em></p>
</div>

<div align="center">
    <img src="images/example_9_1.png" alt="Deque with 8, 3, 4">
    <img src="images/example_9_2.png" alt="Add 4 to the back">
    <p><em>(9) The ninth operation is an Add Right 4 operation so 4 is added to the back of the deque (now the 8 is in the front and 4 is in the back)</em></p>
</div>

<div align="center">
    <img src="images/example_10_1.png" alt="Deque with 8, 3, 4, 4">
    <img src="images/example_10_2.png" alt="Remove from the back">
    <p><em>(10) The tenth operation is a Remove Right operation so the item in the right is removed (4) (now the 8 is in the front and 4 is in the back)</em></p>
</div>


## Queue Implementation
=== "C++"
```c++
#include <iostream>
#include <deque>//deque header

using namespace std;

int main(){
    /*
    In C++:
        push_front(): is used to add items to the front of the deque
        push_back(): is used to add items to the back of the deque
        pop_front(): is used to remove items to the front of the deque
        pop_back(): is used to remove items to the back of the deque
        front(): is used to return the item in the front of the deque
        back(): is used to return the item in the back of the deque
    */

    //Create an empty deque
    deque<int> items;

    //Add Left 5
    items.push_front(5);
    //5 is added to the front of the deque 
    //items = 5 <- item in the front and back
        
    //Add Left 3
    items.push_front(3);
    //items = 3, 5 <- item in the back

    //Remove Right
    items.pop_back();
    //items = 3 <- item in the front and back

    //Add Right 4
    items.push_back(4);
    //4 is added to the back of the deque 
    //items = 3, 4 <- item in the back

    //Get Left
    cout << "Item on front is: " << items.front() << endl;
    //Prints the item in the front of the deque (3)

    //Get Right
    cout << "Item on back is: " << items.back() << endl;
    //Prints the item in the back of the deque (4)

    //Add Left 8
    items.push_front(8);
    //items = 8, 3, 4 <- item in the back

    //Get Left
    cout << "Item on front is: " << items.front() << endl;
    //Prints the item in the front of the deque (8)

    //Add Right 4
    items.push_back(4);
    //4 is added to the back of the deque 
    //items = 8, 3, 4, 4 <- item in the back

    //Remove Right
    items.pop_back();
    //items = 8, 3, 4 <- item in the back
}
```
=== "Python"
```python
from collections import deque

# In Python:
# appendleft(): add to front
# append(): add to back
# popleft(): remove from front
# pop(): remove from back
# [0]: front item
# [-1]: back item

# Create an empty deque
items = deque()

# Add Left 5
items.appendleft(5)
# items = 5

# Add Left 3
items.appendleft(3)
# items = 3, 5

# Remove Right
items.pop()
# items = 3

# Add Right 4
items.append(4)
# items = 3, 4

# Get Left
print("Item on front is:", items[0])
# Prints 3

# Get Right
print("Item on back is:", items[-1])
# Prints 4

# Add Left 8
items.appendleft(8)
# items = 8, 3, 4

# Get Left
print("Item on front is:", items[0])
# Prints 8

# Add Right 4
items.append(4)
# items = 8, 3, 4, 4

# Remove Right
items.pop()
# items = 8, 3, 4
```

```plaintext
Output:
Item on front is: 3
Item on back is: 4
Item on front is: 8
```
