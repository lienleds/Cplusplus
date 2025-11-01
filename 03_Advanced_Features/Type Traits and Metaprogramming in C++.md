# Type Traits and Metaprogramming in C++

Type traits provide compile-time information about types, enabling powerful template metaprogramming techniques.

---

## Overview

Type traits and metaprogramming enable:
- **Compile-time type inspection**
- **SFINAE (Substitution Failure Is Not An Error)**
- **Template specialization based on type properties**
- **Compile-time computations**

---

## 1. Basic Type Traits

### Type Categories

```cpp
#include <iostream>
#include <type_traits>

int main() {
    // Fundamental type checks
    std::cout << std::boolalpha;
    std::cout << "is_integral<int>: " << std::is_integral<int>::value << '\n';
    std::cout << "is_floating_point<double>: " << std::is_floating_point<double>::value << '\n';
    std::cout << "is_pointer<int*>: " << std::is_pointer<int*>::value << '\n';
    std::cout << "is_array<int[5]>: " << std::is_array<int[5]>::value << '\n';
    
    // Compound type checks
    std::cout << "is_class<std::string>: " << std::is_class<std::string>::value << '\n';
    std::cout << "is_enum<std::byte>: " << std::is_enum<std::byte>::value << '\n';
    
    // C++17 variable templates
    std::cout << "is_integral_v<int>: " << std::is_integral_v<int> << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Compile-time type checking
- ✅ No runtime overhead
- ✅ Enables type-safe generic programming
- 📝 `_v` suffix (C++17) simplifies syntax

### Type Properties

```cpp
#include <iostream>
#include <type_traits>

class MyClass {
public:
    MyClass() = default;
    MyClass(const MyClass&) = delete;
    virtual ~MyClass() = default;
};

int main() {
    std::cout << std::boolalpha;
    
    // Construction/Destruction properties
    std::cout << "is_default_constructible<MyClass>: " 
              << std::is_default_constructible_v<MyClass> << '\n';
    std::cout << "is_copy_constructible<MyClass>: " 
              << std::is_copy_constructible_v<MyClass> << '\n';
    std::cout << "is_polymorphic<MyClass>: " 
              << std::is_polymorphic_v<MyClass> << '\n';
    
    // Type modifiers
    std::cout << "is_const<const int>: " 
              << std::is_const_v<const int> << '\n';
    std::cout << "is_volatile<volatile int>: " 
              << std::is_volatile_v<volatile int> << '\n';
    std::cout << "is_reference<int&>: " 
              << std::is_reference_v<int&> << '\n';
    
    return 0;
}
```

---

## 2. SFINAE (Substitution Failure Is Not An Error)

### Enable If

```cpp
#include <iostream>
#include <type_traits>

// Only enabled for integral types
template<typename T>
typename std::enable_if<std::is_integral<T>::value, T>::type
double_value(T value) {
    std::cout << "Integral version\n";
    return value * 2;
}

// Only enabled for floating-point types
template<typename T>
typename std::enable_if<std::is_floating_point<T>::value, T>::type
double_value(T value) {
    std::cout << "Floating-point version\n";
    return value * 2.0;
}

// C++14 simplification
template<typename T>
std::enable_if_t<std::is_integral_v<T>, T>
triple_value(T value) {
    return value * 3;
}

int main() {
    std::cout << double_value(5) << '\n';        // Calls integral version
    std::cout << double_value(5.5) << '\n';      // Calls floating-point version
    std::cout << triple_value(10) << '\n';       // C++14 version
    
    return 0;
}
```

**Analysis:**
- ✅ Compile-time function selection
- ✅ Type-safe overload resolution
- ❌ Verbose syntax (pre-C++20)
- 💡 C++20 concepts provide cleaner syntax

### SFINAE with Expressions

```cpp
#include <iostream>
#include <type_traits>
#include <string>

// Check if type has begin() method
template<typename T, typename = void>
struct has_begin : std::false_type {};

template<typename T>
struct has_begin<T, std::void_t<decltype(std::declval<T>().begin())>> 
    : std::true_type {};

template<typename T>
inline constexpr bool has_begin_v = has_begin<T>::value;

// Function enabled only for containers with begin()
template<typename Container>
std::enable_if_t<has_begin_v<Container>>
print_container(const Container& c) {
    std::cout << "Container size: " << std::distance(c.begin(), c.end()) << '\n';
}

int main() {
    std::string str = "Hello";
    print_container(str); // Works because std::string has begin()
    
    // print_container(42); // Won't compile - int doesn't have begin()
    
    return 0;
}
```

---

## 3. Type Transformations

### Remove/Add Qualifiers

```cpp
#include <iostream>
#include <type_traits>

template<typename T>
void show_type_info() {
    using Plain = std::remove_cv_t<std::remove_reference_t<T>>;
    using ConstRef = std::add_lvalue_reference_t<std::add_const_t<Plain>>;
    
    std::cout << "Original is const: " << std::is_const_v<T> << '\n';
    std::cout << "Original is reference: " << std::is_reference_v<T> << '\n';
    std::cout << "Plain is const: " << std::is_const_v<Plain> << '\n';
    std::cout << "ConstRef is const: " << std::is_const_v<std::remove_reference_t<ConstRef>> << '\n';
}

