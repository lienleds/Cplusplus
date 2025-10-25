# RTTI (Run-Time Type Information) in C++

## What is RTTI?

Run-Time Type Information (RTTI) is a mechanism in C++ that provides information about an object's type at runtime. It allows you to determine the type of an object during program execution, which is particularly useful in polymorphic scenarios.

## Why Use RTTI?

- **Type Identification:** Determine the actual type of an object in a polymorphic hierarchy.
- **Dynamic Casting:** Safely cast pointers or references to their derived types.
- **Debugging and Logging:** Useful for debugging and logging type information.

## Key Features

1. **`typeid` Operator:**
   - Used to get the type information of an object or type.
   - Returns a `std::type_info` object.

2. **`dynamic_cast`:**
   - Used to safely cast pointers or references to their derived types.
   - Returns `nullptr` if the cast is invalid (for pointers) or throws `std::bad_cast` (for references).

## Example: Using `typeid`

```cpp
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual ~Base() = default;
};

class Derived : public Base {};

int main() {
    Base* base = new Derived();

    std::cout << "Type of base: " << typeid(*base).name() << std::endl;

    delete base;
    return 0;
}
```

### Output
```
Type of base: Derived
```

## Example: Using `dynamic_cast`

```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void sayHello() {
        std::cout << "Hello from Derived!" << std::endl;
    }
};

int main() {
    Base* base = new Derived();

    Derived* derived = dynamic_cast<Derived*>(base);
    if (derived) {
        derived->sayHello();
    } else {
        std::cout << "Invalid cast" << std::endl;
    }

    delete base;
    return 0;
}
```

### Output
```
Hello from Derived!
```

## Best Practices

- Use RTTI only when necessary, as it can introduce runtime overhead.
- Prefer `dynamic_cast` over `static_cast` for polymorphic types to ensure type safety.
- Avoid excessive use of RTTI, as it may indicate design issues in your code.

## Limitations

- RTTI works only with polymorphic types (classes with at least one virtual function).
- Using RTTI can increase the size of the binary and add runtime overhead.

## Additional Resources

- [C++ Reference: RTTI](https://en.cppreference.com/w/cpp/language/rtti)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)