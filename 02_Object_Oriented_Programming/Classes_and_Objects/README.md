# Classes and Objects in C++

Classes are user-defined types that represent real-world entities. Objects are instances of classes.

## Defining a Class

```cpp
class Car {
public:
	std::string brand;
	int year;
	void honk() {
		std::cout << "Beep!" << std::endl;
	}
};
```

## Creating Objects

```cpp
Car myCar;
myCar.brand = "Toyota";
myCar.year = 2020;
myCar.honk();
```

## Access Specifiers
- `public`: Accessible from outside the class
- `private`: Accessible only within the class
- `protected`: Accessible in derived classes

## Constructors and Destructors
- Constructor: Initializes objects
- Destructor: Cleans up resources

## Best Practices
- Use classes to model real-world entities.
- Keep data members private and provide public methods for access.
- Use constructors for initialization.

## Best Practices for Declaring a Class

1. **Use Meaningful Names:**
   - Choose class names that clearly represent the entity being modeled.
   - Example:
     ```cpp
     class Car; // Represents a car
     class Employee; // Represents an employee
     ```

2. **Keep Data Members Private:**
   - Use private access specifiers for data members to enforce encapsulation.
   - Provide public getter and setter methods for controlled access.
   - Example:
     ```cpp
     class Car {
     private:
         std::string brand;
     public:
         void setBrand(const std::string& b) { brand = b; }
         std::string getBrand() const { return brand; }
     };
     ```

3. **Group Related Members Together:**
   - Organize data members and methods logically to improve readability.
   - Example:
     ```cpp
     class Car {
     private:
         std::string brand;
         int year;
     public:
         void setBrand(const std::string& b) { brand = b; }
         std::string getBrand() const { return brand; }
         void setYear(int y) { year = y; }
         int getYear() const { return year; }
     };
     ```

4. **Use Constructors for Initialization:**
   - Define constructors to initialize objects with default or specific values.
   - Example:
     ```cpp
     class Car {
     private:
         std::string brand;
         int year;
     public:
         Car(const std::string& b, int y) : brand(b), year(y) {}
     };
     ```

5. **Avoid Using Public Data Members:**
   - Public data members break encapsulation and make the class harder to maintain.
   - Always use private or protected members with public methods for access.

6. **Provide a Destructor if Necessary:**
   - If the class allocates dynamic memory or other resources, define a destructor to release them.
   - Example:
     ```cpp
     class Car {
     private:
         std::string* brand;
     public:
         Car(const std::string& b) { brand = new std::string(b); }
         ~Car() { delete brand; }
     };
     ```

7. **Use Inline Methods Sparingly:**
   - Define simple methods inline, but keep complex methods in the `.cpp` file to improve readability.
   - Example:
     ```cpp
     class Car {
     public:
         void honk() { std::cout << "Beep!" << std::endl; } // Inline method
     };
     ```

8. **Document the Class:**
   - Add comments to describe the purpose of the class and its members.
   - Example:
     ```cpp
     // Represents a car with a brand and year
     class Car {
         // ...
     };
     ```

---
This folder contains resources and examples related to C++ classes and objects.
