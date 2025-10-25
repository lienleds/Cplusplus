# `std::filesystem` in Modern C++

`std::filesystem`, introduced in C++17, provides a standard way to work with the file system. It includes utilities for file and directory manipulation, path handling, and querying file properties. This README provides an overview of `std::filesystem`, its components, and examples.

---

## What is `std::filesystem`?

- `std::filesystem` is part of the `<filesystem>` header.
- It provides a portable and type-safe way to interact with the file system.
- It includes classes and functions for paths, directories, and file operations.

---

## Key Components

### 1. **Paths**
- Represent file or directory paths.
- Use the `std::filesystem::path` class.

### 2. **File and Directory Operations**
- Functions for creating, removing, and querying files and directories.

### 3. **Iterators**
- Iterate over directories using `std::filesystem::directory_iterator` and `std::filesystem::recursive_directory_iterator`.

---

## Syntax and Examples

### Example 1: Working with Paths
```cpp
#include <iostream>
#include <filesystem>

int main() {
    std::filesystem::path p = "/tmp/example.txt";

    std::cout << "Path: " << p << "\n";
    std::cout << "Filename: " << p.filename() << "\n";
    std::cout << "Parent path: " << p.parent_path() << "\n";

    return 0;
}
```

### Example 2: Creating and Removing Directories
```cpp
#include <iostream>
#include <filesystem>

int main() {
    std::filesystem::path dir = "example_dir";

    if (!std::filesystem::exists(dir)) {
        std::filesystem::create_directory(dir);
        std::cout << "Directory created: " << dir << "\n";
    }

    std::filesystem::remove(dir);
    std::cout << "Directory removed: " << dir << "\n";

    return 0;
}
```

### Example 3: Iterating Over a Directory
```cpp
#include <iostream>
#include <filesystem>

int main() {
    std::filesystem::path dir = ".";

    for (const auto& entry : std::filesystem::directory_iterator(dir)) {
        std::cout << entry.path() << "\n";
    }

    return 0;
}
```

### Example 4: Querying File Properties
```cpp
#include <iostream>
#include <filesystem>

int main() {
    std::filesystem::path file = "example.txt";

    if (std::filesystem::exists(file)) {
        std::cout << "File size: " << std::filesystem::file_size(file) << " bytes\n";
    } else {
        std::cout << "File does not exist." << std::endl;
    }

    return 0;
}
```

---

## Best Practices

- Use `std::filesystem` for all file system operations to ensure portability.
- Check for errors using exceptions or `std::error_code`.
- Use `std::filesystem::path` for constructing and manipulating paths.

---

## Common Pitfalls

1. **Error Handling**
   - Be prepared to handle exceptions thrown by `std::filesystem` functions.

2. **Platform Differences**
   - Ensure that paths and operations are portable across platforms.

3. **Performance**
   - Be cautious when using recursive directory iterators on large directories.

---

## Conclusion

`std::filesystem` is a powerful library for interacting with the file system in modern C++. By understanding its components and best practices, developers can write portable and efficient file system code.