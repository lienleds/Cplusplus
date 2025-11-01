# Regular Expressions in C++

Regular expressions provide powerful pattern matching capabilities for string processing.

---

## Overview

The `<regex>` library (C++11) provides:
- **Pattern matching**
- **Search and replace**
- **Text validation**
- **Tokenization**

---

## 1. Basic Pattern Matching

### Simple Match

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog";
    std::regex pattern("fox");
    
    if (std::regex_search(text, pattern)) {
        std::cout << "Pattern 'fox' found in text\n";
    } else {
        std::cout << "Pattern not found\n";
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Simple and readable
- ✅ Built-in library
- ⚠️ Regex compilation can be expensive
- 💡 Reuse compiled regex objects

### Exact Match

```cpp
#include <iostream>
#include <regex>
#include <string>

bool is_valid_email(const std::string& email) {
    // Simple email pattern
    std::regex pattern(R"(^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$)");
    return std::regex_match(email, pattern);
}

int main() {
    std::string email1 = "user@example.com";
    std::string email2 = "invalid.email";
    
    std::cout << email1 << " is " << (is_valid_email(email1) ? "valid" : "invalid") << '\n';
    std::cout << email2 << " is " << (is_valid_email(email2) ? "valid" : "invalid") << '\n';
    
    return 0;
}
```

---

## 2. Capturing Groups

### Extract Submatches

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "John Doe, age 30";
    std::regex pattern(R"((\w+)\s+(\w+),\s+age\s+(\d+))");
    std::smatch matches;
    
    if (std::regex_search(text, matches, pattern)) {
        std::cout << "Full match: " << matches[0] << '\n';
        std::cout << "First name: " << matches[1] << '\n';
        std::cout << "Last name: " << matches[2] << '\n';
        std::cout << "Age: " << matches[3] << '\n';
    }
    
    return 0;
}
```

**Output:**
```
Full match: John Doe, age 30
First name: John
Last name: Doe
Age: 30
```

**Analysis:**
- ✅ Extracts specific parts of matched text
- ✅ Groups indexed from 0 (full match) to N (subgroups)
- 📝 Use raw strings `R"(...)"` for regex patterns

### Named Captures (C++11 doesn't support, but here's a workaround)

```cpp
#include <iostream>
#include <regex>
#include <string>
#include <map>

struct NamedMatches {
    std::map<std::string, std::string> groups;
    
    void add(const std::string& name, const std::string& value) {
        groups[name] = value;
    }
    
    std::string operator[](const std::string& name) const {
        auto it = groups.find(name);
        return it != groups.end() ? it->second : "";
    }
};

NamedMatches parse_date(const std::string& date) {
    std::regex pattern(R"((\d{4})-(\d{2})-(\d{2}))");
    std::smatch matches;
    NamedMatches result;
    
    if (std::regex_match(date, matches, pattern)) {
        result.add("year", matches[1]);
        result.add("month", matches[2]);
        result.add("day", matches[3]);
    }
    
    return result;
}

int main() {
    auto date = parse_date("2024-11-02");
    
    std::cout << "Year: " << date["year"] << '\n';
    std::cout << "Month: " << date["month"] << '\n';
    std::cout << "Day: " << date["day"] << '\n';
    
    return 0;
}
```

---

## 3. Search and Replace

### Basic Replace

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog";
    std::regex pattern("fox");
    
    std::string result = std::regex_replace(text, pattern, "cat");
    std::cout << result << '\n';
    
    return 0;
}
```

**Output:**
```
The quick brown cat jumps over the lazy dog
```

### Replace with Backreferences

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    // Swap first and last name
    std::string text = "John Doe";
    std::regex pattern(R"((\w+)\s+(\w+))");
    
    std::string result = std::regex_replace(text, pattern, "$2, $1");
    std::cout << result << '\n'; // Output: Doe, John
    
    return 0;
}
```

### Multiple Replacements

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string html = "<p>Hello</p> <p>World</p>";
    std::regex tag_pattern("<[^>]+>");
    
    // Remove all HTML tags
    std::string plain_text = std::regex_replace(html, tag_pattern, "");
    std::cout << plain_text << '\n'; // Output: Hello World
    
    return 0;
}
```

---

## 4. Iterating Over Matches

### Find All Matches

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "Phone numbers: 123-456-7890, 987-654-3210, 555-123-4567";
    std::regex pattern(R"(\d{3}-\d{3}-\d{4})");
    
    auto matches_begin = std::sregex_iterator(text.begin(), text.end(), pattern);
    auto matches_end = std::sregex_iterator();
    
    std::cout << "Found " << std::distance(matches_begin, matches_end) << " phone numbers:\n";
    
    for (auto it = matches_begin; it != matches_end; ++it) {
        std::cout << "  " << it->str() << '\n';
    }
    
    return 0;
}
```

**Output:**
```
Found 3 phone numbers:
  123-456-7890
  987-654-3210
  555-123-4567
```

### Token Iterator

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "apple,banana,orange,grape";
    std::regex delimiter(",");
    
    std::sregex_token_iterator tokens(text.begin(), text.end(), delimiter, -1);
    std::sregex_token_iterator end;
    
    std::cout << "Tokens:\n";
    for (auto it = tokens; it != end; ++it) {
        std::cout << "  " << *it << '\n';
    }
    
    return 0;
}
```

---

## 5. Regex Options

