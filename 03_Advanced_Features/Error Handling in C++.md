# Error Handling in C++

Error handling is crucial for writing robust C++ applications. Beyond exceptions, C++ provides several mechanisms for handling errors.

---

## Overview

Error handling techniques in C++:
- **Exceptions** - For exceptional, rare error conditions
- **Error Codes** - For expected, recoverable errors
- **`std::error_code`** - Standard error code representation
- **`errno`** - C-style error handling
- **Return values** - Simple success/failure indicators

---

## 1. Error Codes

### Basic Error Codes
```cpp
#include <iostream>

enum class ErrorCode {
    Success = 0,
    FileNotFound,
    PermissionDenied,
    InvalidInput
};

ErrorCode openFile(const std::string& filename) {
    // Simulated file opening
    if (filename.empty()) {
        return ErrorCode::InvalidInput;
    }
    // ... file opening logic
    return ErrorCode::Success;
}

int main() {
    ErrorCode result = openFile("test.txt");
    
    if (result != ErrorCode::Success) {
        std::cerr << "Error opening file: " << static_cast<int>(result) << '\n';
        return 1;
    }
    
    std::cout << "File opened successfully\n";
    return 0;
}
```

**Analysis:**
- ✅ Simple and efficient
- ✅ No overhead
- ❌ Requires checking return values
- ❌ Can be ignored by caller

---

## 2. std::error_code and std::error_condition

C++11 introduced a standard error handling system.

### Using std::error_code
```cpp
#include <iostream>
#include <system_error>
#include <fstream>

void readFile(const std::string& filename, std::error_code& ec) {
    std::ifstream file(filename);
    
    if (!file) {
        ec = std::make_error_code(std::errc::no_such_file_or_directory);
        return;
    }
    
    // Read file...
    ec.clear(); // Success
}

int main() {
    std::error_code ec;
    readFile("nonexistent.txt", ec);
    
    if (ec) {
        std::cerr << "Error: " << ec.message() << " (code: " << ec.value() << ")\n";
        return 1;
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Standard, portable error representation
- ✅ Rich error information
- ✅ Composable with different error categories
- ❌ More verbose than simple error codes

### Custom Error Category
```cpp
#include <system_error>
#include <string>

enum class DatabaseError {
    ConnectionFailed = 1,
    QueryFailed,
    Timeout
};

class DatabaseErrorCategory : public std::error_category {
public:
    const char* name() const noexcept override {
        return "database";
    }
    
    std::string message(int ev) const override {
        switch (static_cast<DatabaseError>(ev)) {
            case DatabaseError::ConnectionFailed:
                return "Connection to database failed";
            case DatabaseError::QueryFailed:
                return "Database query failed";
            case DatabaseError::Timeout:
                return "Database operation timed out";
            default:
                return "Unknown database error";
        }
    }
};

const DatabaseErrorCategory& database_category() {
    static DatabaseErrorCategory instance;
    return instance;
}

std::error_code make_error_code(DatabaseError e) {
    return {static_cast<int>(e), database_category()};
}

namespace std {
    template <>
    struct is_error_code_enum<DatabaseError> : true_type {};
}

// Usage
void connectToDatabase(std::error_code& ec) {
    // Simulate connection failure
    ec = DatabaseError::ConnectionFailed;
}

int main() {
    std::error_code ec;
    connectToDatabase(ec);
    
    if (ec) {
        std::cerr << "Database error: " << ec.message() << '\n';
    }
}
```

---

## 3. errno - C-Style Error Handling

### Using errno
```cpp
#include <iostream>
#include <cerrno>
#include <cstring>
#include <fstream>

void readFileWithErrno(const char* filename) {
    errno = 0; // Clear previous errors
    
    FILE* file = fopen(filename, "r");
    
    if (!file) {
        std::cerr << "Error opening file: " 
                  << strerror(errno) << " (errno: " << errno << ")\n";
        return;
    }
    
    // ... read file
    fclose(file);
}

int main() {
    readFileWithErrno("nonexistent.txt");
    return 0;
}
```

**Analysis:**
- ✅ Compatible with C libraries
- ✅ Simple to use
- ❌ Global state (not thread-safe without care)
- ❌ Limited to system errors

---

## 4. Expected/Result Types (C++23 and beyond)

### std::expected (C++23)
```cpp
#include <expected>
#include <string>
#include <iostream>

std::expected<int, std::string> divide(int a, int b) {
    if (b == 0) {
        return std::unexpected("Division by zero");
    }
    return a / b;
}

int main() {
    auto result = divide(10, 2);
    
    if (result) {
        std::cout << "Result: " << *result << '\n';
    } else {
        std::cerr << "Error: " << result.error() << '\n';
    }
    
    auto errorResult = divide(10, 0);
    
    if (!errorResult) {
        std::cerr << "Error: " << errorResult.error() << '\n';
    }
}
```

---

## 5. Combining Error Handling Strategies

```cpp
#include <iostream>
#include <system_error>
#include <optional>
#include <string>

// Strategy 1: Optional for simple cases
std::optional<int> parseInt(const std::string& str) {
    try {
        return std::stoi(str);
    } catch (...) {
        return std::nullopt;
    }
}

// Strategy 2: Error code for detailed errors
enum class ParseError {
    Empty,
    InvalidFormat,
    OutOfRange
};

struct ParseResult {
    int value;
    ParseError error;
    bool success;
};

ParseResult parseIntDetailed(const std::string& str) {
    if (str.empty()) {
        return {0, ParseError::Empty, false};
    }
    
    try {
        int value = std::stoi(str);
        return {value, ParseError::Empty, true};
    } catch (const std::invalid_argument&) {
        return {0, ParseError::InvalidFormat, false};
    } catch (const std::out_of_range&) {
        return {0, ParseError::OutOfRange, false};
    }
}

int main() {
    // Using optional
    if (auto result = parseInt("123")) {
        std::cout << "Parsed: " << *result << '\n';
    }
    
    // Using detailed error
    auto detailedResult = parseIntDetailed("abc");
    if (!detailedResult.success) {
        std::cout << "Parse failed with error code: " 
                  << static_cast<int>(detailedResult.error) << '\n';
    }
}
```

---

## Best Practices

1. **Choose the Right Mechanism:**
   - Use exceptions for exceptional, unrecoverable errors
   - Use error codes for expected, recoverable errors
   - Use `std::optional` for operations that may not produce a value
   - Use `std::error_code` for system-level errors

2. **Be Consistent:**
   - Use the same error handling strategy throughout your codebase
   - Document your error handling approach

3. **Don't Mix Strategies Carelessly:**
   - Avoid mixing exceptions and error codes in the same function
   - Be clear about which errors are exceptional vs expected

4. **Always Check Error Conditions:**
   - Don't ignore error codes
   - Handle errors at the appropriate level

5. **Provide Context:**
   - Include meaningful error messages
   - Log errors for debugging

---

## Performance Considerations

| Mechanism | Overhead | Use Case |
|-----------|----------|----------|
| Error codes | Minimal | Hot paths, expected errors |
| `std::error_code` | Low | System errors, library boundaries |
| Exceptions | High (when thrown) | Rare, exceptional conditions |
| `errno` | Minimal | C library interop |

---

## Summary

Modern C++ provides multiple error handling mechanisms. Choose based on:
- **Performance requirements**
- **Error frequency**
- **Code clarity**
- **Interoperability needs**

---

### References
- [C++ Error Handling](https://en.cppreference.com/w/cpp/error)
- [std::error_code](https://en.cppreference.com/w/cpp/error/error_code)
- [C++ Core Guidelines - Error Handling](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-errors)