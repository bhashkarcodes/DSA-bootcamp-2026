# Strings

Home > Week 2 > Strings

Week 2 · Topic 1 of 4 · Prerequisites: Arrays, STL Basics

## Why This Topic Now

Strings are arrays of characters — you already know arrays. But strings come with their own rules: immutability in most languages, a rich set of built-in operations, and a small alphabet (only 26 lowercase letters, 256 ASCII characters) that unlocks clever frequency-counting tricks. Most real interview problems involve strings in some form.

## Key Facts Before You Start

| Language | Mutable? | Type |
|----------|----------|------|
| C++ (`std::string`) | Yes | Object with built-in methods |
| C (char array) | Yes | Raw array, null-terminated (`\0`) |
| Java (`String`) | No | Immutable object; use `StringBuilder` to build |
| Python (`str`) | No | Immutable; use list of chars or `join()` to build |

> **Why does immutability matter?** In Java and Python, every `s = s + c` inside a loop creates a new string — O(n²) total. Always use `StringBuilder` (Java) or a list + `''.join()` (Python) when building strings character by character.

## Memory Representation

In C++, strings are stored as a contiguous array of characters ending with `\0`. In Java and Python, the runtime manages memory internally — you never see the null terminator.

![String Memory Diagram](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week%202/wncc_string_diagram.png)

---

## Are Strings Mutable in Different Languages?

Mutability means whether you can change individual characters of a string after it's created. This matters a lot for how you write efficient string-manipulation code.

- In **C** and **C++**, string *literals* (e.g. `const char* s = "hello"`) are stored in read-only memory — trying to modify them is undefined behaviour.
- In **C++**, `std::string` *objects* are mutable — you can change characters freely.
- In **Java** and **Python**, all strings are immutable — every "modification" actually creates a brand new string in memory.

### In C++

The code below tries to modify a string literal — this is a compile-time error because the pointer points to read-only memory:

```cpp
#include <iostream>
using namespace std;

int main() {
    const char* str = "Hello, world!";
    str[0] = 'h';  // Error: assignment to read-only location
    cout << str;
    return 0;
}
```

If you want a mutable string in C++, use `std::string` instead of `const char*`.

### In Java

`concat()` does not modify `s1` — it returns a new string that is immediately discarded here. This is a very common beginner mistake:

```java
import java.io.*;

class Define {
    public static void main(String[] args) {
        String s1 = "java";
        s1.concat(" rules");  // returns a new String — s1 is unchanged

        // s1 is not changed because strings are immutable
        System.out.println("s1 refers to " + s1);  // prints: s1 refers to java
    }
}
```

To actually append, you must reassign: `s1 = s1.concat(" rules");` or use `StringBuilder`.

### In Python

Attempting to assign to a character index raises a `TypeError` at runtime:

```python
s = "WnCC"

# This will cause a TypeError because strings are immutable
s[1] = 'f'

print(s)
```

To "modify" a string in Python, build a new one: `s = s[0] + 'f' + s[2:]`

---

## Declaration

**C++**
```cpp
#include <string>
using namespace std;

string s1 = "Welcome to DSA Bootcamp";   // double quotes only
string s2("WnCC");                        // constructor form — same result
```

**Java**
```java
String s = "WnCC";                  // string literal — stored in string pool (interned)
String s1 = new String("WnCC");     // explicit heap object — rarely needed, avoids interning
```

**Python**
```python
s1 = 'Welcome to DSA Bootcamp'     # single quotes
s2 = "WnCC"                         # double quotes — no difference in Python
s3 = '''Multi
line string'''                       # triple quotes for multi-line strings
```

---

## Core Operations

### Accessing characters

Use `[]` for fast access with no safety check, or `.at()` / `.charAt()` for bounds-checked access that throws an exception on out-of-range index.

**C++**
```cpp
string s = "Hello World";
cout << s[0];        //'H' - Access using index operator []
cout << s.at(6);     //'W' -  Access using at()
```

**Java**
```java
String s = "Hello World";
System.out.println(s.charAt(0));   // 'H' 
System.out.println(s.charAt(6));   // 'W' — // Access using charAt() (equivalent to at())
```

**Python**
```python
s = "Hello World"
print(s[0])    # 'H'
print(s[6])    # 'W' — # Access using index operator []
```

---

### Length

**C++**
```cpp
string s = "WnCC";
cout << s.size();    // 4 — preferred
cout << s.length();  // 4 — identical, both work
```

**Java**
```java
String s = "WnCC";
System.out.println(s.length());   // 4 — note: arrays use .length (no parens), strings use .length()
```

**Python**
```python
s = "WnCC"
print(len(s))   # 4 — len() works on any sequence: strings, lists, tuples
```

---

### Equality check

**C++**
```cpp
string s1 = "abc", s2 = "abcd";
cout << (s1 == s2);   // 0 (false) — == compares content, works correctly
```

**Java**
```java
// NEVER use == for strings in Java — it compares memory addresses (references), not content.
// Two strings with the same characters may live at different addresses and == will return false.
String s1 = "abc", s2 = "abcd";
System.out.println(s1.equals(s2));          // false — correct: compares content
System.out.println(s1.equalsIgnoreCase(s2)); // false — use this for case-insensitive comparison
```

