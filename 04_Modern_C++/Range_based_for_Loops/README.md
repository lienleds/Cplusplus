# Range-Based For Loops in Modern C++

Range-based for loops, introduced in C++11, provide a concise and readable way to iterate over elements in a container or range. This README provides an overview of range-based for loops, their syntax, and examples.

---

## What are Range-Based For Loops?

- Range-based for loops simplify iteration by eliminating the need for explicit iterators or index variables.
- They work with any container or range that provides a `begin()` and `end()` function.
- They can also be used with arrays and initializer lists.

---

## Syntax

### Basic Syntax
```cpp
for (declaration : range) {
    // Loop body
}
```

- **`declaration`**: Declares a variable to hold each element in the range.
- **`range`**: The container or range to iterate over.

---

## Examples

### Example 1: Iterating Over a Vector
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    for (int num : numbers) {
        std::cout << num << " ";
    }

    return 0;
}
```

### Example 2: Using References
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    for (int& num : numbers) {
        num *= 2; // Modify elements in place
    }

    for (int num : numbers) {
        std::cout << num << " ";
    }

    return 0;
}
```

### Example 3: Iterating Over an Array
```cpp
#include <iostream>

int main() {
    int arr[] = {10, 20, 30, 40, 50};

    for (int num : arr) {
        std::cout << num << " ";
    }

    return 0;
}
```

### Example 4: Using `auto`
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<std::pair<int, std::string>> data = {
        {1, "one"}, {2, "two"}, {3, "three"}
    };

    for (const auto& [key, value] : data) {
        std::cout << key << ": " << value << "\n";
    }

    return 0;
}
```

---

## Best Practices

- Use `const` references (`const auto&`) when elements do not need to be modified.
- Use references (`auto&`) to modify elements in place.
- Avoid copying elements unnecessarily to improve performance.

---

## Common Pitfalls

1. **Unintended Copies**
   - Declaring the loop variable by value can result in unnecessary copies.

2. **Modifying Elements**
   - Use references to modify elements in the container.

3. **Iterating Over Temporary Containers**
   - Be cautious when iterating over temporary containers, as they may be destroyed after the loop.

---

## Conclusion

Range-based for loops are a powerful feature in modern C++ that simplify iteration and improve code readability. By understanding their syntax and best practices, developers can write cleaner and more efficient code.