int main() {
    show_type_info<const int&>();
    
    return 0;
}
```

### Decay

```cpp
#include <type_traits>
#include <iostream>

template<typename T>
void print_decayed_type(T) {
    using Decayed = std::decay_t<T>;
    
    std::cout << "Is array: " << std::is_array_v<T> << '\n';
    std::cout << "Decayed is pointer: " << std::is_pointer_v<Decayed> << '\n';
}

int main() {
    int arr[5];
    print_decayed_type(arr); // Array decays to pointer
    
    return 0;
}
```

---

## 4. Template Metaprogramming

### Compile-Time Factorial

```cpp
#include <iostream>

// Recursive template metaprogramming
template<int N>
struct Factorial {
    static constexpr int value = N * Factorial<N - 1>::value;
};

template<>
struct Factorial<0> {
    static constexpr int value = 1;
};

// C++14 constexpr function (cleaner)
constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}

int main() {
    std::cout << "5! = " << Factorial<5>::value << '\n';
    std::cout << "5! = " << factorial(5) << '\n';
    
    // Compile-time constant
    constexpr int fact6 = factorial(6);
    static_assert(fact6 == 720, "Factorial calculation error");
    
    return 0;
}
```

**Analysis:**
- ✅ Computed at compile-time
- ✅ No runtime overhead
- ❌ Can increase compilation time
- 💡 Modern constexpr is cleaner than templates

### Type List

```cpp
#include <iostream>
#include <type_traits>

// Type list
template<typename... Types>
struct TypeList {};

// Get size
template<typename List>
struct Length;

template<typename... Types>
struct Length<TypeList<Types...>> {
    static constexpr std::size_t value = sizeof...(Types);
};

// Get element at index
template<std::size_t Index, typename List>
struct At;

template<typename Head, typename... Tail>
struct At<0, TypeList<Head, Tail...>> {
    using type = Head;
};

template<std::size_t Index, typename Head, typename... Tail>
struct At<Index, TypeList<Head, Tail...>> {
    using type = typename At<Index - 1, TypeList<Tail...>>::type;
};

// Helper alias
template<std::size_t Index, typename List>
using At_t = typename At<Index, List>::type;

int main() {
    using MyTypes = TypeList<int, double, char>;
    
    std::cout << "Length: " << Length<MyTypes>::value << '\n';
    std::cout << "First is int: " << std::is_same_v<At_t<0, MyTypes>, int> << '\n';
    std::cout << "Second is double: " << std::is_same_v<At_t<1, MyTypes>, double> << '\n';
    
    return 0;
}
```

---

## 5. Conditional Types

### std::conditional

```cpp
#include <iostream>
#include <type_traits>

template<typename T>
using LargeType = std::conditional_t<
    (sizeof(T) > 8),
    T,
    long long
>;

template<typename T>
void process() {
    using Type = LargeType<T>;
    std::cout << "Using type of size: " << sizeof(Type) << " bytes\n";
}

int main() {
    process<char>();        // Uses long long (8 bytes)
    process<double>();      // Uses double (8 bytes)
    process<long double>(); // Uses long double (16 bytes on some systems)
    
    return 0;
}
```

---

## 6. Detection Idiom (C++17)

```cpp
#include <iostream>
#include <type_traits>

// Detection idiom
template<typename, typename = void>
struct has_value_type : std::false_type {};

template<typename T>
struct has_value_type<T, std::void_t<typename T::value_type>> 
    : std::true_type {};

template<typename T>
inline constexpr bool has_value_type_v = has_value_type<T>::value;

// Use with containers
#include <vector>
#include <list>

int main() {
    std::cout << std::boolalpha;
    std::cout << "vector has value_type: " << has_value_type_v<std::vector<int>> << '\n';
    std::cout << "list has value_type: " << has_value_type_v<std::list<int>> << '\n';
    std::cout << "int has value_type: " << has_value_type_v<int> << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Prefer C++17/20 Features:**
   - Use `_v` and `_t` suffixes for brevity
   - Use `std::void_t` for detection idiom
   - Use concepts (C++20) instead of SFINAE when possible

2. **Keep Templates Simple:**
   - Don't over-engineer with metaprogramming
   - Use `constexpr` functions instead of template recursion
   - Document complex template code

3. **Use Standard Traits:**
   - Don't reinvent the wheel
   - `<type_traits>` provides comprehensive utilities

4. **Test Thoroughly:**
   - Template errors can be cryptic
   - Use `static_assert` for compile-time validation

---

## Summary

Type traits enable powerful compile-time programming:
- **Type inspection** without runtime overhead
- **SFINAE** for conditional compilation
- **Metaprogramming** for compile-time computations
- **Modern C++** provides cleaner syntax than classic templates

---

### References
- [std::type_traits](https://en.cppreference.com/w/cpp/header/type_traits)
- [SFINAE](https://en.cppreference.com/w/cpp/language/sfinae)
- [Template Metaprogramming](https://en.cppreference.com/w/cpp/language/templates)
- [C++ Templates: The Complete Guide](http://www.tmplbook.com/)