### Case Insensitive

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "Hello World";
    std::regex pattern("hello", std::regex::icase);
    
    if (std::regex_search(text, pattern)) {
        std::cout << "Match found (case insensitive)\n";
    }
    
    return 0;
}
```

### Multiple Options

```cpp
#include <iostream>
#include <regex>
#include <string>

int main() {
    std::string text = "Line 1\nLine 2\nLine 3";
    
    // Multiline and case insensitive
    std::regex pattern("^line", std::regex::icase | std::regex::multiline);
    
    auto matches_begin = std::sregex_iterator(text.begin(), text.end(), pattern);
    auto matches_end = std::sregex_iterator();
    
    std::cout << "Matches: " << std::distance(matches_begin, matches_end) << '\n';
    
    return 0;
}
```

**Common Options:**
| Flag | Description |
|------|-------------|
| `std::regex::icase` | Case-insensitive matching |
| `std::regex::multiline` | `^` and `$` match line boundaries |
| `std::regex::ECMAScript` | ECMAScript grammar (default) |
| `std::regex::basic` | POSIX basic regular expressions |
| `std::regex::extended` | POSIX extended regular expressions |
| `std::regex::grep` | POSIX grep grammar |
| `std::regex::egrep` | POSIX egrep grammar |

---

## 6. Validation Examples

### URL Validation

```cpp
#include <iostream>
#include <regex>
#include <string>

bool is_valid_url(const std::string& url) {
    std::regex pattern(
        R"(^https?://[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}(/.*)?$)"
    );
    return std::regex_match(url, pattern);
}

int main() {
    std::vector<std::string> urls = {
        "http://example.com",
        "https://www.example.com/path",
        "ftp://example.com",
        "not a url"
    };
    
    for (const auto& url : urls) {
        std::cout << url << " is " 
                  << (is_valid_url(url) ? "valid" : "invalid") << '\n';
    }
    
    return 0;
}
```

### Phone Number Validation

```cpp
#include <iostream>
#include <regex>
#include <string>

bool is_valid_phone(const std::string& phone) {
    // Matches: (123) 456-7890, 123-456-7890, 1234567890
    std::regex pattern(
        R"(^(\(\d{3}\)\s?|\d{3}-)?\d{3}-?\d{4}$)"
    );
    return std::regex_match(phone, pattern);
}

int main() {
    std::vector<std::string> phones = {
        "(123) 456-7890",
        "123-456-7890",
        "1234567890",
        "123-45-6789"
    };
    
    for (const auto& phone : phones) {
        std::cout << phone << " is " 
                  << (is_valid_phone(phone) ? "valid" : "invalid") << '\n';
    }
    
    return 0;
}
```

---

## 7. Performance Considerations

### Compile Regex Once

```cpp
#include <iostream>
#include <regex>
#include <string>
#include <vector>
#include <chrono>

// ❌ BAD: Compiles regex every time
bool check_slow(const std::string& text) {
    std::regex pattern("[0-9]+"); // Compiled every call!
    return std::regex_search(text, pattern);
}

// ✅ GOOD: Compile regex once
bool check_fast(const std::string& text) {
    static const std::regex pattern("[0-9]+"); // Compiled once
    return std::regex_search(text, pattern);
}

int main() {
    std::vector<std::string> data(10000, "test123");
    
    auto start = std::chrono::high_resolution_clock::now();
    for (const auto& text : data) {
        check_slow(text);
    }
    auto slow_time = std::chrono::high_resolution_clock::now() - start;
    
    start = std::chrono::high_resolution_clock::now();
    for (const auto& text : data) {
        check_fast(text);
    }
    auto fast_time = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Slow: " << std::chrono::duration_cast<std::chrono::milliseconds>(slow_time).count() << "ms\n";
    std::cout << "Fast: " << std::chrono::duration_cast<std::chrono::milliseconds>(fast_time).count() << "ms\n";
    
    return 0;
}
```

---

## Common Regex Patterns

```cpp
// Email
R"(^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$)"

// URL
R"(^https?://[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}(/.*)?$)"

// Phone (US)
R"(^(\(\d{3}\)\s?|\d{3}-)?\d{3}-?\d{4}$)"

// Date (YYYY-MM-DD)
R"(^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$)"

// IP Address
R"(^(\d{1,3}\.){3}\d{1,3}$)"

// Hex Color
R"(^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$)"

// Username
R"(^[a-zA-Z0-9_-]{3,16}$)"

// Password (min 8 chars, 1 uppercase, 1 lowercase, 1 digit)
R"(^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$)"
```

---

## Best Practices

1. **Use Raw Strings:**
   - Always use `R"(...)"` for regex patterns
   - Avoids escaping backslashes

2. **Compile Once:**
   - Store `std::regex` objects as static or member variables
   - Don't compile in loops

3. **Test Thoroughly:**
   - Regex can be tricky
   - Test edge cases
   - Use online regex testers

4. **Keep It Simple:**
   - Don't over-complicate patterns
   - Consider string functions for simple cases

5. **Handle Errors:**
   - Regex compilation can throw `std::regex_error`
   - Validate patterns at runtime if user-provided

---

## Summary

Regular expressions in C++ provide:
- **Powerful pattern matching**
- **Text validation and extraction**
- **Search and replace capabilities**
- **Standard library support** (no external dependencies)

---

### References
- [std::regex](https://en.cppreference.com/w/cpp/regex)
- [Regular Expressions Tutorial](https://www.regular-expressions.info/)
- [Regex101 - Online Tester](https://regex101.com/)