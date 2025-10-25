# `nullptr` in Modern C++

`nullptr` is a keyword introduced in C++11 to represent a null pointer constant. It replaces the traditional `NULL` macro and provides a type-safe way to indicate null pointers. This README provides an overview of `nullptr`, its advantages, and examples.

---

## What is `nullptr`?

- `nullptr` is a keyword that represents a null pointer constant.
- It is of type `std::nullptr_t`, which can be implicitly converted to any pointer type.
- Unlike `NULL`, which is typically defined as `0`, `nullptr` is type-safe and avoids ambiguity.

---

## Key Features

1. **Type Safety**
   - `nullptr` eliminates ambiguity between null pointers and integers.

2. **Improved Readability**
   - Code using `nullptr` is more expressive and easier to understand.

3. **Compatibility**
   - `nullptr` can be used with all pointer types, including function pointers.

---

## Syntax

### Declaring a Null Pointer
```cpp
int* ptr = nullptr; // Null pointer
```

### Checking for Null
```cpp
if (ptr == nullptr) {
    std::cout << "Pointer is null." << std::endl;
}
```

### Using `std::nullptr_t`
```cpp
std::nullptr_t nullValue = nullptr;
```

---

## Examples

### Example 1: Basic Usage
```cpp
#include <iostream>

void printPointer(int* ptr) {
    if (ptr == nullptr) {
        std::cout << "Pointer is null." << std::endl;
    } else {
        std::cout << "Pointer value: " << *ptr << std::endl;
    }
}

int main() {
    int* ptr = nullptr;
    printPointer(ptr);

    int value = 42;
    ptr = &value;
    printPointer(ptr);

    return 0;
}
```

### Example 2: Function Overloading
```cpp
#include <iostream>

void process(int* ptr) {
    std::cout << "Processing pointer." << std::endl;
}

void process(int value) {
    std::cout << "Processing integer." << std::endl;
}

int main() {
    process(nullptr); // Calls the pointer version
    process(0);       // Calls the integer version

    return 0;
}
```

### Example 3: Compatibility with Function Pointers
```cpp
#include <iostream>

void myFunction() {
    std::cout << "Function called." << std::endl;
}

int main() {
    void (*funcPtr)() = nullptr;

    if (funcPtr == nullptr) {
        std::cout << "Function pointer is null." << std::endl;
    }

    funcPtr = myFunction;
    funcPtr();

    return 0;
}
```

---

## Best Practices

- Use `nullptr` instead of `NULL` or `0` for null pointers.
- Avoid using `nullptr` with non-pointer types to maintain clarity.
- Use `std::nullptr_t` for generic programming when working with null pointers.

---

## Common Pitfalls

1. **Ambiguity with `NULL`**
   - Avoid mixing `nullptr` and `NULL` in the same codebase to maintain consistency.

2. **Implicit Conversions**
   - Be cautious of implicit conversions when using `nullptr` with overloaded functions.

---

## Conclusion

`nullptr` is a type-safe and expressive way to represent null pointers in modern C++. By replacing `NULL` with `nullptr`, developers can write clearer and more maintainable code.