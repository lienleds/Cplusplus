# RAII (Resource Acquisition Is Initialization) in C++

## What is RAII?

RAII (Resource Acquisition Is Initialization) is a programming idiom in C++ that ties the lifecycle of a resource (e.g., memory, file handles, sockets) to the lifetime of an object. When the object is created, it acquires the resource, and when the object is destroyed, it releases the resource.

## Why Use RAII?

- **Automatic Resource Management:** Ensures resources are properly released when they go out of scope.
- **Exception Safety:** Prevents resource leaks even in the presence of exceptions.
- **Simplified Code:** Reduces the need for manual resource management.

## Key Concepts

1. **Constructor:** Acquires the resource.
2. **Destructor:** Releases the resource.
3. **Scope:** The resource is tied to the object's scope.

## Example: Managing a File Resource

```cpp
#include <iostream>
#include <fstream>
#include <string>

class FileHandler {
public:
    FileHandler(const std::string& filename) {
        file.open(filename);
        if (!file.is_open()) {
            throw std::runtime_error("Failed to open file");
        }
    }

    ~FileHandler() {
        if (file.is_open()) {
            file.close();
        }
    }

    void write(const std::string& data) {
        file << data;
    }

private:
    std::ofstream file;
};

int main() {
    try {
        FileHandler fh("example.txt");
        fh.write("Hello, RAII!");
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
    }

    return 0;
}
```

### Output
- Creates a file `example.txt` and writes "Hello, RAII!" to it.

## RAII and Smart Pointers

Smart pointers like `std::unique_ptr` and `std::shared_ptr` are examples of RAII in action. They manage dynamic memory and ensure it is properly released when the pointer goes out of scope.

### Example: Using `std::unique_ptr`

```cpp
#include <iostream>
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Resource acquired" << std::endl; }
    ~Resource() { std::cout << "Resource released" << std::endl; }
};

int main() {
    {
        std::unique_ptr<Resource> res = std::make_unique<Resource>();
    } // Resource is automatically released here

    return 0;
}
```

### Output
```
Resource acquired
Resource released
```

## Best Practices

- Use RAII for all resource management tasks, such as memory, file handles, and locks.
- Prefer smart pointers over raw pointers for dynamic memory management.
- Ensure destructors are noexcept to avoid undefined behavior during stack unwinding.

## Additional Resources

- [C++ Reference: RAII](https://en.cppreference.com/w/cpp/language/raii)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
