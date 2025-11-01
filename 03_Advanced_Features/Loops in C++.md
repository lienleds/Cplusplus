# Loops in C++

Loops allow repeated execution of code blocks. C++ supports several types of loops for different iteration patterns.

---

## Overview

C++ provides several loop constructs:
- **`for` loop** - When iteration count is known
- **`while` loop** - Condition-based iteration
- **`do-while` loop** - Execute at least once
- **Range-based `for` loop** (C++11) - Iterate over containers
- **`goto` statement** - Jump-based (avoid in modern C++)

---

## 1. For Loop

### Basic For Loop

```cpp
#include <iostream>

int main() {
    // Standard for loop
    for (int i = 0; i < 5; ++i) {
        std::cout << i << " ";
    }
    std::cout << '\n';
    
    // Multiple initializers
    for (int i = 0, j = 10; i < 5; ++i, --j) {
        std::cout << "i=" << i << ", j=" << j << '\n';
    }
    
    // Different step sizes
    for (int i = 0; i < 10; i += 2) {
        std::cout << i << " ";
    }
    std::cout << '\n';
    
    // Reverse iteration
    for (int i = 5; i > 0; --i) {
        std::cout << i << " ";
    }
    std::cout << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Best for counted loops
- ✅ All loop control in one line
- ✅ Loop variable scope limited
- 📝 Use prefix `++i` for efficiency (habit)

### Infinite For Loop

```cpp
#include <iostream>

