# Exception Handling

This folder contains resources and examples related to exception handling in C++.

## Step-by-Step Guide to Exception Handling in C++

1. **Understand the Concept:**
   - Exception handling is a mechanism to handle runtime errors in a program.
   - It ensures that the program can recover gracefully from unexpected situations.

2. **Include the Required Header:**
   - The `<exception>` header provides standard exception classes.
   - The `<stdexcept>` header provides additional exception classes like `std::runtime_error` and `std::logic_error`.

3. **Use the `try`, `catch`, and `throw` Keywords:**
   - **`try` Block:** Contains the code that might throw an exception.
   - **`throw` Statement:** Used to signal an exception.
   - **`catch` Block:** Handles the exception.
   - Example:
     ```cpp
     #include <iostream>

     int main() {
         try {
             throw std::runtime_error("An error occurred");
         } catch (const std::exception& e) {
             std::cout << "Caught exception: " << e.what() << std::endl;
         }
         return 0;
     }
     ```

4. **Understand Standard Exception Classes:**
   - C++ provides several standard exception classes in the `<exception>` and `<stdexcept>` headers.
   - Common classes include:
     - `std::exception`: Base class for all standard exceptions.
     - `std::runtime_error`: Represents runtime errors.
     - `std::logic_error`: Represents logical errors in the program.
   - Example:
     ```cpp
     #include <stdexcept>

     void checkValue(int value) {
         if (value < 0) {
             throw std::invalid_argument("Value cannot be negative");
         }
     }
     ```

5. **Create Custom Exceptions:**
   - You can define your own exception classes by inheriting from `std::exception`.
   - Example:
     ```cpp
     #include <exception>
     #include <string>

     class MyException : public std::exception {
     private:
         std::string message;
     public:
         MyException(const std::string& msg) : message(msg) {}
         const char* what() const noexcept override {
             return message.c_str();
         }
     };

     int main() {
         try {
             throw MyException("Custom exception occurred");
         } catch (const std::exception& e) {
             std::cout << e.what() << std::endl;
         }
         return 0;
     }
     ```

6. **Use `noexcept` for Functions That Do Not Throw Exceptions:**
   - The `noexcept` specifier indicates that a function does not throw exceptions.
   - Example:
     ```cpp
     void safeFunction() noexcept {
         // This function guarantees no exceptions
     }
     ```

7. **Best Practices:**
   - Always catch exceptions by reference to avoid slicing.
   - Use specific exception types in `catch` blocks for better error handling.
   - Avoid throwing exceptions in destructors.
   - Use `std::exception` as a base class for custom exceptions.
   - Document the exceptions a function might throw.

## Standard C++ Support for Exception Handling

C++ provides robust support for exception handling through its standard library. Here are the key components:

1. **Standard Exception Classes:**
   - Defined in the `<exception>` and `<stdexcept>` headers.
   - Commonly used classes include:
     - `std::exception`: Base class for all standard exceptions.
     - `std::runtime_error`: Represents runtime errors.
     - `std::logic_error`: Represents logical errors.
     - `std::bad_alloc`: Thrown when memory allocation fails.
     - `std::bad_cast`: Thrown when a dynamic cast fails.
     - `std::out_of_range`: Thrown when accessing an element out of range in a container.
     - `std::invalid_argument`: Thrown when an invalid argument is passed to a function.

2. **`std::terminate` and `std::unexpected`:**
   - `std::terminate`: Called when an exception is not caught.
   - `std::unexpected`: Called when a function throws an exception not listed in its exception specification (deprecated in C++17).

3. **`std::current_exception` and `std::rethrow_exception`:**
   - Introduced in C++11 to work with exceptions in a more flexible way.
   - `std::current_exception`: Captures the current exception.
   - `std::rethrow_exception`: Rethrows the captured exception.
   - Example:
     ```cpp
     #include <exception>
     #include <iostream>

     void handleException() {
         try {
             throw;
         } catch (const std::exception& e) {
             std::cout << "Caught: " << e.what() << std::endl;
         }
     }

     int main() {
         try {
             throw std::runtime_error("An error occurred");
         } catch (...) {
             std::exception_ptr eptr = std::current_exception();
             handleException();
         }
         return 0;
     }
     ```

4. **`std::noexcept` Specifier:**
   - Introduced in C++11 to specify whether a function throws exceptions.
   - Functions marked with `noexcept` guarantee that they will not throw exceptions.

5. **C++17 Enhancements:**
   - `std::uncaught_exceptions`: Returns the number of uncaught exceptions.
   - Example:
     ```cpp
     #include <exception>
     #include <iostream>

     int main() {
         try {
             throw std::runtime_error("Error");
         } catch (...) {
             std::cout << "Uncaught exceptions: " << std::uncaught_exceptions() << std::endl;
         }
         return 0;
     }
     ```

By leveraging these standard features, C++ developers can implement robust and maintainable exception handling mechanisms in their programs.
