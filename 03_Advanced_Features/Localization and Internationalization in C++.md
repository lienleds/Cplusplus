# Localization and Internationalization in C++

Localization (l10n) and internationalization (i18n) enable applications to support multiple languages and regional formats.

---

## Overview

C++ provides localization through:
- **`<locale>`** - Locale-specific formatting
- **`std::codecvt`** - Character encoding conversion
- **Facets** - Customizable locale components
- **Unicode support** - UTF-8, UTF-16, UTF-32

---

## 1. Basic Locale Usage

### Global Locale

```cpp
#include <iostream>
#include <locale>

int main() {
    // Get current locale
    std::locale current = std::locale();
    std::cout << "Current locale: " << current.name() << '\n';
    
    // Set global locale to user's preferred
    try {
        std::locale::global(std::locale(""));
        std::cout << "Set to user's locale: " << std::locale().name() << '\n';
    } catch (const std::runtime_error& e) {
        std::cout << "Failed to set locale: " << e.what() << '\n';
    }
    
    // Set specific locale
    try {
        std::locale::global(std::locale("en_US.UTF-8"));
        std::cout << "Set to en_US.UTF-8\n";
    } catch (const std::runtime_error& e) {
        std::cout << "Failed: " << e.what() << '\n';
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Locale determines formatting rules
- ⚠️ Locale names are platform-specific
- 📝 Default locale is "C"

### Stream-Specific Locale

```cpp
#include <iostream>
#include <locale>
#include <iomanip>

int main() {
    double value = 1234567.89;
    
    // Default (C locale)
    std::cout << "Default: " << value << '\n';
    
    // German locale
    try {
        std::locale de("de_DE.UTF-8");
        std::cout.imbue(de);
        std::cout << "German: " << value << '\n';
    } catch (...) {
        std::cout << "German locale not available\n";
    }
    
    // French locale
    try {
        std::locale fr("fr_FR.UTF-8");
        std::cout.imbue(fr);
        std::cout << "French: " << value << '\n';
    } catch (...) {
        std::cout << "French locale not available\n";
    }
    
    return 0;
}
```

---

## 2. Number Formatting

### Locale-Aware Number Output

```cpp
#include <iostream>
#include <locale>
#include <iomanip>

int main() {
    double value = 1234567.89;
    
    std::cout << std::fixed << std::setprecision(2);
    
    // C locale (default)
    std::cout.imbue(std::locale("C"));
    std::cout << "C locale: " << value << '\n';
    
    // Try different locales
    std::vector<std::string> locales = {
        "en_US.UTF-8",
        "de_DE.UTF-8",
        "fr_FR.UTF-8",
        "ja_JP.UTF-8"
    };
    
    for (const auto& loc_name : locales) {
        try {
            std::locale loc(loc_name);
            std::cout.imbue(loc);
            std::cout << loc_name << ": " << value << '\n';
        } catch (...) {
            std::cout << loc_name << ": not available\n";
        }
    }
    
    return 0;
}
```

### Number Parsing

```cpp
#include <iostream>
#include <locale>
#include <sstream>

int main() {
    std::string german_number = "1.234.567,89";
    
    try {
        std::locale de("de_DE.UTF-8");
        std::istringstream iss(german_number);
        iss.imbue(de);
        
        double value;
        iss >> value;
        
        if (iss) {
            std::cout << "Parsed value: " << value << '\n';
        } else {
            std::cout << "Failed to parse\n";
        }
    } catch (...) {
        std::cout << "German locale not available\n";
    }
    
    return 0;
}
```

---

## 3. Date and Time Formatting

### Locale-Aware Date Output

```cpp
#include <iostream>
#include <locale>
#include <iomanip>
#include <ctime>

int main() {
    std::time_t now = std::time(nullptr);
    std::tm* local_time = std::localtime(&now);
    
    std::vector<std::string> locales = {
        "en_US.UTF-8",
        "de_DE.UTF-8",
        "fr_FR.UTF-8",
        "ja_JP.UTF-8"
    };
    
    for (const auto& loc_name : locales) {
        try {
            std::locale loc(loc_name);
            std::cout.imbue(loc);
            std::cout << loc_name << ": ";
            std::cout << std::put_time(local_time, "%x %X") << '\n';
        } catch (...) {
            std::cout << loc_name << ": not available\n";
        }
    }
    
    return 0;
}
```

---

## 4. Currency Formatting

### Locale-Aware Currency

```cpp
#include <iostream>
#include <locale>
#include <iomanip>

