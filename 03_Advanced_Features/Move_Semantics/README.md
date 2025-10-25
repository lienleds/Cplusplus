# Move Semantics

This folder contains resources and examples related to move semantics in C++.

## Overview
Move semantics, introduced in C++11, optimize the performance of programs by allowing the transfer of resources from one object to another without copying. This is particularly useful for objects that manage dynamic memory or other resources.

## Key Concepts

### 1. **Lvalue and Rvalue**
- **Lvalue:** Refers to an object that persists beyond a single expression.
  - Example:
    ```cpp
    int x = 10; // x is an lvalue
    ```
- **Rvalue:** Refers to a temporary object or a value that does not persist beyond the expression.
  - Example:
    ```cpp
    int y = x + 5; // (x + 5) is an rvalue
    ```

### 2. **Move Constructor**
- Transfers resources from one object to another without copying.
- Example:
  ```cpp
  #include <iostream>
  #include <vector>

  class MyClass {
  private:
      std::vector<int> data;
  public:
      MyClass(std::vector<int> vec) : data(std::move(vec)) {}
      MyClass(MyClass&& other) noexcept : data(std::move(other.data)) {
          std::cout << "Move constructor called" << std::endl;
      }
  };

  int main() {
      std::vector<int> vec = {1, 2, 3};
      MyClass obj1(vec);
      MyClass obj2(std::move(obj1)); // Move constructor is called
      return 0;
  }
  ```

### 3. **Move Assignment Operator**
- Transfers resources from one object to another during assignment.
- Example:
  ```cpp
  class MyClass {
  private:
      std::vector<int> data;
  public:
      MyClass& operator=(MyClass&& other) noexcept {
          if (this != &other) {
              data = std::move(other.data);
          }
          return *this;
      }
  };
  ```

### 4. **`std::move`**
- A utility function that converts an lvalue to an rvalue reference.
- Example:
  ```cpp
  std::vector<int> vec = {1, 2, 3};
  std::vector<int> newVec = std::move(vec); // Transfers ownership
  ```

### 5. **Rvalue References**
- Used to implement move semantics.
- Declared using `&&`.
- Example:
  ```cpp
  void process(std::vector<int>&& vec) {
      std::cout << "Processing vector" << std::endl;
  }

  int main() {
      process(std::move(std::vector<int>{1, 2, 3}));
      return 0;
  }
  ```

## Benefits of Move Semantics
- **Performance Optimization:** Avoids unnecessary copying of resources.
- **Efficient Resource Management:** Transfers ownership of resources instead of duplicating them.
- **Improved Code Clarity:** Makes ownership transfer explicit.

## Best Practices
- Use move semantics for classes that manage dynamic memory or other resources.
- Always implement both the move constructor and move assignment operator if your class manages resources.
- Use `std::move` judiciously to avoid unintended behavior.
- Prefer `std::vector` and other STL containers that support move semantics.

By mastering move semantics, you can write more efficient and modern C++ programs.
