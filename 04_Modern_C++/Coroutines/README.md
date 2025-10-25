# Coroutines in Modern C++

Coroutines are a powerful feature introduced in C++20 that enable cooperative multitasking by allowing functions to be paused and resumed. This README provides an overview of coroutines, their syntax, and practical examples.

---

## What are Coroutines?

- Coroutines are special functions that can suspend execution and later resume from the point where they were suspended.
- They are useful for tasks like asynchronous programming, generators, and cooperative multitasking.
- Coroutines do not require threads; they operate within a single thread.

---

## Key Concepts

### Coroutine State
- A coroutine maintains its state between suspensions.
- The state includes local variables, the program counter, and other execution context.

### Coroutine Handles
- Coroutines are managed using `std::coroutine_handle`.
- The handle provides control over the coroutine's lifecycle.

### Awaitables
- Awaitables are objects that define how a coroutine suspends and resumes.
- They implement the `await_ready`, `await_suspend`, and `await_resume` methods.

---

## Syntax

### Basic Coroutine
```cpp
#include <coroutine>
#include <iostream>

struct MyCoroutine {
    struct promise_type {
        MyCoroutine get_return_object() { return {}; }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_never final_suspend() noexcept { return {}; }
        void return_void() {}
        void unhandled_exception() {}
    };
};

MyCoroutine myCoroutine() {
    std::cout << "Hello, ";
    co_await std::suspend_always{};
    std::cout << "World!\n";
}

int main() {
    auto coro = myCoroutine();
}
```

### Awaitable Example
```cpp
#include <coroutine>
#include <iostream>

struct Awaitable {
    bool await_ready() { return false; }
    void await_suspend(std::coroutine_handle<>) {}
    void await_resume() { std::cout << "Resumed!\n"; }
};

struct MyCoroutine {
    struct promise_type {
        MyCoroutine get_return_object() { return {}; }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_never final_suspend() noexcept { return {}; }
        void return_void() {}
        void unhandled_exception() {}
    };
};

MyCoroutine myCoroutine() {
    co_await Awaitable{};
}

int main() {
    auto coro = myCoroutine();
}
```

---

## Use Cases

1. **Asynchronous Programming**
   - Coroutines simplify asynchronous code by eliminating the need for callbacks.

2. **Generators**
   - Coroutines can produce a sequence of values lazily.

3. **State Machines**
   - Coroutines can represent state machines in a natural way.

---

## Examples

### Example 1: Asynchronous Task
```cpp
#include <coroutine>
#include <iostream>
#include <thread>

struct AsyncTask {
    struct promise_type {
        AsyncTask get_return_object() { return {}; }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_never final_suspend() noexcept { return {}; }
        void return_void() {}
        void unhandled_exception() {}
    };
};

AsyncTask asyncTask() {
    std::cout << "Task started\n";
    co_await std::suspend_always{};
    std::cout << "Task resumed\n";
}

int main() {
    auto task = asyncTask();
}
```

### Example 2: Generator
```cpp
#include <coroutine>
#include <iostream>
#include <vector>

struct Generator {
    struct promise_type {
        int current_value;
        Generator get_return_object() { return Generator{this}; }
        std::suspend_always initial_suspend() { return {}; }
        std::suspend_always final_suspend() noexcept { return {}; }
        std::suspend_always yield_value(int value) {
            current_value = value;
            return {};
        }
        void return_void() {}
        void unhandled_exception() {}
    };

    struct iterator {
        std::coroutine_handle<promise_type> handle;
        bool operator!=(std::default_sentinel_t) const { return !handle.done(); }
        void operator++() { handle.resume(); }
        int operator*() const { return handle.promise().current_value; }
    };

    iterator begin() { return iterator{handle}; }
    std::default_sentinel_t end() { return {}; }

private:
    explicit Generator(promise_type* p) : handle(std::coroutine_handle<promise_type>::from_promise(*p)) {}
    std::coroutine_handle<promise_type> handle;
};

Generator generateNumbers() {
    for (int i = 0; i < 5; ++i) {
        co_yield i;
    }
}

int main() {
    for (int value : generateNumbers()) {
        std::cout << value << " ";
    }
}
```

---

## Best Practices

- Use coroutines for tasks that benefit from suspension and resumption.
- Avoid using coroutines for simple tasks that can be achieved with regular functions.
- Manage coroutine handles carefully to avoid resource leaks.

---

## Common Pitfalls

1. **Resource Management**
   - Ensure that coroutine handles are properly destroyed to avoid memory leaks.

2. **Complexity**
   - Coroutines can introduce complexity; use them judiciously.

---

## Conclusion

Coroutines are a powerful addition to modern C++ that enable efficient and expressive asynchronous programming. By understanding their syntax and use cases, developers can leverage coroutines to write cleaner and more maintainable code.