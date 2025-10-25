# Evolution of C++ Standards

C++ has undergone significant evolution since its inception, with each standard introducing new features and improvements. This document summarizes the key features introduced in each version of the C++ standard.

---

## C++98 (1998)
- **First ISO Standard for C++.**
- Introduced the Standard Template Library (STL):
  - Containers (e.g., `std::vector`, `std::map`).
  - Algorithms (e.g., `std::sort`, `std::find`).
  - Iterators.
- Exception handling with `try`, `catch`, and `throw`.
- Namespaces.
- `const_cast`, `dynamic_cast`, `reinterpret_cast`, and `static_cast`.

---

## C++03 (2003)
- **Bug fixes and improvements** to C++98.
- Added value initialization.
- Improved library features (e.g., `std::string` performance).

---

## C++11 (2011)
- **Major update, often called "Modern C++."**
- Core language features:
  - `auto` keyword.
  - Range-based for loops.
  - Lambda expressions.
  - Rvalue references and move semantics.
  - `nullptr`.
  - Uniform initialization.
  - `constexpr`.
  - Variadic templates.
- Standard Library additions:
  - Smart pointers (`std::unique_ptr`, `std::shared_ptr`).
  - `std::thread` and multithreading support.
  - `std::chrono`.
  - `std::tuple`.
  - `std::function` and `std::bind`.

---

## C++14 (2014)
- **Incremental improvements over C++11.**
- Core language features:
  - Generic lambdas.
  - `decltype(auto)`.
  - Relaxed `constexpr` restrictions.
- Standard Library additions:
  - `std::make_unique`.

---

## C++17 (2017)
- **Focus on simplification and performance.**
- Core language features:
  - Structured bindings.
  - `if constexpr`.
  - Inline variables.
  - Fold expressions.
- Standard Library additions:
  - `std::optional`, `std::variant`, and `std::any`.
  - Filesystem library (`std::filesystem`).
  - Parallel algorithms.

---

## C++20 (2020)
- **Massive update, often called "C++2a."**
- Core language features:
  - Concepts and constraints.
  - Modules.
  - Coroutines.
  - Three-way comparison operator (`<=>`).
  - Ranges library.
- Standard Library additions:
  - `std::span`.
  - Calendar and time zone library.
  - `std::format`.

---

## C++23 (2023)
- **Latest standard with further refinements.**
- Core language features:
  - Deduction guides for `std::pair` and `std::tuple`.
  - Improved support for constexpr.
- Standard Library additions:
  - `std::expected`.
  - `std::flat_map` and `std::flat_set`.

---

## Summary
C++ has evolved significantly over the years, introducing powerful features and libraries to make programming more efficient and expressive. Each standard builds on the previous one, ensuring backward compatibility while adding modern capabilities.

---

### References
- [C++ Standards on cppreference](https://en.cppreference.com/w/cpp/standard)
- [ISO C++ Committee](https://isocpp.org/)