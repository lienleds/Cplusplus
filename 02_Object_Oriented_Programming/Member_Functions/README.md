
# Member Functions in C++

Member functions are functions defined inside a class that operate on its data members.

## Declaration and Definition

```cpp
class Rectangle {
public:
	int width, height;
	int area(); // Declaration
};

int Rectangle::area() { // Definition
	return width * height;
}
```

## Accessing Member Functions

```cpp
Rectangle rect;
rect.width = 5;
rect.height = 4;
std::cout << rect.area();
```

## Const Member Functions
- Declared with `const` keyword; do not modify object state.

Example:
```cpp
class Circle {
public:
	double radius;
	double getArea() const {
		return 3.14159 * radius * radius;
	}
};
```

## Best Practices
- Keep member functions short and focused.
- Use const correctness.
- Document function purpose and parameters.

---
This folder contains resources and examples related to member functions in C++.
