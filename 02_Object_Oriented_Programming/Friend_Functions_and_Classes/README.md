
# Friend Functions and Classes in C++

Friend functions and classes can access private and protected members of another class.

## Friend Function Example
```cpp
class Box {
private:
	double width;
public:
	Box(double w) : width(w) {}
	friend void printWidth(const Box& b);
};
void printWidth(const Box& b) {
	std::cout << "Width: " << b.width << std::endl;
}
```

## Friend Class Example
```cpp
class A {
	friend class B; // B can access A's private members
private:
	int secret;
};
```

## Best Practices
- Use friends sparingly
- Avoid breaking encapsulation
- Document why friendship is needed

---
This folder contains resources and examples related to friend functions and classes in C++.
