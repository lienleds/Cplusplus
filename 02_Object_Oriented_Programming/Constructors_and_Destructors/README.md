# Constructors and Destructors in C++

Constructors initialize objects when they are created. Destructors clean up resources when objects are destroyed.

## Constructor

- Special member function with the same name as the class.
- Can be overloaded.
- Can have default arguments.

Example:
```cpp
class Point {
public:
	int x, y;
	Point() { x = 0; y = 0; } // Default constructor
	Point(int a, int b) { x = a; y = b; } // Parameterized constructor
};
```

## Destructor

- Special member function with the same name as the class, prefixed with `~`.
- Called automatically when object goes out of scope.

Example:
```cpp
class File {
public:
	File() { /* open file */ }
	~File() { /* close file */ }
};
```

### Default Destructor

A default destructor is a destructor that the compiler automatically generates if no destructor is explicitly defined in the class. It is sufficient for classes that do not manage dynamic resources or require custom cleanup logic.

#### Key Points:
- The compiler-generated destructor performs a shallow cleanup of the class members.
- It is automatically invoked when an object goes out of scope or is explicitly deleted.
- If the class contains members that require deep cleanup (e.g., dynamically allocated memory), a custom destructor should be defined.

#### Example of Default Destructor:
```cpp
class Simple {
public:
    int x;
    // No explicit destructor defined, so the compiler generates a default destructor.
};

int main() {
    Simple obj;
    obj.x = 10;
    // Default destructor is called automatically when 'obj' goes out of scope.
    return 0;
}
```

#### When to Use Default Destructor:
- Use the default destructor when the class does not allocate dynamic memory or manage resources explicitly.
- For classes with simple data members (e.g., primitive types, standard containers), the default destructor is usually sufficient.

#### Explicitly Declaring a Default Destructor:
- Starting from C++11, you can explicitly declare a default destructor using `= default`.
- This is useful when you want to make the destructor explicitly visible in the class definition.
- Example:
  ```cpp
  class ExplicitDefault {
  public:
      ~ExplicitDefault() = default; // Explicitly declares the default destructor
  };
  ```

## Best Practices
- Always release resources in destructors.
- Avoid throwing exceptions from destructors.
- Use initializer lists for member initialization.

## Additional Best Practices

1. **Use Virtual Destructors in Base Classes:**
   - If a class is intended to be used as a base class, declare its destructor as `virtual`.
   - This ensures that the destructor of the derived class is called when deleting an object through a base class pointer.
   - Example:
     ```cpp
     class Base {
     public:
         virtual ~Base() { std::cout << "Base destructor"; }
     };
     class Derived : public Base {
     public:
         ~Derived() { std::cout << "Derived destructor"; }
     };
     Base* obj = new Derived();
     delete obj; // Calls both Base and Derived destructors
     ```

2. **Use Initializer Lists for Member Initialization:**
   - Prefer initializer lists over assignment in the constructor body for initializing class members.
   - This is especially important for `const` members and reference members.
   - Example:
     ```cpp
     class Point {
     private:
         const int x;
         const int y;
     public:
         Point(int a, int b) : x(a), y(b) {}
     };
     ```

3. **Avoid Resource Leaks:**
   - Ensure that all resources (e.g., dynamic memory, file handles) are released in the destructor.
   - Use smart pointers (e.g., `std::unique_ptr`, `std::shared_ptr`) to manage dynamic memory automatically.
   - Example:
     ```cpp
     class Resource {
     private:
         std::unique_ptr<int> data;
     public:
         Resource() : data(std::make_unique<int>(42)) {}
     };
     ```

4. **Avoid Throwing Exceptions in Destructors:**
   - Throwing exceptions in destructors can lead to undefined behavior if another exception is already active.
   - Instead, handle errors gracefully within the destructor.

5. **Declare Destructors as `default` or `delete` When Appropriate:**
   - Use `= default` to explicitly declare a default destructor.
   - Use `= delete` to prevent the creation of a destructor.
   - Example:
     ```cpp
     class NonDeletable {
     public:
         ~NonDeletable() = delete; // Prevents object destruction
     };
     ```

6. **Follow the Rule of Three/Five/Zero:**
   - If a class requires a custom destructor, copy constructor, or copy assignment operator, it likely needs all three.
   - With C++11 and later, consider the Rule of Five, which includes the move constructor and move assignment operator.
   - Example:
     ```cpp
     class RuleOfFive {
     private:
         int* data;
     public:
         RuleOfFive() : data(new int(0)) {}
         ~RuleOfFive() { delete data; }
         RuleOfFive(const RuleOfFive& other) : data(new int(*other.data)) {}
         RuleOfFive& operator=(const RuleOfFive& other) {
             if (this != &other) {
                 *data = *other.data;
             }
             return *this;
         }
         RuleOfFive(RuleOfFive&& other) noexcept : data(other.data) {
             other.data = nullptr;
         }
         RuleOfFive& operator=(RuleOfFive&& other) noexcept {
             if (this != &other) {
                 delete data;
                 data = other.data;
                 other.data = nullptr;
             }
             return *this;
         }
     };
     ```

---
This folder contains resources and examples related to constructors and destructors in C++.
