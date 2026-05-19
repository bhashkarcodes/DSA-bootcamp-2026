# Standard Libraries of DSA
Collection of pre-built classes and functions used for efficient data handeling and programming.
It provides ready-to-used data structures and algorithms that simplify development

Consists of Containers and Algorithms.

## Containers:-
There are 4 types of containers:-
1) Sequence Containers:-Arrays, Vectors, Deque, List
2) Containers Adaptors:-Stack, Queue, Priority Queue
3) Associative Containers:-Set, Multiset, Map, Multimap
4) Unordered Associative:-unordered set, unordered multiset, unordered map, unordered multimap

## Algorithms:-
Binary Search, Lower/Upper bound, min/max, reverse/rotate, sort/ swap etc.

# Containers:-

## Sequence Containers:-
### Arrays:-
Array is a collection of homogeneous objects and this array container is defined for constant size arrays or (static size). This container wraps around fixed-size arrays and the information of its size are not lost when declared to a pointer. 

## When to use? 
When you know the size at compile time and don't need to resize. Fastest access, zero overhead. Use for fixed-size data like matrix rows, RGB values, or lookup tables.

### In cpp
```
#include<iostream>
#include<array>
using namespace std;

int main(){
    
    array<int, 4> arr = {1, 2, 3, 4};
    int size = arr.size();

    for(int i = 0; i<size; i++){//to access all the elements of an array
        cout<<arr[i]<<endl;
    }
    cout<<arr.at(2)<<endl;//use .at to access the element at index 2
    cout<<arr.empty()<<endl;//returns boolean
    cout<<arr.front()<<endl;//returns first element of the array
    cout<<arr.back()<<endl;//returns last element of the array

}

```
### In Java
```
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4};
        int size = arr.length;

        // Access all elements
        for (int i = 0; i < size; i++) {
            System.out.println(arr[i]);
        }

        // arr.at(2) equivalent — safe access
        System.out.println(arr[2]);         // 3

        // arr.empty() — no built-in, check length
        System.out.println(arr.length == 0); // false

        // arr.front() equivalent
        System.out.println(arr[0]);          // 1

        // arr.back() equivalent
        System.out.println(arr[arr.length - 1]); // 4
    }
}

```
### In Python
```
arr = [1, 2, 3, 4]
size = len(arr)

# Access all elements
for i in range(size):
    print(arr[i])

# arr.at(2) # Python raises IndexError automatically
print(arr[2])           # 3

# arr.empty()
print(len(arr) == 0)    # False

# arr.front()
print(arr[0])           # 1

# arr.back()
print(arr[-1])          # 4

# arr.fill(x) equivalent
arr = [0] * 4
print(arr)              # [0, 0, 0, 0]
```

### Vectors:-
Vector represents a dynamic array which allocates contineous memory to the elements. When a vector is full and another element is added, it's size doubles by creating another vector of double size, copies all previous elements from old vector to the new one.

## When to use?
Your go-to dynamic container for most problems. Use when you need a resizable array with fast random access. Perfect for storing results, BFS/DFS visited nodes, or any list whose size you don't know in advance.

### In Cpp
```
#include<iostream>
#include<vector>
using namespace std;

int main(){
    vector<int>v;
    cout<<v.size()<<endl; //how many elements are present in the vector rn
    cout<<v.capacity()<<endl; //how many max elements can be stored in the vector
    v.push_back(1);
    cout<<v.size()<<endl;
    cout<<v.capacity()<<endl;
    v.push_back(2);
    cout<<v.size()<<endl;
    cout<<v.capacity()<<endl;//when element 2 is pushed then the capacity doubles from 1 to 2
    v.push_back(3);
    cout<<v.size()<<endl;
    cout<<v.capacity()<<endl;//when element 3 is pushed then the capacity doubles from 2 to 4

    cout<<v.at(2)<<" "<<"or"<<" "<<v[2]<<endl;//.at funtion is used to extract element at index 2
    cout<<v.front()<<endl;
    cout<<v.back()<<endl;

    for(int v_i:v){
        cout<<v_i<<endl;
    }
    v.pop_back();//pops out the last element
    for(int v_i:v){
        cout<<v_i<<endl;
    }
    v.clear();
    cout<<v.size()<<endl;
    cout<<v.capacity()<<endl;//on clearing the size becomes 0 but the capacity remains the same

    vector<int>a(5);//5 is the number of elements and all eleents are initialised to 0
    for(int a_i:a){
        cout<<a_i;
    }
    cout<<endl;
    vector<int>b(5, 1); //5 is number of elements with all elements being 1 
    for(int b_i:b){
        cout<<b_i;
    }
    cout<<endl;
    //copying of a vector into a new vector
    vector<int>last(a);
    for(int i : last){
        cout<<i;
    }
}
```

