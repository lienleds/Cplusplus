
# Operators in C++

C++ operators are special symbols or keywords that perform operations on operands. Operators are fundamental to C++ programming and are used in expressions to manipulate data and variables.

## Types of Operators

1. **Arithmetic Operators**
	- `+` Addition
	- `-` Subtraction
	- `*` Multiplication
	- `/` Division
	- `%` Modulus

2. **Relational Operators**
	- `==` Equal to
	- `!=` Not equal to
	- `>` Greater than
	- `<` Less than
	- `>=` Greater than or equal to
	- `<=` Less than or equal to

3. **Logical Operators**
	- `&&` Logical AND
	- `||` Logical OR
	- `!` Logical NOT

4. **Bitwise Operators**
	- `&` Bitwise AND
	- `|` Bitwise OR
	- `^` Bitwise XOR
	- `~` Bitwise NOT
	- `<<` Left shift
	- `>>` Right shift

5. **Assignment Operators**
	- `=` Assignment
	- `+=` Add and assign
	- `-=` Subtract and assign
	- `*=` Multiply and assign
	- `/=` Divide and assign
	- `%=` Modulus and assign
	- `<<=` Left shift and assign
	- `>>=` Right shift and assign
	- `&=` Bitwise AND and assign
	- `^=` Bitwise XOR and assign
	- `|=` Bitwise OR and assign

6. **Increment and Decrement Operators**
	- `++` Increment
	- `--` Decrement

7. **Conditional (Ternary) Operator**
	- `?:` Conditional expression

8. **Other Operators**
	- `sizeof` Returns the size of a data type
	- `typeid` Returns the type information
	- `,` Comma operator
	- `->` Member access operator
	- `.` Member access operator
	- `[]` Array subscript
	- `()` Function call operator

## Operator Precedence and Associativity

Operators have precedence that determines the order in which operations are performed. Associativity determines the direction of evaluation when operators have the same precedence.

Refer to the official C++ documentation for a complete precedence table.

## Examples

```cpp
int a = 5, b = 2;
int sum = a + b;        // Arithmetic
bool isEqual = (a == b); // Relational
bool result = (a > 0 && b > 0); // Logical
a += 3;                 // Assignment
int c = a << 1;         // Bitwise
int max = (a > b) ? a : b; // Conditional
```

## Best Practices

- Use parentheses to clarify complex expressions.
- Avoid mixing too many operators in a single statement.
- Be cautious with operator precedence and associativity.
- Use meaningful variable names for readability.

## Operator Overloading

C++ allows you to define custom behavior for operators in user-defined types (classes). See the Operator Overloading section for details.

---
This folder contains resources and examples related to C++ operators.
