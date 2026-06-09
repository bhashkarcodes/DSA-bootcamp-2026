# Stack Data Structure

## What is a Stack?

A Stack is a linear data structure that follows **LIFO** (Last In, First Out) order — the element inserted last is the first one to come out.

Think of it as a stack of plates:
- **Push** — add a new plate on top
- **Pop** — remove the top plate

You can only ever interact with the top. There is no direct access to any element below it.

![Stack Diagram](./images/stack.png)

---

## Fixed vs Variable Stack

| Type | Backed by | Max size | Overflow possible? |
|------|-----------|----------|--------------------|
| **Fixed Stack** | Array | Set at creation | Yes — stack overflow if full |
| **Variable Stack** | Linked List / Deque | Grows dynamically | No (until memory runs out) |

Use a fixed stack when the maximum size is known in advance and you want cache-friendly memory. Use a variable stack when the size is unpredictable.

---

## Operations

| Operation | Description | Time Complexity |
|-----------|-------------|-----------------|
| `push(x)` | Insert element `x` onto the top | O(1) |
| `pop()` | Remove and return the top element | O(1) |
| `top()` / `peek()` | Return the top element without removing it | O(1) |
| `isEmpty()` | Return true if the stack has no elements | O(1) |
| `size()` | Return the number of elements | O(1) |

All standard stack operations are O(1) — this is what makes it so useful.
[Stack in STL](https://github.com/wncc/DSA-Bootcamp-2026/tree/main/Week-1/03-STL)

## Implementation

### Using an Array (Fixed Stack)
A stack can be implemented using an array where we maintain:

- An integer array to store elements.
- A variable capacity to represent the maximum size of the stack.
- A variable top to track the index of the top element. Initially, top = -1 to indicate an empty stack.

**C++**
```cpp
class myStack {
    int* arr;       // array to store elements
    int capacity;   // maximum size of stack
    int top;        // index of top element

public:
    myStack(int cap) {
        capacity = cap;
        arr = new int[capacity];
        top = -1;
    }

    void push(int x) {
        if (top == capacity - 1) {
            cout << "Stack Overflow\n";
            return;
        }
        arr[++top] = x;
    }

    int pop() {
        if (top == -1) {
            cout << "Stack Underflow\n";
            return -1;
        }
        return arr[top--];
    }

    int peek() {
        if (top == -1) {
            cout << "Stack is Empty\n";
            return -1;
        }
        return arr[top];
    }

    bool isEmpty() { return top == -1; }
    bool isFull()  { return top == capacity - 1; }
};
```

**Java**
```java
class myStack {
    private int[] arr;       // array to store elements
    private int capacity;    // maximum size of stack
    private int top;         // index of top element

    public myStack(int cap) {
        capacity = cap;
        arr = new int[capacity];
        top = -1;
    }

    public void push(int x) {
        if (top == capacity - 1) {
            System.out.println("Stack Overflow");
            return;
        }
        arr[++top] = x;
    }

    public int pop() {
        if (top == -1) {
            System.out.println("Stack Underflow");
            return -1;
        }
        return arr[top--];
    }

    public int peek() {
        if (top == -1) {
            System.out.println("Stack is Empty");
            return -1;
        }
        return arr[top];
    }

    public boolean isEmpty() { return top == -1; }
    public boolean isFull()  { return top == capacity - 1; }
}
```

**Python**
```python
class myStack:
    def __init__(self, cap):
        self.capacity = cap
        self.arr = [0] * self.capacity
        self.top = -1

    def push(self, x):
        if self.top == self.capacity - 1:
            print("Stack Overflow")
            return
        self.top += 1
        self.arr[self.top] = x

    def pop(self):
        if self.top == -1:
            print("Stack Underflow")
            return -1
        val = self.arr[self.top]
        self.top -= 1
        return val

    def peek(self):
        if self.top == -1:
            print("Stack is Empty")
            return -1
        return self.arr[self.top]

    def isEmpty(self): return self.top == -1
    def isFull(self):  return self.top == self.capacity - 1
```

---
### Using a Linked List (Variable Stack)
A stack can be implemented using a linked list where we maintain:

- A Node structure/class that contains:
- data → to store the element.
- next → pointer/reference to the next node in the stack.
- A pointer/reference top that always points to the current top node of the stack.
- Initially, top = null to represent an empty stack.

**C++**
```cpp
class Node {
public:
    int data;
    Node* next;
    Node(int x) : data(x), next(nullptr) {}
};

class myStack {
    Node* top;
    int count;

public:
    myStack() : top(nullptr), count(0) {}

    void push(int x) {
        Node* temp = new Node(x);
        temp->next = top;
        top = temp;
        count++;
    }

    int pop() {
        if (top == nullptr) {
            cout << "Stack Underflow\n";
            return -1;
        }
        Node* temp = top;
        top = top->next;
        int val = temp->data;
        delete temp;   // free memory — important in C++
        count--;
        return val;
    }

    int peek() {
        if (top == nullptr) {
            cout << "Stack is Empty\n";
            return -1;
        }
        return top->data;
    }

    bool isEmpty() { return top == nullptr; }
    int size()     { return count; }
};
```

**Java**
```java
class Node {
    int data;
    Node next;
    Node(int x) { data = x; next = null; }
}

class myStack {
    Node top;
    int count;

    myStack() { top = null; count = 0; }

    void push(int x) {
        Node temp = new Node(x);
        temp.next = top;
        top = temp;
        count++;
    }

    int pop() {
        if (top == null) {
            System.out.println("Stack Underflow");
            return -1;
        }
        int val = top.data;
        top = top.next;   // GC handles memory in Java
        count--;
        return val;
    }

    int peek() {
        if (top == null) {
            System.out.println("Stack is Empty");
            return -1;
        }
        return top.data;
    }

    boolean isEmpty() { return top == null; }
    int size()        { return count; }
}
```

**Python**
```python
class Node:
    def __init__(self, x):
        self.data = x
        self.next = None

class myStack:
    def __init__(self):
        self.top = None
        self.count = 0

    def push(self, x):
        temp = Node(x)
        temp.next = self.top
        self.top = temp
        self.count += 1

    def pop(self):
        if self.top is None:
            print("Stack Underflow")
            return -1
        val = self.top.data
        self.top = self.top.next   # Python GC handles deallocation
        self.count -= 1
        return val

    def peek(self):
        if self.top is None:
            print("Stack is Empty")
            return -1
        return self.top.data

    def isEmpty(self): return self.top is None
    def size(self):    return self.count
```

---
## Array vs Linked List — Which to Use?

| | Array-based | Linked List-based |
|---|---|---|
| Memory layout | Contiguous (cache friendly) | Scattered (pointer overhead) |
| Max size | Fixed at creation | Dynamic |
| Overhead | Low | One extra pointer per node |
| Overflow risk | Yes | No (until memory exhausted) |
| **Use when** | Size is known in advance | Size is unpredictable |

---
## Resources
[Stack Data Structure - GeeksforGeeks](https://www.geeksforgeeks.org/dsa/stack-data-structure/)
[Stacks using Arrays - GeeksforGeeks](https://www.geeksforgeeks.org/dsa/implement-stack-using-array/)
[Stacks using Linked list -  GeeksforGeeks](https://www.geeksforgeeks.org/dsa/implement-a-stack-using-singly-linked-list/)