### In Java
```
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {

        ArrayList<Integer> v = new ArrayList<>();
        System.out.println(v.size());       // 0  — no capacity() in Java

        v.add(1);
        System.out.println(v.size());       // 1

        v.add(2);
        System.out.println(v.size());       // 2

        v.add(3);
        System.out.println(v.size());       // 3

        // .at(2) equivalent — both safe in Java (throws IndexOutOfBoundsException)
        System.out.println(v.get(2) + " or " + v.get(2));  // 3 or 3
        
        System.out.println(v.get(0));                        // front → 1
        System.out.println(v.get(v.size() - 1));             // back  → 3

        // range-based for loop
        for (int v_i : v) {
            System.out.println(v_i);
        }
        // pop_back equivalent
        v.remove(v.size() - 1);
        for (int v_i : v) {
            System.out.println(v_i);
        }
        // clear
        v.clear();
        System.out.println(v.size());       // 0

        // vector a(5) — 5 zeros
        ArrayList<Integer> a = new ArrayList<>(Collections.nCopies(5, 0));
        for (int a_i : a) System.out.print(a_i);
        System.out.println();

        // vector b(5, 1) — 5 ones
        ArrayList<Integer> b = new ArrayList<>(Collections.nCopies(5, 1));
        for (int b_i : b) System.out.print(b_i);
        System.out.println();

        // copy a into last
        ArrayList<Integer> last = new ArrayList<>(a);
        for (int i : last) System.out.print(i);
    }
}
```

### In python
```
import sys
import copy

v = []
print(len(v))                        # size → 0
print(sys.getsizeof(v))              # capacity (in bytes) → 56

v.append(1)
print(len(v))                        # 1
print(sys.getsizeof(v))              # grows

v.append(2)
print(len(v))                        # 2
print(sys.getsizeof(v))              # doubles

v.append(3)
print(len(v))                        # 3
print(sys.getsizeof(v))              # doubles again

# .at(2) and v[2] equivalent
print(str(v[2]) + " or " + str(v[2]))   # 3 or 3

print(v[0])                          # front → 1
print(v[-1])                         # back  → 3

# range-based for loop
for v_i in v:
    print(v_i)

# pop_back
v.pop()
for v_i in v:
    print(v_i)

# clear
v.clear()
print(len(v))                        # 0
print(sys.getsizeof(v))              # size 0 but memory may remain

# vector a(5) — 5 zeros
a = [0] * 5
for a_i in a:
    print(a_i, end="")
print()

# vector b(5, 1) — 5 ones
b = [1] * 5
for b_i in b:
    print(b_i, end="")
print()

# copy a into last
last = copy.copy(a)        # shallow copy (same as vector last(a))
for i in last:
    print(i, end="")
```
### Deque:-
Deque stands for Double-Ended Queue. It's a sequence container that allows you to add or remove elements efficiently from both the front and the back.

## When to use?
When you need fast insertion/deletion at BOTH ends. Use for sliding window problems, implementing a queue with push/pop from both sides, or browser history (back/forward).

### In cpp
```
#include<iostream>
#include<deque>
using namespace std;

int main(){
   deque<int>d;

   d.push_back(1);
   d.push_front(2);
   for(int d_i:d){
    cout<<d_i<<endl;
   }
   /*
   d.pop_back();
   for(int d_i:d){
    cout<<d_i<<endl;;
   }
   d.pop_front();
   for(int d_i:d){
    cout<<d_i<<endl;
   }
   cout<<d.empty()<<endl;
   */
   cout<<d.at(1)<<endl;
   cout<<d.front()<<endl;
   cout<<d.back()<<endl;

   d.erase(d.begin(),d.begin()+1);//start from begin and will delete the numbers starting from begin
   for(int d_i:d){
    cout<<d_i<<endl;
   }
   
}
```

