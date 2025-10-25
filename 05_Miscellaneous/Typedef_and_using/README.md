# `typedef` and `using` in C++

In C++, `typedef` and `using` are used to create type aliases, making code more readable and easier to maintain. While `typedef` has been part of C++ since its inception, `using` was introduced in C++11 and offers more flexibility.

---

## `typedef` Syntax
The `typedef` keyword creates a new name (alias) for an existing type.

### Example: Basic `typedef`
```cpp
#include <iostream>

typedef unsigned int uint;

typedef struct {
    int x;
    int y;
} Point;

int main() {
    uint age = 25;
    Point p = {10, 20};

    std::cout << "Age: " << age << std::endl;
    std::cout << "Point: (" << p.x << ", " << p.y << ")" << std::endl;

    return 0;
}
```

---

## `using` Syntax
The `using` keyword provides a modern and more versatile way to create type aliases.

### Example: Basic `using`
```cpp
#include <iostream>

using uint = unsigned int;

using Point = struct {
    int x;
    int y;
};

int main() {
    uint age = 30;
    Point p = {15, 25};

    std::cout << "Age: " << age << std::endl;
    std::cout << "Point: (" << p.x << ", " << p.y << ")" << std::endl;

    return 0;
}
```

---

## `using` for Templates
`using` is particularly useful for creating type aliases for templates, which can be cumbersome with `typedef`.

### Example: Template Aliases with `using`
```cpp
#include <vector>
#include <iostream>

using IntVector = std::vector<int>;

int main() {
    IntVector numbers = {1, 2, 3, 4, 5};

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

---

## Comparison: `typedef` vs `using`

| Feature                | `typedef`                  | `using`                   |
|------------------------|----------------------------|---------------------------|
| Readability            | Less intuitive             | More intuitive            |
| Template Support       | Limited                   | Full support              |
| Modern C++ Compatibility | Legacy                   | Recommended for C++11+    |

---

## Best Practices
- Prefer `using` over `typedef` in modern C++ code for better readability and template support.
- Use type aliases to simplify complex type declarations.
- Avoid overusing type aliases, as they can obscure the underlying types.

---

## Summary
`typedef` and `using` are powerful tools for creating type aliases in C++. While `typedef` is still valid, `using` is the preferred choice in modern C++ due to its improved readability and support for templates.

---

### References
- [C++ Reference for `typedef`](https://en.cppreference.com/w/cpp/language/typedef)
- [C++ Reference for `using`](https://en.cppreference.com/w/cpp/language/type_alias)