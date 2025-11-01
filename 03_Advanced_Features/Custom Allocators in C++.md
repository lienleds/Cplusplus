# Custom Allocators in C++

Custom allocators provide control over memory allocation strategies for STL containers and other classes.

---

## Overview

Custom allocators allow you to:
- **Control memory allocation** - Use custom memory pools
- **Optimize performance** - Reduce allocations/deallocations
- **Track memory usage** - Debug and profile
- **Use special memory** - Shared memory, GPU memory, etc.

---

## 1. Basic Allocator Concept

### Minimal Allocator Interface

```cpp
#include <iostream>
#include <memory>
#include <vector>

template<typename T>
class MinimalAllocator {
public:
    using value_type = T;
    
    MinimalAllocator() = default;
    
    template<typename U>
    MinimalAllocator(const MinimalAllocator<U>&) {}
    
    T* allocate(std::size_t n) {
        std::cout << "Allocating " << n << " objects of size " 
                  << sizeof(T) << '\n';
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }
    
    void deallocate(T* p, std::size_t n) {
        std::cout << "Deallocating " << n << " objects\n";
        ::operator delete(p);
    }
};

template<typename T, typename U>
bool operator==(const MinimalAllocator<T>&, const MinimalAllocator<U>&) {
    return true;
}

template<typename T, typename U>
bool operator!=(const MinimalAllocator<T>&, const MinimalAllocator<U>&) {
    return false;
}

int main() {
    std::vector<int, MinimalAllocator<int>> vec;
    
    vec.push_back(1);
    vec.push_back(2);
    vec.push_back(3);
    
    std::cout << "Vector contents: ";
    for (int val : vec) {
        std::cout << val << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Basic allocator implementation
- ✅ Works with STL containers
- 📝 Must define `allocate()` and `deallocate()`
- 💡 Equality operators required for compatibility

---

## 2. Stateful Allocator

### Allocator with State

```cpp
#include <iostream>
#include <memory>
#include <vector>

template<typename T>
class StatefulAllocator {
    int* allocation_count_;
    
public:
    using value_type = T;
    
    explicit StatefulAllocator(int* count) 
        : allocation_count_(count) {}
    
    template<typename U>
    StatefulAllocator(const StatefulAllocator<U>& other)
        : allocation_count_(other.allocation_count_) {}
    
    T* allocate(std::size_t n) {
        ++(*allocation_count_);
        std::cout << "Allocation #" << *allocation_count_ 
                  << ": " << n << " objects\n";
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }
    
    void deallocate(T* p, std::size_t n) {
        std::cout << "Deallocating " << n << " objects\n";
        ::operator delete(p);
    }
    
    template<typename U>
    friend class StatefulAllocator;
};

template<typename T, typename U>
bool operator==(const StatefulAllocator<T>& a, const StatefulAllocator<U>& b) {
    return a.allocation_count_ == b.allocation_count_;
}

template<typename T, typename U>
bool operator!=(const StatefulAllocator<T>& a, const StatefulAllocator<U>& b) {
    return !(a == b);
}

