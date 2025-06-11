# Complete Search

Complete search is a brute-force technique where you literally try all possible combinations.

Let's take an example of the following problem:

You are given an even positive integer $n$. You should print all regular bracket sequences of length $n$.

A regular bracket sequence (RBS) is a sequence of brackets that is correctly balanced and properly nested. Formally, a sequence consisting of `(` and `)` is a regular bracket sequence if:

- Every opening bracket `(` has a corresponding closing bracket `)`.

- At any point in the sequence, the number of closing brackets `)` never exceeds the number of opening brackets `(` when reading from left to right.

### Examples

`()`, `(())()`, `((()))`, `()()` are regular bracket sequences. `)(`, `(()`, `())(` are not regular (invalid).

A brute-force solution for this problem would be to generate all possible sequences of length $n$, then check for each sequence whether it is regular or not.

We can use this code to check if a string is an RBS or not:


=== c++

```c++
bool is_rbs(string &s) {
    int opening = 0;
    for (int i = 0; i < (int)s.size(); i++) {
        if (s[i] == '(')
            opening++;
        else
            opening--;
         //There is a closing bracket that doesn't have a corresponding opening
        if (opening < 0)
            return false;
    }
    // return true if each opening has a corresponding closing
    return opening == 0;
}
```

=== Java

```java
public static boolean isRbs(String s) {
    int opening = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i) == '(')
            opening++;
        else
            opening--;
        // There is a closing bracket that doesn't have a corresponding opening
        if (opening < 0)
            return false;
    }
    // return true if each opening has a corresponding closing
    return opening == 0;
}
```

=== Python

```python
def is_rbs(s):
    opening = 0
    for c in s:
        if c == '(':
            opening += 1
        else:
            opening -= 1
        # There is a closing bracket that doesn't have a corresponding opening
        if opening < 0:
            return False
    # return True if each opening has a corresponding closing
    return opening == 0
```

Now, the problem is to generate all possible sequences of length $n$, where each character is either `(` or `)`.

One known way to do this on paper is to draw a tree as follows:

- You start at the root of the tree with an empty sequence.

- The first character has two options, either `(` or `)`, so you draw two children, one with `(` and one with `)`.

- For each child, repeat this process by adding two children, one with `(` and one with `)`, forming all combinations of length $2$.

- A leaf is a node that has no children, and the depth of the tree is the distance between the leaf nodes and the root.

- At this point, we have a tree of depth $2$, and each path from the root node to a leaf represents a different combination, so all leaves together give all combinations of length $2$.

- If we want to get all combinations of length $3$, we can draw two children for all leaves, one with `(` and another with `)`.

- So, if we have a tree of depth $d$ and we draw two children for all leaves, we will get a tree of depth $d+1$.

- To generate all possible sequences of length $n$, we need to draw a tree of depth $n$.

- In the end, we can check each sequence to determine if it is a regular bracket sequence or not.

Here is an example where $n = 4$:

![Alt Text](gifs/draw-tree.gif)

### Recursive Generation

To write a function that generates all these sequences, we can follow the tree concept. We start at depth $0$, where each choice leads to depth $1$, and each depth level doubles the possibilities until we reach depth $n$.

One way to implement this is by writing a function for each depth level. We start by calling the function at depth `0`, and each function calls the next depth level with the two possible choices.

For example, when $n = 4$, the following code generates all sequences:

```c++

void depth0(string s);
void depth1(string s);
void depth2(string s);
void depth3(string s);
void depth4(string s);

void depth0() {
    string s;
    depth1(s + '(');
    depth1(s + ')');
}

void depth1(string s) {
    depth2(s + '(');
    depth2(s + ')');
}

void depth2(string s) {
    depth3(s + '(');
    depth3(s + ')');
}

void depth3(string s) {
    depth4(s + '(');
    depth4(s + ')');
}

void depth4(string s) {
    if (is_rbs(s))
        cout << s << endl;
}

int main() {
    depth0();
    return 0;
} 
```

=== Java