### In Java
```
import java.util.ArrayDeque;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {

        ArrayDeque<Integer> d = new ArrayDeque<>();

        d.addLast(1);   // push_back
        d.addFirst(2);  // push_front

        for (int d_i : d) {
            System.out.println(d_i);
        }

        // d.pop_back()  → d.removeLast()
        // d.pop_front() → d.removeFirst()
        // d.empty()     → d.isEmpty()

        // .at(1) — convert to list for index access
        ArrayList<Integer> temp = new ArrayList<>(d);
        System.out.println(temp.get(1));         // at(1)
        System.out.println(d.peekFirst());       // front
        System.out.println(d.peekLast());        // back

        // erase(begin, begin+1) — removes first element
        d.removeFirst();
        for (int d_i : d) {
            System.out.println(d_i);
        }
    }
}
```

### In Python
```
from collections import deque

d = deque()

d.append(1)        # push_back
d.appendleft(2)    # push_front

for d_i in d:
    print(d_i)

# d.pop_back()  → d.pop()
# d.pop_front() → d.popleft()
# d.empty()     → len(d) == 0

print(d[1])        # at(1)
print(d[0])        # front
print(d[-1])       # back

# erase(begin, begin+1) — removes first element
del d[0]
for d_i in d:
    print(d_i)
```

