## Bit Manipulation 


Bit manipulation is one of the most powerful toolkits in computer science, optimizing execution time and reducing spatial footprints down to the absolute physical limits of hardware. Every piece of data is ultimately stored as low-level binary sequences ( `0` s and `1` s). By operating directly via the CPU's Arithmetic Logic Unit (ALU), bitwise algorithms execute in a single clock cycle, bypassing high-level arithmetic overhead. 

## 1. Basics & Data Representation 

First lets understand how numbers are fundamentally handled inside a machine. 

## Decimal vs. Binary Layout 

The human numeric system operates on base 10, while computing operates on base 2. 

- Base-10 (Decimal): _12310 = 1 × 10[2] + 2 × 10[1] + 3 × 10[0]_ 

• Base-2 (Binary): _1310 = 11012 = 1 × 2[3] + 1 × 2[2] + 0 × 2[1] + 1 × 2[0] = 8 + 4 + 0 + 1_ 

Bits are indexed from right to left, starting at index `0` (the Least Significant Bit, or LSB). 

## Signed vs. Unsigned Bit Representation 

An _n_ -bit variable can store representations differently based on its type definition: 

- Unsigned Representation: All _n_ bits store the magnitude. Ranges from _0_ to _2[n] - 1_ . 

- Signed Representation (Two's Complement): The left-most bit (index _n-1_ ) serves as the Sign Bit ( `0` for non-negative, `1` for negative). It ranges from _-2[n-1]_ to _2[n-1] - 1_ . 

## Two's Complement 

Most  modern  operating  systems  and  modern  processing  units  rely  heavily  on Two's  Complement to represent negative integers because of its elegant structural math. 

To negate a number: 

1. Invert all bits (One's Complement). 

2. Add `1` to the result. 

## _∼ Negative Representation of x → -x = (∼x) + 1_ 

_Example:_ Let _x = 4310_ in an 8-bit system: 

- _43 = 00101011 10 2_ 

- Invert bits ( _∼43_ ): _110101002_ 




- Add 1: _110101012_ (which equals _-4310_ ) 

Why do systems use this? It unifies standard subtraction and addition operations into the exact same hardware circuit loop. The ALU can ignore signed vs. unsigned identities during integer addition, meaning it does not need a separate logic circuit for subtraction. However, this architectural design simplicity does not carry over to hardware division algorithms. 

## Overflow Dynamics 

When an operations exceeds bounded limits: 

- Unsigned: Exceeding _2[n] - 1_ wraps cleanly back to `0` . 

- Signed: Exceeding _2[n-1] - 1_ overflows directly into the sign bit, wrapping around to the minimum negative value _-2[n-1]_ . For example, in a 32-bit signed integer space, `2,147,483,647 + 1` instantly becomes `-2,147,483,648` . 

## 2. The 6 Primitive Bitwise Operators 

Here is a breakdown of the standard primitive operators that act as your basic programming building blocks. 

## 1. Bitwise AND ( `&` ) 

Compares each bit position. Yields `1` if and only if both bits are `1` . 

- _Identity properties:_ `X & 0 = 0` , `X & 1s = X` , `X & X = X` 

## 2. Bitwise OR ( `|` ) 

Yields `1` if at least one corresponding bit position is `1` . 

- _Identity properties:_ `X | 0 = X` , `X | 1s = 1s` , `X | X = X` 

## 3. Bitwise XOR ( `^` ) 

_Exclusive Or_ . Yields `1` if the compared bits are opposite ( `0` and `1` ). If they match, it yields `0` . 

- _Crucial mathematical properties for tracking states:_ 

- Commutative: _A μ B = B μ A_ 

- Associative: _A μ (B μ C) = (A μ B) μ C_ 

- Self-Inverse identity: _X μ X = 0_ 

- Identity element: _X μ 0 = X_ 

## `~` 4. Bitwise NOT ( ) 

A unary operator that flips every individual bit ( `1` becomes `0` , `0` becomes `1` ). 



_Caution:_ Flipping numbers transforms leading unassigned structural zeros into ones. Thus, `~9` in a restricted 4-bit simulation produces `6` , but inside real-world 32-bit compilation hardware environments, it produces `-10` . 

## 5. Left Shift ( `<<` ) 

Shifts bit tracks to the left by filling empty LSB vacancies with trailing `0` blocks. Shifting a value left by _k_ positions effectively multiplies it by _2[k]_ : 

==> picture [74 x 14] intentionally omitted <==

## 6. Right Shift ( `>>` and `>>>` ) 

Moves bit tracks down to the right, discarding overflowing LSB values. 

- Arithmetic Right Shift ( `>>` ): Retains sign information. If the number is negative, it fills empty spaces on the left with `1` s; if it's positive, it fills them with `0` s. This acts as integer division: _⌊n / 2[k] ⌋_ . 

- Logical Right Shift ( `>>>` ): Always fills empty spaces on the left with `0` s, regardless of the sign. This treats the value purely as an unsigned sequence. (Note: Java supports `>>>` natively, while C++ and Python handle this contextually based on data type sizing rules). 

## Undefined Behavior Alerts 

- Shifting by a value equal to or greater than the bit-width of the type (e.g., shifting a 32-bit `int` by 32 or more) is undefined in C++. 

- Shifting by negative counts yields unpredictable, undefined results across systems. 

- 

- Bitwise operators have lower precedence than arithmetic and comparison operators. Always wrap bit expressions in parentheses: `if ((num & 1) == 0)` instead of `if (num & 1 == 0)` . 

## 3. Core Bitmasking Mechanics 

These foundational single-line bit tricks form the basis for more advanced problem-solving patterns. 

## 1. Inspecting a Target Bit (Get Bit) 

To determine if the bit at index _i_ is set ( `1` ) or unset ( `0` ), shift `1` left by _i_ positions to create a mask, then perform an AND operation. If the result is non-zero, the bit is set. 

## 2. Setting a Target Bit (Set Bit) 

To force the bit at index _i_ to become `1` without modifying any other values, use an OR operation with a shifted bit mask. 

## 3. Clearing a Target Bit (Clear/Unset Bit) 

To force the bit at index _i_ to become `0` , invert the bitmask using a NOT operator ( `~` ), then combine it with the number using an AND operation. 

## 4. Toggling a Target Bit (Flip Bit) 

To invert the bit at index _i_ (swapping `1` to `0` or `0` to `1` ), perform an XOR operation with your shifted mask. 

## Code 

## C++ 

```
class BitCore {
public:
    static bool getBit(int n, int i) {
        return (n & (1 << i)) != 0;
    }
    static int setBit(int n, int i) {
        return n | (1 << i);
    }
    static int clearBit(int n, int i) {
        return n & ~(1 << i);
    }
    static int toggleBit(int n, int i) {
        return n ^ (1 << i);
    }
};
```

## Java 

```
public class BitCore {
    public static boolean getBit(int n, int i) {
        return (n & (1 << i)) != 0;
    }
    public static int setBit(int n, int i) {
        return n | (1 << i);
    }
    public static int clearBit(int n, int i) {
        return n & ~(1 << i);
    }
    public static int toggleBit(int n, int i) {
        return n ^ (1 << i);
    }
}
```


## Python 

```
class BitCore:
    @staticmethod
    def get_bit(n: int, i: int) -> bool:
        return (n & (1 << i)) != 0
    @staticmethod
    def set_bit(n: int, i: int) -> int:
        return n | (1 << i)
    @staticmethod
    def clear_bit(n: int, i: int) -> int:
        return n & ~(1 << i)
    @staticmethod
    def toggle_bit(n: int, i: int) -> int:
        return n ^ (1 << i)
```

## 4. Advanced Bitwise Optimization Tactics 

## Clearing the Lowest Set Bit ( `n & (n - 1)` ) 

This operation strips away the rightmost active `1` from a binary sequence. 

- How is works: Subtracting `1` from a binary number flips all the bits from the right up to the first `1` it encounters. Combining `n` with `n-1` via a bitwise AND cleanly eliminates that rightmost `1` . 

- Application: This approach is central to Brian Kernighan’s Algorithm , which counts the number of set bits in a value. Instead of checking every single bit sequentially through 32 iterations, this algorithm runs only as many times as there are active `1` bits. 

## Finding the Lowest Set Bit ( `n & -n` ) 

Isolates the lowest set bit, returning a value where only that specific bit is active. 

- How it works: In two's complement, `-n` equals `~n + 1` . This transformation ensures that all bits to the left of the lowest set bit are inverted, while the lowest set bit itself and any trailing zeros remain unchanged. Performing an AND operation between `n` and `-n` cancels out everything except that single lowest `1` . 

## Range Clears 

- Clear from LSB up to index _i_ : `n &= ~((1 << (i + 1)) - 1)` 

- Clear from MSB down to index _i_ : `n &= ((1 << i) - 1)` 


## Bitwise String Conversions (Character Case Manipulation) 

In the ASCII table, uppercase and lowercase letters are separated by exactly 32 digits, which corresponds directly to bit index 5 ( `1 << 5` , or the space character `' '` ). 

- In Uppercase letters, bit 5 is unset ( `0` ). 

- In Lowercase letters, bit 5 is set ( `1` ). 

This structure enables incredibly fast case conversions: 

- Convert to Lowercase: `ch | 32` (or `ch | ' '` ) 

- 

- Convert to Uppercase: `ch & ~32` (or `ch & '_'` ) 

- Invert Case: `ch ^ 32` 

## 5. Algorithmic Problem-Solving Design Patterns 

These six structured patterns cover the vast majority of bit manipulation interview problems and competitive programming challenges. 

## Pattern 1: Power of Two Validation 

An integer is a power of two if it is greater than zero and contains exactly one set bit in its binary layout. If you clear its lowest set bit using `n & (n - 1)` , the value must drop to zero. 

## Code Implementation 

- C++: `bool isPowerOfTwo(int n) { return n > 0 && (n & (n - 1)) == 0; }` 

## Java: 

- 

```
public static boolean isPowerOfTwo(int n) { return n > 0 && (n & (n - 1)) == 0; }
```

- Python: `def is_power_of_two(n: int) -> bool: return n > 0 and (n & (n - 1)) == 0` 

## Pattern 2: Single Number / Frequency Isolation 

This pattern uses the unique properties of the XOR operation to find elements with anomalous frequencies in an array. 

## Variations 

1. Every element appears twice except for one: XORing all the numbers together cancels out the duplicates ( _X μ X = 0_ ), leaving behind the single unique value. 

2. Every element appears twice except for two unique numbers ( _x_ and _y_ ): XORing all elements yields 

   - `xor_xy = x ^ y` . Since _x_ and _y_ are distinct, `xor_xy` must have at least one set bit. You can isolate 

   - this bit using `xor_xy & -xor_xy` , and then use it as a mask to split the array into two groups. XORing each group separately isolates _x_ and _y_ . 



## C++ 

```
#include <vector>
#include <utility>

std::pair<int, int> findTwoUnique(const std::vector<int>& nums) {
    int xor_all = 0;
    for (int num : nums) xor_all ^= num;
    int rsb = xor_all & -xor_all;
    int x = 0, y = 0;
    for (int num : nums) {
        if ((num & rsb) != 0) x ^= num;
        else y ^= num;
    }
    return {x, y};
}
```

## Java 

```
public class SingleNumber {
    public static int[] findTwoUnique(int[] nums) {
        int xorAll = 0;
        for (int num : nums) xorAll ^= num;
        int rsb = xorAll & -xorAll;
        int x = 0, y = 0;
        for (int num : nums) {
            if ((num & rsb) != 0) x ^= num;
            else y ^= num;
        }
        return new int[]{x, y};
    }
}
```


## Python 

```
from typing import List, Tuple
def find_two_unique(nums: List[int]) -> Tuple[int, int]:
    xor_all = 0
    for num in nums:
        xor_all ^= num
    rsb = xor_all & -xor_all
    x, y = 0, 0
    for num in nums:
        if num & rsb:
            x ^= num
        else:
            y ^= num
    return x, y
```

## Pattern 3: Subsequence Generation via Combinatorial Masking 

An array of size _N_ has _2[N]_ possible subsequences. You can map each subsequence to a unique integer ranging from _0_ to _2[N] - 1_ , using the individual bits of the integer to decide whether to include or exclude an element. 

## C++ 

```
#include <iostream>
#include <vector>
void generateSubsequences(const std::vector<int>& arr) {
    int n = arr.size();
    int total = 1 << n;
    for (int mask = 0; mask < total; ++mask) {
        std::cout << "[ ";
        for (int i = 0; i < n; ++i) {
            if ((mask & (1 << i)) != 0) {
                std::cout << arr[i] << " ";
            }
        }
        std::cout << "]
";
    }
}
```



Java 

```
public class Combinatorics {
    public static void generateSubsequences(int[] arr) {
        int n = arr.length;
        int total = 1 << n;
        for (int mask = 0; mask < total; mask++) {
            System.out.print("[ ");
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    System.out.print(arr[i] + " ");
                }
            }
            System.out.println("]");
        }
    }
}
```

## Python 

```
from typing import List

def generate_subsequences(arr: List[int]) -> None:
    n = len(arr)
    total = 1 << n
    for mask in range(total):
        subsequence = [arr[i] for i in range(n) if (mask & (1 << i)) != 0]
        print(subsequence)
```

## Pattern 4: Consecutive Range Accumulation (XOR from 1 to _N_ ) 

Calculating the cumulative XOR sum from 1 to _N_ sequentially takes _O(N)_ time. However, the prefix XOR sequence follows a predictable pattern that repeats every 4 steps, allowing you to compute the result in _O(1)_ constant time. 

|Range Max (_N_)|Binary Structural Behavior Pattern|Output Value|
|---|---|---|
|_N % 4 == 0_|Bits naturally stabilize at full range balance|_N_|
|_N % 4 == 1_|Unpaired LSB single bit leftovers remain|`1`|
|_N % 4 == 2_|Value rolls over into next step index|_N + 1_|
|_N % 4 == 3_|All matching bit combinations cancel out|`0`|


## C++ 

```
int computePrefixXOR(int n) {
    if (n % 4 == 0) return n;
    if (n % 4 == 1) return 1;
    if (n % 4 == 2) return n + 1;
    return 0;
}
```

## Java 

```
public class RangeXOR {
    public static int computePrefixXOR(int n) {
        if (n % 4 == 0) return n;
        if (n % 4 == 1) return 1;
        if (n % 4 == 2) return n + 1;
        return 0;
    }
}
```

## Python 

```
def compute_prefix_xor(n: int) -> int:
    if n % 4 == 0: return n
    if n % 4 == 1: return 1
    if n % 4 == 2: return n + 1
    return 0
```

## Pattern 5: Multi-Bit Range Queries using Prefix Sum Arrays 

When working with an array of numbers, finding the bitwise AND, OR, or XOR across a specific range _[L, R]_ typically requires checking every element in that range. However, you can optimize this process by using a 2D prefix sum array to track the distribution of bits at each index. 

Since integers are bounded (e.g., 32 bits), you can maintain a prefix sum array for each bit position. For example, `psum[i][j]` tracks how many times the _j_ -th bit is active from the start of the array up to index _i_ . To find the bitwise AND across a range _[L, R]_ , check the bit count for each position _j_ : 

## _count = psum[R+1][j] - psum[L][j]_ 

If _count == R - L + 1_ (meaning the bit is active in every single element in that range), then the _j_ -th bit of the final result is set to `1` . Otherwise, it is set to `0` . This reduces the query time from _O(N)_ to a constant _O(32) → O(1)_ operations. 


C++ 

```
#include <vector>
```

```
class RangeBitQuery {
    std::vector<std::vector<int>> psum;
public:
    RangeBitQuery(const std::vector<int>& nums) {
        int n = nums.size();
        psum.assign(n + 1, std::vector<int>(32, 0));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < 32; ++j) {
                psum[i + 1][j] = psum[i][j] + ((nums[i] >> j) & 1);
            }
        }
    }
    int queryRangeAND(int L, int R) {
        int result = 0;
        int rangeSize = R - L + 1;
        for (int j = 0; j < 32; ++j) {
            int activeBitsCount = psum[R + 1][j] - psum[L][j];
            if (activeBitsCount == rangeSize) {
                result |= (1 << j);
            }
        }
        return result;
    }
};
```
 

Java 

```
public class RangeBitQuery {
    private int[][] psum;
    public RangeBitQuery(int[] nums) {
        int n = nums.length;
        psum = new int[n + 1][32];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 32; j++) {
                psum[i + 1][j] = psum[i][j] + ((nums[i] >> j) & 1);
            }
        }
    }
    public int queryRangeAND(int L, int R) {
        int result = 0;
        int rangeSize = R - L + 1;
        for (int j = 0; j < 32; j++) {
            int activeBitsCount = psum[R + 1][j] - psum[L][j];
            if (activeBitsCount == rangeSize) {
                result |= (1 << j);
            }
        }
        return result;
    }
}
```


Python 

```
from typing import List
class RangeBitQuery:
    def __init__(self, nums: List[int]):
        n = len(nums)
        self.psum = [[0] * 32 for _ in range(n + 1)]
        for i in range(n):
            for j in range(32):
                self.psum[i + 1][j] = self.psum[i][j] + ((nums[i] >> j) & 1)
    def query_range_and(self, L: int, R: int) -> int:
        result = 0
        range_size = R - L + 1
        for j in range(32):
            active_bits_count = self.psum[R + 1][j] - self.psum[L][j]
            if active_bits_count == range_size:
                result |= (1 << j)
        return result
```

## Pattern 6: Arithmetic Operator Restrictions (Bitwise Math Emulation) 

This pattern focuses on performing standard arithmetic operations—such as addition, multiplication, division, or exponentiation—without using traditional mathematical operators. 

Example: Dividing Two Integers without `*` , `/` , or `%` 

You can find the quotient of a division problem by using bit shifting to subtract large, power-of-two multiples of the divisor from the dividend. This approach mimics long division but operates in binary, cutting the execution time down to _O(log(Dividend))_ . 


C++ 

```
#include <climits>
#include <cmath>

int divideIntegers(long long dividend, long long divisor) {
    if (dividend == divisor) return 1;

    bool isNegative = (dividend < 0) ^ (divisor < 0);

    unsigned long long absoluteDividend = std::abs(dividend);
    unsigned long long absoluteDivisor = std::abs(divisor);
    unsigned long long result = 0;

    for (int i = 31; i >= 0; --i) {
        if ((absoluteDivisor << i) <= absoluteDividend) {
            absoluteDividend -= (absoluteDivisor << i);
            result += (1ULL << i);
        }
    }

    if (result > INT_MAX && !isNegative) return INT_MAX;
    return isNegative ? -static_cast<int>(result) : static_cast<int>(result);
}
```

## Java 

```
public class BitwiseMath {

    public static int divideIntegers(int dividend, int divisor) {
        if (dividend == divisor) return 1;

        boolean isNegative = (dividend < 0) ^ (divisor < 0);

        long absoluteDividend = Math.abs((long) dividend);
        long absoluteDivisor = Math.abs((long) divisor);
        long result = 0;

        for (int i = 31; i >= 0; i--) {
            if ((absoluteDivisor << i) <= absoluteDividend) {
                absoluteDividend -= (absoluteDivisor << i);
                result += (1L << i);
            }
        }

        if (result > Integer.MAX_VALUE && !isNegative) return Integer.MAX_VALUE;
        return isNegative ? (int) -result : (int) result;
    }
}
```


## Python 

```
def divide_integers(dividend: int, divisor: int) -> int:
    INT_MAX = 231 - 1
    INT_MIN = -231
    if dividend == divisor: return 1
    is_negative = (dividend < 0) ^ (divisor < 0)
    absolute_dividend = abs(dividend)
    absolute_divisor = abs(divisor)
    result = 0
    for i in range(31, -1, -1):
        if (absolute_divisor << i) <= absolute_dividend:
            absolute_dividend -= (absolute_divisor << i)
            result += (1 << i)
    if is_negative:
        result = -result
    return min(max(INT_MIN, result), INT_MAX)
```

## 6. Built-in Language Support (Compiler Specializations) 

Most  modern  languages  provide  built-in,  highly  optimized  functions  that  map  directly  to  specific  CPU instructions, running significantly faster than manual loops. 

|Operational<br>Feature|C++ Built-in (GCC)|Java Standard Library|Python Bit<br>Methods|
|---|---|---|---|
|Count<br>Active Set<br>Bits|`__builtin_popcount(x)`|`Integer.bitCount(x)`|`x.bit_count()`|
|Count<br>Leading<br>Zeros|`__builtin_clz(x)`|`Integer.numberOfLeadingZeros(x)`|Manual<br>Calculation|
|Count<br>Trailing<br>Zeros|`__builtin_ctz(x)`|`Integer.numberOfTrailingZeros(x)`| Manual <br> Calculation|

 

## Quick Code References 

## C++ 

```
int x = 43; // 00101011
int set_bits = __builtin_popcount(x);
int lead_zeros = __builtin_clz(x);
int trail_zeros = __builtin_ctz(x);
```

## Java 

```
int x = 43;
int setBits = Integer.bitCount(x);
int leadZeros = Integer.numberOfLeadingZeros(x);
int trailZeros = Integer.numberOfTrailingZeros(x);
```

## Python 

```
x = 43
set_bits = x.bit_count()
# For trailing zeros using bit properties
trail_zeros = (x & -x).bit_length() - 1 if x else 0
```

## 7. Strategic Cheatsheet 

1. Bitwise vs. Logical Operators: Don't confuse single operators ( `&` , `|` ) with double logical operators ( `&&` , `||` ). Mixing them up can introduce subtle bugs and bypass short-circuit evaluation logic. 

2. Parentheses are Mandatory: Because operators like `==` and `+` take precedence over `&` , `|` , and `^` , always wrap your bitwise comparisons in parentheses: `if ((x & 1) == 0)` . 

3. Use Type Extensions: When shifting values in 64-bit spaces, make sure your literals match the scale. In C++ and Java, use long literals ( `1LL << i` or `1L << i` ) to avoid integer overflow. 

4. Isolate Before Partitioning: When a problem involves two distinct hidden values, find the total combined XOR sum first, isolate the lowest set bit using `x & -x` , and use that bit as a mask to separate the data into manageable groups. 


## Resources

### GeeksforGeeks Data Structures & Algorithms
* [All About Bit Manipulation](https://www.geeksforgeeks.org/dsa/all-about-bit-manipulation/)
    
* [Bit Manipulation Important Tactics](https://www.geeksforgeeks.org/dsa/bits-manipulation-important-tactics/)
   

###  CP-Algorithms (Competitive Programming Engine)
* [Algebra: Bit Manipulation Techniques](https://cp-algorithms.com/algebra/bit-manipulation.html)
    

###  LeetCode Discuss (Interview Patterns Repository)
* [All Types of Patterns for Bit Manipulation](https://leetcode.com/discuss/post/3695233/all-types-of-patterns-for-bits-manipulat-qezp/)
    
