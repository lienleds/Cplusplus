# Namespaces in C++

## What are Namespaces?

Namespaces in C++ are a way to group related classes, functions, and variables under a single name to avoid naming conflicts. They provide a scope for identifiers and help organize code in large projects.

## Why Use Namespaces?

- **Avoid Naming Conflicts:** Prevents clashes between identifiers in different libraries or parts of a program.
- **Code Organization:** Groups related functionality together, making the code easier to understand and maintain.
- **Modularity:** Enables modular design by separating different parts of a program into distinct namespaces.

## Syntax

```cpp
namespace NamespaceName {
    // Declarations and definitions
}
```

### Example

```cpp
#include <iostream>

namespace Math {
    const double PI = 3.14159;

    double add(double a, double b) {
        return a + b;
    }
}

int main() {
    std::cout << "PI: " << Math::PI << std::endl;
    std::cout << "Sum: " << Math::add(5, 3) << std::endl;
    return 0;
}
```

### Output
```
PI: 3.14159
Sum: 8
```

## Nested Namespaces

C++17 introduced nested namespace definitions for better readability.

### Example

```cpp
namespace A::B::C {
    void display() {
        std::cout << "Inside namespace A::B::C" << std::endl;
    }
}

int main() {
    A::B::C::display();
    return 0;
}
```

## Using Directive

The `using` directive allows you to use identifiers from a namespace without qualifying them.

### Example

```cpp
#include <iostream>

namespace Math {
    double multiply(double a, double b) {
        return a * b;
    }
}

using namespace Math;

int main() {
    std::cout << "Product: " << multiply(4, 5) << std::endl;
    return 0;
}
```

### Output
```
Product: 20
```

## Best Practices

- Avoid using the `using` directive in header files to prevent polluting the global namespace.
- Use namespaces to group related functionality and avoid naming conflicts.
- Prefer nested namespaces for better organization in large projects.

## Additional Resources

- [C++ Reference: Namespaces](https://en.cppreference.com/w/cpp/language/namespace)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
