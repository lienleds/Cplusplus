# Concurrency Utilities in C++

Concurrency utilities provide low-level primitives for synchronization and atomic operations beyond basic multithreading.

---

## Overview

Modern C++ provides powerful concurrency tools:
- **`std::atomic`** - Lock-free atomic operations
- **Memory ordering** - Control over memory visibility
- **`std::condition_variable`** - Thread synchronization
- **Lock-free programming** - High-performance concurrent data structures

---

## 1. std::atomic

### Basic Atomic Operations

```cpp
#include <iostream>
#include <atomic>
#include <thread>
#include <vector>

std::atomic<int> counter{0};

void increment(int iterations) {
    for (int i = 0; i < iterations; ++i) {
        counter.fetch_add(1, std::memory_order_relaxed);
    }
}

int main() {
    const int num_threads = 10;
    const int iterations = 10000;
    
    std::vector<std::thread> threads;
    
    for (int i = 0; i < num_threads; ++i) {
        threads.emplace_back(increment, iterations);
    }
    
    for (auto& t : threads) {
        t.join();
    }
    
    std::cout << "Final counter value: " << counter.load() << '\n';
    std::cout << "Expected: " << (num_threads * iterations) << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Thread-safe without locks
- ✅ No data races
- ✅ Better performance than mutex for simple operations
- ⚠️ Limited to simple types

### Atomic Operations

```cpp
#include <atomic>
#include <iostream>

int main() {
    std::atomic<int> value{10};
    
    // Load
    int current = value.load(std::memory_order_acquire);
    std::cout << "Current value: " << current << '\n';
    
    // Store
    value.store(20, std::memory_order_release);
    
    // Exchange
    int old = value.exchange(30, std::memory_order_acq_rel);
    std::cout << "Old value: " << old << '\n';
    
    // Compare-and-swap
    int expected = 30;
    bool success = value.compare_exchange_strong(
        expected, 40, std::memory_order_seq_cst
    );
    
    if (success) {
        std::cout << "CAS succeeded, new value: " << value.load() << '\n';
    } else {
        std::cout << "CAS failed, expected was: " << expected << '\n';
    }
    
    // Fetch and add
    int previous = value.fetch_add(5, std::memory_order_relaxed);
    std::cout << "Previous: " << previous << ", Current: " << value.load() << '\n';
    
    return 0;
}
```

---

## 2. Memory Ordering

### Memory Order Types

```cpp
#include <atomic>
#include <thread>
#include <iostream>

// Relaxed ordering - no synchronization
std::atomic<int> x{0}, y{0};
std::atomic<int> r1{0}, r2{0};

void thread1_relaxed() {
    x.store(1, std::memory_order_relaxed);
    r1.store(y.load(std::memory_order_relaxed), std::memory_order_relaxed);
}

void thread2_relaxed() {
    y.store(1, std::memory_order_relaxed);
    r2.store(x.load(std::memory_order_relaxed), std::memory_order_relaxed);
}

// Acquire-Release ordering
std::atomic<bool> ready{false};
int data = 0;

void producer() {
    data = 42; // Non-atomic write
    ready.store(true, std::memory_order_release); // Release
}

void consumer() {
    while (!ready.load(std::memory_order_acquire)) { // Acquire
        // Wait
    }
    std::cout << "Data: " << data << '\n'; // Safe to read
}

// Sequential consistency (strongest)
std::atomic<int> seq_x{0}, seq_y{0};
std::atomic<int> seq_r1{0}, seq_r2{0};

void thread1_seq() {
    seq_x.store(1, std::memory_order_seq_cst);
    seq_r1.store(seq_y.load(std::memory_order_seq_cst), std::memory_order_seq_cst);
}

void thread2_seq() {
    seq_y.store(1, std::memory_order_seq_cst);
    seq_r2.store(seq_x.load(std::memory_order_seq_cst), std::memory_order_seq_cst);
}

