# STL Iterators in C++

## Overview

Iterators in the Standard Template Library (STL) are objects that allow traversal of elements in a container. They provide a generic way to access and manipulate elements.

## Types of Iterators

1. **Input Iterators:**
   - Read-only access to elements.
2. **Output Iterators:**
   - Write-only access to elements.
3. **Forward Iterators:**
   - Read and write access; can move forward.
4. **Bidirectional Iterators:**
   - Can move both forward and backward.
5. **Random Access Iterators:**
   - Can move to any element in constant time.

## Example: Using Iterators with `std::vector`

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";
    }

    return 0;
}
```

### Output
```
1 2 3 4 5
```

## Best Practices

- Use range-based for loops for simpler iteration when possible.
- Prefer STL algorithms over manual iteration for common operations.
- Understand the iterator category of a container to use it effectively.

## Additional Resources

- [C++ Reference: STL Iterators](https://en.cppreference.com/w/cpp/iterator)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)