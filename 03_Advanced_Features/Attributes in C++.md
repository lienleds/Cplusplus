# Attributes in C++

Attributes provide a standardized way to add compiler-specific hints and metadata to code.

---

## Overview

C++ attributes (C++11 onwards) allow you to:
- **Provide hints to the compiler**
- **Suppress warnings**
- **Mark deprecated features**
- **Optimize code generation**

---

## 1. [[nodiscard]] (C++17)

### Basic Usage

```cpp
#include <iostream>

[[nodiscard]] int calculate() {
    return 42;
}

[[nodiscard]] bool is_valid() {
    return true;
}

int main() {
    // Warning: ignoring return value
    calculate(); 
    
    // Correct usage
    int result = calculate();
    std::cout << "Result: " << result << '\n';
    
    if (is_valid()) {
        std::cout << "Valid!\n";
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Prevents accidentally ignoring important return values
- ✅ Compile-time check
- ⚠️ Generates warning (not error) if ignored
- 💡 Great for error codes and resource handles

### With Reason (C++20)

```cpp
#include <iostream>
#include <memory>

[[nodiscard("Resource leak if not used")]]
std::unique_ptr<int> create_resource() {
    return std::make_unique<int>(42);
}

int main() {
    // Warning with custom message
    create_resource();
    
    // Correct
    auto resource = create_resource();
    
    return 0;
}
```

---

## 2. [[deprecated]] (C++14)

### Mark Deprecated Functions

```cpp
#include <iostream>

[[deprecated]]
void old_function() {
    std::cout << "This is deprecated\n";
}

[[deprecated("Use new_function() instead")]]
void another_old_function() {
    std::cout << "This is also deprecated\n";
}

void new_function() {
    std::cout << "This is the new way\n";
}

int main() {
    // Warning: function is deprecated
    old_function();
    
    // Warning with custom message
    another_old_function();
    
    // No warning
    new_function();
    
    return 0;
}
```

### Deprecated Types

```cpp
#include <iostream>

[[deprecated("Use NewClass instead")]]
class OldClass {
public:
    void do_something() {
        std::cout << "Old implementation\n";
    }
};

class NewClass {
public:
    void do_something() {
        std::cout << "New implementation\n";
    }
};

int main() {
    // Warning: OldClass is deprecated
    OldClass old_obj;
    old_obj.do_something();
    
    // No warning
    NewClass new_obj;
    new_obj.do_something();
    
    return 0;
}
```

---

## 3. [[fallthrough]] (C++17)

### Intentional Switch Fallthrough

```cpp
#include <iostream>

void process_value(int value) {
    switch (value) {
        case 1:
            std::cout << "Processing 1\n";
            [[fallthrough]]; // Intentional fallthrough
            
        case 2:
            std::cout << "Processing 1 or 2\n";
            break;
            
        case 3:
            std::cout << "Processing 3\n";
            // Warning: fallthrough without [[fallthrough]]
            
        case 4:
            std::cout << "Processing 3 or 4\n";
            break;
            
        default:
            std::cout << "Default\n";
    }
}

int main() {
    process_value(1);
    process_value(3);
    
    return 0;
}
```

**Analysis:**
- ✅ Documents intentional fallthrough
- ✅ Suppresses warnings
- 📝 Makes code intent clear

---

## 4. [[maybe_unused]] (C++17)

### Suppress Unused Warnings

```cpp
#include <iostream>

void function([[maybe_unused]] int param1, 
              [[maybe_unused]] int param2) {
    #ifdef DEBUG
        std::cout << "Debug: " << param1 << ", " << param2 << '\n';
    #endif
    // param1 and param2 only used in debug builds
}

int main() {
    [[maybe_unused]] int debug_value = 42;
    
    #ifdef DEBUG
        std::cout << "Debug value: " << debug_value << '\n';
    #endif
    
    function(1, 2);
    
    return 0;
}
```

### Lambda Captures

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;
    
    auto lambda = [x, [[maybe_unused]] y]() {
        std::cout << "X: " << x << '\n';
        // y is captured but not used (no warning)
    };
    
    lambda();
    
    return 0;
}
```

---

## 5. [[likely]] and [[unlikely]] (C++20)

### Branch Prediction Hints

```cpp
#include <iostream>

int process(int value) {
    if (value > 0) [[likely]] {
        // This branch is likely to be taken
        return value * 2;
    } else [[unlikely]] {
        // This branch is unlikely
        return -1;
    }
}

void validate(int value) {
    if (value < 0) [[unlikely]] {
        throw std::runtime_error("Negative value");
    }
    
    // Normal processing (likely path)
    std::cout << "Value: " << value << '\n';
}

int main() {
    std::cout << process(5) << '\n';
    std::cout << process(-3) << '\n';
    
    validate(10);
    
    return 0;
}
```

**Analysis:**
- ✅ Helps compiler optimize branch prediction
- ✅ Can improve performance in hot paths
- ⚠️ Use only when profiling shows benefit
- 💡 Misuse can hurt performance

---

## 6. [[noreturn]] (C++11)

### Functions That Never Return

