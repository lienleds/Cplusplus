
# Control Flow in C++

Control flow statements determine the order in which instructions are executed in a program.

## Conditional Statements

- **if**: Executes code if condition is true
- **else**: Executes code if condition is false
- **else if**: Checks multiple conditions
- **switch**: Selects code to execute based on value

Example:
```cpp
int x = 10;
if (x > 5) {
	std::cout << "x is greater than 5";
} else {
	std::cout << "x is not greater than 5";
}
```

## Switch Statement
```cpp
int day = 3;
switch (day) {
	case 1: std::cout << "Monday"; break;
	case 2: std::cout << "Tuesday"; break;
	case 3: std::cout << "Wednesday"; break;
	default: std::cout << "Other day";
}
```

## Best Practices
- Use braces `{}` for all blocks.
- Avoid deeply nested control flow.
- Use switch for multiple discrete values.

---
This folder contains resources and examples related to C++ control flow statements.
