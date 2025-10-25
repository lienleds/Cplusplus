
# Operator Overloading in C++

Operator overloading allows you to redefine the behavior of operators for user-defined types (classes).

## Example: Overloading '+' Operator
```cpp
class Complex {
public:
	double real, imag;
	Complex(double r, double i) : real(r), imag(i) {}
	Complex operator+(const Complex& other) {
		return Complex(real + other.real, imag + other.imag);
	}
};
Complex c1(1.0, 2.0), c2(3.0, 4.0);
Complex c3 = c1 + c2;
```

## Overloadable Operators
- Arithmetic: +, -, *, /
- Comparison: ==, !=, <, >
- Assignment: =, +=, -=
- Stream: <<, >>

## Best Practices
- Overload operators only when it makes logical sense
- Maintain expected operator semantics
- Keep operator functions simple

---
This folder contains resources and examples related to operator overloading in C++.
