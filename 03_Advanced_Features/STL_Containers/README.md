# STL Containers in C++

## Overview

The Standard Template Library (STL) in C++ provides a variety of containers to store and manage collections of data. These containers are generic and can store any data type.

## Types of STL Containers

1. **Sequence Containers:**
   - Examples: `std::vector`, `std::deque`, `std::list`
2. **Associative Containers:**
   - Examples: `std::set`, `std::map`, `std::multiset`, `std::multimap`
3. **Unordered Containers:**
   - Examples: `std::unordered_set`, `std::unordered_map`
4. **Container Adapters:**
   - Examples: `std::stack`, `std::queue`, `std::priority_queue`

## Example: Using `std::vector`

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    for (int num : vec) {
        std::cout << num << " ";
    }

    return 0;
}
```

### Output
```
1 2 3 4 5
```

## Best Practices

- Choose the appropriate container based on the use case.
- Use `std::vector` for dynamic arrays and `std::map` for key-value pairs.
- Prefer STL containers over raw arrays for better safety and functionality.

## Additional Resources

- [C++ Reference: STL Containers](https://en.cppreference.com/w/cpp/container)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)