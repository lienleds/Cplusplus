# `static_cast` vs `dynamic_cast` in C++

## Overview

Both `static_cast` and `dynamic_cast` are C++ casting operators used to convert one type to another. While they may seem similar, they serve different purposes and have distinct use cases. Understanding their differences is crucial for writing safe and efficient C++ code.

---

## `static_cast`

### What is `static_cast`?
`static_cast` is used for compile-time type conversions. It performs a direct conversion between types and does not involve any runtime checks.

### Use Cases
- Converting between related types (e.g., base class to derived class).
- Converting between primitive types (e.g., `int` to `float`).
- Removing qualifiers like `const` (though `const_cast` is preferred).

### Example: Upcasting in Polymorphism
```cpp
#include <iostream>

class Base {
public:
    virtual void display() {
        std::cout << "Base class" << std::endl;
    }
};

class Derived : public Base {
public:
    void display() override {
        std::cout << "Derived class" << std::endl;
    }
};

int main() {
    Derived d;
    Base* b = static_cast<Base*>(&d); // Upcasting

    b->display();

    return 0;
}
```

#### Step-by-Step Analysis
1. **Create Polymorphic Classes:** `Base` and `Derived` classes demonstrate polymorphism.
2. **Upcast Derived to Base:** `Base* b = static_cast<Base*>(&d);` safely upcasts the `Derived` object to a `Base` pointer.
3. **Call Virtual Function:** `b->display();` calls the overridden `display` method in `Derived`.

#### Output
```
Derived class
```

---

## `dynamic_cast`

### What is `dynamic_cast`?
`dynamic_cast` is used for safe downcasting in polymorphic hierarchies. It performs runtime type checking to ensure the cast is valid.

### Use Cases
- Downcasting from a base class to a derived class in polymorphic hierarchies.
- Checking the type of an object at runtime.

### Example: Safe Downcasting
```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default; // Ensure polymorphism
};

class Derived : public Base {
public:
    void sayHello() {
        std::cout << "Hello from Derived!" << std::endl;
    }
};

int main() {
    Base* base = new Derived(); // Base pointer to Derived object

    Derived* derived = dynamic_cast<Derived*>(base); // Safe downcasting
    if (derived) {
        derived->sayHello();
    } else {
        std::cout << "Invalid cast" << std::endl;
    }

    delete base;
    return 0;
}
```

#### Step-by-Step Analysis
1. **Create Polymorphic Base Class:** `virtual ~Base()` ensures the class is polymorphic.
2. **Create Derived Class:** `Derived` inherits from `Base` and adds a `sayHello` method.
3. **Base Pointer to Derived Object:** `Base* base = new Derived();` creates a base pointer pointing to a derived object.
4. **Safe Downcasting:** `dynamic_cast<Derived*>(base)` safely casts the base pointer to a derived pointer.
5. **Check Validity:** If the cast is valid, call `derived->sayHello()`.

#### Output
```
Hello from Derived!
```

---

## Comparison

| Feature                | `static_cast`                  | `dynamic_cast`                 |
|------------------------|---------------------------------|---------------------------------|
| **Type Checking**      | Compile-time                  | Runtime                        |
| **Safety**             | Unsafe if types are incompatible | Safe, checks validity at runtime |
| **Performance**        | Faster                        | Slower due to runtime checks   |
| **Use Case**           | Related types, primitive conversions | Polymorphic hierarchies        |
| **Failure Handling**   | Undefined behavior            | Returns `nullptr` on failure   |

---

## Best Practices

1. **Use `static_cast` for Compile-Time Conversions:**
   - Prefer `static_cast` for conversions that are guaranteed to be safe at compile time.
2. **Use `dynamic_cast` for Polymorphic Hierarchies:**
   - Use `dynamic_cast` when working with polymorphic hierarchies and need type safety.
3. **Avoid Using `static_cast` for Downcasting:**
   - Do not use `static_cast` for downcasting in polymorphism unless you are absolutely sure of the object's type.
4. **Document Usage:**
   - Clearly document why a specific cast is being used to ensure maintainability.

---

## Additional Resources

- [C++ Reference: static_cast](https://en.cppreference.com/w/cpp/language/static_cast)
- [C++ Reference: dynamic_cast](https://en.cppreference.com/w/cpp/language/dynamic_cast)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)