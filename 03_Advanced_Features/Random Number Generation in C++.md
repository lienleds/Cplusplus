# Random Number Generation in C++

Modern C++ provides sophisticated random number generation through the `<random>` library, replacing the old `rand()` function.

---

## Overview

The `<random>` library provides:
- **Random engines** - Generate random bits
- **Distributions** - Transform random bits into desired ranges
- **Seed management** - Control randomness reproducibility

---

## 1. Basic Random Number Generation

### Simple Random Integer

```cpp
#include <iostream>
#include <random>

int main() {
    // Create random device for seeding
    std::random_device rd;
    
    // Create random engine
    std::mt19937 gen(rd());
    
    // Create distribution
    std::uniform_int_distribution<> dist(1, 6); // Dice roll
    
    // Generate 10 random numbers
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

**Analysis:**
- ✅ High-quality random numbers
- ✅ Uniform distribution
- ✅ Reproducible with same seed
- 📝 Much better than old `rand()`

### Random Floating Point

```cpp
#include <iostream>
#include <random>
#include <iomanip>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_real_distribution<> dist(0.0, 1.0);
    
    std::cout << std::fixed << std::setprecision(4);
    
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 2. Random Engines

### Common Random Engines

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    
    // Mersenne Twister (most common)
    std::mt19937 mt(rd());        // 32-bit
    std::mt19937_64 mt64(rd());   // 64-bit
    
    // Linear Congruential Generator (fast but lower quality)
    std::minstd_rand lcg(rd());
    
    // Subtract with Carry
    std::ranlux24 rl24(rd());
    
    // Generate some numbers
    std::uniform_int_distribution<> dist(1, 100);
    
    std::cout << "MT19937: " << dist(mt) << '\n';
    std::cout << "MT19937_64: " << dist(mt64) << '\n';
    std::cout << "LCG: " << dist(lcg) << '\n';
    std::cout << "RANLUX24: " << dist(rl24) << '\n';
    
    return 0;
}
```

**Engine Comparison:**

| Engine | Speed | Quality | Period | Use Case |
|--------|-------|---------|--------|----------|
| `std::mt19937` | Fast | High | 2^19937-1 | General purpose |
| `std::mt19937_64` | Fast | High | 2^19937-1 | 64-bit systems |
| `std::minstd_rand` | Very Fast | Low | 2^31-2 | Quick simulations |
| `std::ranlux24` | Slow | Very High | ~10^171 | Cryptography (not recommended) |

---

## 3. Distributions

### Uniform Distribution

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // Integer: [1, 6]
    std::uniform_int_distribution<> int_dist(1, 6);
    
    // Real: [0.0, 1.0)
    std::uniform_real_distribution<> real_dist(0.0, 1.0);
    
    std::cout << "Dice roll: " << int_dist(gen) << '\n';
    std::cout << "Random fraction: " << real_dist(gen) << '\n';
    
    return 0;
}
```

### Normal (Gaussian) Distribution

```cpp
#include <iostream>
#include <random>
#include <map>
#include <iomanip>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // Mean = 100, Standard deviation = 15 (like IQ scores)
    std::normal_distribution<> dist(100.0, 15.0);
    
    // Generate histogram
    std::map<int, int> histogram;
    for (int i = 0; i < 10000; ++i) {
        int value = std::round(dist(gen));
        ++histogram[value];
    }
    
    // Print histogram
    for (auto [value, count] : histogram) {
        if (value >= 70 && value <= 130) {
            std::cout << std::setw(3) << value << ": ";
            std::cout << std::string(count / 50, '*') << '\n';
        }
    }
    
    return 0;
}
```

**Output Example:**
```
 70: *
 75: ***
 80: ******
 85: **********
 90: **************
 95: ******************
100: ********************
105: ******************
110: **************
115: **********
120: ******
125: ***
130: *
```

### Bernoulli Distribution

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // 70% chance of true
    std::bernoulli_distribution dist(0.7);
    
    int successes = 0;
    for (int i = 0; i < 1000; ++i) {
        if (dist(gen)) {
            ++successes;
        }
    }
    
    std::cout << "Successes: " << successes << " out of 1000\n";
    std::cout << "Expected: ~700\n";
    
    return 0;
}
```

### Binomial Distribution

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // Flip 10 coins, each with 50% success rate
    std::binomial_distribution<> dist(10, 0.5);
    
    std::cout << "Number of heads in 10 flips:\n";
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

### Poisson Distribution

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // Mean = 4 events per interval
    std::poisson_distribution<> dist(4.0);
    
    std::cout << "Events per interval:\n";
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 4. Seeding and Reproducibility

### Fixed Seed for Reproducibility

```cpp
#include <iostream>
#include <random>

