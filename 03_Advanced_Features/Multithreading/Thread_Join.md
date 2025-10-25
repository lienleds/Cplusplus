# Thread Join in C++

## What is `std::thread::join`?

`std::thread::join` is a method in the C++ Standard Library that allows the main thread (or the thread that created another thread) to wait for the completion of the newly created thread. It ensures that the thread has finished executing before the program continues.

## Why Use `join`?

- **Synchronization:** Ensures that the main thread waits for the child thread to complete.
- **Resource Management:** Prevents undefined behavior by ensuring the thread's resources are properly cleaned up.
- **Avoiding Detached Threads:** Threads that are not joined or detached will cause the program to terminate with an exception.

## Syntax

```cpp
void join();
```

## Example

```cpp
#include <iostream>
#include <thread>

void threadFunction() {
    std::cout << "Thread is running..." << std::endl;
}

int main() {
    std::thread t(threadFunction);

    // Wait for the thread to finish
    t.join();

    std::cout << "Thread has finished." << std::endl;
    return 0;
}
```

### Output
```
Thread is running...
Thread has finished.
```

## Key Points

1. **Blocking Call:** `join` blocks the calling thread until the thread it is called on finishes execution.
2. **One-Time Use:** A thread can only be joined once. Attempting to join a thread multiple times will result in an exception.
3. **Thread Joinable State:** Before calling `join`, ensure the thread is in a joinable state using `std::thread::joinable()`.

## Common Pitfalls

- **Not Joining Threads:** Forgetting to join a thread can lead to resource leaks or undefined behavior.
- **Joining Non-Joinable Threads:** Always check if a thread is joinable before calling `join`.

## Best Practices

- Always ensure threads are joined or detached before the program exits.
- Use RAII wrappers like `std::jthread` (C++20) or custom thread wrappers to manage thread lifecycle automatically.

## Additional Resources

- [C++ Reference: std::thread::join](https://en.cppreference.com/w/cpp/thread/thread/join)
- [Multithreading in C++](https://en.cppreference.com/w/cpp/thread)