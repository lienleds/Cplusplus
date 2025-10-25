# Multithreading in C++

This folder contains resources and examples related to multithreading in C++.

## Overview
Multithreading allows a program to execute multiple threads concurrently, enabling better utilization of CPU resources and improving performance for tasks that can be parallelized. It is particularly useful for computationally intensive tasks, real-time systems, and applications requiring responsiveness.

## Key Concepts

### 1. **Threads**
- A thread is the smallest unit of execution in a program.
- Threads share the same memory space but have their own stack.
- C++ provides the `<thread>` library to create and manage threads.
- Example:
  ```cpp
  #include <iostream>
  #include <thread>

  void printMessage() {
      std::cout << "Hello from thread!" << std::endl;
  }

  int main() {
      std::thread t(printMessage); // Create a thread
      t.join(); // Wait for the thread to finish
      return 0;
  }
  ```

### 2. **Thread Management**
- **`join`:** Waits for a thread to complete. The main thread blocks until the joined thread finishes execution.
- **`detach`:** Detaches a thread, allowing it to run independently. The main thread does not wait for the detached thread to finish.
- **Thread Lifecycle:**
  - A thread is created, executed, and then either joined or detached.
  - If neither `join` nor `detach` is called, the program may terminate with undefined behavior.
- Example:
  ```cpp
  std::thread t([]() {
      std::cout << "Detached thread" << std::endl;
  });
  t.detach();
  ```

### 3. **Synchronization**
- Synchronization ensures that multiple threads can access shared resources safely.
- **Mutex:** A mutual exclusion object that prevents multiple threads from accessing a resource simultaneously.
  - Example:
    ```cpp
    #include <iostream>
    #include <thread>
    #include <mutex>

    std::mutex mtx;

    void printSafeMessage(const std::string& message) {
        std::lock_guard<std::mutex> lock(mtx);
        std::cout << message << std::endl;
    }

    int main() {
        std::thread t1(printSafeMessage, "Thread 1");
        std::thread t2(printSafeMessage, "Thread 2");
        t1.join();
        t2.join();
        return 0;
    }
    ```
- **Deadlocks:**
  - Occur when two or more threads are waiting for each other to release resources.
  - Avoid deadlocks by acquiring locks in a consistent order.

### 4. **Condition Variables**
- Used to block a thread until a condition is met.
- Works in conjunction with a mutex to synchronize threads.
- Example:
  ```cpp
  #include <iostream>
  #include <thread>
  #include <mutex>
  #include <condition_variable>

  std::mutex mtx;
  std::condition_variable cv;
  bool ready = false;

  void worker() {
      std::unique_lock<std::mutex> lock(mtx);
      cv.wait(lock, [] { return ready; });
      std::cout << "Worker thread is running" << std::endl;
  }

  int main() {
      std::thread t(worker);
      {
          std::lock_guard<std::mutex> lock(mtx);
          ready = true;
      }
      cv.notify_one();
      t.join();
      return 0;
  }
  ```

### 5. **Thread Pools**
- A thread pool is a collection of pre-created threads that can be reused for multiple tasks.
- Thread pools improve performance by reducing the overhead of creating and destroying threads.
- Example:
  ```cpp
  #include <iostream>
  #include <vector>
  #include <thread>

  void task(int id) {
      std::cout << "Task " << id << " is running" << std::endl;
  }

  int main() {
      std::vector<std::thread> threads;
      for (int i = 0; i < 5; ++i) {
          threads.emplace_back(task, i);
      }
      for (auto& t : threads) {
          t.join();
      }
      return 0;
  }
  ```

### 6. **High-Level Concurrency Utilities**
- **`std::async`:** Launches a task asynchronously and returns a `std::future`.
  - Example:
    ```cpp
    #include <iostream>
    #include <future>

    int compute() {
        return 42;
    }

    int main() {
        std::future<int> result = std::async(compute);
        std::cout << "Result: " << result.get() << std::endl;
        return 0;
    }
    ```
- **`std::future` and `std::promise`:** Used for communication between threads.
  - Example:
    ```cpp
    #include <iostream>
    #include <thread>
    #include <future>

    void compute(std::promise<int> p) {
        p.set_value(42);
    }

    int main() {
        std::promise<int> p;
        std::future<int> f = p.get_future();
        std::thread t(compute, std::move(p));
        std::cout << "Result: " << f.get() << std::endl;
        t.join();
        return 0;
    }
    ```

### 7. **Thread Library Overview**

The C++ Standard Library provides the `<thread>` header, which includes classes and functions for creating and managing threads. Below are some key components:

#### **Classes**
- **`std::thread`:** Represents a single thread of execution.
- **`std::jthread` (C++20):** A RAII-style thread wrapper that automatically joins or stops the thread when it goes out of scope.
- **`std::mutex`:** A mutual exclusion object for synchronizing access to shared resources.
- **`std::recursive_mutex`:** A mutex that can be locked multiple times by the same thread.
- **`std::timed_mutex`:** A mutex that supports timed locking operations.
- **`std::shared_mutex` (C++17):** A mutex that allows multiple readers or one writer.

#### **Functions**
- **`std::this_thread::get_id`:** Returns the ID of the current thread.
- **`std::this_thread::sleep_for`:** Puts the current thread to sleep for a specified duration.
- **`std::this_thread::sleep_until`:** Puts the current thread to sleep until a specified time point.
- **`std::this_thread::yield`:** Suggests to the scheduler to allow other threads to run.

#### **Thread Identification**
- Each thread has a unique ID, which can be retrieved using `std::thread::get_id`.
- Example:
  ```cpp
  #include <iostream>
  #include <thread>

  void printThreadId() {
      std::cout << "Thread ID: " << std::this_thread::get_id() << std::endl;
  }

  int main() {
      std::thread t(printThreadId);
      t.join();
      return 0;
  }
  ```

#### **Thread Safety**
- The `<thread>` library provides tools to ensure thread safety, such as mutexes and condition variables.
- Always use synchronization primitives when accessing shared resources to avoid race conditions.

### 8. **Advanced Thread Management**

#### **Thread Priorities**
- The C++ Standard Library does not provide direct support for setting thread priorities.
- Thread priorities are platform-dependent and can be managed using platform-specific APIs (e.g., `pthread` on POSIX systems or Windows API).

#### **Thread Affinity**
- Thread affinity binds a thread to a specific CPU core.
- This is useful for performance optimization in real-time systems.
- Thread affinity is also platform-dependent and requires platform-specific APIs.

#### **Custom Thread Wrappers**
- Custom thread wrappers can be used to manage thread lifecycle and exceptions.
- Example:
  ```cpp
  #include <iostream>
  #include <thread>

  class ThreadWrapper {
  public:
      ThreadWrapper(std::thread t) : thread_(std::move(t)) {}
      ~ThreadWrapper() {
          if (thread_.joinable()) {
              thread_.join();
          }
      }

  private:
      std::thread thread_;
  };

  void task() {
      std::cout << "Task running" << std::endl;
  }

  int main() {
      ThreadWrapper tw(std::thread(task));
      return 0;
  }
  ```

## Thread Lifecycle Diagram

Below is a diagram representing the lifecycle of a thread in C++:

```
+-------------------+
|   Not Started     |
| (Thread Created)  |
+-------------------+
          |
          v
+-------------------+
|     Runnable      |
| (Ready to Run)    |
+-------------------+
          |
          v
+-------------------+
|     Running       |
| (Executing Code)  |
+-------------------+
     /      \
    v        v
+------+   +-------------------+
| Dead |   |     Blocked       |
|      |   | (Waiting for I/O,|
|      |   | Mutex, etc.)     |
+------+   +-------------------+
                |
                v
          +-------------------+
          |     Runnable      |
          | (Ready to Run)    |
          +-------------------+
```

### Explanation of Thread States
1. **Not Started:**
   - The thread object is created but not yet started.
   - Example: `std::thread t;` (before calling `t.join()` or `t.detach()`).

2. **Runnable:**
   - The thread is ready to run but is waiting for the CPU to schedule it.

3. **Running:**
   - The thread is actively executing code on the CPU.

4. **Blocked:**
   - The thread is waiting for a resource (e.g., I/O operation, mutex, or condition variable).
   - It transitions back to the Runnable state once the resource becomes available.

5. **Dead:**
   - The thread has finished execution and is terminated.
   - Example: After `t.join()` or `t.detach()` is called.

This lifecycle diagram helps visualize the states a thread can go through during its execution in a multithreaded C++ program.

## Best Practices
- Always join or detach threads to avoid undefined behavior.
- Use synchronization primitives like mutexes and condition variables to protect shared resources.
- Avoid deadlocks by acquiring locks in a consistent order.
- Prefer thread pools for managing a large number of short-lived tasks.
- Use high-level concurrency utilities like `std::async` and `std::future` for simpler thread management.
- Minimize the use of global variables to reduce the risk of race conditions.
- Profile and test multithreaded code thoroughly to identify performance bottlenecks and synchronization issues.

By mastering multithreading, you can write efficient and responsive C++ programs that take full advantage of modern multi-core processors.