void generate_with_seed(unsigned seed) {
    std::mt19937 gen(seed);
    std::uniform_int_distribution<> dist(1, 100);
    
    for (int i = 0; i < 5; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
}

int main() {
    std::cout << "First run (seed=42):\n";
    generate_with_seed(42);
    
    std::cout << "Second run (seed=42):\n";
    generate_with_seed(42); // Same sequence
    
    std::cout << "Third run (seed=123):\n";
    generate_with_seed(123); // Different sequence
    
    return 0;
}
```

### Proper Seeding with std::random_device

```cpp
#include <iostream>
#include <random>

int main() {
    std::random_device rd;
    
    // Seed with multiple random values
    std::seed_seq seed{rd(), rd(), rd(), rd(), rd(), rd(), rd(), rd()};
    std::mt19937 gen(seed);
    
    std::uniform_int_distribution<> dist(1, 100);
    
    for (int i = 0; i < 10; ++i) {
        std::cout << dist(gen) << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

---

## 5. Practical Examples

### Shuffle a Container

```cpp
#include <iostream>
#include <random>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> deck(52);
    std::iota(deck.begin(), deck.end(), 1); // Fill with 1-52
    
    std::random_device rd;
    std::mt19937 gen(rd());
    
    std::shuffle(deck.begin(), deck.end(), gen);
    
    std::cout << "Shuffled deck (first 10 cards):\n";
    for (int i = 0; i < 10; ++i) {
        std::cout << deck[i] << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

### Random Sampling

```cpp
#include <iostream>
#include <random>
#include <vector>
#include <algorithm>

template<typename T>
std::vector<T> random_sample(const std::vector<T>& population, size_t sample_size) {
    std::random_device rd;
    std::mt19937 gen(rd());
    
    std::vector<T> sample;
    std::sample(
        population.begin(), 
        population.end(),
        std::back_inserter(sample),
        sample_size,
        gen
    );
    
    return sample;
}

int main() {
    std::vector<int> data(100);
    std::iota(data.begin(), data.end(), 1); // 1-100
    
    auto sample = random_sample(data, 10);
    
    std::cout << "Random sample of 10 numbers:\n";
    for (int num : sample) {
        std::cout << num << ' ';
    }
    std::cout << '\n';
    
    return 0;
}
```

### Monte Carlo Simulation (Estimate π)

```cpp
#include <iostream>
#include <random>
#include <iomanip>

double estimate_pi(size_t iterations) {
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_real_distribution<> dist(-1.0, 1.0);
    
    size_t inside_circle = 0;
    
    for (size_t i = 0; i < iterations; ++i) {
        double x = dist(gen);
        double y = dist(gen);
        
        if (x*x + y*y <= 1.0) {
            ++inside_circle;
        }
    }
    
    return 4.0 * inside_circle / iterations;
}

int main() {
    std::cout << std::fixed << std::setprecision(6);
    
    for (size_t n : {1000, 10000, 100000, 1000000}) {
        double pi = estimate_pi(n);
        std::cout << "n = " << std::setw(8) << n 
                  << ", π ≈ " << pi << '\n';
    }
    
    return 0;
}
```

---

## 6. Common Distribution Types

```cpp
#include <random>

// Uniform distributions
std::uniform_int_distribution<int> uniform_int(1, 6);
std::uniform_real_distribution<double> uniform_real(0.0, 1.0);

// Normal (Gaussian)
std::normal_distribution<double> normal(0.0, 1.0); // mean, stddev

// Bernoulli (coin flip)
std::bernoulli_distribution bernoulli(0.5); // probability

// Binomial (n trials)
std::binomial_distribution<int> binomial(10, 0.5); // trials, probability

// Poisson (events per interval)
std::poisson_distribution<int> poisson(4.0); // mean

// Exponential
std::exponential_distribution<double> exponential(1.0); // lambda

// Geometric
std::geometric_distribution<int> geometric(0.5); // probability

// Chi-squared
std::chi_squared_distribution<double> chi_squared(5.0); // degrees of freedom
```

---

## Best Practices

1. **Use Modern Random:**
   - ❌ Don't use `rand()` and `srand()`
   - ✅ Use `<random>` library

2. **Choose the Right Engine:**
   - `std::mt19937` for most cases
   - `std::mt19937_64` for 64-bit systems

3. **Seed Properly:**
   - Use `std::random_device` for non-deterministic seeding
   - Use fixed seeds only for testing/debugging

4. **Reuse Engines:**
   - Don't create new engines frequently
   - Store as member variables or static

5. **Pick Appropriate Distribution:**
   - Uniform for dice, shuffling
   - Normal for natural phenomena
   - Bernoulli for yes/no events

---

## Performance Comparison

```cpp
#include <iostream>
#include <random>
#include <chrono>

int main() {
    const int N = 10'000'000;
    
    // Old way (DON'T USE)
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < N; ++i) {
        volatile int x = rand() % 100;
    }
    auto old_time = std::chrono::high_resolution_clock::now() - start;
    
    // Modern way
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dist(0, 99);
    
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < N; ++i) {
        volatile int x = dist(gen);
    }
    auto modern_time = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Old rand(): " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(old_time).count() 
              << "ms\n";
    std::cout << "Modern <random>: " 
              << std::chrono::duration_cast<std::chrono::milliseconds>(modern_time).count() 
              << "ms\n";
    
    return 0;
}
```

---

## Summary

Modern C++ random number generation provides:
- **High-quality randomness**
- **Variety of distributions**
- **Reproducible results** (when needed)
- **Better than old `rand()`** in every way

---

### References
- [std::random](https://en.cppreference.com/w/cpp/numeric/random)
- [Random Number Generation in C++11](https://www.pcg-random.org/)
- [C++ Random Number Distributions](https://en.cppreference.com/w/cpp/numeric/random#Random_number_distributions)