# Bit Manipulation

Bit manipulation is a technique used to perform operations at the bit level. It is a powerful tool in programming, especially in scenarios where performance and memory efficiency are critical.

---

## Why Use Bit Manipulation?
- **Efficiency:** Bit-level operations are faster than arithmetic operations.
- **Memory Optimization:** Useful for compact data representation.
- **Low-Level Programming:** Essential for systems programming, embedded systems, and hardware interfacing.

---

## Common Bitwise Operators

| Operator | Symbol | Description |
|----------|--------|-------------|
| AND      | `&`    | Sets each bit to 1 if both bits are 1. |
| OR       | `|`    | Sets each bit to 1 if one of the bits is 1. |
| XOR      | `^`    | Sets each bit to 1 if only one of the bits is 1. |
| NOT      | `~`    | Inverts all the bits. |
| Left Shift | `<<` | Shifts bits to the left, filling with 0s. |
| Right Shift | `>>` | Shifts bits to the right, filling with the sign bit. |

### Example: Basic Bitwise Operations
```cpp
#include <iostream>

int main() {
    int a = 5;  // 0101 in binary
    int b = 3;  // 0011 in binary

    std::cout << "a & b: " << (a & b) << '\n'; // 0001 -> 1
    std::cout << "a | b: " << (a | b) << '\n'; // 0111 -> 7
    std::cout << "a ^ b: " << (a ^ b) << '\n'; // 0110 -> 6
    std::cout << "~a: " << (~a) << '\n';       // 1010 -> -6 (2's complement)
    std::cout << "a << 1: " << (a << 1) << '\n'; // 1010 -> 10
    std::cout << "a >> 1: " << (a >> 1) << '\n'; // 0010 -> 2

    return 0;
}
```

---

## Common Applications

### 1. Checking if a Number is Power of Two
A number is a power of two if it has only one bit set.
```cpp
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

### 2. Counting Set Bits
Count the number of 1s in the binary representation of a number.
```cpp
int countSetBits(int n) {
    int count = 0;
    while (n) {
        count += n & 1;
        n >>= 1;
    }
    return count;
}
```

### 3. Swapping Two Numbers Without a Temporary Variable
```cpp
void swap(int &a, int &b) {
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}
```

### 4. Reversing Bits
Reverse the bits of a number.
```cpp
unsigned int reverseBits(unsigned int n) {
    unsigned int result = 0;
    for (int i = 0; i < 32; ++i) {
        result <<= 1;
        result |= (n & 1);
        n >>= 1;
    }
    return result;
}
```

---

## Analysis
- **Advantages:**
  - High performance for low-level operations.
  - Compact code for specific tasks.
- **Disadvantages:**
  - Reduced code readability.
  - Error-prone for complex operations.

---

## Best Practices
- Use bit manipulation for performance-critical code.
- Add comments to improve code readability.
- Test thoroughly to avoid subtle bugs.

---

## Summary
Bit manipulation is a fundamental technique in programming, offering high performance and memory efficiency. While it can be challenging to master, its applications are vast and invaluable in many domains.

---

### References
- [C++ Reference for Bitwise Operators](https://en.cppreference.com/w/cpp/language/operator_arithmetic)
- [GeeksforGeeks: Bit Manipulation](https://www.geeksforgeeks.org/bit-manipulation/)