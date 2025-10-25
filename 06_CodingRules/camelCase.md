# camelCase in C++

`camelCase` is a naming convention in which the first letter of the first word is lowercase, and the first letter of each subsequent word is capitalized. This convention is widely used in C++ for naming variables, functions, and methods.

---

## Why Use camelCase?
- **Readability:** Improves the readability of variable and function names.
- **Consistency:** Ensures uniformity across the codebase.
- **Industry Standard:** Commonly used in many C++ coding guidelines.

---

## Examples of camelCase

### Variables
```cpp
int userAge = 25;
float accountBalance = 1000.50;
std::string firstName = "John";
```

### Functions
```cpp
void calculateInterest(float principal, float rate) {
    // Function logic
}

int getUserAge() {
    return 25;
}
```

### Classes and Methods
```cpp
class BankAccount {
private:
    float accountBalance;

public:
    void deposit(float amount) {
        accountBalance += amount;
    }

    float getBalance() {
        return accountBalance;
    }
};
```

---

## Best Practices
- **Consistency:** Always use camelCase for variables and functions unless the project specifies a different convention.
- **Avoid Abbreviations:** Use descriptive names to improve code clarity.
- **Follow Project Guidelines:** Adhere to the naming conventions specified in the project's coding standards.

---

## Comparison with Other Naming Conventions

| Convention      | Example           | Common Usage                  |
|-----------------|-------------------|-------------------------------|
| `camelCase`     | `userAge`         | Variables, functions          |
| `PascalCase`    | `BankAccount`     | Class names                   |
| `snake_case`    | `user_age`        | Constants, macros (sometimes) |

---

## Summary
Using `camelCase` in C++ helps maintain a clean and consistent codebase. By following this convention, developers can improve the readability and maintainability of their code.

---

### References
- [C++ Coding Standards](https://isocpp.org/wiki/faq/coding-standards)
- [Best Practices for Naming Conventions](https://www.geeksforgeeks.org/naming-conventions-in-programming/)