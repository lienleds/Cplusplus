
# Functions in C++

Functions are blocks of code that perform specific tasks. They help organize code and enable reuse.

## Function Declaration and Definition

```cpp
// Declaration
int add(int a, int b);

// Definition
int add(int a, int b) {
	return a + b;
}
```

## Calling Functions
```cpp
int result = add(2, 3);
std::cout << "Result: " << result;
```

## Function Parameters
- Pass by value
- Pass by reference
- Default arguments

Example:
```cpp
void greet(std::string name = "Guest") {
	std::cout << "Hello, " << name << "!";
}
```

## Return Types
- Functions can return any data type.
- Use `void` for no return value.

## Best Practices
- Keep functions short and focused.
- Use meaningful names.
- Document parameters and return values.

---
This folder contains resources and examples related to C++ functions.
