# Smart Pointers

This folder contains resources and examples related to smart pointers in C++.

# Smart Pointers in C++

## What are Smart Pointers?

Smart pointers in C++ are objects that manage the lifetime of dynamically allocated memory. They automatically release the memory when it is no longer needed, preventing memory leaks and dangling pointers.

## Why Use Smart Pointers?

- **Automatic Memory Management:** Automatically deallocates memory when the smart pointer goes out of scope.
- **Exception Safety:** Ensures proper cleanup even in the presence of exceptions.
- **Improved Code Readability:** Makes ownership and lifetime of resources explicit.

## Types of Smart Pointers

### 1. **`std::unique_ptr`**
- **Description:** A smart pointer that owns and manages a resource exclusively. Only one `std::unique_ptr` can own a resource at a time.
- **Use Case:** Use when you want sole ownership of a resource.
- **Example:**

```cpp
#include <iostream>
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Resource acquired" << std::endl; }
    ~Resource() { std::cout << "Resource released" << std::endl; }
};

int main() {
    std::unique_ptr<Resource> res = std::make_unique<Resource>();
    return 0;
}
```

### 2. **`std::shared_ptr`**
- **Description:** A smart pointer that allows multiple `std::shared_ptr` instances to share ownership of the same resource. The resource is released when the last `std::shared_ptr` is destroyed.
- **Use Case:** Use when multiple parts of the program need shared ownership of a resource.
- **Example:**

```cpp
#include <iostream>
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Resource acquired" << std::endl; }
    ~Resource() { std::cout << "Resource released" << std::endl; }
};

int main() {
    std::shared_ptr<Resource> res1 = std::make_shared<Resource>();
    {
        std::shared_ptr<Resource> res2 = res1; // Shared ownership
    } // res2 goes out of scope, but resource is not released
    return 0; // res1 goes out of scope, resource is released
}
```

### 3. **`std::weak_ptr`**
- **Description:** A non-owning smart pointer that works with `std::shared_ptr`. It does not affect the reference count of the shared resource.
- **Use Case:** Use to break circular references in `std::shared_ptr`.
- **Example:**

```cpp
#include <iostream>
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Resource acquired" << std::endl; }
    ~Resource() { std::cout << "Resource released" << std::endl; }
};

int main() {
    std::shared_ptr<Resource> res = std::make_shared<Resource>();
    std::weak_ptr<Resource> weakRes = res; // Non-owning reference

    if (auto sharedRes = weakRes.lock()) {
        std::cout << "Resource is still alive" << std::endl;
    }

    return 0;
}
```

## Best Practices

- Prefer `std::unique_ptr` for sole ownership to avoid unnecessary overhead.
- Use `std::shared_ptr` only when shared ownership is required.
- Use `std::weak_ptr` to break circular references in `std::shared_ptr`.
- Avoid raw pointers for dynamic memory management.

## Additional Resources

- [C++ Reference: Smart Pointers](https://en.cppreference.com/w/cpp/memory)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