```java
public class Main {
    static void depth0() {
        String s = "";
        depth1(s + "(");
        depth1(s + ")");
    }

    static void depth1(String s) {
        depth2(s + "(");
        depth2(s + ")");
    }

    static void depth2(String s) {
        depth3(s + "(");
        depth3(s + ")");
    }

    static void depth3(String s) {
        depth4(s + "(");
        depth4(s + ")");
    }

    static void depth4(String s) {
        if (isRbs(s))
            System.out.println(s);
    }

    public static void main(String[] args) {
        depth0();
    }
}
```

=== Python

```python
def depth0():
    s = ""
    depth1(s + "(")
    depth1(s + ")")

def depth1(s):
    depth2(s + "(")
    depth2(s + ")")

def depth2(s):
    depth3(s + "(")
    depth3(s + ")")

def depth3(s):
    depth4(s + "(")
    depth4(s + ")")

def depth4(s):
    if is_rbs(s):
        print(s)

depth0()
```

### Optimized Recursive Approach

The previous approach requires writing $n$ separate functions. However, since each function at depth $i$ tries two options and calls the function at depth $i + 1$, we can replace this with a single recursive function where the base case is when the depth reaches $n$.

The optimized recursive solution looks like this:

=== c++

```c++
void generate_rbs(int depth, string s, int n) {
    if (depth == n) {
        if (is_rbs(s)) {
            cout << s << endl;
        }
        return;
    }
    generate_rbs(depth + 1, s + "(", n);
    generate_rbs(depth + 1, s + ")", n);
}

int main() {
    int n;
    cin >> n;
    generate_rbs(0, "", n);
    return 0;
}
```


=== Java

```java
import java.util.Scanner;

public class Main {

    static void generateRbs(int depth, String s, int n) {
        if (depth == n) {
            if (isRbs(s)) {
                System.out.println(s);
            }
            return;
        }
        generateRbs(depth + 1, s + "(", n);
        generateRbs(depth + 1, s + ")", n);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        generateRbs(0, "", n);
    }
}
```

=== Python

```python
def generate_rbs(depth, s, n):
    if depth == n:
        if is_rbs(s):
            print(s)
        return
    generate_rbs(depth + 1, s + "(", n)
    generate_rbs(depth + 1, s + ")", n)

n = int(input())
generate_rbs(0, "", n)
```

This recursive function correctly generates all bracket sequences of length $n$, ensuring that only valid sequences are printed.

### Complexity Analysis

- Number of recursive calls:
  
  - The recursion forms a binary tree of height $n$.
  
  - The number of leaves is equal to $2^n$, which is also the number of possibilities.
  
  - The total number of calls is $1 + 2 + 4 + \ldots + 2^n = 2^{n+1} - 1$.

- Work Done at Non-Leaves:
  
  - Each recursive call constructs a new string by appending `'('` or `')'` to $s$. Appending a character to a string of length $k$ (in C++) costs $O(k)$ (since it copies the string).

- Work Done at Each Leaf:
  
  - At each leaf (when `depth == n`), `is_rbs(s)` is called on a string of length $n$.
  
  - Checking if a string is an RBS can be done in $O(n)$ time.

- Total Work:
  
  - At each leaf: $O(n)$
  
  - Number of leaves: $2^n$
  
  - Total time for checking: $O(n \cdot 2^n)$
  
  - Over all calls, the cumulative cost of string operations also sums up to $O(n \cdot 2^n)$, since each leaf is reached through a path of $n$ appends, and there are $2^n$ leaves.

So the total complexity is $O(n \cdot 2^n)$.

### Different Implementation

Since passing the string $s$ as a parameter makes the complexity of the call $O(\text{len}(s))$, we can send $s$ by reference to improve efficiency. 

Any ways Whenever the function ends and returns to the caller, we need to ensure that $s$ looks the same as it did before the function was called. To achieve this, we must roll back the changes, so the corrected code will look like this:

=== c++

