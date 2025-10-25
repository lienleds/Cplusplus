# Design Patterns

Design patterns are reusable solutions to common software design problems. They provide a standard approach to solving recurring challenges, improving code readability, maintainability, and scalability.

---

## Categories of Design Patterns
Design patterns are broadly categorized into three types:

### 1. Creational Patterns
- **Purpose:** Deal with object creation mechanisms.
- **Examples:**
  - Singleton
  - Factory Method
  - Abstract Factory
  - Builder
  - Prototype

### 2. Structural Patterns
- **Purpose:** Deal with object composition and relationships.
- **Examples:**
  - Adapter
  - Bridge
  - Composite
  - Decorator
  - Facade
  - Flyweight
  - Proxy

### 3. Behavioral Patterns
- **Purpose:** Deal with object interaction and responsibility.
- **Examples:**
  - Chain of Responsibility
  - Command
  - Interpreter
  - Iterator
  - Mediator
  - Memento
  - Observer
  - State
  - Strategy
  - Template Method
  - Visitor

---

## Commonly Used Design Patterns

### 1. Singleton Pattern
- **Purpose:** Ensure a class has only one instance and provide a global point of access to it.
- **Example:**
```cpp
#include <iostream>

class Singleton {
private:
    static Singleton* instance;
    Singleton() {}

public:
    static Singleton* getInstance() {
        if (!instance) {
            instance = new Singleton();
        }
        return instance;
    }

    void showMessage() {
        std::cout << "Singleton instance" << std::endl;
    }
};

Singleton* Singleton::instance = nullptr;

int main() {
    Singleton* s = Singleton::getInstance();
    s->showMessage();
    return 0;
}
```

### 2. Factory Method Pattern
- **Purpose:** Define an interface for creating objects but let subclasses decide which class to instantiate.
- **Example:**
```cpp
#include <iostream>
#include <memory>

class Product {
public:
    virtual void use() = 0;
};

class ConcreteProductA : public Product {
public:
    void use() override {
        std::cout << "Using Product A" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void use() override {
        std::cout << "Using Product B" << std::endl;
    }
};

class Factory {
public:
    virtual std::unique_ptr<Product> createProduct() = 0;
};

class FactoryA : public Factory {
public:
    std::unique_ptr<Product> createProduct() override {
        return std::make_unique<ConcreteProductA>();
    }
};

class FactoryB : public Factory {
public:
    std::unique_ptr<Product> createProduct() override {
        return std::make_unique<ConcreteProductB>();
    }
};

int main() {
    FactoryA factoryA;
    auto productA = factoryA.createProduct();
    productA->use();

    FactoryB factoryB;
    auto productB = factoryB.createProduct();
    productB->use();

    return 0;
}
```

### 3. Observer Pattern
- **Purpose:** Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.
- **Example:**
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Observer {
public:
    virtual void update(int value) = 0;
};

class Subject {
private:
    std::vector<Observer*> observers;
    int state;

public:
    void attach(Observer* observer) {
        observers.push_back(observer);
    }

    void detach(Observer* observer) {
        observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
    }

    void notify() {
        for (auto observer : observers) {
            observer->update(state);
        }
    }

    void setState(int value) {
        state = value;
        notify();
    }
};

class ConcreteObserver : public Observer {
private:
    std::string name;

public:
    ConcreteObserver(const std::string& name) : name(name) {}

    void update(int value) override {
        std::cout << name << " received update: " << value << std::endl;
    }
};

int main() {
    Subject subject;

    ConcreteObserver observer1("Observer 1");
    ConcreteObserver observer2("Observer 2");

    subject.attach(&observer1);
    subject.attach(&observer2);

    subject.setState(42);

    return 0;
}
```

---

## Analysis
- **Advantages of Design Patterns:**
  - Provide proven solutions to common problems.
  - Improve code readability and maintainability.
  - Facilitate communication among developers.
- **Challenges:**
  - May introduce unnecessary complexity.
  - Require understanding of the pattern to use effectively.

---

## Best Practices
- Use design patterns judiciously; avoid overengineering.
- Choose patterns that fit the problem context.
- Document the use of patterns to aid future developers.

---

## Summary
Design patterns are essential tools for software design, offering reusable solutions to common problems. By understanding and applying these patterns, developers can create robust, maintainable, and scalable software systems.

---

### References
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns)
- [Refactoring.Guru: Design Patterns](https://refactoring.guru/design-patterns)