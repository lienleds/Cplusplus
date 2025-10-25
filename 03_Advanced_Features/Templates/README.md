# Templates

This folder contains resources and examples related to templates in C++.

# Templates in C++

## What are Templates?

Templates in C++ are a powerful feature that allows you to write generic and reusable code. They enable functions and classes to operate with different data types without rewriting the code for each type.

## Why Use Templates?

- **Code Reusability:** Write a single implementation for multiple data types.
- **Type Safety:** Ensures type correctness at compile time.
- **Flexibility:** Allows the creation of generic algorithms and data structures.

## Types of Templates

### 1. **Function Templates**
- **Description:** A blueprint for creating functions that work with any data type.
- **Example:**

```cpp
#include <iostream>

template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    std::cout << "Int: " << add(3, 4) << std::endl;
    std::cout << "Double: " << add(3.5, 4.5) << std::endl;
    return 0;
}
```

### Output
```
Int: 7
Double: 8
```

### 2. **Class Templates**
- **Description:** A blueprint for creating classes that work with any data type.
- **Example:**

```cpp
#include <iostream>

template <typename T>
class Box {
public:
    Box(T value) : value(value) {}
    void display() {
        std::cout << "Value: " << value << std::endl;
    }

private:
    T value;
};

int main() {
    Box<int> intBox(42);
    Box<std::string> strBox("Hello");

    intBox.display();
    strBox.display();

    return 0;
}
```

### Output
```
Value: 42
Value: Hello
```

### 3. **Template Specialization**
- **Description:** Allows you to define a custom implementation for a specific data type.
- **Example:**

```cpp
#include <iostream>

template <typename T>
class Printer {
public:
    void print(T value) {
        std::cout << "Generic: " << value << std::endl;
    }
};

// Specialization for int
template <>
class Printer<int> {
public:
    void print(int value) {
        std::cout << "Integer: " << value << std::endl;
    }
};

int main() {
    Printer<std::string> strPrinter;
    Printer<int> intPrinter;

    strPrinter.print("Hello");
    intPrinter.print(42);

    return 0;
}
```

### Output
```
Generic: Hello
Integer: 42
```

## Best Practices

- Use templates to write generic and reusable code.
- Avoid overusing template specialization, as it can make the code harder to maintain.
- Prefer `typename` over `class` for template parameters (both are functionally equivalent).
- Use concepts (C++20) to constrain template parameters for better readability and error messages.

## Additional Resources

- [C++ Reference: Templates](https://en.cppreference.com/w/cpp/language/template)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
