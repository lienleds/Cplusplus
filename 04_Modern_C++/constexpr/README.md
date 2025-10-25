# `constexpr` in Modern C++

`constexpr` is a keyword introduced in C++11 and enhanced in subsequent standards. It allows the evaluation of expressions at compile time, enabling optimizations and ensuring correctness. This README provides an overview of `constexpr`, its use cases, and examples.

---

## What is `constexpr`?

- `constexpr` stands for "constant expression."
- It is used to declare variables, functions, and objects that can be evaluated at compile time.
- Compile-time evaluation ensures that the values are computed during compilation, leading to faster runtime performance.

---

## Key Features

### C++11
- Introduced `constexpr` for variables and functions.
- Functions marked as `constexpr` must have a single return statement and cannot contain loops or other runtime constructs.

### C++14
- Relaxed restrictions on `constexpr` functions:
  - Allowed loops and conditional statements.
  - Made `constexpr` functions more versatile.

### C++17
- Introduced `constexpr` for `if` statements.

### C++20
- Added `constexpr` for destructors.
- Expanded the scope of `constexpr` to include more constructs.

---

## Syntax

### `constexpr` Variables
```cpp
constexpr int compileTimeValue = 42; // Evaluated at compile time
```

### `constexpr` Functions
```cpp
constexpr int square(int x) {
    return x * x;
}

constexpr int result = square(5); // Computed at compile time
```

### `constexpr` with Classes
```cpp
struct Point {
    int x, y;

    constexpr Point(int x, int y) : x(x), y(y) {}
    constexpr int getX() const { return x; }
};

constexpr Point p(1, 2);
constexpr int x = p.getX();
```

---

## Use Cases

1. **Compile-Time Constants**
   - Define constants that are computed at compile time.

2. **Optimizations**
   - Enable the compiler to optimize code by precomputing values.

3. **Template Metaprogramming**
   - Use `constexpr` in templates to compute values at compile time.

4. **Static Assertions**
   - Combine `constexpr` with `static_assert` to enforce compile-time checks.

---

## Examples

### Example 1: Factorial
```cpp
constexpr int factorial(int n) {
    return (n <= 1) ? 1 : (n * factorial(n - 1));
}

constexpr int fact5 = factorial(5); // Computed at compile time
```

### Example 2: Compile-Time Array Size
```cpp
constexpr int arraySize(bool condition) {
    return condition ? 10 : 20;
}

int arr[arraySize(true)]; // Array size determined at compile time
```

### Example 3: `constexpr` with `std::array`
```cpp
#include <array>

constexpr std::array<int, 3> createArray() {
    return {1, 2, 3};
}

constexpr auto arr = createArray();
```

---

## Best Practices

- Use `constexpr` for values and functions that can be computed at compile time.
- Avoid overusing `constexpr` for runtime-only constructs.
- Combine `constexpr` with `static_assert` for compile-time validation.

---

## Common Pitfalls

1. **Runtime Constructs in `constexpr` Functions**
   - Ensure that all expressions in a `constexpr` function can be evaluated at compile time.

2. **Overhead in Compilation**
   - Extensive use of `constexpr` may increase compilation time.

---

## Conclusion

`constexpr` is a powerful feature in modern C++ that enables compile-time computation, leading to optimized and safer code. By understanding its capabilities and limitations, developers can write more efficient and maintainable programs.