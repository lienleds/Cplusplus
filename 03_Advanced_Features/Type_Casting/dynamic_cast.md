# `dynamic_cast` in C++

## What is `dynamic_cast`?

`dynamic_cast` is a type of casting operator in C++ that performs safe downcasting in polymorphic hierarchies. It ensures that the cast is valid at runtime using runtime type information (RTTI).

---

## Why Use `dynamic_cast`?

### 1. Type Safety
`dynamic_cast` ensures that the cast is valid at runtime, preventing undefined behavior.

### 2. Polymorphism
It allows safe downcasting in polymorphic class hierarchies, enabling dynamic behavior.

### 3. Error Handling
`dynamic_cast` returns `nullptr` for invalid pointer casts and throws `std::bad_cast` for invalid reference casts.

---

## Syntax

The syntax for `dynamic_cast` is:

```cpp
dynamic_cast<new_type>(expression);
```

- `new_type`: The type you want to cast to.
- `expression`: The variable or object you want to cast.

---

## Step-by-Step Examples

### Example 1: Safe Downcasting

#### Problem
You have a base class pointer pointing to a derived class object, and you need to access the derived class's members.

#### Code
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

### Example 2: Invalid Cast Handling

#### Problem
You attempt to cast a base class pointer to an unrelated class pointer.

#### Code
```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() = default;
};

class Unrelated {};

int main() {
    Base* base = new Base();

    // Attempting an invalid cast
    Unrelated* unrelated = dynamic_cast<Unrelated*>(base);
    if (!unrelated) {
        std::cout << "Invalid cast detected" << std::endl;
    }

    delete base;
    return 0;
}
```

#### Step-by-Step Analysis
1. **Create Polymorphic Base Class:** `virtual ~Base()` ensures the class is polymorphic.
2. **Create Unrelated Class:** `Unrelated` is not related to `Base`.
3. **Invalid Cast:** `dynamic_cast<Unrelated*>(base)` attempts to cast a `Base*` to an `Unrelated*`.
4. **Check Validity:** The cast fails, and `unrelated` is set to `nullptr`.

#### Output
```
Invalid cast detected
```

---

### Example 3: Reference Casting

#### Problem
You attempt to cast a base class reference to a derived class reference.

#### Code
```cpp
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual ~Base() = default;
};

class Derived : public Base {};

int main() {
    Base base;
    try {
        Derived& derived = dynamic_cast<Derived&>(base); // Invalid reference cast
    } catch (const std::bad_cast& e) {
        std::cout << "Exception: " << e.what() << std::endl;
    }

    return 0;
}
```

#### Step-by-Step Analysis
1. **Create Polymorphic Base Class:** `virtual ~Base()` ensures the class is polymorphic.
2. **Create Derived Class:** `Derived` inherits from `Base`.
3. **Invalid Reference Cast:** `dynamic_cast<Derived&>(base)` attempts to cast a `Base&` to a `Derived&`.
4. **Handle Exception:** The cast throws a `std::bad_cast` exception, which is caught and handled.

#### Output
```
Exception: std::bad_cast
```

---

## Best Practices

1. **Use Only in Polymorphic Hierarchies:**
   - Ensure the base class has at least one virtual function.
2. **Check Pointer Casts:**
   - Always check the result of `dynamic_cast` for pointers.
3. **Avoid Overuse:**
   - Excessive use of `dynamic_cast` may indicate design issues.

---

## Limitations

- **Requires RTTI:** `dynamic_cast` relies on runtime type information, which may increase binary size.
- **Polymorphic Types Only:** It works only with polymorphic types.
- **Exception Handling:** Invalid reference casts throw exceptions, which must be handled properly.

---

## Additional Resources

- [C++ Reference: dynamic_cast](https://en.cppreference.com/w/cpp/language/dynamic_cast)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)