int main() {
    int count = 0;
    
    // Infinite loop with break
    for (;;) {
        std::cout << count << " ";
        ++count;
        
        if (count >= 5) {
            break;
        }
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 2. While Loop

### Basic While Loop

```cpp
#include <iostream>

int main() {
    int i = 0;
    
    while (i < 5) {
        std::cout << i << " ";
        ++i;
    }
    std::cout << '\n';
    
    // While with complex condition
    int x = 10, y = 0;
    while (x > 0 && y < 5) {
        std::cout << "x=" << x << ", y=" << y << '\n';
        --x;
        ++y;
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Best when iteration count unknown
- ✅ Condition checked before execution
- ⚠️ Easy to create infinite loops
- 💡 Good for input validation

### While with Input

```cpp
#include <iostream>

int main() {
    int number;
    
    std::cout << "Enter positive numbers (0 to quit):\n";
    while (std::cin >> number && number != 0) {
        std::cout << "You entered: " << number << '\n';
    }
    
    std::cout << "Done!\n";
    
    return 0;
}
```

---

## 3. Do-While Loop

### Basic Do-While

```cpp
#include <iostream>

int main() {
    int i = 0;
    
    do {
        std::cout << i << " ";
        ++i;
    } while (i < 5);
    std::cout << '\n';
    
    // Executes at least once even if condition is false
    int j = 10;
    do {
        std::cout << "This prints once: " << j << '\n';
    } while (j < 5);
    
    return 0;
}
```

**Analysis:**
- ✅ Executes at least once
- ✅ Good for menu systems
- 📝 Condition checked after execution
- 💡 Less common than `for` or `while`

### Menu Example

```cpp
#include <iostream>

int main() {
    int choice;
    
    do {
        std::cout << "\n=== Menu ===\n";
        std::cout << "1. Option 1\n";
        std::cout << "2. Option 2\n";
        std::cout << "3. Exit\n";
        std::cout << "Enter choice: ";
        std::cin >> choice;
        
        switch (choice) {
            case 1:
                std::cout << "You selected Option 1\n";
                break;
            case 2:
                std::cout << "You selected Option 2\n";
                break;
            case 3:
                std::cout << "Exiting...\n";
                break;
            default:
                std::cout << "Invalid choice\n";
        }
    } while (choice != 3);
    
    return 0;
}
```

---

## 4. Range-Based For Loop (C++11)

### Iterating Containers

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>

int main() {
    // Array
    int arr[] = {1, 2, 3, 4, 5};
    for (int val : arr) {
        std::cout << val << " ";
    }
    std::cout << '\n';
    
    // Vector
    std::vector<std::string> words = {"Hello", "World", "C++"};
    for (const std::string& word : words) {
        std::cout << word << " ";
    }
    std::cout << '\n';
    
    // Map
    std::map<std::string, int> ages = {
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 35}
    };
    
    for (const auto& [name, age] : ages) { // C++17 structured binding
        std::cout << name << " is " << age << " years old\n";
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Clean, readable syntax
- ✅ Works with any container
- ✅ Prevents off-by-one errors
- 📝 Use `const&` to avoid copies
- 💡 Use `auto` for type deduction

### Modifying Elements

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    
    // Read-only (copy)
    for (int num : numbers) {
        num *= 2; // Doesn't modify original
    }
    
    // Modify through reference
    for (int& num : numbers) {
        num *= 2; // Modifies original
    }
    
    // Print
    for (const int& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 5. Loop Control Statements

### Break Statement

```cpp
#include <iostream>

int main() {
    // Exit loop early
    for (int i = 0; i < 10; ++i) {
        if (i == 5) {
            break; // Exit loop when i is 5
        }
        std::cout << i << " ";
    }
    std::cout << '\n';
    
    // Find first occurrence
    std::vector<int> numbers = {1, 3, 5, 7, 9, 2, 4};
    for (int num : numbers) {
        if (num % 2 == 0) {
            std::cout << "First even number: " << num << '\n';
            break;
        }
    }
    
    return 0;
}
```

### Continue Statement

```cpp
#include <iostream>

int main() {
    // Skip specific iterations
    for (int i = 0; i < 10; ++i) {
        if (i % 2 == 0) {
            continue; // Skip even numbers
        }
        std::cout << i << " ";
    }
    std::cout << '\n';
    
    // Process valid items only
    std::vector<int> numbers = {1, -2, 3, -4, 5, -6};
    for (int num : numbers) {
        if (num < 0) {
            continue; // Skip negative numbers
        }
        std::cout << num << " ";
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 6. Nested Loops

### 2D Pattern Printing

```cpp
#include <iostream>

int main() {
    // Print a pattern
    for (int i = 1; i <= 5; ++i) {
        for (int j = 1; j <= i; ++j) {
            std::cout << "* ";
        }
        std::cout << '\n';
    }
    
    // Multiplication table
    for (int i = 1; i <= 5; ++i) {
        for (int j = 1; j <= 5; ++j) {
            std::cout << i * j << "\t";
        }
        std::cout << '\n';
    }
    
    return 0;
}
```

### 2D Array Iteration

```cpp
#include <iostream>

int main() {
    int matrix[3][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };
    
    // Iterate 2D array
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 4; ++j) {
            std::cout << matrix[i][j] << " ";
        }
        std::cout << '\n';
    }
    
    // Range-based for with 2D array
    for (const auto& row : matrix) {
        for (const auto& element : row) {
            std::cout << element << " ";
        }
        std::cout << '\n';
    }
    
    return 0;
}
```

---

## 7. Loop Performance Considerations

### Pre-increment vs Post-increment

```cpp
#include <iostream>
#include <chrono>

int main() {
    const int N = 100000000;
    
    // Post-increment (creates temporary)
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i++) {
        // Empty loop
    }
    auto post_time = std::chrono::high_resolution_clock::now() - start;
    
    // Pre-increment (no temporary)
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < N; ++i) {
        // Empty loop
    }
    auto pre_time = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Post-increment: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(post_time).count() 
              << "ms\n";
    std::cout << "Pre-increment: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(pre_time).count() 
              << "ms\n";
    
    return 0;
}
```

**Note:** For primitive types like `int`, modern compilers optimize both equally. For iterators and complex types, prefer pre-increment.

### Loop Invariant Code Motion

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers(1000);
    
    // ❌ BAD: Calls size() every iteration
    for (size_t i = 0; i < numbers.size(); ++i) {
        // Process numbers[i]
    }
    
    // ✅ GOOD: Cache size
    size_t n = numbers.size();
    for (size_t i = 0; i < n; ++i) {
        // Process numbers[i]
    }
    
    // ✅ BEST: Range-based for
    for (int& num : numbers) {
        // Process num
    }
    
    return 0;
}
```

---

## 8. Common Patterns

### Sum of Elements

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    
    int sum = 0;
    for (int num : numbers) {
        sum += num;
    }
    
    std::cout << "Sum: " << sum << '\n';
    
    return 0;
}
```

### Find Maximum

```cpp
#include <iostream>
#include <vector>
#include <limits>

int main() {
    std::vector<int> numbers = {3, 7, 2, 9, 1, 5};
    
    int max_val = std::numeric_limits<int>::min();
    for (int num : numbers) {
        if (num > max_val) {
            max_val = num;
        }
    }
    
    std::cout << "Maximum: " << max_val << '\n';
    
    return 0;
}
```

### Count Occurrences

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 2, 4, 2, 5};
    int target = 2;
    
    int count = 0;
    for (int num : numbers) {
        if (num == target) {
            ++count;
        }
    }
    
    std::cout << "Count of " << target << ": " << count << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Choose the Right Loop:**
   - `for`: Known iteration count
   - `while`: Unknown iteration count
   - `do-while`: Execute at least once
   - Range-based `for`: Iterating containers

2. **Avoid Infinite Loops:**
   - Always have a termination condition
   - Be careful with floating-point comparisons
   - Use break for safety exits

3. **Use Break and Continue Judiciously:**
   - Makes code harder to follow
   - Consider restructuring if used too much
   - Document why you're using them

4. **Prefer Range-Based For:**
   - Cleaner, safer code
   - No index errors
   - Works with any container

5. **Optimize Performance:**
   - Cache loop-invariant values
   - Use pre-increment for iterators
   - Consider loop unrolling for critical sections

---

## Common Pitfalls

```cpp
// ❌ Off-by-one error
for (int i = 0; i <= 5; ++i) { /* 6 iterations instead of 5 */ }

// ❌ Modifying loop counter inside loop
for (int i = 0; i < 10; ++i) {
    i++; // Don't do this!
}

// ❌ Floating-point equality
for (double d = 0.0; d != 1.0; d += 0.1) { /* May never terminate */ }

// ❌ Infinite loop
while (true) {
    // No break statement
}

// ❌ Unintended empty loop
for (int i = 0; i < 10; ++i); // Semicolon creates empty loop
    std::cout << i; // Only executes once
```

---

## Summary

C++ loops provide:
- **Flexible iteration** - Multiple loop types
- **Container iteration** - Range-based for
- **Performance** - Compiler optimizations
- **Control flow** - Break, continue

---

### References
- [C++ Loops](https://en.cppreference.com/w/cpp/language/for)
- [Range-based for](https://en.cppreference.com/w/cpp/language/range-for)
- [C++ Core Guidelines - Loops](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es71-prefer-a-range-for-statement-to-a-for-statement-when-there-is-a-choice)