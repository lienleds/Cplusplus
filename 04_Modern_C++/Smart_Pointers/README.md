# Smart Pointers in Modern C++

Smart pointers, introduced in C++11, are a safer and more convenient alternative to raw pointers. They automatically manage the lifetime of dynamically allocated objects, reducing the risk of memory leaks and dangling pointers. This README provides an overview of smart pointers, their types, and examples.

---

## What are Smart Pointers?

- Smart pointers are wrapper classes around raw pointers that manage the memory of the objects they point to.
- They automatically release the memory when the smart pointer goes out of scope.
- Smart pointers are part of the `<memory>` header.

---

## Types of Smart Pointers

### 1. `std::unique_ptr`
- Represents sole ownership of a dynamically allocated object.
- Cannot be copied, but can be moved.

### 2. `std::shared_ptr`
- Represents shared ownership of a dynamically allocated object.
- The object is destroyed when the last `std::shared_ptr` owning it is destroyed.

### 3. `std::weak_ptr`
- A non-owning reference to an object managed by `std::shared_ptr`.
- Prevents circular references in `std::shared_ptr`.

---

## Syntax and Examples

### Example 1: `std::unique_ptr`
```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::unique_ptr<MyClass> ptr = std::make_unique<MyClass>();
    // No need to delete the object; it will be destroyed automatically.

    return 0;
}
```

### Example 2: `std::shared_ptr`
```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>();
    {
        std::shared_ptr<MyClass> ptr2 = ptr1; // Shared ownership
        std::cout << "Shared count: " << ptr1.use_count() << "\n";
    }
    // ptr2 goes out of scope, but the object is not destroyed.

    std::cout << "Shared count: " << ptr1.use_count() << "\n";
    // Object is destroyed when the last shared_ptr (ptr1) is destroyed.

    return 0;
}
```

### Example 3: `std::weak_ptr`
```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() { std::cout << "MyClass created\n"; }
    ~MyClass() { std::cout << "MyClass destroyed\n"; }
};

int main() {
    std::shared_ptr<MyClass> shared = std::make_shared<MyClass>();
    std::weak_ptr<MyClass> weak = shared; // Non-owning reference

    if (auto ptr = weak.lock()) { // Check if the object is still valid
        std::cout << "Object is still alive\n";
    }

    shared.reset(); // Destroy the object

    if (weak.expired()) {
        std::cout << "Object has been destroyed\n";
    }

    return 0;
}
```

---

## Best Practices

- Use `std::unique_ptr` for sole ownership and `std::shared_ptr` for shared ownership.
- Avoid using `std::shared_ptr` unless shared ownership is necessary.
- Use `std::weak_ptr` to break circular references in `std::shared_ptr`.
- Prefer `std::make_unique` and `std::make_shared` for creating smart pointers.

---

## Common Pitfalls

1. **Circular References**
   - Avoid circular references with `std::shared_ptr` by using `std::weak_ptr`.

2. **Unnecessary Overhead**
   - Use `std::unique_ptr` instead of `std::shared_ptr` when shared ownership is not required.

3. **Dangling References**
   - Ensure that `std::weak_ptr` is checked before accessing the object.

---

## Conclusion

Smart pointers are an essential feature of modern C++ that simplify memory management and improve code safety. By understanding their types and use cases, developers can write more robust and maintainable programs.