### List:-
List stores elements in a non-contiguous memory. It is implemented as a doubly linked list meaning each element points to both its previous and next elements.For the curious ones - [Doubly Linked List|GeeksforGeeks](https://www.geeksforgeeks.org/dsa/doubly-linked-list/)

## When to use?
When you need frequent insertions/deletions in the MIDDLE of the list. Unlike vectors, no shifting is needed. Use for LRU cache, playlist management, or any frequent mid-list edits. Avoid if you need random access.

### In cpp
```
#include<iostream>
#include<list>

using namespace std;

int main(){
    list<int> l;
    
    l.push_back(2);
    l.push_front(1);

    for(int l_i:l){
        cout<<l_i<<endl;
    }
    l.erase(l.begin());//will erase each of the element one by one by trasversing the whole list
    for(int l_i:l){
        cout<<l_i<<endl;
    }
    //copying into new list
    list<int> n(l);
    for(int i:n){
        cout<<i<<endl;

    }
    list<int>a(5);
    list<int>b(5,2);
    for(int a_i:a){
        cout<<a_i<<endl;
    }
    for(int b_i:b){
        cout<<b_i<<endl;
    }
}
```

### In Java
```
import java.util.LinkedList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {

        LinkedList<Integer> l = new LinkedList<>();

        l.addLast(2);   // push_back
        l.addFirst(1);  // push_front

        for (int l_i : l) {
            System.out.println(l_i);
        }

        // erase(begin) — removes first element
        l.removeFirst();
        for (int l_i : l) {
            System.out.println(l_i);
        }

        // copy into new list
        LinkedList<Integer> n = new LinkedList<>(l);
        for (int i : n) {
            System.out.println(i);
        }

        // list<int> a(5) — 5 zeros
        LinkedList<Integer> a = new LinkedList<>(Collections.nCopies(5, 0));
        for (int a_i : a) {
            System.out.println(a_i);
        }

        // list<int> b(5, 2) — 5 twos
        LinkedList<Integer> b = new LinkedList<>(Collections.nCopies(5, 2));
        for (int b_i : b) {
            System.out.println(b_i);
        }
    }
}
```

### In Python
```
from collections import deque

l = deque()

l.append(2)       # push_back
l.appendleft(1)   # push_front

for l_i in l:
    print(l_i)

# erase(begin) — removes first element
l.popleft()
for l_i in l:
    print(l_i)

# copy into new list
n = l.copy()
for i in n:
    print(i)

# list<int> a(5) — 5 zeros
a = deque([0] * 5)
for a_i in a:
    print(a_i)

# list<int> b(5, 2) — 5 twos
b = deque([2] * 5)
for b_i in b:
    print(b_i)
```

## Containers Adaptors:-
### Stack:-
Stack container follows LIFO (Last In First Out) order of insertion and deletion. It means that most recently inserted element is removed first and the first inserted element will be removed last. This is done by inserting and deleting elements at only one end of the stack which is generally called the top of the stack.

## When to use?
When your problem needs LIFO order. Classic use cases — balanced parentheses, undo/redo, function call simulation, DFS, and next greater element problems.

### In cpp
```
#include<iostream>
#include<stack>
using namespace std;

int main(){
    stack<string> s;
    s.push("A");
    s.push("B");
    s.push("C");
    s.push("D");
    /*
    int n = s.size();//fix the initial number of elements
    for(int i = 0; i<n; i++){
        cout<<s.top()<<" ";
        s.pop();
    }
        */
    cout<<"First element : "<<s.top()<<endl;//the element which goes in last comes out first
    s.pop();
    cout<<"First element now : "<<s.top()<<endl;
    s.pop();
    cout<<"First element now then : "<<s.top()<<endl;
    cout<<s.empty()<<endl;

}
```

### In Java
```
import java.util.Stack;

public class Main {
    public static void main(String[] args) {

        Stack<String> s = new Stack<>();
        s.push("A");
        s.push("B");
        s.push("C");
        s.push("D");

        /*
        int n = s.size();
        for (int i = 0; i < n; i++) {
            System.out.print(s.peek() + " ");
            s.pop();
        }
        */

        System.out.println("First element : " + s.peek());   // D
        s.pop();
        System.out.println("First element now : " + s.peek()); // C
        s.pop();
        System.out.println("First element now then : " + s.peek()); // B
        System.out.println(s.isEmpty());  // false
    }
}
```
### In Python
```
s = []
s.append("A")
s.append("B")
s.append("C")
s.append("D")

# n = len(s)
# for i in range(n):
#     print(s[-1], end=" ")
#     s.pop()

print("First element :", s[-1])        # D
s.pop()
print("First element now :", s[-1])    # C
s.pop()
print("First element now then :", s[-1])  # B
print(len(s) == 0)                     # False
```

### Queue:-
Queue is a container adapter that stores elements in FIFO (First In, First Out) order. It allows elements to be inserted from the back and removed from the front, ensuring the first inserted element is removed first.

## When to use? 
When your problem needs FIFO order. Classic use cases — BFS, task scheduling, printer queue, and level-order traversal of trees.

### In cpp
```
#include<iostream>
#include<queue>

using namespace std;

int main(){
    queue<string> q;

    q.push("A");
    q.push("B");
    q.push("C");

    cout<<q.size()<<endl;
    
    cout<<q.front()<<endl;//returns the element which was entered first
    q.pop();
    cout<<q.front()<<endl;
    q.pop();
    cout<<q.front()<<endl;
    cout<<q.empty()<<endl;//empty or not, //returns boolean

}
```

### In Java
```
import java.util.LinkedList;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {

        Queue<String> q = new LinkedList<>();

        q.add("A");
        q.add("B");
        q.add("C");

        System.out.println(q.size());       // 3

        System.out.println(q.peek());       // A
        q.poll();
        System.out.println(q.peek());       // B
        q.poll();
        System.out.println(q.peek());       // C
        System.out.println(q.isEmpty());    // false
    }
}
```

### In Python
```
from collections import deque

q = deque()

q.append("A")
q.append("B")
q.append("C")

print(len(q))       # 3

print(q[0])         # A
q.popleft()
print(q[0])         # B
q.popleft()
print(q[0])         # C
print(len(q) == 0)  # False
```

### Priority Queue:-
A Prority Queue adds and removes elements based on priority. Internally it is based on heap data structure.
Max Heap-max element comes out first
Min heap-min element comes out first

## When to use? 
When you repeatedly need the largest or smallest element. Use for Dijkstra's algorithm, heap sort, scheduling by priority, and Top-K problems.

### In cpp
```
#include<iostream>
#include<queue>
using namespace std;

int main(){
    //max heap
    priority_queue<int> max;
    max.push(8);
    max.push(3);
    max.push(7);

     int n = max.size();
    for(int i = 0; i<n; i++){
        cout<<max.top()<<" ";// returns 8 7 3
        max.pop();
    }
    cout<<endl;

    //min heap
    priority_queue<int, vector<int> , greater<int> >min;
    
    min.push(3);
    min.push(7);
    min.push(2);

    int n2 = min.size();
    for(int i = 0; i<n2; i++){
        cout<<min.top()<<" ";// returns 2 3 7
        min.pop(); 
    }
    cout<<endl;

}
```

### In Java
```
import java.util.PriorityQueue;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {

        // max heap
        PriorityQueue<Integer> max = new PriorityQueue<>(Collections.reverseOrder());
        max.add(8);
        max.add(3);
        max.add(7);

        int n = max.size();
        for (int i = 0; i < n; i++) {
            System.out.print(max.peek() + " "); // 8 7 3
            max.poll();
        }
        System.out.println();

        // min heap
        PriorityQueue<Integer> min = new PriorityQueue<>(); // default is min heap in Java
        min.add(3);
        min.add(7);
        min.add(2);

        int n2 = min.size();
        for (int i = 0; i < n2; i++) {
            System.out.print(min.peek() + " "); // 2 3 7
            min.poll();
        }
        System.out.println();
    }
}
```

### In Python
```
import heapq

# max heap
# Python only has min heap, negate values for max heap
max_heap = []
heapq.heappush(max_heap, -8)
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -7)

n = len(max_heap)
for i in range(n):
    print(-max_heap[0], end=" ")  # negate back to get 8 7 3
    heapq.heappop(max_heap)
print()

# min heap
min_heap = []
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 7)
heapq.heappush(min_heap, 2)

n2 = len(min_heap)
for i in range(n2):
    print(min_heap[0], end=" ")  # 2 3 7
    heapq.heappop(min_heap)
print()
```

## Associative Containers:-
### Set:-
A set stores only unique elements, no repetition allowed, elements are always sorted in ascending order by default. It is an implementation of Self-Balancing Binary search Tree, specifically a Red-Black Tree. For the curious ones - [Red-Black Tree|GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/)

## When to use?
When you need unique elements in sorted order. Use for removing duplicates, checking membership, and problems requiring sorted unique data. Note: O(log n) operations — slower than unordered_set.

### In cpp
```
#include<iostream>
#include<set>

using namespace std;

int main(){
    set <int> s;
    s.insert(5);
    s.insert(5);
    s.insert(6);
    s.insert(3);
    s.insert(4);

    for(auto i :s){
        cout<<i<<endl;
    }
    s.erase(s.begin());//from the sorted set the first element is removed
    for(auto i :s){
        cout<<i<<endl;
    }

    set<int>::iterator it = s.begin();//erasing in a loop(when we have erase more than one element as we could erese the elements one by one in sets)
    it++;
    s.erase(it); 
    
    cout<<s.count(5)<<endl;//to check an element is present or not
    
    set<int>::iterator itr = s.find(5);//if 5 is presnt then at which index
    cout<<(*itr)<<endl;
    //An iterator is like a pointer that points to an element inside a container
    for(auto it = itr; it!=s.end(); it++){
        cout<<(*it)<<" ";
    }
    cout<<endl;

}
```
### In Java
```
import java.util.TreeSet;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {

        TreeSet<Integer> s = new TreeSet<>();
        s.add(5);
        s.add(5); // duplicates ignored automatically
        s.add(6);
        s.add(3);
        s.add(4);

        for (int i : s) {
            System.out.println(i);
        }

        // erase(begin) — removes first element
        s.pollFirst();
        for (int i : s) {
            System.out.println(i);
        }

        // iterator erase — move to 2nd element and erase it
        Iterator<Integer> it = s.iterator();
        it.next();      // move to first
        it.next();      // move to second
        it.remove();    // erase it

        // count(5) — check if present (returns 0 or 1)
        System.out.println(s.contains(5) ? 1 : 0);

        // find(5) — get iterator from 5 onwards
        if (s.contains(5)) {
            System.out.println(5);
            for (int val : s.tailSet(5)) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}
```

### In Python
```
from sortedcontainers import SortedList

s = SortedList()
s.add(5)
s.add(5)  # duplicates ignored via discard below — SortedList allows dups
s.add(6)
s.add(3)
s.add(4)

# set ignores duplicates — use discard to simulate
s = SortedList(set([5, 5, 6, 3, 4]))

for i in s:
    print(i)

# erase(begin) — remove first element
s.pop(0)
for i in s:
    print(i)

# iterator erase — remove element at index 1
del s[1]

# count(5) — 0 or 1
print(1 if 5 in s else 0)

# find(5) — index of 5, then print from there
if 5 in s:
    idx = s.index(5)
    print(s[idx])
    for val in s[idx:]:
        print(val, end=" ")
    print()
```

### Multset:-
Please refer:- [Multiset|geeksforGeeks](https://www.geeksforgeeks.org/cpp/multiset-in-cpp-stl/)

### Map:-
Maps are associative containers that store key–value pairs in sorted order using a self-balancing Red-Black Tree. They provide efficient O(log n) time complexity for insertion, deletion, and searching operations.
Maps do not allow duplicate keys.
For the curious ones - [Red-Black Tree|GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/)
They support ordered traversal and functions like upper_bound() and lower_bound().

## When to use?
When you need key-value pairs in sorted key order. Use for frequency counting with sorted output, ordered dictionaries, and range queries on keys. Use unordered_map if you don't need sorted order — it's faster (O(1)).

### In cpp
```
#include<iostream>
#include<map>//key value pairs similar to dictionary in python
using namespace std;

int main(){
    map<int, string>m;
    m[1] = "A";
    m[3] = "Boy";
    m[10] = "Top";

    m.insert({5, "Good"});

    for(auto i : m){
        cout<<i.first<<" "<<i.second<<endl;
    }
    cout<<m.count(3)<<endl;//to check whether 3 as a key is present or not
    
    m.erase(3);
    for(auto i : m){
        cout<<i.first<<" "<<i.second<<endl;
    }
    //iterator
    auto itr = m.find(10);//if a key is present then whats is value
    for(auto i = itr; i!=m.end(); i++){
        cout<<(*i).first<<" "<<(*i).second<<endl;
    }
}
```

### In Java
```
import java.util.TreeMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {

        TreeMap<Integer, String> m = new TreeMap<>();
        m.put(1, "A");
        m.put(3, "Boy");
        m.put(10, "Top");

        m.put(5, "Good");  // insert equivalent

        for (Map.Entry<Integer, String> i : m.entrySet()) {
            System.out.println(i.getKey() + " " + i.getValue());
        }

        // count(3) — check if key present (0 or 1)
        System.out.println(m.containsKey(3) ? 1 : 0);

        // erase(3)
        m.remove(3);
        for (Map.Entry<Integer, String> i : m.entrySet()) {
            System.out.println(i.getKey() + " " + i.getValue());
        }

        // find(10) — iterate from key 10 onwards
        for (Map.Entry<Integer, String> i : m.tailMap(10).entrySet()) {
            System.out.println(i.getKey() + " " + i.getValue());
        }
    }
}
```

### In Python
```
# Python dict is ordered (since Python 3.7) but not sorted by key
# Use dict for direct equivalent, or SortedDict for sorted order like C++ map

d = {}
d[1] = "A"
d[3] = "Boy"
d[10] = "Top"

d[5] = "Good"  # insert equivalent

for key, value in sorted(d.items()):  # sorted to match C++ map order
    print(key, value)

# count(3) — check if key present
print(1 if 3 in d else 0)

# erase(3)
del d[3]
for key, value in sorted(d.items()):
    print(key, value)

# find(10) — iterate from key 10 onwards
for key in sorted(d.keys()):
    if key >= 10:
        print(key, d[key])
```

### MultiMap:-
Please refer :- [MultiMap|GeeksforGeeks](https://www.geeksforgeeks.org/cpp/multimap-associative-containers-the-c-standard-template-library-stl/)

## Unordered Associative Containers:-
### Unordered Set:-
Please refer:-[Unordered Set|GeeksforGeeks](https://www.geeksforgeeks.org/cpp/unordered_set-in-cpp-stl/)

### Unordered Multiset:-
Please refer:-[Unordered Multiset|GeeksforGeeks](https://www.geeksforgeeks.org/cpp/cpp-unordered_multiset/)

### Unordered Map:-
Please refer:-[Unordered Map|GeeksforGeeks](https://www.geeksforgeeks.org/cpp/unordered_map-in-cpp-stl/)

### Unordered Multimap:-
Please refer:-[Unordered Multimap|GeeksforGeeks](https://www.geeksforgeeks.org/cpp/unordered_multimap-and-its-application/)


# Algorithms:-

Algorithms are ready-made functions that help you perform common operations like searching, sorting, counting, and comparing on containers (like arrays, vectors, sets, etc.). These algorithms are defined in the <algorithm> and <numeric> header files.

### In cpp
```
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main(){
    vector <int>v;
    v.push_back(5);
    v.push_back(2);
    v.push_back(4);
    v.push_back(10);

    cout<<binary_search(v.begin(), v.end(),4)<<endl;
    cout<<lower_bound(v.begin(), v.end(), 6)-v.begin()<<endl;
    //It finds the first element ≥ target in a sorted container and returns an iterator to it
    cout<<upper_bound(v.begin(), v.end(), 6)-v.begin()<<endl;
    //It finds the first element strictly > target and returns an iterator to it.

    int a = 9;
    int b = 7;
    cout<<max(a, b)<<endl;
    cout<<min(a, b)<<endl;

    swap(a, b);
    cout<<a<<" "<<b<<endl;

    string str = "abcd";
    reverse(str.begin(), str.end());
    cout<<str<<endl;

    rotate(v.begin(), v.begin()+1,v.end());//5 2 4 10->rotate about 2
    for(int v_i:v){
        cout<<v_i<<" ";
    }
    cout<<endl;
    
    sort(v.begin(), v.end());
    for(int v_i:v){
        cout<<v_i<<" ";
    }
    cout<<endl;
}
```

### In Java
```
import java.util.Vector;
import java.util.Collections;
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {

        ArrayList<Integer> v = new ArrayList<>();
        v.add(5);
        v.add(2);
        v.add(4);
        v.add(10);

        // binary_search — list must be sorted first
        ArrayList<Integer> sorted = new ArrayList<>(v);
        Collections.sort(sorted);
        System.out.println(Collections.binarySearch(sorted, 4) >= 0 ? 1 : 0); // 1

        // lower_bound — first index >= 6
        System.out.println(lowerBound(sorted, 6)); // index

        // upper_bound — first index > 6
        System.out.println(upperBound(sorted, 6)); // index

        int a = 9;
        int b = 7;
        System.out.println(Math.max(a, b));  // 9
        System.out.println(Math.min(a, b));  // 7

        // swap
        int temp = a;
        a = b;
        b = temp;
        System.out.println(a + " " + b);     // 7 9

        // reverse string
        String str = "abcd";
        str = new StringBuilder(str).reverse().toString();
        System.out.println(str);             // dcba

        // rotate — move first element to end (rotate by 1)
        v.add(v.remove(0));
        for (int v_i : v) {
            System.out.print(v_i + " ");     // 2 4 10 5
        }
        System.out.println();

        // sort
        Collections.sort(v);
        for (int v_i : v) {
            System.out.print(v_i + " ");     // 2 4 5 10
        }
        System.out.println();
    }

    static int lowerBound(ArrayList<Integer> v, int target) {
        int lo = 0, hi = v.size();
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (v.get(mid) < target) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }

    static int upperBound(ArrayList<Integer> v, int target) {
        int lo = 0, hi = v.size();
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (v.get(mid) <= target) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }
}
```

### In Python
```
from bisect import bisect_left, bisect_right

v = []
v.append(5)
v.append(2)
v.append(4)
v.append(10)

# binary_search — list must be sorted first
sorted_v = sorted(v)
idx = bisect_left(sorted_v, 4)
print(1 if idx < len(sorted_v) and sorted_v[idx] == 4 else 0)  # 1

# lower_bound — first index >= 6
print(bisect_left(sorted_v, 6))   # index

# upper_bound — first index > 6
print(bisect_right(sorted_v, 6))  # index

a = 9
b = 7
print(max(a, b))   # 9
print(min(a, b))   # 7

# swap
a, b = b, a
print(a, b)        # 7 9

# reverse string
str1 = "abcd"
str1 = str1[::-1]
print(str1)        # dcba

# rotate by 1 — move first element to end
v = v[1:] + v[:1]
for v_i in v:
    print(v_i, end=" ")   # 2 4 10 5
print()

# sort
v.sort()
for v_i in v:
    print(v_i, end=" ")   # 2 4 5 10
print()
```

## Resources
[C++ STL - GeeksforGeeks](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/)

[Java Collections Framework - GeeksforGeeks](https://www.geeksforgeeks.org/collections-in-java-2/)

[Python Collections Module - GeeksforGeeks](https://www.geeksforgeeks.org/python-collections-module/)
