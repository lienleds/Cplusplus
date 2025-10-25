# STL Algorithms in C++

## Overview

The Standard Template Library (STL) in C++ provides a rich set of algorithms for performing operations on data structures. These algorithms are generic and work seamlessly with STL containers and iterators.

## Categories of STL Algorithms

1. **Non-Modifying Algorithms:**
   - Examples: `std::find`, `std::count`, `std::equal`
2. **Modifying Algorithms:**
   - Examples: `std::copy`, `std::transform`, `std::replace`
3. **Sorting Algorithms:**
   - Examples: `std::sort`, `std::stable_sort`, `std::partial_sort`
4. **Numeric Algorithms:**
   - Examples: `std::accumulate`, `std::inner_product`
5. **Set Operations:**
   - Examples: `std::set_union`, `std::set_intersection`

## Example: Using `std::sort`

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {5, 2, 9, 1, 5, 6};

    std::sort(vec.begin(), vec.end());

    for (int num : vec) {
        std::cout << num << " ";
    }

    return 0;
}
```

### Output
```
1 2 5 5 6 9
```

## Best Practices

- Use STL algorithms to simplify code and improve readability.
- Prefer algorithms over manual loops for common operations.
- Ensure compatibility between algorithms and container types.

## Additional Resources

- [C++ Reference: STL Algorithms](https://en.cppreference.com/w/cpp/algorithm)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)