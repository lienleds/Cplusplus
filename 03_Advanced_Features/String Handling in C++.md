# String Handling in C++

Modern C++ provides powerful string handling capabilities through `std::string`, `std::string_view`, and related utilities.

---

## Overview

C++ string handling includes:
- **`std::string`** - Dynamic string class
- **`std::string_view`** (C++17) - Non-owning string view
- **String operations** - Search, modify, format
- **String conversions** - To/from numbers

---

## 1. std::string Basics

### Creating Strings

```cpp
#include <iostream>
#include <string>

int main() {
    // Different ways to create strings
    std::string s1;                          // Empty string
    std::string s2 = "Hello";                // From literal
    std::string s3("World");                 // Constructor
    std::string s4(5, 'x');                  // Repeat character: "xxxxx"
    std::string s5(s2);                      // Copy constructor
    std::string s6 = s2 + " " + s3;         // Concatenation
    
    std::cout << "s1: '" << s1 << "'\n";
    std::cout << "s2: " << s2 << '\n';
    std::cout << "s3: " << s3 << '\n';
    std::cout << "s4: " << s4 << '\n';
    std::cout << "s5: " << s5 << '\n';
    std::cout << "s6: " << s6 << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Automatic memory management
- ✅ Rich API for string operations
- ✅ Type-safe
- 📝 Stored on heap (unlike C-strings)

### String Operations

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello, World!";
    
    // Length/Size
    std::cout << "Length: " << str.length() << '\n';
    std::cout << "Size: " << str.size() << '\n';
    std::cout << "Empty: " << std::boolalpha << str.empty() << '\n';
    
    // Access
    std::cout << "First char: " << str.front() << '\n';
    std::cout << "Last char: " << str.back() << '\n';
    std::cout << "At index 7: " << str.at(7) << '\n';
    std::cout << "With []: " << str[7] << '\n';
    
    // Modification
    str.push_back('?');
    std::cout << "After push_back: " << str << '\n';
    
    str.pop_back();
    std::cout << "After pop_back: " << str << '\n';
    
    str.append(" How are you?");
    std::cout << "After append: " << str << '\n';
    
    str.insert(5, " there");
    std::cout << "After insert: " << str << '\n';
    
    str.erase(5, 6);
    std::cout << "After erase: " << str << '\n';
    
    str.clear();
    std::cout << "After clear: '" << str << "'\n";
    
    return 0;
}
```

---

## 2. String Search and Find

### Finding Substrings

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog";
    
    // Find first occurrence
    size_t pos = text.find("fox");
    if (pos != std::string::npos) {
        std::cout << "Found 'fox' at position: " << pos << '\n';
    }
    
    // Find from position
    pos = text.find("the", 10);
    if (pos != std::string::npos) {
        std::cout << "Found 'the' after position 10 at: " << pos << '\n';
    }
    
    // Find last occurrence
    pos = text.rfind("the");
    std::cout << "Last 'the' at position: " << pos << '\n';
    
    // Find first of any character
    pos = text.find_first_of("aeiou");
    std::cout << "First vowel at position: " << pos << '\n';
    
    // Find first not of
    pos = text.find_first_not_of("The ");
    std::cout << "First non-'The ' char at: " << pos << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Returns `std::string::npos` if not found
- ✅ Multiple search functions for different needs
- 📝 Case-sensitive by default

### Contains (C++23)

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "Hello, World!";
    
    // C++23 contains
    #if __cplusplus >= 202302L
        if (text.contains("World")) {
            std::cout << "Contains 'World'\n";
        }
    #else
        // Pre-C++23 equivalent
        if (text.find("World") != std::string::npos) {
            std::cout << "Contains 'World'\n";
        }
    #endif
    
    return 0;
}
```

---

## 3. String Manipulation

### Substring

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "Hello, World!";
    
    // Extract substring
    std::string sub1 = text.substr(0, 5);     // "Hello"
    std::string sub2 = text.substr(7);         // "World!"
    std::string sub3 = text.substr(7, 5);      // "World"
    
    std::cout << "sub1: " << sub1 << '\n';
    std::cout << "sub2: " << sub2 << '\n';
    std::cout << "sub3: " << sub3 << '\n';
    
    return 0;
}
```

