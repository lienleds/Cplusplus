
# Input/Output in C++

Input and output (I/O) operations allow programs to interact with users and external systems. C++ provides standard streams for I/O.

## Standard I/O Streams

- **cin**: Standard input stream (keyboard)
- **cout**: Standard output stream (console)
- **cerr**: Standard error stream
- **clog**: Standard log stream

## Basic Usage

```cpp
#include <iostream>

int main() {
	int age;
	std::cout << "Enter your age: ";
	std::cin >> age;
	std::cout << "You are " << age << " years old." << std::endl;
	return 0;
}
```

## File I/O

Use `<fstream>`, `<ifstream>`, and `<ofstream>` for file operations.

Example:
```cpp
#include <fstream>

std::ofstream outFile("output.txt");
outFile << "Hello, file!";
outFile.close();
```

## Best Practices
- Always check for I/O errors.
- Close files after use.
- Use `std::endl` to flush output.

---
This folder contains resources and examples related to C++ input and output operations.
