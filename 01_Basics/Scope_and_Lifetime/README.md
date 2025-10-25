
# Scope and Lifetime in C++

Scope determines where a variable or function can be accessed. Lifetime refers to how long a variable exists during program execution.

## Types of Scope

- **Local Scope**: Declared inside a block or function; accessible only within that block.
- **Global Scope**: Declared outside all functions; accessible throughout the file.
- **Class/Namespace Scope**: Members accessible within the class or namespace.

## Lifetime

- **Automatic (local) variables**: Exist during block execution.
- **Static variables**: Exist for the lifetime of the program.
- **Dynamic variables**: Created with `new`; exist until deleted.

## Examples

```cpp
int globalVar = 10; // Global scope

void func() {
	int localVar = 5; // Local scope
	static int staticVar = 0; // Static lifetime
}
```

## Best Practices
- Minimize global variables.
- Prefer local scope for variables.
- Use static variables for persistent state.
- Always delete dynamically allocated memory.

---
This folder contains resources and examples related to scope and lifetime in C++.