### Replace

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "Hello, World!";
    
    // Replace range
    text.replace(7, 5, "C++");
    std::cout << "After replace: " << text << '\n';
    
    // Replace all occurrences
    std::string str = "foo bar foo baz foo";
    std::string from = "foo";
    std::string to = "qux";
    
    size_t pos = 0;
    while ((pos = str.find(from, pos)) != std::string::npos) {
        str.replace(pos, from.length(), to);
        pos += to.length();
    }
    std::cout << "After replace all: " << str << '\n';
    
    return 0;
}
```

### Trim (Custom Implementation)

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cctype>

// Left trim
std::string ltrim(const std::string& str) {
    auto it = std::find_if(str.begin(), str.end(), 
                          [](unsigned char ch) { return !std::isspace(ch); });
    return std::string(it, str.end());
}

// Right trim
std::string rtrim(const std::string& str) {
    auto it = std::find_if(str.rbegin(), str.rend(),
                          [](unsigned char ch) { return !std::isspace(ch); });
    return std::string(str.begin(), it.base());
}

// Trim both sides
std::string trim(const std::string& str) {
    return ltrim(rtrim(str));
}

int main() {
    std::string text = "   Hello, World!   ";
    
    std::cout << "Original: '" << text << "'\n";
    std::cout << "Ltrim: '" << ltrim(text) << "'\n";
    std::cout << "Rtrim: '" << rtrim(text) << "'\n";
    std::cout << "Trim: '" << trim(text) << "'\n";
    
    return 0;
}
```

---

## 4. std::string_view (C++17)

### Non-Owning String View

```cpp
#include <iostream>
#include <string>
#include <string_view>

void process_string(std::string_view sv) {
    std::cout << "Processing: " << sv << '\n';
    std::cout << "Length: " << sv.length() << '\n';
}

int main() {
    // From string literal
    process_string("Hello");
    
    // From std::string
    std::string str = "World";
    process_string(str);
    
    // From C-string
    const char* cstr = "C-string";
    process_string(cstr);
    
    // Substring view (no copy!)
    std::string text = "Hello, World!";
    std::string_view view = text;
    std::string_view sub = view.substr(7, 5);
    
    std::cout << "Substring view: " << sub << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ No memory allocation
- ✅ Zero-copy substring operations
- ✅ Works with different string types
- ⚠️ Doesn't own the data (must ensure lifetime)
- 💡 Perfect for function parameters

### String View Operations

```cpp
#include <iostream>
#include <string_view>

int main() {
    std::string_view sv = "Hello, World!";
    
    // Remove prefix/suffix
    std::string_view sv1 = sv;
    sv1.remove_prefix(7);
    std::cout << "After remove_prefix: " << sv1 << '\n';
    
    std::string_view sv2 = sv;
    sv2.remove_suffix(7);
    std::cout << "After remove_suffix: " << sv2 << '\n';
    
    // Starts/ends with (C++20)
    #if __cplusplus >= 202002L
        std::cout << "Starts with 'Hello': " 
                  << sv.starts_with("Hello") << '\n';
        std::cout << "Ends with 'World!': " 
                  << sv.ends_with("World!") << '\n';
    #endif
    
    return 0;
}
```

---

## 5. String Conversions

### String to Number

```cpp
#include <iostream>
#include <string>
#include <stdexcept>