int main() {
    int count = 0;
    StatefulAllocator<int> alloc(&count);
    
    std::vector<int, StatefulAllocator<int>> vec(alloc);
    
    for (int i = 0; i < 10; ++i) {
        vec.push_back(i);
    }
    
    std::cout << "Total allocations: " << count << '\n';
    
    return 0;
}
```

---

## 3. Memory Pool Allocator

### Simple Pool Allocator

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <cstddef>

template<typename T, std::size_t PoolSize = 1024>
class PoolAllocator {
    struct alignas(T) Block {
        char data[sizeof(T)];
    };
    
    Block pool_[PoolSize];
    bool used_[PoolSize] = {};
    
public:
    using value_type = T;
    
    PoolAllocator() = default;
    
    template<typename U>
    PoolAllocator(const PoolAllocator<U, PoolSize>&) {}
    
    T* allocate(std::size_t n) {
        if (n > PoolSize) {
            throw std::bad_alloc();
        }
        
        // Find contiguous free blocks
        for (std::size_t i = 0; i <= PoolSize - n; ++i) {
            bool available = true;
            for (std::size_t j = 0; j < n; ++j) {
                if (used_[i + j]) {
                    available = false;
                    break;
                }
            }
            
            if (available) {
                for (std::size_t j = 0; j < n; ++j) {
                    used_[i + j] = true;
                }
                std::cout << "Allocated " << n << " blocks at index " << i << '\n';
                return reinterpret_cast<T*>(&pool_[i]);
            }
        }
        
        throw std::bad_alloc();
    }
    
    void deallocate(T* p, std::size_t n) {
        std::size_t index = reinterpret_cast<Block*>(p) - pool_;
        std::cout << "Deallocating " << n << " blocks at index " << index << '\n';
        
        for (std::size_t i = 0; i < n; ++i) {
            used_[index + i] = false;
        }
    }
};

template<typename T, typename U, std::size_t N>
bool operator==(const PoolAllocator<T, N>&, const PoolAllocator<U, N>&) {
    return true;
}

template<typename T, typename U, std::size_t N>
bool operator!=(const PoolAllocator<T, N>&, const PoolAllocator<U, N>&) {
    return false;
}

int main() {
    PoolAllocator<int, 100> alloc;
    std::vector<int, PoolAllocator<int, 100>> vec(alloc);
    
    for (int i = 0; i < 10; ++i) {
        vec.push_back(i);
    }
    
    std::cout << "Vector size: " << vec.size() << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ Pre-allocated memory pool
- ✅ Fast allocation (no system calls)
- ✅ Reduced fragmentation
- ❌ Fixed size limit
- 💡 Great for objects with similar lifetimes

---

## 4. Arena Allocator

### Monotonic Arena Allocator

```cpp
#include <iostream>
#include <vector>
#include <memory>

class Arena {
    char* buffer_;
    std::size_t size_;
    std::size_t offset_ = 0;
    
public:
    Arena(std::size_t size) 
        : buffer_(new char[size]), size_(size) {}
    
    ~Arena() {
        delete[] buffer_;
    }
    
    Arena(const Arena&) = delete;
    Arena& operator=(const Arena&) = delete;
    
    void* allocate(std::size_t n, std::size_t alignment = alignof(std::max_align_t)) {
        // Align offset
        std::size_t aligned_offset = (offset_ + alignment - 1) & ~(alignment - 1);
        
        if (aligned_offset + n > size_) {
            throw std::bad_alloc();
        }
        
        void* result = buffer_ + aligned_offset;
        offset_ = aligned_offset + n;
        
        std::cout << "Arena allocated " << n << " bytes at offset " 
                  << aligned_offset << '\n';
        
        return result;
    }
    
    void reset() {
        offset_ = 0;
        std::cout << "Arena reset\n";
    }
    
    std::size_t used() const { return offset_; }
    std::size_t available() const { return size_ - offset_; }
};

template<typename T>
class ArenaAllocator {
    Arena* arena_;
    
public:
    using value_type = T;
    
    explicit ArenaAllocator(Arena* arena) : arena_(arena) {}
    
    template<typename U>
    ArenaAllocator(const ArenaAllocator<U>& other)
        : arena_(other.arena_) {}
    
    T* allocate(std::size_t n) {
        return static_cast<T*>(arena_->allocate(n * sizeof(T), alignof(T)));
    }
    
    void deallocate(T*, std::size_t) {
        // Arena allocator doesn't deallocate individual objects
    }
    
    template<typename U>
    friend class ArenaAllocator;
};

template<typename T, typename U>
bool operator==(const ArenaAllocator<T>& a, const ArenaAllocator<U>& b) {
    return a.arena_ == b.arena_;
}

template<typename T, typename U>
bool operator!=(const ArenaAllocator<T>& a, const ArenaAllocator<U>& b) {
    return !(a == b);
}

