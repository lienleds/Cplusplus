# Inheritance in C++

Inheritance allows a class to acquire properties and behaviors from another class.

## Types of Inheritance
- **Single Inheritance**: One base class
- **Multiple Inheritance**: More than one base class
- **Multilevel Inheritance**: Inheritance chain
- **Hierarchical Inheritance**: One base, multiple derived

## Example

```cpp
class Animal {
public:
	void eat() { std::cout << "Eating"; }
};

class Dog : public Animal {
public:
	void bark() { std::cout << "Barking"; }
};

Dog d;
d.eat(); // Inherited
d.bark(); // Own method
```

## Access Specifiers in Inheritance
- `public`, `protected`, `private` inheritance

## Best Practices
- Use inheritance for "is-a" relationships.
- Avoid deep inheritance hierarchies.
- Prefer composition over inheritance when possible.

## Step-by-Step Guide to Understanding Inheritance

1. **Understand the Concept:**
   - Inheritance is a mechanism where one class (derived class) acquires the properties and behaviors of another class (base class).
   - It promotes code reuse and establishes a relationship between classes.

2. **Learn the Syntax:**
   - Use the `:` symbol to specify inheritance.
   - Example:
     ```cpp
     class Base {
     public:
         void display() { std::cout << "Base class"; }
     };

     class Derived : public Base {
     public:
         void show() { std::cout << "Derived class"; }
     };
     ```

3. **Explore Types of Inheritance:**
   - **Single Inheritance:** One base class and one derived class.
     ```cpp
     class Animal {
     public:
         void eat() { std::cout << "Eating"; }
     };

     class Dog : public Animal {
     public:
         void bark() { std::cout << "Barking"; }
     };
     ```
   - **Multiple Inheritance:** A derived class inherits from more than one base class.
     ```cpp
     class A {
     public:
         void funcA() { std::cout << "Class A"; }
     };

     class B {
     public:
         void funcB() { std::cout << "Class B"; }
     };

     class C : public A, public B {
     public:
         void funcC() { std::cout << "Class C"; }
     };
     ```
   - **Multilevel Inheritance:** A chain of inheritance.
     ```cpp
     class A {
     public:
         void funcA() { std::cout << "Class A"; }
     };

     class B : public A {
     public:
         void funcB() { std::cout << "Class B"; }
     };

     class C : public B {
     public:
         void funcC() { std::cout << "Class C"; }
     };
     ```
   - **Hierarchical Inheritance:** Multiple derived classes inherit from a single base class.
     ```cpp
     class Animal {
     public:
         void eat() { std::cout << "Eating"; }
     };

     class Dog : public Animal {
     public:
         void bark() { std::cout << "Barking"; }
     };

     class Cat : public Animal {
     public:
         void meow() { std::cout << "Meowing"; }
     };
     ```

4. **Understand Access Specifiers in Inheritance:**
   - **Public Inheritance:** Public and protected members of the base class remain public and protected in the derived class.
   - **Protected Inheritance:** Public and protected members of the base class become protected in the derived class.
   - **Private Inheritance:** Public and protected members of the base class become private in the derived class.
   - Example:
     ```cpp
     class Base {
     protected:
         int value;
     };

     class Derived : private Base {
     public:
         void setValue(int val) { value = val; }
     };
     ```

5. **Understand the Benefits of Inheritance:**
   - Promotes code reuse by allowing derived classes to use base class functionality.
   - Simplifies code maintenance by centralizing common functionality in the base class.
   - Establishes a natural hierarchy between classes.

6. **Practice Inheritance:**
   - Create classes with different inheritance types.
   - Experiment with access specifiers to understand their impact.
   - Implement real-world examples to see how inheritance simplifies code.

7. **Follow Best Practices:**
   - Use inheritance only for "is-a" relationships (e.g., a Dog "is a" type of Animal).
   - Avoid deep inheritance hierarchies as they can make the code harder to understand and maintain.
   - Prefer composition over inheritance when appropriate.

By following these steps, you can effectively understand and implement inheritance in your C++ programs.

### Diamond Inheritance

Diamond inheritance occurs when a derived class inherits from two classes that have a common base class. This can lead to ambiguity in accessing the base class members.

#### Example:
```cpp
class A {
public:
    void display() { std::cout << "Class A"; }
};

class B : public A {};

class C : public A {};

class D : public B, public C {};

int main() {
    D obj;
    // obj.display(); // Error: Ambiguity in accessing 'display' from Class A
    return 0;
}
```

#### Resolving Diamond Inheritance:
- Use `virtual` inheritance to ensure that only one instance of the base class is shared among all derived classes.

#### Example with Virtual Inheritance:
```cpp
class A {
public:
    void display() { std::cout << "Class A"; }
};

class B : virtual public A {};

class C : virtual public A {};

class D : public B, public C {};

int main() {
    D obj;
    obj.display(); // No ambiguity
    return 0;
}
```

#### Key Points:
- Virtual inheritance ensures that only one instance of the base class exists in the inheritance hierarchy.
- It resolves ambiguity and prevents redundant copies of the base class members.

---
This folder contains resources and examples related to inheritance in C++.
