# Memory Management in C++

This folder contains resources and examples related to memory management in C++.

## Overview
Memory management in C++ involves the allocation, use, and deallocation of memory during the runtime of a program. Proper memory management ensures efficient use of resources and prevents issues like memory leaks and dangling pointers.

## Topics Covered

### 1. **Stack vs. Heap Memory**
- **Stack Memory:**
  - Allocated automatically when a function is called.
  - Memory is released when the function exits.
  - Faster allocation and deallocation.
  - Limited in size.
  - Example:
    ```cpp
    void example() {
        int x = 10; // Allocated on the stack
    }
    ```
- **Heap Memory:**
  - Allocated manually using `new`.
  - Memory must be explicitly released using `delete`.
  - Slower allocation but more flexible.
  - Example:
    ```cpp
    void example() {
        int* x = new int(10); // Allocated on the heap
        delete x; // Deallocated manually
    }
    ```

### 2. **Dynamic Memory Allocation**
- **Using `new` and `delete`:**
  - `new` allocates memory on the heap.
  - `delete` releases the allocated memory.
  - Example:
    ```cpp
    int* ptr = new int(5); // Allocate memory
    delete ptr; // Free memory
    ```
- **Arrays:**
  - Use `new[]` to allocate arrays and `delete[]` to deallocate them.
  - Example:
    ```cpp
    int* arr = new int[10]; // Allocate array
    delete[] arr; // Free array
    ```

### 3. **Smart Pointers**
- **`std::unique_ptr`:**
  - Exclusive ownership of a resource.
  - Automatically deletes the resource when it goes out of scope.
  - Example:
    ```cpp
    #include <memory>

    std::unique_ptr<int> ptr = std::make_unique<int>(10);
    ```
- **`std::shared_ptr`:**
  - Shared ownership of a resource.
  - Resource is deleted when the last `std::shared_ptr` goes out of scope.
  - Example:
    ```cpp
    #include <memory>

    std::shared_ptr<int> ptr1 = std::make_shared<int>(10);
    std::shared_ptr<int> ptr2 = ptr1; // Shared ownership
    ```
- **`std::weak_ptr`:**
  - Non-owning reference to a `std::shared_ptr`.
  - Prevents circular references.
  - Example:
    ```cpp
    #include <memory>

    std::shared_ptr<int> shared = std::make_shared<int>(10);
    std::weak_ptr<int> weak = shared; // Non-owning reference
    ```

### 4. **RAII (Resource Acquisition Is Initialization)**
- Ensures resource cleanup using constructors and destructors.
- Example:
  ```cpp
  class Resource {
  public:
      Resource() { std::cout << "Acquired" << std::endl; }
      ~Resource() { std::cout << "Released" << std::endl; }
  };

  int main() {
      Resource res; // Automatically cleaned up
      return 0;
  }
  ```

### 5. **Common Pitfalls**
- **Memory Leaks:**
  - Occur when allocated memory is not deallocated.
  - Example:
    ```cpp
    int* ptr = new int(10);
    // Memory leak: `delete ptr;` is missing
    ```
- **Dangling Pointers:**
  - Occur when a pointer points to memory that has been deallocated.
  - Example:
    ```cpp
    int* ptr = new int(10);
    delete ptr;
    // Dangling pointer: `ptr` still points to deallocated memory
    ```
- **Double Deletion:**
  - Occurs when `delete` is called twice on the same pointer.
  - Example:
    ```cpp
    int* ptr = new int(10);
    delete ptr;
    delete ptr; // Undefined behavior
    ```

## Best Practices
- Always pair `new` with `delete` to avoid memory leaks.
- Prefer smart pointers over raw pointers for automatic memory management.
- Use `std::make_unique` and `std::make_shared` for creating smart pointers.
- Avoid manual memory management when possible; use STL containers and smart pointers.
- Use tools like Valgrind to detect memory leaks and other memory-related issues.

By mastering these concepts, you can write efficient and safe C++ programs that manage memory effectively.