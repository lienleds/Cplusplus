# Smart Pointers in C++

Smart pointers are a feature of modern C++ that provide automatic memory management. They help prevent memory leaks and dangling pointers by ensuring that dynamically allocated memory is properly deallocated when it is no longer needed.

## Types of Smart Pointers

### 1. `std::unique_ptr`
- Provides exclusive ownership of a resource.
- Automatically deletes the resource when the `std::unique_ptr` goes out of scope.
- Cannot be copied, but can be moved.
- Example:
  ```cpp
  #include <iostream>
  #include <memory>

  int main() {
      std::unique_ptr<int> ptr = std::make_unique<int>(10);
      std::cout << *ptr << std::endl; // Outputs: 10
      return 0;
  }
  ```

### 2. `std::shared_ptr`
- Provides shared ownership of a resource.
- The resource is deleted when the last `std::shared_ptr` owning it goes out of scope.
- Example:
  ```cpp
  #include <iostream>
  #include <memory>

  int main() {
      std::shared_ptr<int> ptr1 = std::make_shared<int>(20);
      std::shared_ptr<int> ptr2 = ptr1; // Shared ownership
      std::cout << *ptr1 << std::endl; // Outputs: 20
      return 0;
  }
  ```

### 3. `std::weak_ptr`
- Provides a non-owning reference to a `std::shared_ptr`.
- Used to break circular references in `std::shared_ptr`.
- Example:
  ```cpp
  #include <iostream>
  #include <memory>

  int main() {
      std::shared_ptr<int> shared = std::make_shared<int>(30);
      std::weak_ptr<int> weak = shared; // Non-owning reference

      if (auto ptr = weak.lock()) { // Check if the resource is still available
          std::cout << *ptr << std::endl; // Outputs: 30
      }
      return 0;
  }
  ```

## Comparison of `std::unique_ptr`, `std::shared_ptr`, and `std::weak_ptr`

| Feature                | `std::unique_ptr`          | `std::shared_ptr`          | `std::weak_ptr`            |
|------------------------|----------------------------|----------------------------|----------------------------|
| **Ownership**          | Exclusive ownership        | Shared ownership           | Non-owning reference       |
| **Copyable**           | No                        | Yes                       | No                        |
| **Movable**            | Yes                       | Yes                       | No                        |
| **Reference Count**    | Not applicable            | Yes                       | Depends on `std::shared_ptr` |
| **Use Case**           | Single owner of a resource | Multiple owners of a resource | Break circular references |
| **Automatic Cleanup**  | Yes                       | Yes                       | No (depends on `std::shared_ptr`) |

### Schema for Understanding Smart Pointers

Below is a schema to help visualize the relationships and use cases of smart pointers:

```
std::unique_ptr
  |
  |-- Exclusive ownership
  |-- Cannot be copied, only moved
  |-- Automatically deletes resource when out of scope

std::shared_ptr
  |
  |-- Shared ownership
  |-- Multiple `std::shared_ptr` instances can own the same resource
  |-- Resource is deleted when the last `std::shared_ptr` goes out of scope

std::weak_ptr
  |
  |-- Non-owning reference to a `std::shared_ptr`
  |-- Does not contribute to the reference count
  |-- Used to break circular references
```

### Example: Circular Reference and `std::weak_ptr`

```cpp
#include <iostream>
#include <memory>

class Node {
public:
    std::shared_ptr<Node> next;
    std::weak_ptr<Node> prev; // Use weak_ptr to break circular reference

    ~Node() {
        std::cout << "Node destroyed" << std::endl;
    }
};

int main() {
    auto node1 = std::make_shared<Node>();
    auto node2 = std::make_shared<Node>();

    node1->next = node2;
    node2->prev = node1; // Weak reference prevents circular ownership

    return 0; // Both nodes are properly destroyed
}
```

**Analysis:**
- `std::weak_ptr` prevents a circular reference between `node1` and `node2`.
- Without `std::weak_ptr`, the reference count would never reach zero, causing a memory leak.

## Benefits of Smart Pointers
- **Automatic Memory Management:** Automatically deallocates memory, reducing the risk of memory leaks.
- **Exception Safety:** Ensures proper cleanup even in the presence of exceptions.
- **Improved Code Readability:** Makes ownership semantics explicit.

## Best Practices
- Prefer `std::make_unique` and `std::make_shared` for creating smart pointers.
- Use `std::unique_ptr` for exclusive ownership.
- Use `std::shared_ptr` for shared ownership, but avoid overusing it.
- Use `std::weak_ptr` to break circular references in `std::shared_ptr`.
- Avoid using raw pointers unless absolutely necessary.

By mastering smart pointers, you can write safer and more maintainable C++ code.

By understanding the differences and use cases of `std::unique_ptr`, `std::shared_ptr`, and `std::weak_ptr`, you can choose the right smart pointer for your specific needs.