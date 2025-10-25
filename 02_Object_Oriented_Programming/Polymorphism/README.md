# Polymorphism in C++

Polymorphism allows objects to be treated as instances of their base type, enabling dynamic method calls.

## Types of Polymorphism
- **Compile-time (static)**: Function overloading, operator overloading
- **Run-time (dynamic)**: Virtual functions

## Example: Virtual Functions
```cpp
class Animal {
public:
	virtual void speak() { std::cout << "Animal sound"; }
};
class Dog : public Animal {
public:
	void speak() override { std::cout << "Woof!"; }
};
Animal* a = new Dog();
a->speak(); // Outputs: Woof!
```

## Best Practices
- Use virtual destructors in base classes.
- Prefer override keyword for clarity.
- Avoid excessive use of polymorphism for simple cases.

## Step-by-Step Guide to Understanding Polymorphism

1. **Understand the Concept:**
   - Polymorphism means "many forms." It allows a single interface to represent different underlying forms (data types or classes).
   - In C++, polymorphism enables objects to be treated as instances of their base type, allowing dynamic method calls.

2. **Learn the Types of Polymorphism:**
   - **Compile-time (Static) Polymorphism:** Achieved using function overloading and operator overloading.
   - **Run-time (Dynamic) Polymorphism:** Achieved using virtual functions and inheritance.

3. **Explore Compile-time Polymorphism:**
   - **Function Overloading:** Multiple functions with the same name but different parameter lists.
     ```cpp
     class Math {
     public:
         int add(int a, int b) { return a + b; }
         double add(double a, double b) { return a + b; }
     };
     ```
   - **Operator Overloading:** Overloading operators to work with user-defined types.
     ```cpp
     class Complex {
     private:
         double real, imag;
     public:
         Complex(double r, double i) : real(r), imag(i) {}
         Complex operator+(const Complex& other) {
             return Complex(real + other.real, imag + other.imag);
         }
     };
     ```

4. **Explore Run-time Polymorphism:**
   - **Virtual Functions:** Allow derived classes to override base class methods.
     ```cpp
     class Animal {
     public:
         virtual void speak() { std::cout << "Animal sound"; }
     };

     class Dog : public Animal {
     public:
         void speak() override { std::cout << "Woof!"; }
     };

     Animal* a = new Dog();
     a->speak(); // Outputs: Woof!
     ```
   - **Key Points:**
     - Use the `virtual` keyword in the base class method.
     - Use the `override` keyword in the derived class method for clarity.

5. **Understand the Role of Virtual Destructors:**
   - Always use virtual destructors in base classes to ensure proper cleanup of derived class objects.
     ```cpp
     class Base {
     public:
         virtual ~Base() { std::cout << "Base destructor"; }
     };

     class Derived : public Base {
     public:
         ~Derived() { std::cout << "Derived destructor"; }
     };

     Base* obj = new Derived();
     delete obj; // Calls both Base and Derived destructors
     ```

6. **Understand the Benefits of Polymorphism:**
   - Promotes code reuse by allowing a single interface to work with different types.
   - Simplifies code maintenance by centralizing common functionality in the base class.
   - Enables dynamic behavior at runtime.

7. **Practice Polymorphism:**
   - Create classes with virtual functions and override them in derived classes.
   - Experiment with function overloading and operator overloading.
   - Implement real-world examples to see how polymorphism simplifies code.

8. **Follow Best Practices:**
   - Use the `override` keyword in derived classes to avoid accidental method hiding.
   - Avoid excessive use of polymorphism for simple cases.
   - Use virtual destructors in base classes to ensure proper cleanup.

By following these steps, you can effectively understand and implement polymorphism in your C++ programs.

---
This folder contains resources and examples related to polymorphism in C++.
