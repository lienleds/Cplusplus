# Lambda Expressions

This folder contains resources and examples related to lambda expressions in C++.

## Step-by-Step Guide to Understanding Lambda Expressions

1. **Understand the Concept:**
   - Lambda expressions are anonymous functions that can be defined inline.
   - They are useful for short, concise operations, especially in algorithms and functional programming.
   - Introduced in C++11 and enhanced in later standards.

2. **Learn the Syntax:**
   - Basic syntax:
     ```cpp
     [capture_list](parameters) -> return_type {
         // Function body
     };
     ```
   - **Capture List:** Specifies variables from the surrounding scope to be used in the lambda.
   - **Parameters:** Input arguments to the lambda.
   - **Return Type:** Optional; deduced automatically if omitted.

3. **Explore Basic Examples:**
   - Example 1: Simple Lambda
     ```cpp
     #include <iostream>

     int main() {
         auto greet = []() {
             std::cout << "Hello, Lambda!" << std::endl;
         };
         greet(); // Outputs: Hello, Lambda!
         return 0;
     }
     ```
     **Analysis:**
     - `[]`: Empty capture list; no external variables are used.
     - `auto greet`: Lambda assigned to a variable.
     - `greet()`: Invokes the lambda.

   - Example 2: Lambda with Parameters
     ```cpp
     #include <iostream>

     int main() {
         auto add = [](int a, int b) {
             return a + b;
         };
         std::cout << add(3, 5) << std::endl; // Outputs: 8
         return 0;
     }
     ```
     **Analysis:**
     - `[](int a, int b)`: Lambda takes two parameters.
     - `return a + b;`: Returns the sum of the parameters.

4. **Understand the Capture List:**
   - The capture list allows the lambda to access variables from the surrounding scope.
   - **Capture Modes:**
     - `[=]`: Captures all variables by value.
     - `[&]`: Captures all variables by reference.
     - `[x]`: Captures `x` by value.
     - `[&x]`: Captures `x` by reference.
   - Example:
     ```cpp
     #include <iostream>

     int main() {
         int x = 10, y = 20;
         auto printSum = [=]() {
             std::cout << x + y << std::endl;
         };
         printSum(); // Outputs: 30
         return 0;
     }
     ```
     **Analysis:**
     - `[=]`: Captures `x` and `y` by value.
     - `printSum()`: Uses the captured values.

5. **Use Lambdas in Algorithms:**
   - Lambdas are commonly used with STL algorithms.
   - Example:
     ```cpp
     #include <algorithm>
     #include <vector>
     #include <iostream>

     int main() {
         std::vector<int> nums = {1, 2, 3, 4, 5};
         std::for_each(nums.begin(), nums.end(), [](int n) {
             std::cout << n * n << " ";
         });
         // Outputs: 1 4 9 16 25
         return 0;
     }
     ```
     **Analysis:**
     - `[](int n)`: Lambda takes one parameter.
     - `std::for_each`: Applies the lambda to each element in the vector.

6. **Mutable Lambdas:**
   - By default, lambdas capture variables by value and do not allow modification.
   - Use the `mutable` keyword to modify captured variables.
   - Example:
     ```cpp
     #include <iostream>

     int main() {
         int count = 0;
         auto increment = [count]() mutable {
             count++;
             std::cout << count << std::endl;
         };
         increment(); // Outputs: 1
         increment(); // Outputs: 1 (count is reset each time)
         return 0;
     }
     ```
     **Analysis:**
     - `mutable`: Allows modification of `count` inside the lambda.
     - Each invocation resets `count` to its initial value.

7. **Return Type Deduction:**
   - The return type of a lambda is deduced automatically.
   - Specify the return type explicitly if needed.
   - Example:
     ```cpp
     auto divide = [](int a, int b) -> double {
         return static_cast<double>(a) / b;
     };
     ```

8. **Best Practices:**
   - Use lambdas for short, concise operations.
   - Avoid capturing variables by reference if the lambda outlives the scope.
   - Use meaningful names for lambdas assigned to variables.
   - Prefer explicit return types for clarity in complex lambdas.

By following these steps, you can effectively understand and use lambda expressions in your C++ programs.
