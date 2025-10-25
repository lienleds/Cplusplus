
# Data Types in C++

C++ supports various data types to store different kinds of values. Understanding data types is fundamental to programming in C++.

## Fundamental Data Types

- **int**: Integer values (e.g., 1, -5)
- **float**: Floating-point numbers (e.g., 3.14)
- **double**: Double-precision floating-point numbers
- **char**: Single character (e.g., 'A')
- **bool**: Boolean values (`true` or `false`)

## Derived Data Types

- **Array**: Collection of elements of the same type
- **Pointer**: Stores memory address
- **Reference**: Alias for another variable
- **Function**: Can be used as a type

## User-Defined Data Types

- **struct**: Group of variables
- **class**: Blueprint for objects
- **union**: Stores different data types in the same memory location
- **enum**: Set of named integer constants

## Example

```cpp
int age = 25;
float height = 5.9f;
double pi = 3.14159;
char grade = 'A';
bool isActive = true;

int numbers[5] = {1, 2, 3, 4, 5};
struct Person {
	std::string name;
	int age;
};
```

## Type Modifiers

- **signed**, **unsigned**: Specify sign
- **short**, **long**: Specify size

Example:
```cpp
unsigned int count = 10;
long double bigNumber = 1e10;
```

## Best Practices
- Choose the most appropriate data type for your needs.
- Be aware of memory usage and limits.
- Use `const` for values that should not change.

---
This folder contains resources and examples related to C++ data types.
