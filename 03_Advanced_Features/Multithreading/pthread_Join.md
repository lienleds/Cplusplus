# pthread_join in C++

## What is `pthread_join`?

`pthread_join` is a function provided by the POSIX threads (pthreads) library that allows one thread to wait for the termination of another thread. It is commonly used in C and C++ programs that utilize the pthreads library for multithreading.

## Why Use `pthread_join`?

- **Synchronization:** Ensures that the main thread (or another thread) waits for the specified thread to complete execution.
- **Resource Management:** Cleans up resources associated with the terminated thread.
- **Avoiding Detached Threads:** Prevents resource leaks by ensuring threads are properly joined.

## Syntax

```c
int pthread_join(pthread_t thread, void** retval);
```

- **Parameters:**
  - `thread`: The thread ID of the thread to wait for.
  - `retval`: A pointer to the return value of the terminated thread (can be `NULL` if not needed).
- **Return Value:**
  - Returns `0` on success.
  - On failure, returns an error code (e.g., `ESRCH`, `EINVAL`, `EDEADLK`).

## Example

```cpp
#include <iostream>
#include <pthread.h>

void* threadFunction(void* arg) {
    std::cout << "Thread is running..." << std::endl;
    return nullptr;
}

int main() {
    pthread_t thread;

    // Create a new thread
    if (pthread_create(&thread, nullptr, threadFunction, nullptr) != 0) {
        std::cerr << "Failed to create thread" << std::endl;
        return 1;
    }

    // Wait for the thread to finish
    if (pthread_join(thread, nullptr) != 0) {
        std::cerr << "Failed to join thread" << std::endl;
        return 1;
    }

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

1. **Blocking Call:** `pthread_join` blocks the calling thread until the specified thread terminates.
2. **Return Value:** The return value of the terminated thread can be retrieved using the `retval` parameter.
3. **Thread Joinable State:** Ensure the thread is in a joinable state before calling `pthread_join`.

## Common Pitfalls

- **Not Joining Threads:** Forgetting to join a thread can lead to resource leaks.
- **Joining Detached Threads:** Attempting to join a detached thread results in undefined behavior.
- **Deadlocks:** Avoid scenarios where two threads attempt to join each other, leading to a deadlock.

## Best Practices

- Always join threads unless they are explicitly detached.
- Check the return value of `pthread_join` to handle errors appropriately.
- Use synchronization mechanisms like mutexes to avoid race conditions.

## Additional Resources

- [POSIX Threads Programming](https://man7.org/linux/man-pages/man7/pthreads.7.html)
- [pthread_join Reference](https://man7.org/linux/man-pages/man3/pthread_join.3.html)