```c++
void generate_rbs(int depth, string &s, int n) {
    if (depth == n) {
        if (is_rbs(s)) {
            cout << s << endl;
        }
        return;
    }
    s += "(";
    generate_rbs(depth + 1, s, n);
    s.pop_back();
    s += ")";
    generate_rbs(depth + 1, s, n);
    s.pop_back();
    // After calling generate_rbs(depth + 1, s, n), we ensure that string s
    // is the same as it was before the call. Using pop_back() removes
    // the change we made, so we return with no modifications to s.
}

int main() {
    int n;
    cin >> n;
    string s = "";
    generate_rbs(0, s, n);
    return 0;
}
```

=== Java

```java
import java.util.Scanner;

public class Main {

    static void generateRbs(int depth, StringBuilder s, int n) {
        if (depth == n) {
            if (isRbs(s.toString())) {
                System.out.println(s.toString());
            }
            return;
        }
        s.append('(');
        generateRbs(depth + 1, s, n);
        s.deleteCharAt(s.length() - 1);

        s.append(')');
        generateRbs(depth + 1, s, n);
        s.deleteCharAt(s.length() - 1);
        // After calling generateRbs(depth + 1, s, n), we ensure that s
        // is the same as it was before the call. Using deleteCharAt removes
        // the change we made, so we return with no modifications to s.
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        StringBuilder s = new StringBuilder();
        generateRbs(0, s, n);
    }
}
```

=== Python

```python
def generate_rbs(depth, s, n):
    if depth == n:
        if is_rbs(''.join(s)):
            print(''.join(s))
        return
    s.append('(')
    generate_rbs(depth + 1, s, n)
    s.pop()

    s.append(')')
    generate_rbs(depth + 1, s, n)
    s.pop()
    # After calling generate_rbs(depth + 1, s, n), we ensure that s
    # is the same as it was before the call. Using pop() removes
    # the change we made, so we return with no modifications to s.

n = int(input())
s = []
generate_rbs(0, s, n)
```

We reduced the overhead from string copying in recursive calls, but the check function on each leaf still has complexity $O(n)$, so the total complexity remains $O(n \cdot 2^n)$.

We can improve this by doing the check while making the recursive calls. We can add a counter parameter that counts the difference between the number of opening and closing brackets. If the counter becomes negative, we can return immediately, since all sequences under this branch would be invalid:

=== c++

```c++
void generate_rbs(int depth, string &s, int n, int counter) {
    if (counter < 0)
        return;
    if (depth == n) {
        if (counter == 0) {
            cout << s << endl;
        }
        return;
    }
    s += "(";
    generate_rbs(depth + 1, s, n, counter + 1);
    s.pop_back();
    s += ")";
    generate_rbs(depth + 1, s, n, counter - 1);
    s.pop_back();
}

int main() {
    int n;
    cin >> n;
    string s = "";
    generate_rbs(0, s, n, 0);
    return 0;
}
```

=== Java

```java
import java.util.Scanner;

public class Main {
    static void generateRbs(int depth, StringBuilder s, int n, int counter) {
        if (counter < 0)
            return;
        if (depth == n) {
            if (counter == 0) {
                System.out.println(s.toString());
            }
            return;
        }
        s.append('(');
        generateRbs(depth + 1, s, n, counter + 1);
        s.deleteCharAt(s.length() - 1);

        s.append(')');
        generateRbs(depth + 1, s, n, counter - 1);
        s.deleteCharAt(s.length() - 1);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        StringBuilder s = new StringBuilder();
        generateRbs(0, s, n, 0);
    }
}
```

=== Python

```python
def generate_rbs(depth, s, n, counter):
    if counter < 0:
        return
    if depth == n:
        if counter == 0:
            print(''.join(s))
        return
    s.append('(')
    generate_rbs(depth + 1, s, n, counter + 1)
    s.pop()

    s.append(')')
    generate_rbs(depth + 1, s, n, counter - 1)
    s.pop()

n = int(input())
s = []
generate_rbs(0, s, n, 0)
```

Now, the work done at each leaf and non-leaf node is $O(1)$. With the negative counter check and early return, we are skipping many branches, which makes the solution much faster, but the worst-case complexity is still close to $O(2^n)$.
