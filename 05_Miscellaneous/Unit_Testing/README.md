# Unit Testing in C++

Unit testing is a software testing technique where individual units or components of a program are tested in isolation to ensure they work as expected. In C++, unit testing is commonly performed using frameworks like Google Test (gtest), Catch2, or Boost.Test.

---

## Why Unit Testing?
- **Ensures Code Quality:** Detects bugs early in the development cycle.
- **Facilitates Refactoring:** Provides confidence when modifying code.
- **Improves Design:** Encourages modular and maintainable code.

---

## Google Test (gtest)
Google Test is a popular C++ testing framework that provides a rich set of assertions and test utilities.

### Example: Basic Test Case
```cpp
#include <gtest/gtest.h>

int add(int a, int b) {
    return a + b;
}

TEST(AdditionTest, PositiveNumbers) {
    EXPECT_EQ(add(2, 3), 5);
}

TEST(AdditionTest, NegativeNumbers) {
    EXPECT_EQ(add(-2, -3), -5);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

### Steps to Run Google Test
1. Install Google Test library.
2. Compile the test file with `-lgtest` and `-lgtest_main`.
3. Run the compiled binary to execute tests.

---

## Catch2
Catch2 is another lightweight and easy-to-use testing framework for C++.

### Example: Basic Test Case
```cpp
#define CATCH_CONFIG_MAIN
#include <catch2/catch.hpp>

int multiply(int a, int b) {
    return a * b;
}

TEST_CASE("Multiplication works correctly", "[multiply]") {
    REQUIRE(multiply(2, 3) == 6);
    REQUIRE(multiply(-2, 3) == -6);
}
```

### Steps to Run Catch2
1. Include the Catch2 header file.
2. Compile the test file.
3. Run the compiled binary to execute tests.

---

## Best Practices
- **Write Tests Early:** Write unit tests as you develop code.
- **Test Edge Cases:** Cover edge cases and invalid inputs.
- **Keep Tests Independent:** Ensure tests do not depend on each other.
- **Automate Testing:** Use Continuous Integration (CI) tools to automate test execution.

---

## Summary
Unit testing is an essential practice for ensuring code quality and maintainability in C++. Frameworks like Google Test and Catch2 make it easy to write and execute tests. By adhering to best practices, developers can create robust and reliable software.

---

### References
- [Google Test Documentation](https://google.github.io/googletest/)
- [Catch2 Documentation](https://github.com/catchorg/Catch2)
- [Boost.Test Documentation](https://www.boost.org/doc/libs/release/libs/test/)