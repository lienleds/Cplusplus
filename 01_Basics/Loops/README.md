
# Loops in C++

Loops allow repeated execution of code blocks. C++ supports several types of loops.

## Types of Loops

- **for loop**: Used when the number of iterations is known.
- **while loop**: Used when the number of iterations is unknown.
- **do-while loop**: Similar to while, but executes at least once.

## Examples

### For Loop
```cpp
for (int i = 0; i < 5; ++i) {
	std::cout << i << " ";
}
```

### While Loop
```cpp
int i = 0;
while (i < 5) {
	std::cout << i << " ";
	++i;
}
```

### Do-While Loop
```cpp
int i = 0;
do {
	std::cout << i << " ";
	++i;
} while (i < 5);
```

## Best Practices
- Avoid infinite loops.
- Use break and continue judiciously.
- Prefer for loops for counting.

---
This folder contains resources and examples related to C++ loops.
