
# Variables and Constants in C++

Variables are used to store data that can change during program execution. Constants are fixed values that do not change.

## Variables

- Declared with a type and a name.
- Can be assigned and modified.

Example:
```cpp
int score = 100;
float temperature = 36.6;
std::string name = "Alice";
score = 120; // value changed
```

## Constants

- Declared using `const` keyword or `#define` preprocessor directive.
- Value cannot be changed after initialization.

Example:
```cpp
const double PI = 3.14159;
#define MAX_SIZE 100
```

## Scope and Lifetime
- Variables can be local, global, or static.
- Constants are usually global or file-scoped.

## Best Practices
- Use meaningful names for variables and constants.
- Prefer `const` over `#define` for type safety.
- Initialize variables before use.
- Use constants for values that should not change.

---
This folder contains resources and examples related to C++ variables and constants.