```cpp
#include <iostream>
#include <cstdlib>

[[noreturn]] void fatal_error(const char* message) {
    std::cerr << "Fatal error: " << message << '\n';
    std::exit(1);
    // Never returns
}

[[noreturn]] void infinite_loop() {
    while (true) {
        // Do something forever
    }
}

int main() {
    int value = -1;
    
    if (value < 0) {
        fatal_error("Negative value not allowed");
        // Compiler knows execution never reaches here
    }
    
    std::cout << "Value: " << value << '\n';
    
    return 0;
}
```

---

## 7. [[carries_dependency]] (C++11)

### Memory Ordering Optimization

```cpp
#include <atomic>

struct Node {
    int data;
    Node* next;
};

std::atomic<Node*> head;

// Preserve dependency chain for memory ordering
[[carries_dependency]] Node* get_head() {
    return head.load(std::memory_order_consume);
}

void process() {
    Node* node = get_head();
    if (node) {
        // Dependency preserved
        int value = node->data;
    }
}
```

**Note:** This is advanced and rarely used. Most developers don't need it.

---

## 8. Combining Attributes

### Multiple Attributes

```cpp
#include <iostream>

[[nodiscard, deprecated("Use get_value_v2()")]]
int get_value() {
    return 42;
}

[[nodiscard("Memory leak if not freed")]]
[[deprecated("Use smart pointers instead")]]
int* allocate_memory() {
    return new int(42);
}

int main() {
    // Multiple warnings
    get_value();
    allocate_memory();
    
    return 0;
}
```

---

## 9. Compiler-Specific Attributes

### GCC/Clang

```cpp
// GNU-specific attributes (not standard)
__attribute__((always_inline))
inline int fast_function() {
    return 42;
}

__attribute__((noinline))
int no_inline_function() {
    return 42;
}
```

### MSVC

```cpp
// MSVC-specific
__declspec(noinline)
int msvc_function() {
    return 42;
}
```

**Note:** Prefer standard attributes when available!

---

## 10. Practical Examples

### Error Handling

```cpp
#include <iostream>
#include <optional>

[[nodiscard("Error code must be checked")]]
int divide(int a, int b, int& result) {
    if (b == 0) {
        return -1; // Error code
    }
    result = a / b;
    return 0; // Success
}

[[nodiscard]]
std::optional<int> safe_divide(int a, int b) {
    if (b == 0) {
        return std::nullopt;
    }
    return a / b;
}

int main() {
    int result;
    
    // Warning if ignored
    if (divide(10, 2, result) == 0) {
        std::cout << "Result: " << result << '\n';
    }
    
    // Warning if ignored
    if (auto opt = safe_divide(10, 0)) {
        std::cout << "Result: " << *opt << '\n';
    }
    
    return 0;
}
```

### Performance Hints

```cpp
#include <iostream>
#include <vector>

int search(const std::vector<int>& vec, int target) {
    for (size_t i = 0; i < vec.size(); ++i) {
        if (vec[i] == target) [[likely]] {
            // Found (common case)
            return i;
        }
    }
    
    // Not found (rare case)
    return -1;
}

int main() {
    std::vector<int> data = {1, 2, 3, 4, 5};
    
    int index = search(data, 3);
    if (index != -1) {
        std::cout << "Found at index: " << index << '\n';
    }
    
    return 0;
}
```

---

## Summary of Standard Attributes

| Attribute | Since | Purpose |
|-----------|-------|---------|
| `[[noreturn]]` | C++11 | Function never returns |
| `[[carries_dependency]]` | C++11 | Memory ordering optimization |
| `[[deprecated]]` | C++14 | Mark as deprecated |
| `[[deprecated("msg")]]` | C++14 | Deprecated with reason |
| `[[fallthrough]]` | C++17 | Intentional switch fallthrough |
| `[[nodiscard]]` | C++17 | Return value should not be ignored |
| `[[maybe_unused]]` | C++17 | Suppress unused warnings |
| `[[nodiscard("msg")]]` | C++20 | nodiscard with reason |
| `[[likely]]` | C++20 | Branch likely taken |
| `[[unlikely]]` | C++20 | Branch unlikely taken |
| `[[no_unique_address]]` | C++20 | Optimize empty member size |

---

## Best Practices

1. **Use Standard Attributes:**
   - Prefer standard over compiler-specific
   - Portable across compilers

2. **[[nodiscard]] for Important Returns:**
   - Error codes
   - Resource handles
   - Non-trivial computations

3. **[[deprecated]] for API Evolution:**
   - Provide migration path
   - Include helpful message

4. **[[likely]]/[[unlikely]] with Care:**
   - Profile first
   - Only for hot paths
   - Can hurt if wrong

5. **Document Intent:**
   - Attributes make code self-documenting
   - Use meaningful messages

---

## Summary

Attributes provide:
- **Compiler hints** without changing behavior
- **Better warnings** and error checking
- **Performance optimization** opportunities
- **API evolution** support

---

### References
- [C++ Attributes](https://en.cppreference.com/w/cpp/language/attributes)
- [[[nodiscard]]](https://en.cppreference.com/w/cpp/language/attributes/nodiscard)
- [[[deprecated]]](https://en.cppreference.com/w/cpp/language/attributes/deprecated)
- [[[likely]] and [[unlikely]]](https://en.cppreference.com/w/cpp/language/attributes/likely)
