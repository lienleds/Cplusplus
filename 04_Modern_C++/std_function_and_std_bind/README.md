# std::function and std::bind

`std::function` and `std::bind` are two powerful utilities in the C++ Standard Library that enable advanced functional programming techniques. They are particularly useful for creating flexible and reusable code.

## std::function

`std::function` is a general-purpose polymorphic function wrapper. It can store, copy, and invoke any callable target, such as:
- Functions
- Lambda expressions
- Bind expressions
- Function objects (functors)
- Member function pointers

### Example: Using `std::function`
```cpp
#include <iostream>
#include <functional>

void freeFunction(int x) {
    std::cout << "Free function called with: " << x << '\n';
}

int main() {
    std::function<void(int)> func;

    // Assign a free function
    func = freeFunction;
    func(10);

    // Assign a lambda
    func = [](int x) { std::cout << "Lambda called with: " << x << '\n'; };
    func(20);

    // Assign a functor
    struct Functor {
        void operator()(int x) const {
            std::cout << "Functor called with: " << x << '\n';
        }
    };
    func = Functor();
    func(30);

    return 0;
}
```

### Key Features
- Type erasure: `std::function` hides the type of the callable object.
- Copyable and assignable.
- Can be empty (null), in which case invoking it throws `std::bad_function_call`.

---

## std::bind

`std::bind` is used to create function objects by binding arguments to a callable object. It allows partial application of function arguments.

### Example: Using `std::bind`
```cpp
#include <iostream>
#include <functional>

void print(int a, int b) {
    std::cout << "a: " << a << ", b: " << b << '\n';
}

int main() {
    // Bind the first argument to 10
    auto boundFunc = std::bind(print, 10, std::placeholders::_1);

    // Call the bound function with the second argument
    boundFunc(20); // Output: a: 10, b: 20

    return 0;
}
```

### Placeholders
Placeholders are used to specify which arguments are bound and which are provided at the time of invocation. They are defined in the `std::placeholders` namespace.

### Key Features
- Enables partial application of arguments.
- Can be used with `std::function` to create flexible callbacks.

---

## Combining `std::function` and `std::bind`

`std::bind` is often used in conjunction with `std::function` to create flexible and reusable callbacks.

### Example
```cpp
#include <iostream>
#include <functional>

void printSum(int a, int b) {
    std::cout << "Sum: " << (a + b) << '\n';
}

int main() {
    // Bind the first argument to 5
    auto boundFunc = std::bind(printSum, 5, std::placeholders::_1);

    // Store in std::function
    std::function<void(int)> func = boundFunc;

    // Invoke the function
    func(10); // Output: Sum: 15

    return 0;
}
```

---

## Best Practices
- Use `std::function` when you need a type-erased callable object.
- Prefer lambdas over `std::bind` for better readability and performance in modern C++.

---

## Summary
- `std::function` is a versatile function wrapper for storing and invoking callable objects.
- `std::bind` allows partial application of arguments and creates function objects.
- Together, they enable powerful functional programming patterns in C++.

---

### References
- [C++ Reference for std::function](https://en.cppreference.com/w/cpp/utility/functional/function)
- [C++ Reference for std::bind](https://en.cppreference.com/w/cpp/utility/functional/bind)