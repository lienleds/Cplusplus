# Uniform Initialization

Uniform initialization, introduced in C++11, provides a consistent syntax for initializing variables and objects. It uses curly braces `{}` to initialize all types of objects, including arrays, structs, and classes.

---

## Benefits of Uniform Initialization
- **Consistency:** A single syntax for all types of initialization.
- **Prevention of Narrowing Conversions:** Prevents implicit conversions that may lose data.
- **Improved Readability:** Makes initialization syntax more uniform and clear.

---

## Syntax

### Example: Basic Usage
```cpp
#include <iostream>
#include <vector>

int main() {
    int x{10}; // Uniform initialization
    double y{3.14};
    std::vector<int> vec{1, 2, 3, 4, 5};

    std::cout << "x: " << x << ", y: " << y << '\n';
    for (int val : vec) {
        std::cout << val << " ";
    }
    std::cout << '\n';

    return 0;
}
```

---

## Prevention of Narrowing Conversions
Uniform initialization prevents narrowing conversions, which can lead to data loss.

### Example: Narrowing Conversion Error
```cpp
int main() {
    int x = 3.14; // Allowed (narrowing conversion)
    // int y{3.14}; // Error: Narrowing conversion

    return 0;
}
```

---

## Uniform Initialization with Classes
Uniform initialization can be used with user-defined types.

### Example: Initializing a Class
```cpp
#include <iostream>

struct Point {
    int x;
    int y;
};

int main() {
    Point p{10, 20}; // Uniform initialization
    std::cout << "Point: (" << p.x << ", " << p.y << ")\n";

    return 0;
}
```

---

## Uniform Initialization with `std::initializer_list`
Uniform initialization works seamlessly with `std::initializer_list`.

### Example: Using `std::initializer_list`
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec{1, 2, 3, 4, 5};
    for (int val : vec) {
        std::cout << val << " ";
    }
    std::cout << '\n';

    return 0;
}
```

---

## Limitations
- **Ambiguity with `std::initializer_list`:** Uniform initialization may lead to ambiguity when a class has a constructor that takes `std::initializer_list`.
- **Not Always Preferred:** In some cases, traditional initialization syntax may be clearer.

### Example: Ambiguity
```cpp
#include <iostream>
#include <vector>

struct MyClass {
    MyClass(int x, int y) {
        std::cout << "Two-arg constructor\n";
    }

    MyClass(std::initializer_list<int> list) {
        std::cout << "Initializer list constructor\n";
    }
};

int main() {
    MyClass obj{10, 20}; // Calls initializer list constructor

    return 0;
}
```

---

## Best Practices
- Use uniform initialization for consistency and to avoid narrowing conversions.
- Be cautious of ambiguity with `std::initializer_list`.
- Prefer traditional initialization when it improves code clarity.

---

## Summary
- Uniform initialization provides a consistent syntax for initializing variables and objects.
- It prevents narrowing conversions and improves code readability.
- Be aware of its limitations, such as ambiguity with `std::initializer_list`.

---

### References
- [C++ Reference for Uniform Initialization](https://en.cppreference.com/w/cpp/language/list_initialization)