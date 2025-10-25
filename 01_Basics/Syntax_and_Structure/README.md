
# Syntax and Structure in C++

C++ syntax defines the rules for writing valid C++ programs. Understanding the basic structure is essential for all C++ development.

## Basic Program Structure

Every C++ program consists of functions and statements. The entry point is the `main()` function.

```cpp
#include <iostream>

int main() {
	std::cout << "Hello, World!" << std::endl;
	return 0;
}
```

### Key Elements
- **Preprocessor Directives**: Begin with `#` (e.g., `#include`, `#define`).
- **Functions**: Blocks of code that perform tasks. The `main()` function is required.
- **Statements**: Instructions that end with a semicolon (`;`).
- **Blocks**: Groups of statements enclosed in `{}`.
- **Comments**: Single-line (`//`) and multi-line (`/* ... */`).

## Naming Conventions
- Variable and function names should be descriptive.
- Use camelCase or snake_case consistently.

## Indentation and Formatting
- Indent code for readability.
- Use spaces or tabs consistently.

## Example: Simple Program

```cpp
#include <iostream>

// This program adds two numbers
int main() {
	int a = 5;
	int b = 3;
	int sum = a + b;
	std::cout << "Sum: " << sum << std::endl;
	return 0;
}
```

## Best Practices
- Always use braces `{}` for blocks, even for single statements.
- Comment your code for clarity.
- Keep code organized and modular.

---
This folder contains resources and examples related to C++ syntax and program structure.