int main() {
    double amount = 1234.56;
    
    std::vector<std::string> locales = {
        "en_US.UTF-8",
        "de_DE.UTF-8",
        "ja_JP.UTF-8",
        "en_GB.UTF-8"
    };
    
    for (const auto& loc_name : locales) {
        try {
            std::locale loc(loc_name);
            std::cout.imbue(loc);
            std::cout << loc_name << ": " 
                      << std::showbase << std::put_money(amount * 100) 
                      << '\n';
        } catch (...) {
            std::cout << loc_name << ": not available\n";
        }
    }
    
    return 0;
}
```

---

## 5. String Collation

### Locale-Aware String Comparison

```cpp
#include <iostream>
#include <locale>
#include <string>
#include <vector>
#include <algorithm>

int main() {
    std::vector<std::string> words = {
        "zebra", "Äpfel", "apple", "ändern", "zoo"
    };
    
    // Default sorting
    std::vector<std::string> default_sorted = words;
    std::sort(default_sorted.begin(), default_sorted.end());
    
    std::cout << "Default sorting:\n";
    for (const auto& word : default_sorted) {
        std::cout << "  " << word << '\n';
    }
    
    // German locale sorting
    try {
        std::locale de("de_DE.UTF-8");
        std::vector<std::string> locale_sorted = words;
        
        std::sort(locale_sorted.begin(), locale_sorted.end(),
                 [&de](const std::string& a, const std::string& b) {
                     return de(a, b);
                 });
        
        std::cout << "\nGerman locale sorting:\n";
        for (const auto& word : locale_sorted) {
            std::cout << "  " << word << '\n';
        }
    } catch (...) {
        std::cout << "German locale not available\n";
    }
    
    return 0;
}
```

---

## 6. Character Classification

### Locale-Aware Character Functions

```cpp
#include <iostream>
#include <locale>
#include <string>

void analyze_character(wchar_t ch, const std::locale& loc) {
    std::cout << "Character: " << ch << '\n';
    std::cout << "  Is alpha: " << std::isalpha(ch, loc) << '\n';
    std::cout << "  Is digit: " << std::isdigit(ch, loc) << '\n';
    std::cout << "  Is space: " << std::isspace(ch, loc) << '\n';
    std::cout << "  Is upper: " << std::isupper(ch, loc) << '\n';
    std::cout << "  Is lower: " << std::islower(ch, loc) << '\n';
    std::cout << "  To upper: " << std::toupper(ch, loc) << '\n';
    std::cout << "  To lower: " << std::tolower(ch, loc) << '\n';
}

int main() {
    try {
        std::locale loc("en_US.UTF-8");
        
        analyze_character(L'a', loc);
        std::cout << '\n';
        analyze_character(L'5', loc);
        std::cout << '\n';
        analyze_character(L'Ä', loc);
    } catch (...) {
        std::cout << "Locale not available\n";
    }
    
    return 0;
}
```

---

## 7. Unicode Support

### UTF-8 Strings

```cpp
#include <iostream>
#include <string>
#include <locale>
#include <codecvt>

int main() {
    // UTF-8 string
    std::string utf8_str = u8"Hello, 世界! 🌍";
    std::cout << "UTF-8 string: " << utf8_str << '\n';
    std::cout << "Byte length: " << utf8_str.length() << '\n';
    
    // Convert UTF-8 to UTF-16
    std::wstring_convert<std::codecvt_utf8_utf16<char16_t>, char16_t> converter;
    std::u16string utf16_str = converter.from_bytes(utf8_str);
    std::cout << "UTF-16 length: " << utf16_str.length() << '\n';
    
    // Convert back
    std::string back_to_utf8 = converter.to_bytes(utf16_str);
    std::cout << "Back to UTF-8: " << back_to_utf8 << '\n';
    
    return 0;
}
```

**Note:** `std::codecvt` is deprecated in C++17, but still widely used.

### C++20 char8_t

```cpp
#include <iostream>
#include <string>