int main() {
    // String to int
    std::string s1 = "42";
    int i = std::stoi(s1);
    std::cout << "Int: " << i << '\n';
    
    // String to long
    std::string s2 = "1234567890";
    long l = std::stol(s2);
    std::cout << "Long: " << l << '\n';
    
    // String to double
    std::string s3 = "3.14159";
    double d = std::stod(s3);
    std::cout << "Double: " << d << '\n';
    
    // With error handling
    try {
        std::string invalid = "not a number";
        int bad = std::stoi(invalid);
    } catch (const std::invalid_argument& e) {
        std::cout << "Invalid argument: " << e.what() << '\n';
    } catch (const std::out_of_range& e) {
        std::cout << "Out of range: " << e.what() << '\n';
    }
    
    // With base (hexadecimal)
    std::string hex = "FF";
    int value = std::stoi(hex, nullptr, 16);
    std::cout << "Hex FF = " << value << '\n';
    
    return 0;
}
```

### Number to String

```cpp
#include <iostream>
#include <string>

int main() {
    // Integer to string
    int i = 42;
    std::string s1 = std::to_string(i);
    std::cout << "String from int: " << s1 << '\n';
    
    // Double to string
    double d = 3.14159;
    std::string s2 = std::to_string(d);
    std::cout << "String from double: " << s2 << '\n';
    
    // Long to string
    long l = 1234567890L;
    std::string s3 = std::to_string(l);
    std::cout << "String from long: " << s3 << '\n';
    
    return 0;
}
```

---

## 6. String Comparison

### Lexicographical Comparison

```cpp
#include <iostream>
#include <string>

int main() {
    std::string s1 = "apple";
    std::string s2 = "banana";
    std::string s3 = "apple";
    
    // Operators
    std::cout << std::boolalpha;
    std::cout << "s1 == s3: " << (s1 == s3) << '\n';
    std::cout << "s1 < s2: " << (s1 < s2) << '\n';
    std::cout << "s1 > s2: " << (s1 > s2) << '\n';
    
    // compare() method
    int cmp = s1.compare(s2);
    if (cmp < 0) {
        std::cout << "s1 is less than s2\n";
    } else if (cmp > 0) {
        std::cout << "s1 is greater than s2\n";
    } else {
        std::cout << "s1 equals s2\n";
    }
    
    // Partial comparison
    std::string text = "Hello, World!";
    cmp = text.compare(0, 5, "Hello");
    std::cout << "First 5 chars equal 'Hello': " << (cmp == 0) << '\n';
    
    return 0;
}
```

### Case-Insensitive Comparison

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cctype>

bool case_insensitive_equal(const std::string& s1, const std::string& s2) {
    return s1.size() == s2.size() && 
           std::equal(s1.begin(), s1.end(), s2.begin(),
                     [](char a, char b) {
                         return std::tolower(a) == std::tolower(b);
                     });
}

int main() {
    std::string s1 = "Hello";
    std::string s2 = "HELLO";
    std::string s3 = "hello";
    
    std::cout << std::boolalpha;
    std::cout << "s1 == s2 (case-sensitive): " << (s1 == s2) << '\n';
    std::cout << "s1 == s2 (case-insensitive): " 
              << case_insensitive_equal(s1, s2) << '\n';
    std::cout << "s1 == s3 (case-insensitive): " 
              << case_insensitive_equal(s1, s3) << '\n';
    
    return 0;
}
```

---

## 7. String Formatting

### C++20 std::format

```cpp
#include <iostream>
#include <format>
#include <string>

int main() {
    // C++20 format
    #if __cplusplus >= 202002L
        std::string name = "Alice";
        int age = 30;
        
        std::string formatted = std::format("Name: {}, Age: {}", name, age);
        std::cout << formatted << '\n';
        
        // With width and alignment
        std::string aligned = std::format("'{:>10}' '{:<10}'", "right", "left");
        std::cout << aligned << '\n';
        
        // Numbers
        double pi = 3.14159265359;
        std::string num = std::format("Pi: {:.2f}", pi);
        std::cout << num << '\n';
    #endif
    
    return 0;
}
```

### Pre-C++20 Alternatives

