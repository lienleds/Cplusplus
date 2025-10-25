# std::optional, std::variant, and std::any

Modern C++ introduces several utilities to handle optional values, type-safe unions, and type-erased containers. These utilities—`std::optional`, `std::variant`, and `std::any`—are part of the `<optional>`, `<variant>`, and `<any>` headers, respectively.

---

## std::optional

`std::optional` represents an object that may or may not contain a value. It is useful for functions that may fail or return no result.

### Example: Using `std::optional`
```cpp
#include <iostream>
#include <optional>

std::optional<int> divide(int a, int b) {
    if (b == 0) {
        return std::nullopt; // No value
    }
    return a / b;
}

int main() {
    auto result = divide(10, 2);
    if (result) {
        std::cout << "Result: " << *result << '\n';
    } else {
        std::cout << "Division by zero!" << '\n';
    }

    return 0;
}
```

### Key Features
- Provides `std::nullopt` to represent an empty state.
- Use `*` or `.value()` to access the contained value.
- Check if a value exists using `.has_value()` or by converting to `bool`.

---

## std::variant

`std::variant` is a type-safe union that can hold one of several types. It is useful for cases where a variable may hold different types at different times.

### Example: Using `std::variant`
```cpp
#include <iostream>
#include <variant>

int main() {
    std::variant<int, std::string> data;

    data = 42;
    std::cout << "Integer: " << std::get<int>(data) << '\n';

    data = "Hello, World!";
    std::cout << "String: " << std::get<std::string>(data) << '\n';

    return 0;
}
```

### Key Features
- Use `std::get<T>` to access the value of type `T`.
- Use `std::holds_alternative<T>` to check the current type.
- Throws `std::bad_variant_access` if the wrong type is accessed.

---

## std::any

`std::any` is a type-erased container for single values of any type. It is useful for storing values of unknown or varying types.

### Example: Using `std::any`
```cpp
#include <iostream>
#include <any>

int main() {
    std::any value = 42;
    std::cout << "Integer: " << std::any_cast<int>(value) << '\n';

    value = std::string("Hello, World!");
    std::cout << "String: " << std::any_cast<std::string>(value) << '\n';

    return 0;
}
```

### Key Features
- Use `std::any_cast<T>` to retrieve the stored value.
- Check if a value is stored using `.has_value()`.
- Throws `std::bad_any_cast` if the wrong type is accessed.

---

## Comparison

| Feature            | `std::optional` | `std::variant` | `std::any` |
|--------------------|-----------------|----------------|------------|
| Holds a single type | Yes             | No             | No         |
| Type-safe          | Yes             | Yes            | No         |
| Empty state        | Yes             | No             | Yes        |
| Type-erased        | No              | No             | Yes        |

---

## Best Practices
- Use `std::optional` for optional values.
- Use `std::variant` for type-safe unions.
- Use `std::any` sparingly, as it lacks compile-time type safety.

---

## Summary
- `std::optional` is for optional values.
- `std::variant` is for type-safe unions.
- `std::any` is for type-erased containers.

---

### References
- [C++ Reference for std::optional](https://en.cppreference.com/w/cpp/utility/optional)
- [C++ Reference for std::variant](https://en.cppreference.com/w/cpp/utility/variant)
- [C++ Reference for std::any](https://en.cppreference.com/w/cpp/utility/any)