**Python**
```python
s1, s2 = "abc", "abcd"
print(s1 == s2)   # False — == always compares content in Python, safe to use
```

---

### Search for a character

Returns the index of the first occurrence. Returns -1 (or `string::npos` in C++) if not found.

**C++**
```cpp
string s = "DSA Bootcamp";
size_t pos = s.find('k');                   // returns string::npos (a large number) if not found
cout << (pos != string::npos ? (int)pos : -1);  // prints 11
```

**Java**
```java
String s = "DSA Bootcamp";
System.out.println(s.indexOf('k'));    // 11
System.out.println(s.indexOf('z'));    // -1 (not found)
System.out.println(s.indexOf('o', 5)); // search starting from index 5 — finds second 'o'
```

**Python**
```python
s = "DSA Bootcamp"
print(s.find('k'))     # 11
print(s.find('z'))     # -1 (not found)
print(s.index('k'))    # 11 — same as find() but raises ValueError if not found
```

---

### Insert a character

**C++**
```cpp
string s = "WnCC";
s.insert(s.begin() + 3, 'A');   // iterator-based: inserts 'A' before index 3 → "WnCAC"
// or: s.insert(3, 1, 'A');     // position, count, char — more readable
cout << s;
```

**Java**
```java
// String is immutable — use StringBuilder for any modification
StringBuilder sb = new StringBuilder("WnCC");
sb.insert(3, 'A');                    // inserts at index 3 → "WnCAC"
System.out.println(sb.toString());
```

**Python**
```python
s = "WnCC"
# Python strings are immutable — build a new string using slicing
s = s[:3] + 'A' + s[3:]   # "WnCAC"
print(s)
```

---

### Remove a character at a position

**C++**
```cpp
string s = "abcde";
s.erase(1, 1);   // erase(start_index, count) — removes 1 char at index 1 → "acde"
cout << s;
```

**Java**
```java
StringBuilder s = new StringBuilder("abcde");
s.deleteCharAt(1);             // removes character at index 1 → "acde"
System.out.println(s);
```

**Python**
```python
s = "abcde"
s = s[:1] + s[2:]   # skip index 1 → "acde"
print(s)
```

---

### Remove all occurrences of a character

**C++**
```cpp
#include <algorithm>
string s = "ababca";
// remove() shifts non-'a' chars to the front and returns a new end iterator.
// erase() then cuts off the leftover chars at the end.
s.erase(remove(s.begin(), s.end(), 'a'), s.end());
cout << s;   // "bbc"
```

**Java**
```java
String s = "ababca";
s = s.replace("a", "");   // replaces all occurrences of "a" with "" → "bbc"
System.out.println(s);
```

**Python**
```python
s = "ababca"
s = s.replace('a', '')   # replaces all 'a' with nothing → "bbc"
print(s)
```

---

### Concatenation

**C++**
```cpp
string s1 = "Hello ", s2 = "WnCC";
string res = s1 + s2;   // creates a new string
cout << res;            // "Hello WnCC"
```

**Java**
```java
// + is fine for a single concatenation.
// In a loop (e.g. building a string char by char), use StringBuilder — otherwise
// each + creates a new String object and you get O(n²) total copies.
String s1 = "Hello ", s2 = "WnCC";
System.out.println(s1 + s2);   // "Hello WnCC"

// Loop example — use this pattern:
StringBuilder sb = new StringBuilder();
for (char c : s1.toCharArray()) sb.append(c);
String result = sb.toString();
```

**Python**
```python
s1, s2 = "Hello ", "WnCC"
print(s1 + s2)   # "Hello WnCC"

# In a loop, collect into a list and join at the end — O(n) instead of O(n²)
parts = []
for c in s1:
    parts.append(c)
result = ''.join(parts)
```

---

### Reverse a string

**C++**
```cpp
#include <algorithm>
string s = "abcdef";
reverse(s.begin(), s.end());   // modifies s in-place using two-pointer swap
cout << s;   // "fedcba"
```

**Java**
```java
String s = "abcdef";
// StringBuilder has a built-in reverse() — most idiomatic
String rev = new StringBuilder(s).reverse().toString();
System.out.println(rev);   // "fedcba"
```

**Python**
```python
s = "abcdef"
print(s[::-1])   # "fedcba" — slice with step -1 reads the string backwards
```

---

## Topics via Reference

Some operations are better learned by reading the canonical explanation and then implementing yourself:

- [Rotate a String — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/left-rotation-right-rotation-string-2/)
- [Generate all Substrings — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/program-print-substrings-given-string/)
- [Check for Palindrome — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/palindrome-string/)

---

## Before You Move On

- Can you reverse a string without using a built-in reverse function?
- Do you know why `==` is wrong for string comparison in Java?
- Can you check if a string is a palindrome in O(n) time and O(1) space?
- Can you remove all occurrences of a character in a single pass?

## Resources

- [Strings in C++ — GeeksforGeeks](https://www.geeksforgeeks.org/cpp/strings-in-cpp/)
- [Strings in Java — GeeksforGeeks](https://www.geeksforgeeks.org/java/strings-in-java/)
- [Strings in Python — GeeksforGeeks](https://www.geeksforgeeks.org/python/python-string/)