```cpp
#include <iostream>
#include <sstream>
#include <iomanip>

int main() {
    // Using stringstream
    std::ostringstream oss;
    oss << "Name: " << "Alice" << ", Age: " << 30;
    std::string result = oss.str();
    std::cout << result << '\n';
    
    // With formatting
    oss.str("");
    oss.clear();
    double pi = 3.14159265359;
    oss << "Pi: " << std::fixed << std::setprecision(2) << pi;
    std::cout << oss.str() << '\n';
    
    return 0;
}
```

---

## 8. Practical Examples

### Split String

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <sstream>

std::vector<std::string> split(const std::string& str, char delimiter) {
    std::vector<std::string> tokens;
    std::stringstream ss(str);
    std::string token;
    
    while (std::getline(ss, token, delimiter)) {
        tokens.push_back(token);
    }
    
    return tokens;
}

int main() {
    std::string text = "apple,banana,orange,grape";
    auto parts = split(text, ',');
    
    std::cout << "Parts:\n";
    for (const auto& part : parts) {
        std::cout << "  " << part << '\n';
    }
    
    return 0;
}
```

### Join Strings

```cpp
#include <iostream>
#include <string>
#include <vector>

std::string join(const std::vector<std::string>& strings, 
                const std::string& delimiter) {
    if (strings.empty()) return "";
    
    std::string result = strings[0];
    for (size_t i = 1; i < strings.size(); ++i) {
        result += delimiter + strings[i];
    }
    
    return result;
}

int main() {
    std::vector<std::string> words = {"Hello", "World", "from", "C++"};
    std::string joined = join(words, " ");
    
    std::cout << "Joined: " << joined << '\n';
    
    return 0;
}
```

### Reverse String

```cpp
#include <iostream>
#include <string>
#include <algorithm>

int main() {
    std::string text = "Hello, World!";
    
    // In-place reverse
    std::reverse(text.begin(), text.end());
    std::cout << "Reversed: " << text << '\n';
    
    // Create reversed copy
    std::string original = "Hello, World!";
    std::string reversed(original.rbegin(), original.rend());
    std::cout << "Reversed copy: " << reversed << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Use std::string:**
   - ✅ Prefer over C-strings (char*)
   - ✅ Automatic memory management
   - ✅ Rich API

2. **Use std::string_view for Parameters:**
   - ✅ Zero-copy for read-only strings
   - ✅ Works with any string type
   - ⚠️ Be careful with lifetime

3. **Reserve Capacity:**
   - Use `reserve()` if you know the size
   - Reduces reallocations

4. **Use Raw String Literals:**
   - `R"(...)"` for strings with special characters
   - Especially useful for regex patterns

5. **Prefer Standard Functions:**
   - Use STL algorithms
   - Don't reinvent the wheel

---

## Performance Considerations

```cpp
#include <iostream>
#include <string>
#include <chrono>

int main() {
    auto start = std::chrono::high_resolution_clock::now();
    
    // ❌ BAD: Multiple reallocations
    std::string bad;
    for (int i = 0; i < 100000; ++i) {
        bad += "x";
    }
    
    auto bad_time = std::chrono::high_resolution_clock::now() - start;
    
    start = std::chrono::high_resolution_clock::now();
    
    // ✅ GOOD: Reserve space
    std::string good;
    good.reserve(100000);
    for (int i = 0; i < 100000; ++i) {
        good += "x";
    }
    
    auto good_time = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Without reserve: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(bad_time).count() 
              << "ms\n";
    std::cout << "With reserve: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(good_time).count() 
              << "ms\n";
    
    return 0;
}
```

---

## Summary

C++ string handling provides:
- **`std::string`** - Full-featured dynamic strings
- **`std::string_view`** - Efficient non-owning views
- **Rich API** - Search, modify, format
- **Type safety** - Better than C-strings

---

### References
- [std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- [std::string_view](https://en.cppreference.com/w/cpp/string/basic_string_view)
- [std::format (C++20)](https://en.cppreference.com/w/cpp/utility/format/format)