int main() {
    #if __cplusplus >= 202002L
        // C++20 UTF-8 string
        std::u8string utf8 = u8"Hello, 世界!";
        
        // Note: Can't directly output u8string to cout
        std::string str(reinterpret_cast<const char*>(utf8.data()), utf8.size());
        std::cout << str << '\n';
    #endif
    
    return 0;
}
```

---

## 8. Custom Facets

### Custom Number Formatting

```cpp
#include <iostream>
#include <locale>

class my_numpunct : public std::numpunct<char> {
protected:
    char do_thousands_sep() const override { return '\''; }
    std::string do_grouping() const override { return "\3"; }
    char do_decimal_point() const override { return ','; }
};

int main() {
    double value = 1234567.89;
    
    // Default formatting
    std::cout << "Default: " << value << '\n';
    
    // Custom formatting
    std::locale custom(std::locale(), new my_numpunct);
    std::cout.imbue(custom);
    std::cout << std::fixed << std::setprecision(2);
    std::cout << "Custom: " << value << '\n';
    
    return 0;
}
```

---

## 9. Practical Examples

### Multilingual Message System

```cpp
#include <iostream>
#include <string>
#include <map>

class MessageCatalog {
    std::map<std::string, std::map<std::string, std::string>> messages;
    std::string current_lang = "en";
    
public:
    void add_message(const std::string& lang, 
                    const std::string& key, 
                    const std::string& message) {
        messages[lang][key] = message;
    }
    
    void set_language(const std::string& lang) {
        current_lang = lang;
    }
    
    std::string get(const std::string& key) const {
        auto lang_it = messages.find(current_lang);
        if (lang_it != messages.end()) {
            auto msg_it = lang_it->second.find(key);
            if (msg_it != lang_it->second.end()) {
                return msg_it->second;
            }
        }
        return "[Missing: " + key + "]";
    }
};

int main() {
    MessageCatalog catalog;
    
    // English messages
    catalog.add_message("en", "greeting", "Hello");
    catalog.add_message("en", "farewell", "Goodbye");
    
    // German messages
    catalog.add_message("de", "greeting", "Guten Tag");
    catalog.add_message("de", "farewell", "Auf Wiedersehen");
    
    // Japanese messages
    catalog.add_message("ja", "greeting", "こんにちは");
    catalog.add_message("ja", "farewell", "さようなら");
    
    // English
    catalog.set_language("en");
    std::cout << catalog.get("greeting") << '\n';
    std::cout << catalog.get("farewell") << '\n';
    
    // German
    catalog.set_language("de");
    std::cout << catalog.get("greeting") << '\n';
    std::cout << catalog.get("farewell") << '\n';
    
    // Japanese
    catalog.set_language("ja");
    std::cout << catalog.get("greeting") << '\n';
    std::cout << catalog.get("farewell") << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Use UTF-8:**
   - Default to UTF-8 encoding
   - Most compatible with modern systems

2. **Don't Hardcode Strings:**
   - Externalize user-facing strings
   - Use message catalogs

3. **Test with Different Locales:**
   - Don't assume English/US format
   - Test number, date, currency formatting

4. **Handle Missing Locales:**
   - Provide fallbacks
   - Don't crash if locale unavailable

5. **Consider ICU Library:**
   - For complex i18n needs
   - Better Unicode support

---

## Common Locale Names

| Locale | Description |
|--------|-------------|
| `C` | Default/minimal |
| `en_US.UTF-8` | US English |
| `en_GB.UTF-8` | British English |
| `de_DE.UTF-8` | German |
| `fr_FR.UTF-8` | French |
| `ja_JP.UTF-8` | Japanese |
| `zh_CN.UTF-8` | Chinese (Simplified) |
| `es_ES.UTF-8` | Spanish |
| `ru_RU.UTF-8` | Russian |

---

## Summary

C++ localization provides:
- **Locale-aware formatting** for numbers, dates, currency
- **Character classification** respecting locale rules
- **Unicode support** (UTF-8, UTF-16, UTF-32)
- **Customizable facets** for specific needs

---

### References
- [std::locale](https://en.cppreference.com/w/cpp/locale/locale)
- [Localization library](https://en.cppreference.com/w/cpp/locale)
- [ICU - International Components for Unicode](http://site.icu-project.org/)
- [UTF-8 Everywhere](http://utf8everywhere.org/)