int main() {
    // Example with acquire-release
    std::thread t1(producer);
    std::thread t2(consumer);
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Memory Order Comparison:**

| Memory Order | Guarantees | Performance | Use Case |
|--------------|------------|-------------|----------|
| `relaxed` | None | Fastest | Counters, flags (when order doesn't matter) |
| `acquire` | Prevents reordering after | Fast | Loading shared data |
| `release` | Prevents reordering before | Fast | Publishing shared data |
| `acq_rel` | Both acquire and release | Moderate | Read-modify-write operations |
| `seq_cst` | Total order across all threads | Slowest | Default, when order matters globally |

---

## 3. std::condition_variable

### Producer-Consumer Pattern

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <chrono>

std::queue<int> dataQueue;
std::mutex mtx;
std::condition_variable cv;
bool finished = false;

void producer(int id, int count) {
    for (int i = 0; i < count; ++i) {
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
        
        {
            std::lock_guard<std::mutex> lock(mtx);
            dataQueue.push(id * 1000 + i);
            std::cout << "Producer " << id << " produced: " << (id * 1000 + i) << '\n';
        }
        
        cv.notify_one(); // Notify one waiting consumer
    }
}

void consumer(int id) {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        
        // Wait until data is available or finished
        cv.wait(lock, []{ return !dataQueue.empty() || finished; });
        
        if (dataQueue.empty() && finished) {
            break; // No more data and production finished
        }
        
        if (!dataQueue.empty()) {
            int value = dataQueue.front();
            dataQueue.pop();
            lock.unlock();
            
            std::cout << "Consumer " << id << " consumed: " << value << '\n';
        }
    }
}

int main() {
    std::thread prod1(producer, 1, 5);
    std::thread prod2(producer, 2, 5);
    std::thread cons1(consumer, 1);
    std::thread cons2(consumer, 2);
    
    prod1.join();
    prod2.join();
    
    {
        std::lock_guard<std::mutex> lock(mtx);
        finished = true;
    }
    cv.notify_all(); // Wake up all consumers
    
    cons1.join();
    cons2.join();
    
    return 0;
}
```

**Analysis:**
- ✅ Efficient thread synchronization
- ✅ Avoids busy-waiting
- ✅ Supports multiple producers and consumers
- ⚠️ Requires careful mutex management

### Timed Waiting

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <chrono>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void waiter() {
    std::unique_lock<std::mutex> lock(mtx);
    
    // Wait for 2 seconds or until ready
    if (cv.wait_for(lock, std::chrono::seconds(2), []{ return ready; })) {
        std::cout << "Condition met!\n";
    } else {
        std::cout << "Timeout!\n";
    }
}

int main() {
    std::thread t(waiter);
    
    std::this_thread::sleep_for(std::chrono::seconds(1));
    
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();
    
    t.join();
    return 0;
}
```

---

## 4. Lock-Free Programming

### Lock-Free Stack

```cpp
#include <atomic>
#include <memory>
#include <iostream>

template<typename T>
class LockFreeStack {
private:
    struct Node {
        T data;
        Node* next;
        
        Node(const T& value) : data(value), next(nullptr) {}
    };
    
    std::atomic<Node*> head{nullptr};
    
public:
    void push(const T& value) {
        Node* newNode = new Node(value);
        newNode->next = head.load(std::memory_order_relaxed);
        
        // Compare-and-swap loop
        while (!head.compare_exchange_weak(
            newNode->next, 
            newNode,
            std::memory_order_release,
            std::memory_order_relaxed
        )) {
            // Retry if CAS failed
        }
    }
    
    bool pop(T& result) {
        Node* oldHead = head.load(std::memory_order_relaxed);
        
        while (oldHead && !head.compare_exchange_weak(
            oldHead,
            oldHead->next,
            std::memory_order_acquire,
            std::memory_order_relaxed
        )) {
            // Retry if CAS failed
        }
        
        if (oldHead) {
            result = oldHead->data;
            delete oldHead; // Note: This has ABA problem in production
            return true;
        }
        
        return false;
    }
    
    ~LockFreeStack() {
        T dummy;
        while (pop(dummy)) {}
    }
};

int main() {
    LockFreeStack<int> stack;
    
    stack.push(1);
    stack.push(2);
    stack.push(3);
    
    int value;
    while (stack.pop(value)) {
        std::cout << "Popped: " << value << '\n';
    }
    
    return 0;
}
```

**Analysis:**
- ✅ No locks, high throughput
- ✅ No deadlocks
- ❌ Complex to implement correctly
- ❌ ABA problem (needs careful handling)

---

## 5. Atomic Flags

### Simple Spinlock

```cpp
#include <atomic>
#include <thread>
#include <iostream>
#include <vector>

class Spinlock {
    std::atomic_flag flag = ATOMIC_FLAG_INIT;
    
public:
    void lock() {
        while (flag.test_and_set(std::memory_order_acquire)) {
            // Spin
        }
    }
    
    void unlock() {
        flag.clear(std::memory_order_release);
    }
};

Spinlock spinlock;
int sharedData = 0;

void incrementWithSpinlock(int iterations) {
    for (int i = 0; i < iterations; ++i) {
        spinlock.lock();
        ++sharedData;
        spinlock.unlock();
    }
}

int main() {
    std::vector<std::thread> threads;
    
    for (int i = 0; i < 10; ++i) {
        threads.emplace_back(incrementWithSpinlock, 10000);
    }
    
    for (auto& t : threads) {
        t.join();
    }
    
    std::cout << "Final value: " << sharedData << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Choose the Right Tool:**
   - Use `std::atomic` for simple shared variables
   - Use `std::mutex` for complex critical sections
   - Use `std::condition_variable` for thread coordination

2. **Memory Ordering:**
   - Default to `seq_cst` unless you understand the implications
   - Use `relaxed` only for counters where order doesn't matter
   - Use `acquire/release` for producer-consumer patterns

3. **Avoid Spurious Wakeups:**
   - Always use a predicate with `condition_variable::wait`

4. **Lock-Free Algorithms:**
   - Only use when absolutely necessary
   - Thoroughly test for race conditions
   - Be aware of ABA problem

---

## Performance Comparison

| Primitive | Overhead | Scalability | Complexity |
|-----------|----------|-------------|------------|
| `std::atomic` (relaxed) | Very Low | Excellent | Medium |
| `std::atomic` (seq_cst) | Low | Good | Medium |
| `std::mutex` | Medium | Good | Low |
| `std::condition_variable` | Medium | Excellent | Medium |
| Lock-free structures | Very Low | Excellent | Very High |

---

## Summary

Concurrency utilities provide fine-grained control over thread synchronization:
- Use **atomics** for lock-free operations on simple types
- Use **memory ordering** to optimize performance
- Use **condition variables** for efficient thread coordination
- Consider **lock-free structures** only for high-performance requirements

---

### References
- [std::atomic](https://en.cppreference.com/w/cpp/atomic/atomic)
- [Memory Order](https://en.cppreference.com/w/cpp/atomic/memory_order)
- [std::condition_variable](https://en.cppreference.com/w/cpp/thread/condition_variable)
- [C++ Concurrency in Action](https://www.manning.com/books/c-plus-plus-concurrency-in-action-second-edition)