int main() {
    Arena arena(1024);
    ArenaAllocator<int> alloc(&arena);
    
    {
        std::vector<int, ArenaAllocator<int>> vec(alloc);
        for (int i = 0; i < 10; ++i) {
            vec.push_back(i);
        }
        
        std::cout << "Arena used: " << arena.used() << " bytes\n";
        std::cout << "Arena available: " << arena.available() << " bytes\n";
    }
    
    // Arena memory still allocated, can be reset
    arena.reset();
    
    {
        std::vector<int, ArenaAllocator<int>> vec2(alloc);
        for (int i = 0; i < 5; ++i) {
            vec2.push_back(i * 2);
        }
        
        std::cout << "Arena used after reset: " << arena.used() << " bytes\n";
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Very fast allocation (bump pointer)
- ✅ No per-object deallocation overhead
- ✅ Good for temporary objects
- ❌ Can't free individual objects
- 💡 Perfect for per-frame allocations

---

## 5. Tracking Allocator

### Memory Usage Tracker

```cpp
#include <iostream>
#include <memory>
#include <vector>
#include <atomic>

class AllocationTracker {
    std::atomic<std::size_t> allocations_{0};
    std::atomic<std::size_t> deallocations_{0};
    std::atomic<std::size_t> bytes_allocated_{0};
    std::atomic<std::size_t> bytes_deallocated_{0};
    
public:
    void record_allocation(std::size_t bytes) {
        ++allocations_;
        bytes_allocated_ += bytes;
    }
    
    void record_deallocation(std::size_t bytes) {
        ++deallocations_;
        bytes_deallocated_ += bytes;
    }
    
    void print_stats() const {
        std::cout << "\n=== Allocation Statistics ===\n";
        std::cout << "Total allocations: " << allocations_ << '\n';
        std::cout << "Total deallocations: " << deallocations_ << '\n';
        std::cout << "Bytes allocated: " << bytes_allocated_ << '\n';
        std::cout << "Bytes deallocated: " << bytes_deallocated_ << '\n';
        std::cout << "Net bytes: " << (bytes_allocated_ - bytes_deallocated_) << '\n';
        std::cout << "Outstanding allocations: " << (allocations_ - deallocations_) << '\n';
    }
};

template<typename T>
class TrackingAllocator {
    AllocationTracker* tracker_;
    
public:
    using value_type = T;
    
    explicit TrackingAllocator(AllocationTracker* tracker)
        : tracker_(tracker) {}
    
    template<typename U>
    TrackingAllocator(const TrackingAllocator<U>& other)
        : tracker_(other.tracker_) {}
    
    T* allocate(std::size_t n) {
        std::size_t bytes = n * sizeof(T);
        tracker_->record_allocation(bytes);
        return static_cast<T*>(::operator new(bytes));
    }
    
    void deallocate(T* p, std::size_t n) {
        std::size_t bytes = n * sizeof(T);
        tracker_->record_deallocation(bytes);
        ::operator delete(p);
    }
    
    template<typename U>
    friend class TrackingAllocator;
};

template<typename T, typename U>
bool operator==(const TrackingAllocator<T>& a, const TrackingAllocator<U>& b) {
    return a.tracker_ == b.tracker_;
}

template<typename T, typename U>
bool operator!=(const TrackingAllocator<T>& a, const TrackingAllocator<U>& b) {
    return !(a == b);
}

int main() {
    AllocationTracker tracker;
    TrackingAllocator<int> alloc(&tracker);
    
    {
        std::vector<int, TrackingAllocator<int>> vec(alloc);
        
        for (int i = 0; i < 100; ++i) {
            vec.push_back(i);
        }
        
        tracker.print_stats();
    }
    
    std::cout << "\nAfter vector destruction:\n";
    tracker.print_stats();
    
    return 0;
}
```

---

## 6. Polymorphic Allocator (C++17)

### std::pmr::polymorphic_allocator

```cpp
#include <iostream>
#include <memory_resource>
#include <vector>

int main() {
    // Monotonic buffer resource
    char buffer[1024];
    std::pmr::monotonic_buffer_resource mbr(buffer, sizeof(buffer));
    
    {
        std::pmr::vector<int> vec(&mbr);
        
        for (int i = 0; i < 10; ++i) {
            vec.push_back(i);
        }
        
        std::cout << "Vector size: " << vec.size() << '\n';
        std::cout << "Vector contents: ";
        for (int val : vec) {
            std::cout << val << ' ';
        }
        std::cout << '\n';
    }
    
    // Pool resource
    std::pmr::synchronized_pool_resource pool;
    
    {
        std::pmr::vector<std::pmr::string> strings(&pool);
        
        strings.push_back("Hello");
        strings.push_back("World");
        strings.push_back("from");
        strings.push_back("C++");
        
        for (const auto& str : strings) {
            std::cout << str << ' ';
        }
        std::cout << '\n';
    }
    
    return 0;
}
```

**Analysis:**
- ✅ Type-erased allocator
- ✅ Runtime polymorphism
- ✅ Standard library support
- 💡 Great for containers with different allocators

---

## 7. Aligned Allocator

### SIMD-Friendly Allocator

```cpp
#include <iostream>
#include <memory>
#include <vector>

template<typename T, std::size_t Alignment = 64>
class AlignedAllocator {
public:
    using value_type = T;
    
    AlignedAllocator() = default;
    
    template<typename U>
    AlignedAllocator(const AlignedAllocator<U, Alignment>&) {}
    
    T* allocate(std::size_t n) {
        std::size_t bytes = n * sizeof(T);
        void* ptr = nullptr;
        
        #ifdef _WIN32
            ptr = _aligned_malloc(bytes, Alignment);
        #else
            if (posix_memalign(&ptr, Alignment, bytes) != 0) {
                ptr = nullptr;
            }
        #endif
        
        if (!ptr) {
            throw std::bad_alloc();
        }
        
        std::cout << "Allocated " << n << " objects with " 
                  << Alignment << "-byte alignment\n";
        
        return static_cast<T*>(ptr);
    }
    
    void deallocate(T* p, std::size_t n) {
        #ifdef _WIN32
            _aligned_free(p);
        #else
            free(p);
        #endif
        
        std::cout << "Deallocated " << n << " aligned objects\n";
    }
};

template<typename T, typename U, std::size_t A>
bool operator==(const AlignedAllocator<T, A>&, const AlignedAllocator<U, A>&) {
    return true;
}

template<typename T, typename U, std::size_t A>
bool operator!=(const AlignedAllocator<T, A>&, const AlignedAllocator<U, A>&) {
    return false;
}

int main() {
    // 64-byte aligned vector (cache line aligned)
    std::vector<float, AlignedAllocator<float, 64>> vec;
    
    for (int i = 0; i < 16; ++i) {
        vec.push_back(static_cast<float>(i));
    }
    
    std::cout << "Alignment of first element: " 
              << (reinterpret_cast<std::uintptr_t>(&vec[0]) % 64) << '\n';
    
    return 0;
}
```

---

## 8. Practical Example: Game Entity System

### Custom Allocator for Game Objects

```cpp
#include <iostream>
#include <vector>
#include <memory>

struct Entity {
    int id;
    float x, y, z;
    
    Entity(int id, float x, float y, float z)
        : id(id), x(x), y(y), z(z) {
        std::cout << "Entity " << id << " created\n";
    }
    
    ~Entity() {
        std::cout << "Entity " << id << " destroyed\n";
    }
};

template<typename T, std::size_t ChunkSize = 64>
class ChunkedAllocator {
    struct Chunk {
        alignas(T) char data[sizeof(T) * ChunkSize];
        bool used[ChunkSize] = {};
        Chunk* next = nullptr;
    };
    
    Chunk* head_ = nullptr;
    
    Chunk* allocate_chunk() {
        Chunk* chunk = new Chunk;
        chunk->next = head_;
        head_ = chunk;
        std::cout << "Allocated new chunk of " << ChunkSize << " entities\n";
        return chunk;
    }
    
public:
    using value_type = T;
    
    ~ChunkedAllocator() {
        while (head_) {
            Chunk* next = head_->next;
            delete head_;
            head_ = next;
        }
    }
    
    T* allocate(std::size_t n) {
        if (n != 1) {
            throw std::bad_alloc(); // Only single object allocation
        }
        
        // Find free slot
        for (Chunk* chunk = head_; chunk; chunk = chunk->next) {
            for (std::size_t i = 0; i < ChunkSize; ++i) {
                if (!chunk->used[i]) {
                    chunk->used[i] = true;
                    return reinterpret_cast<T*>(&chunk->data[i * sizeof(T)]);
                }
            }
        }
        
        // No free slot, allocate new chunk
        Chunk* chunk = allocate_chunk();
        chunk->used[0] = true;
        return reinterpret_cast<T*>(&chunk->data[0]);
    }
    
    void deallocate(T* p, std::size_t n) {
        if (n != 1) return;
        
        // Find which chunk contains this pointer
        for (Chunk* chunk = head_; chunk; chunk = chunk->next) {
            char* chunk_start = chunk->data;
            char* chunk_end = chunk_start + sizeof(T) * ChunkSize;
            char* ptr = reinterpret_cast<char*>(p);
            
            if (ptr >= chunk_start && ptr < chunk_end) {
                std::size_t index = (ptr - chunk_start) / sizeof(T);
                chunk->used[index] = false;
                return;
            }
        }
    }
    
    template<typename U>
    ChunkedAllocator(const ChunkedAllocator<U, ChunkSize>&) {}
    
    ChunkedAllocator() = default;
};

template<typename T, typename U, std::size_t N>
bool operator==(const ChunkedAllocator<T, N>&, const ChunkedAllocator<U, N>&) {
    return true;
}

template<typename T, typename U, std::size_t N>
bool operator!=(const ChunkedAllocator<T, N>&, const ChunkedAllocator<U, N>&) {
    return false;
}

int main() {
    ChunkedAllocator<Entity, 10> alloc;
    std::vector<Entity, ChunkedAllocator<Entity, 10>> entities(alloc);
    
    // Create entities
    for (int i = 0; i < 25; ++i) {
        entities.emplace_back(i, i * 1.0f, i * 2.0f, i * 3.0f);
    }
    
    std::cout << "\nTotal entities: " << entities.size() << '\n';
    
    return 0;
}
```

---

## Best Practices

1. **Choose the Right Allocator:**
   - Pool allocator for fixed-size objects
   - Arena allocator for temporary allocations
   - Tracking allocator for debugging

2. **Consider Performance:**
   - Custom allocators can be faster than `new`/`delete`
   - But simpler code is often better
   - Profile before optimizing

3. **Thread Safety:**
   - Make allocators thread-safe if needed
   - Use `std::pmr::synchronized_pool_resource`

4. **Test Thoroughly:**
   - Allocator bugs are hard to find
   - Test with different sizes
   - Test reallocation scenarios

5. **Document Limitations:**
   - Pool size limits
   - Thread safety guarantees
   - Alignment requirements

---

## Summary

Custom allocators provide:
- **Memory control** - Pool, arena, tracking
- **Performance optimization** - Reduced allocations
- **Special memory** - Aligned, shared, GPU
- **STL integration** - Works with containers

---

### References
- [std::allocator](https://en.cppreference.com/w/cpp/memory/allocator)
- [std::pmr::polymorphic_allocator](https://en.cppreference.com/w/cpp/memory/polymorphic_allocator)
- [Custom Allocators](https://www.modernescpp.com/index.php/memory-pools)
- [C++ Allocators](https://en.cppreference.com/w/cpp/named_req/Allocator)