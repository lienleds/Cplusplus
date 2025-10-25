# Move Semantics in Modern C++

Move semantics, introduced in C++11, optimize the performance of programs by enabling the transfer of resources from one object to another without copying. This README provides an overview of move semantics, their syntax, and examples.

---

## What are Move Semantics?

- Move semantics allow the resources of a temporary object to be "moved" rather than copied.
- This is particularly useful for objects that manage dynamic memory or other resources.
- Moves are enabled by the introduction of rvalue references.

---

## Key Concepts

### Lvalues and Rvalues
- **Lvalue**: An object that persists beyond a single expression.
- **Rvalue**: A temporary object that is discarded after the expression.

### Rvalue References
- Declared using `&&`.
- Bind to rvalues, enabling move semantics.

### Move Constructor and Move Assignment Operator
- Special member functions that transfer resources from one object to another.

---

## Syntax

### Move Constructor
```cpp
class MyClass {
public:
    MyClass(int* data) : data_(data) {}

    // Move constructor
    MyClass(MyClass&& other) noexcept : data_(other.data_) {
        other.data_ = nullptr; // Nullify the source
    }

    ~MyClass() {
        delete data_;
    }

private:
    int* data_;
};
```

### Move Assignment Operator
```cpp
class MyClass {
public:
    MyClass& operator=(MyClass&& other) noexcept {
        if (this != &other) {
            delete data_; // Free existing resource
            data_ = other.data_; // Transfer ownership
            other.data_ = nullptr; // Nullify the source
        }
        return *this;
    }

private:
    int* data_;
};
```

---

## Examples

### Example 1: Move Constructor
```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
    MyClass(std::vector<int>&& vec) : data_(std::move(vec)) {}

    void print() {
        for (int val : data_) {
            std::cout << val << " ";
        }
        std::cout << "\n";
    }

private:
    std::vector<int> data_;
};

int main() {
    MyClass obj(std::vector<int>{1, 2, 3, 4});
    obj.print();
}
```

### Example 2: Move Assignment Operator
```cpp
#include <iostream>
#include <string>

class MyClass {
public:
    MyClass(std::string str) : data_(std::move(str)) {}

    MyClass& operator=(MyClass&& other) noexcept {
        if (this != &other) {
            data_ = std::move(other.data_);
        }
        return *this;
    }

    void print() {
        std::cout << data_ << "\n";
    }

private:
    std::string data_;
};

int main() {
    MyClass obj1("Hello");
    MyClass obj2("World");

    obj2 = std::move(obj1);
    obj2.print();
}
```

---

## Best Practices

- Use `std::move` to enable move semantics explicitly.
- Implement both move constructor and move assignment operator for classes managing resources.
- Avoid using moved-from objects unless they are reinitialized.

---

## Common Pitfalls

1. **Dangling Pointers**
   - Ensure that moved-from objects are left in a valid state.

2. **Unnecessary Copies**
   - Use `std::move` to avoid unintended copies.

3. **Resource Management**
   - Properly manage dynamic resources to prevent memory leaks.

---

## Conclusion

Move semantics are a powerful feature in modern C++ that improve performance by eliminating unnecessary copies. By understanding their syntax and use cases, developers can write more efficient and maintainable code.