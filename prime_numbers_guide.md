# Chuyên đề Số Nguyên Tố trong Lập Trình Thi Đấu

## Mục lục
1. [Khái niệm cơ bản](#1-khái-niệm-cơ-bản)
2. [Các thuật toán kiểm tra số nguyên tố](#2-các-thuật-toán-kiểm-tra-số-nguyên-tố)
3. [Sàng Eratosthenes](#3-sàng-eratosthenes)
4. [Kiểm tra số nguyên tố lớn](#4-kiểm-tra-số-nguyên-tố-lớn)
5. [Ứng dụng trong bài toán](#5-ứng-dụng-trong-bài-toán)
6. [Bài tập thực hành](#6-bài-tập-thực-hành)

---

## 1. Khái niệm cơ bản

**Số nguyên tố** là số tự nhiên lớn hơn 1 chỉ có đúng hai ước số: 1 và chính nó.

**Ví dụ:** 2, 3, 5, 7, 11, 13, 17, 19, 23, ...

**Số hợp số** là số tự nhiên lớn hơn 1 có nhiều hơn hai ước số.

**Lưu ý quan trọng:**
- Số 1 không phải là số nguyên tố
- Số 2 là số nguyên tố chẵn duy nhất

---

## 2. Các thuật toán kiểm tra số nguyên tố

### 2.1. Thuật toán naïve - O(n)

Kiểm tra tất cả các số từ 2 đến n-1.

**Python:**
```python
def is_prime_naive(n):
    """
    Kiểm tra số nguyên tố bằng cách thử tất cả các ước từ 2 đến n-1
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if n < 2:
        return False
    
    # Kiểm tra từ 2 đến n-1
    for i in range(2, n):
        if n % i == 0:  # Nếu n chia hết cho i
            return False
    
    return True

# Test
print(is_prime_naive(17))  # True
print(is_prime_naive(15))  # False
```

**C++:**
```cpp
#include <iostream>
using namespace std;

bool isPrimeNaive(int n) {
    /*
     * Kiểm tra số nguyên tố bằng cách thử tất cả các ước từ 2 đến n-1
     * Time Complexity: O(n)
     * Space Complexity: O(1)
     */
    if (n < 2) return false;
    
    // Kiểm tra từ 2 đến n-1
    for (int i = 2; i < n; i++) {
        if (n % i == 0) {  // Nếu n chia hết cho i
            return false;
        }
    }
    
    return true;
}

int main() {
    cout << isPrimeNaive(17) << endl;  // 1 (true)
    cout << isPrimeNaive(15) << endl;  // 0 (false)
    return 0;
}
```

### 2.2. Thuật toán tối ưu - O(√n)

Chỉ cần kiểm tra đến √n vì nếu n có ước lớn hơn √n thì phải có ước tương ứng nhỏ hơn √n.

**Python:**
```python
import math

def is_prime_optimized(n):
    """
    Kiểm tra số nguyên tố bằng cách thử các ước từ 2 đến sqrt(n)
    Time Complexity: O(√n)
    Space Complexity: O(1)
    """
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:  # Loại bỏ số chẵn
        return False
    
    # Chỉ kiểm tra số lẻ từ 3 đến sqrt(n)
    for i in range(3, int(math.sqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    
    return True

# Test với số lớn
print(is_prime_optimized(1000003))  # True
print(is_prime_optimized(1000000))  # False
```

**C++:**
```cpp
#include <iostream>
#include <cmath>
using namespace std;

bool isPrimeOptimized(long long n) {
    /*
     * Kiểm tra số nguyên tố bằng cách thử các ước từ 2 đến sqrt(n)
     * Time Complexity: O(√n)
     * Space Complexity: O(1)
     */
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;  // Loại bỏ số chẵn
    
    // Chỉ kiểm tra số lẻ từ 3 đến sqrt(n)
    for (long long i = 3; i * i <= n; i += 2) {
        if (n % i == 0) {
            return false;
        }
    }
    
    return true;
}

int main() {
    cout << isPrimeOptimized(1000003) << endl;  // 1
    cout << isPrimeOptimized(1000000) << endl;  // 0
    return 0;
}
```

---

## 3. Sàng Eratosthenes

Thuật toán hiệu quả nhất để tìm tất cả số nguyên tố từ 1 đến n.

### 3.1. Sàng Eratosthenes cơ bản - O(n log log n)

**Python:**
```python
def sieve_of_eratosthenes(n):
    """
    Tìm tất cả số nguyên tố từ 2 đến n bằng sàng Eratosthenes
    Time Complexity: O(n log log n)
    Space Complexity: O(n)
    """
    if n < 2:
        return []
    
    # Khởi tạo mảng đánh dấu, ban đầu tất cả đều là nguyên tố
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False  # 0 và 1 không phải nguyên tố
    
    # Sàng Eratosthenes
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            # Đánh dấu tất cả bội của i (từ i*i) là hợp số
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    
    # Thu thập kết quả
    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
    
    return primes, is_prime

# Test
primes, is_prime = sieve_of_eratosthenes(30)
print("Các số nguyên tố đến 30:", primes)
print("17 là nguyên tố?", is_prime[17])
```

**C++:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

pair<vector<int>, vector<bool>> sieveOfEratosthenes(int n) {
    /*
     * Tìm tất cả số nguyên tố từ 2 đến n bằng sàng Eratosthenes
     * Time Complexity: O(n log log n)
     * Space Complexity: O(n)
     */
    vector<bool> isPrime(n + 1, true);
    vector<int> primes;
    
    if (n < 2) return {primes, isPrime};
    
    isPrime[0] = isPrime[1] = false;  // 0 và 1 không phải nguyên tố
    
    // Sàng Eratosthenes
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            // Đánh dấu tất cả bội của i (từ i*i) là hợp số
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    
    // Thu thập kết quả
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            primes.push_back(i);
        }
    }
    
    return {primes, isPrime};
}

int main() {
    auto result = sieveOfEratosthenes(30);
    vector<int> primes = result.first;
    vector<bool> isPrime = result.second;
    
    cout << "Các số nguyên tố đến 30: ";
    for (int p : primes) {
        cout << p << " ";
    }
    cout << endl;
    
    cout << "17 là nguyên tố? " << isPrime[17] << endl;
    return 0;
}
```

### 3.2. Sàng Eratosthenes tối ưu bộ nhớ

Chỉ lưu số lẻ để giảm bộ nhớ xuống một nửa.

**Python:**
```python
def optimized_sieve(n):
    """
    Sàng Eratosthenes tối ưu bộ nhớ - chỉ lưu số lẻ
    Time Complexity: O(n log log n)
    Space Complexity: O(n/2)
    """
    if n < 2:
        return []
    if n == 2:
        return [2]
    
    # Chỉ lưu số lẻ từ 3 trở đi
    # Chỉ số i trong mảng tương ứng với số 2*i + 3
    size = (n - 1) // 2
    is_prime = [True] * size
    
    # Sàng chỉ với số lẻ
    for i in range(int((n**0.5 - 3) // 2) + 1):
        if is_prime[i]:
            num = 2 * i + 3  # Số thực tế
            # Bắt đầu từ num^2, chỉ đánh dấu số lẻ
            start = (num * num - 3) // 2
            for j in range(start, size, num):
                is_prime[j] = False
    
    # Thu thập kết quả
    primes = [2]  # Thêm số 2
    for i in range(size):
        if is_prime[i]:
            primes.append(2 * i + 3)
    
    return primes

# Test
primes = optimized_sieve(100)
print("Số nguyên tố đến 100:", len(primes), "số")
print("10 số đầu:", primes[:10])
```

**C++:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> optimizedSieve(int n) {
    /*
     * Sàng Eratosthenes tối ưu bộ nhớ - chỉ lưu số lẻ
     * Time Complexity: O(n log log n)
     * Space Complexity: O(n/2)
     */
    vector<int> primes;
    if (n < 2) return primes;
    if (n == 2) {
        primes.push_back(2);
        return primes;
    }
    
    // Chỉ lưu số lẻ từ 3 trở đi
    int size = (n - 1) / 2;
    vector<bool> isPrime(size, true);
    
    // Sàng chỉ với số lẻ
    for (int i = 0; i * i < size; i++) {
        if (isPrime[i]) {
            int num = 2 * i + 3;  // Số thực tế
            // Bắt đầu từ num^2
            for (int j = (num * num - 3) / 2; j < size; j += num) {
                isPrime[j] = false;
            }
        }
    }
    
    // Thu thập kết quả
    primes.push_back(2);  // Thêm số 2
    for (int i = 0; i < size; i++) {
        if (isPrime[i]) {
            primes.push_back(2 * i + 3);
        }
    }
    
    return primes;
}

int main() {
    vector<int> primes = optimizedSieve(100);
    cout << "Số nguyên tố đến 100: " << primes.size() << " số" << endl;
    cout << "10 số đầu: ";
    for (int i = 0; i < 10 && i < primes.size(); i++) {
        cout << primes[i] << " ";
    }
    cout << endl;
    return 0;
}
```

---

## 4. Kiểm tra số nguyên tố lớn

### 4.1. Miller-Rabin Test

Thuật toán xác suất để kiểm tra số nguyên tố rất lớn.

**Python:**
```python
import random

def miller_rabin(n, k=5):
    """
    Kiểm tra số nguyên tố bằng Miller-Rabin test
    n: số cần kiểm tra
    k: số lần lặp (càng lớn càng chính xác)
    Time Complexity: O(k * log³n)
    """
    if n < 2:
        return False
    if n in [2, 3]:
        return True
    if n % 2 == 0:
        return False
    
    # Viết n-1 = 2^r * d với d lẻ
    r = 0
    d = n - 1
    while d % 2 == 0:
        r += 1
        d //= 2
    
    # Lặp k lần test
    for _ in range(k):
        a = random.randrange(2, n - 1)
        x = pow(a, d, n)  # a^d mod n
        
        if x == 1 or x == n - 1:
            continue
        
        for _ in range(r - 1):
            x = pow(x, 2, n)  # x^2 mod n
            if x == n - 1:
                break
        else:
            return False  # Composite
    
    return True  # Probably prime

# Test với số rất lớn
large_number = 1000000007
print(f"{large_number} là nguyên tố:", miller_rabin(large_number))

# Test với số Mersenne
mersenne = 2**31 - 1  # 2147483647
print(f"Mersenne {mersenne} là nguyên tố:", miller_rabin(mersenne))
```

**C++:**
```cpp
#include <iostream>
#include <random>
using namespace std;

// Tính (base^exp) % mod một cách hiệu quả
long long modPow(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) {
            result = (__int128)result * base % mod;
        }
        base = (__int128)base * base % mod;
        exp >>= 1;
    }
    return result;
}

bool millerRabin(long long n, int k = 5) {
    /*
     * Kiểm tra số nguyên tố bằng Miller-Rabin test
     * n: số cần kiểm tra
     * k: số lần lặp (càng lớn càng chính xác)
     * Time Complexity: O(k * log³n)
     */
    if (n < 2) return false;
    if (n == 2 || n == 3) return true;
    if (n % 2 == 0) return false;
    
    // Viết n-1 = 2^r * d với d lẻ
    int r = 0;
    long long d = n - 1;
    while (d % 2 == 0) {
        r++;
        d /= 2;
    }
    
    // Random number generator
    random_device rd;
    mt19937_64 gen(rd());
    uniform_int_distribution<long long> dis(2, n - 2);
    
    // Lặp k lần test
    for (int i = 0; i < k; i++) {
        long long a = dis(gen);
        long long x = modPow(a, d, n);
        
        if (x == 1 || x == n - 1) {
            continue;
        }
        
        bool composite = true;
        for (int j = 0; j < r - 1; j++) {
            x = (__int128)x * x % n;
            if (x == n - 1) {
                composite = false;
                break;
            }
        }
        
        if (composite) {
            return false;
        }
    }
    
    return true;
}

int main() {
    long long largeNumber = 1000000007LL;
    cout << largeNumber << " là nguyên tố: " << millerRabin(largeNumber) << endl;
    
    long long mersenne = (1LL << 31) - 1;  // 2^31 - 1
    cout << "Mersenne " << mersenne << " là nguyên tố: " << millerRabin(mersenne) << endl;
    
    return 0;
}
```

### 4.2. Fermat Test (đơn giản hơn nhưng kém chính xác)

**Python:**
```python
def fermat_test(n, k=5):
    """
    Kiểm tra số nguyên tố bằng Fermat's Little Theorem
    Lưu ý: Có thể bị lừa bởi số Carmichael
    """
    if n < 2:
        return False
    if n in [2, 3]:
        return True
    if n % 2 == 0:
        return False
    
    for _ in range(k):
        a = random.randrange(2, n - 1)
        if pow(a, n - 1, n) != 1:
            return False
    
    return True

# So sánh với Miller-Rabin
test_numbers = [561, 1105, 1729]  # Số Carmichael
for num in test_numbers:
    print(f"{num}: Fermat={fermat_test(num)}, Miller-Rabin={miller_rabin(num)}")
```

---

## 5. Ứng dụng trong bài toán

### 5.1. Đếm số nguyên tố trong khoảng

**Python:**
```python
def count_primes_in_range(left, right):
    """
    Đếm số lượng số nguyên tố trong khoảng [left, right]
    Sử dụng sàng phân đoạn cho khoảng lớn
    """
    if left > right:
        return 0
    
    # Nếu khoảng nhỏ, dùng sàng thông thường
    if right - left + 1 <= 10**6:
        primes, is_prime = sieve_of_eratosthenes(right)
        count = 0
        for i in range(left, right + 1):
            if i < len(is_prime) and is_prime[i]:
                count += 1
        return count
    
    # Khoảng lớn - sàng phân đoạn
    # Tìm tất cả số nguyên tố đến sqrt(right)
    limit = int(right**0.5) + 1
    small_primes, _ = sieve_of_eratosthenes(limit)
    
    # Sàng phân đoạn
    size = right - left + 1
    is_prime_segment = [True] * size
    
    for prime in small_primes:
        # Tìm số đầu tiên >= left chia hết cho prime
        start = max(prime * prime, ((left + prime - 1) // prime) * prime)
        
        # Đánh dấu các bội của prime
        for j in range(start, right + 1, prime):
            is_prime_segment[j - left] = False
    
    # Đếm số nguyên tố
    count = 0
    for i in range(size):
        if left + i > 1 and is_prime_segment[i]:
            count += 1
    
    return count

# Test
print("Số nguyên tố từ 10 đến 50:", count_primes_in_range(10, 50))
print("Số nguyên tố từ 1000000 đến 1000100:", count_primes_in_range(1000000, 1000100))
```

### 5.2. Tìm số nguyên tố gần nhất

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

int findNearestPrime(int n) {
    /*
     * Tìm số nguyên tố gần n nhất
     * Nếu có 2 số cùng khoảng cách, chọn số nhỏ hơn
     */
    if (n <= 2) return 2;
    
    // Kiểm tra n có phải nguyên tố không
    if (isPrimeOptimized(n)) return n;
    
    // Tìm theo 2 hướng
    int lower = n - 1, upper = n + 1;
    
    while (lower >= 2 || upper <= INT_MAX) {
        // Kiểm tra số nhỏ hơn trước (ưu tiên số nhỏ)
        if (lower >= 2 && isPrimeOptimized(lower)) {
            return lower;
        }
        
        if (upper <= INT_MAX && isPrimeOptimized(upper)) {
            return upper;
        }
        
        lower--;
        upper++;
    }
    
    return -1;  // Không tìm thấy
}

int main() {
    vector<int> test_cases = {10, 20, 97, 100, 1000};
    
    for (int n : test_cases) {
        cout << "Số nguyên tố gần " << n << " nhất: " << findNearestPrime(n) << endl;
    }
    
    return 0;
}
```

### 5.3. Phân tích thừa số nguyên tố

**Python:**
```python
def prime_factorization(n):
    """
    Phân tích thừa số nguyên tố của n
    Trả về list các cặp (thừa số, số mũ)
    """
    factors = []
    
    # Xử lý thừa số 2
    if n % 2 == 0:
        count = 0
        while n % 2 == 0:
            count += 1
            n //= 2
        factors.append((2, count))
    
    # Xử lý thừa số lẻ
    i = 3
    while i * i <= n:
        if n % i == 0:
            count = 0
            while n % i == 0:
                count += 1
                n //= i
            factors.append((i, count))
        i += 2
    
    # Nếu n > 1 thì n là thừa số nguyên tố
    if n > 1:
        factors.append((n, 1))
    
    return factors

def get_all_divisors(n):
    """
    Tìm tất cả ước của n từ phân tích thừa số nguyên tố
    """
    factors = prime_factorization(n)
    divisors = [1]
    
    for prime, power in factors:
        new_divisors = []
        current_power = 1
        for p in range(power + 1):
            for div in divisors:
                new_divisors.append(div * current_power)
            current_power *= prime
        divisors = new_divisors
    
    return sorted(divisors)

# Test
n = 60
factors = prime_factorization(n)
print(f"Phân tích {n} = ", end="")
for i, (prime, power) in enumerate(factors):
    if i > 0:
        print(" × ", end="")
    if power == 1:
        print(f"{prime}", end="")
    else:
        print(f"{prime}^{power}", end="")
print()

print(f"Các ước của {n}: {get_all_divisors(n)}")
```

---

## 6. Bài tập thực hành

### Bài tập 1: Cặp số nguyên tố sinh đôi
Tìm tất cả cặp số nguyên tố sinh đôi (p, p+2) trong khoảng [1, N].

**Python:**
```python
def find_twin_primes(n):
    """
    Tìm tất cả cặp số nguyên tố sinh đôi đến n
    """
    primes, is_prime = sieve_of_eratosthenes(n)
    twin_primes = []
    
    for p in primes:
        if p + 2 < len(is_prime) and is_prime[p + 2]:
            twin_primes.append((p, p + 2))
    
    return twin_primes

# Test
twins = find_twin_primes(100)
print("Các cặp số nguyên tố sinh đôi đến 100:")
for p1, p2 in twins:
    print(f"({p1}, {p2})")
```

### Bài tập 2: Tổng các chữ số nguyên tố
Kiểm tra xem tổng các chữ số của một số có phải là nguyên tố không.

**C++:**
```cpp
#include <iostream>
using namespace std;

int digitSum(int n) {
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

bool isPrimeDigitSum(int n) {
    int sum = digitSum(n);
    return isPrimeOptimized(sum);
}

int main() {
    int count = 0;
    cout << "Số có tổng chữ số là nguyên tố từ 1-100: ";
    
    for (int i = 1; i <= 100; i++) {
        if (isPrimeDigitSum(i)) {
            cout << i << " ";
            count++;
        }
    }
    
    cout << "\nTổng cộng: " << count << " số" << endl;
    return 0;
}
```

### Bài tập 3: Số nguyên tố palindrome
Tìm số nguyên tố đồng thời là số palindrome.

**Python:**
```python
def is_palindrome(n):
    """Kiểm tra số palindrome"""
    s = str(n)
    return s == s[::-1]

def find_prime_palindromes(n):
    """Tìm số nguyên tố palindrome đến n"""
    primes, is_prime = sieve_of_eratosthenes(n)
    prime_palindromes = []
    
    for p in primes:
        if is_palindrome(p):
            prime_palindromes.append(p)
    
    return prime_palindromes

# Test
result = find_prime_palindromes(1000)
print("Số nguyên tố palindrome đến 1000:", result)
```

---

## Tổng kết và Lưu ý

### Độ phức tạp các thuật toán:

| Thuật toán | Time Complexity | Space Complexity | Ứng dụng |
|------------|----------------|------------------|----------|
| Naive | O(n) | O(1) | Số nhỏ |
| Optimized | O(√n) | O(1) | Số vừa |
| Sàng Eratosthenes | O(n log log n) | O(n) | Nhiều truy vấn |
| Miller-Rabin | O(k log³n) | O(1) | Số rất lớn |

### Lời khuyên cho thi đấu:

1. **Tiền xử lý**: Sử dụng sàng Eratosthenes khi có nhiều truy vấn
2. **Tối ưu bộ nhớ**: Chỉ lưu số lẻ nếu bộ nhớ hạn chế
3. **Số lớn**: Dùng Miller-Rabin cho số > 10¹²
4. **Khoảng lớn**: Sàng phân đoạn cho khoảng [L, R] với R-L ≤ 10⁶
5. **Modular arithmetic**: Cẩn thận với overflow khi làm việc với số lớn

### Template code thường dùng:

**Python - Quick Prime Check:**
```python
def is_prime(n):
    if n < 2: return False
    if n == 2: return True
    if n % 2 == 0: return False
    for i in range(3, int(n**0.5) + 1, 2):
        if n % i == 0: return False
    return True
```

**C++ - Fast Sieve:**
```cpp
vector<bool> sieve(int n) {
    vector<bool> is_prime(n + 1, true);
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i * i <= n; i++) {
        if (is_prime[i]) {
            for (int j = i * i; j <= n; j += i) {
                is_prime[j] = false;
            }
        }
    }
    return is_prime;
}
```

---

## 7. Bài toán nâng cao

### 7.1. Sàng phân đoạn (Segmented Sieve)

Để xử lý các truy vấn với số rất lớn (≤ 10¹²) trong khoảng hẹp.

**Python:**
```python
def segmented_sieve(left, right):
    """
    Sàng phân đoạn để tìm số nguyên tố trong [left, right]
    Hiệu quả khi right - left ≤ 10^6 và right ≤ 10^12
    """
    if left > right:
        return []
    
    # Điều chỉnh để left >= 2
    if left < 2:
        left = 2
    
    # Tìm tất cả số nguyên tố đến sqrt(right)
    limit = int(right**0.5) + 1
    primes, _ = sieve_of_eratosthenes(limit)
    
    # Khởi tạo mảng cho đoạn [left, right]
    size = right - left + 1
    is_prime_segment = [True] * size
    
    # Sàng với từng số nguyên tố
    for prime in primes:
        # Tìm số đầu tiên >= left chia hết cho prime
        start = max(prime * prime, ((left + prime - 1) // prime) * prime)
        
        # Đánh dấu các bội của prime trong đoạn
        for j in range(start, right + 1, prime):
            if j >= left:
                is_prime_segment[j - left] = False
    
    # Thu thập kết quả
    result = []
    for i in range(size):
        if is_prime_segment[i]:
            result.append(left + i)
    
    return result

# Test với số lớn
large_primes = segmented_sieve(1000000000000, 1000000000100)
print(f"Số nguyên tố từ 10^12 đến 10^12+100: {len(large_primes)} số")
print("Một vài số đầu:", large_primes[:5])
```

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

vector<long long> segmentedSieve(long long left, long long right) {
    /*
     * Sàng phân đoạn cho khoảng [left, right]
     * Time Complexity: O((right-left+1) * log(log(sqrt(right))))
     */
    vector<long long> result;
    if (left > right) return result;
    
    if (left < 2) left = 2;
    
    // Tìm số nguyên tố đến sqrt(right)
    long long limit = sqrt(right) + 1;
    vector<bool> isPrimeLow(limit + 1, true);
    vector<long long> primes;
    
    // Sàng cơ bản cho [2, sqrt(right)]
    for (long long i = 2; i <= limit; i++) {
        if (isPrimeLow[i]) {
            primes.push_back(i);
            for (long long j = i * i; j <= limit; j += i) {
                isPrimeLow[j] = false;
            }
        }
    }
    
    // Sàng phân đoạn
    vector<bool> isPrimeHigh(right - left + 1, true);
    
    for (long long prime : primes) {
        // Tìm bội đầu tiên của prime >= left
        long long start = max(prime * prime, ((left + prime - 1) / prime) * prime);
        
        for (long long j = start; j <= right; j += prime) {
            isPrimeHigh[j - left] = false;
        }
    }
    
    // Thu thập kết quả
    for (long long i = 0; i <= right - left; i++) {
        if (isPrimeHigh[i]) {
            result.push_back(left + i);
        }
    }
    
    return result;
}

int main() {
    // Test với khoảng lớn
    vector<long long> primes = segmentedSieve(1000000000000LL, 1000000000100LL);
    
    cout << "Số nguyên tố từ 10^12 đến 10^12+100: " << primes.size() << " số" << endl;
    cout << "5 số đầu: ";
    for (int i = 0; i < min(5, (int)primes.size()); i++) {
        cout << primes[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 7.2. Euler's Totient Function (Hàm Euler)

Đếm số lượng số nguyên dương ≤ n mà gcd(số đó, n) = 1.

**Python:**
```python
def euler_totient(n):
    """
    Tính φ(n) - số lượng số ≤ n mà gcd với n = 1
    Sử dụng công thức: φ(n) = n * ∏(1 - 1/p) với p là thừa số nguyên tố của n
    """
    result = n
    
    # Xử lý từng thừa số nguyên tố
    p = 2
    while p * p <= n:
        if n % p == 0:
            # Loại bỏ tất cả thừa số p
            while n % p == 0:
                n //= p
            # Áp dụng công thức φ(n) = φ(n) * (1 - 1/p)
            result -= result // p
        p += 1
    
    # Nếu n > 1 thì n là thừa số nguyên tố
    if n > 1:
        result -= result // n
    
    return result

def sieve_totient(n):
    """
    Tính φ(i) cho tất cả i từ 1 đến n bằng sàng
    Time Complexity: O(n log log n)
    """
    phi = list(range(n + 1))  # Khởi tạo φ(i) = i
    
    for i in range(2, n + 1):
        if phi[i] == i:  # i là số nguyên tố
            # Cập nhật φ cho tất cả bội của i
            for j in range(i, n + 1, i):
                phi[j] -= phi[j] // i
    
    return phi

# Test
print("φ(12) =", euler_totient(12))  # 4 (1,5,7,11)
print("φ(30) =", euler_totient(30))  # 8

# Tính φ cho nhiều số
phi_values = sieve_totient(20)
for i in range(1, 21):
    print(f"φ({i}) = {phi_values[i]}")
```

**C++:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

int eulerTotient(int n) {
    /*
     * Tính φ(n) cho một số n
     * Time Complexity: O(√n)
     */
    int result = n;
    
    for (int p = 2; p * p <= n; p++) {
        if (n % p == 0) {
            // Loại bỏ tất cả thừa số p
            while (n % p == 0) {
                n /= p;
            }
            // result = result * (1 - 1/p) = result * (p-1)/p
            result -= result / p;
        }
    }
    
    if (n > 1) {
        result -= result / n;
    }
    
    return result;
}

vector<int> sieveTotient(int n) {
    /*
     * Tính φ(i) cho tất cả i từ 1 đến n
     * Time Complexity: O(n log log n)
     */
    vector<int> phi(n + 1);
    
    // Khởi tạo φ(i) = i
    for (int i = 1; i <= n; i++) {
        phi[i] = i;
    }
    
    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {  // i là số nguyên tố
            for (int j = i; j <= n; j += i) {
                phi[j] -= phi[j] / i;
            }
        }
    }
    
    return phi;
}

int main() {
    cout << "φ(12) = " << eulerTotient(12) << endl;
    cout << "φ(30) = " << eulerTotient(30) << endl;
    
    vector<int> phi = sieveTotient(20);
    cout << "\nHàm Euler từ 1 đến 20:" << endl;
    for (int i = 1; i <= 20; i++) {
        cout << "φ(" << i << ") = " << phi[i] << endl;
    }
    
    return 0;
}
```

### 7.3. Số nguyên tố trong progression số học

Tìm số nguyên tố trong dãy số học a, a+d, a+2d, ...

**Python:**
```python
def primes_in_arithmetic_progression(a, d, n):
    """
    Tìm n số nguyên tố đầu tiên trong dãy a, a+d, a+2d, ...
    """
    primes = []
    current = a
    
    while len(primes) < n:
        if is_prime_optimized(current):
            primes.append(current)
        current += d
        
        # Tránh vòng lặp vô hạn
        if current > 10**9:
            break
    
    return primes

# Test - tìm số nguyên tố có dạng 6k+1
ap_primes = primes_in_arithmetic_progression(7, 6, 10)
print("10 số nguyên tố đầu tiên có dạng 6k+1:", ap_primes)

# Tìm số nguyên tố có dạng 4k+3
ap_primes_2 = primes_in_arithmetic_progression(3, 4, 10)
print("10 số nguyên tố đầu tiên có dạng 4k+3:", ap_primes_2)
```

### 7.4. Bài toán Goldbach Conjecture

Kiểm tra mọi số chẵn > 2 đều có thể viết thành tổng của 2 số nguyên tố.

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
using namespace std;

pair<int, int> goldbachPair(int n) {
    /*
     * Tìm cặp số nguyên tố có tổng = n (n chẵn)
     * Trả về {-1, -1} nếu không tìm thấy
     */
    if (n <= 2 || n % 2 != 0) {
        return {-1, -1};
    }
    
    // Tạo set các số nguyên tố đến n
    vector<bool> isPrime = sieve(n);
    unordered_set<int> primeSet;
    
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            primeSet.insert(i);
        }
    }
    
    // Tìm cặp (p1, p2) sao cho p1 + p2 = n
    for (int p1 = 2; p1 <= n/2; p1++) {
        if (primeSet.count(p1) && primeSet.count(n - p1)) {
            return {p1, n - p1};
        }
    }
    
    return {-1, -1};
}

void testGoldbach(int limit) {
    cout << "Kiểm tra Goldbach Conjecture cho số chẵn đến " << limit << ":" << endl;
    
    for (int n = 4; n <= limit; n += 2) {
        auto result = goldbachPair(n);
        if (result.first == -1) {
            cout << "PHẢN VÍ DỤ: " << n << " không thể viết thành tổng 2 số nguyên tố!" << endl;
            return;
        }
        
        if (n <= 20) {  // In một vài ví dụ đầu
            cout << n << " = " << result.first << " + " << result.second << endl;
        }
    }
    
    cout << "Tất cả số chẵn đến " << limit << " đều thỏa mãn Goldbach Conjecture!" << endl;
}

int main() {
    testGoldbach(100);
    return 0;
}
```

---

## 8. Các thuật toán đặc biệt

### 8.1. Pollard's Rho Algorithm

Thuật toán để phân tích thừa số nguyên tố của số lớn.

**Python:**
```python
import math

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def pollard_rho(n):
    """
    Thuật toán Pollard's Rho để tìm thừa số của n
    Time Complexity: O(n^(1/4))
    """
    if n % 2 == 0:
        return 2
    
    # Hàm f(x) = x^2 + 1
    x = 2
    y = 2
    d = 1
    
    while d == 1:
        # x chạy 1 bước, y chạy 2 bước
        x = (x * x + 1) % n
        y = (y * y + 1) % n
        y = (y * y + 1) % n
        
        d = gcd(abs(x - y), n)
    
    return d if d != n else None

def complete_factorization(n):
    """
    Phân tích hoàn toàn n thành thừa số nguyên tố
    """
    if n <= 1:
        return []
    
    # Kiểm tra số nguyên tố nhỏ
    factors = []
    for p in [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]:
        while n % p == 0:
            factors.append(p)
            n //= p
    
    if n == 1:
        return factors
    
    # Nếu n là số nguyên tố
    if is_prime_optimized(n):
        factors.append(n)
        return factors
    
    # Sử dụng Pollard's Rho
    stack = [n]
    
    while stack:
        current = stack.pop()
        
        if current == 1:
            continue
            
        if is_prime_optimized(current):
            factors.append(current)
            continue
        
        # Tìm thừa số bằng Pollard's Rho
        factor = pollard_rho(current)
        
        if factor and factor != current:
            stack.append(factor)
            stack.append(current // factor)
        else:
            # Fallback về trial division
            for i in range(37, int(current**0.5) + 1, 2):
                if current % i == 0:
                    stack.append(i)
                    stack.append(current // i)
                    break
            else:
                factors.append(current)
    
    return sorted(factors)

# Test
large_number = 1234567890123456789
print(f"Phân tích {large_number}:")
factors = complete_factorization(large_number)
print("Thừa số:", factors)

# Verification
product = 1
for f in factors:
    product *= f
print(f"Kiểm tra: {product == large_number}")
```

### 8.2. Chinese Remainder Theorem

Giải hệ phương trình đồng dư.

**C++:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

// Extended Euclidean Algorithm
long long extgcd(long long a, long long b, long long &x, long long &y) {
    if (b == 0) {
        x = 1; y = 0;
        return a;
    }
    long long x1, y1;
    long long gcd = extgcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - (a / b) * y1;
    return gcd;
}

long long chineseRemainder(vector<long long> remainders, vector<long long> moduli) {
    /*
     * Giải hệ phương trình:
     * x ≡ remainders[0] (mod moduli[0])
     * x ≡ remainders[1] (mod moduli[1])
     * ...
     * 
     * Điều kiện: các moduli phải đôi một nguyên tố cùng nhau
     */
    long long result = remainders[0];
    long long lcm = moduli[0];
    
    for (int i = 1; i < remainders.size(); i++) {
        long long a = lcm;
        long long b = moduli[i];
        long long c = remainders[i] - result;
        
        long long x, y;
        long long gcd = extgcd(a, b, x, y);
        
        if (c % gcd != 0) {
            return -1;  // Không có nghiệm
        }
        
        result += lcm * (c / gcd) * x;
        lcm *= moduli[i] / gcd;
        result = ((result % lcm) + lcm) % lcm;
    }
    
    return result;
}

int main() {
    // Giải hệ:
    // x ≡ 2 (mod 3)
    // x ≡ 3 (mod 5)  
    // x ≡ 2 (mod 7)
    
    vector<long long> remainders = {2, 3, 2};
    vector<long long> moduli = {3, 5, 7};
    
    long long solution = chineseRemainder(remainders, moduli);
    
    cout << "Nghiệm của hệ phương trình đồng dư: " << solution << endl;
    
    // Kiểm tra
    for (int i = 0; i < remainders.size(); i++) {
        cout << solution << " ≡ " << (solution % moduli[i]) 
             << " (mod " << moduli[i] << ")" << endl;
    }
    
    return 0;
}
```

---

## 9. Bài tập Olympic/ICPC Style

### Bài tập 1: Prime Distance (SPOJ)
Tìm khoảng cách nhỏ nhất và lớn nhất giữa các số nguyên tố liên tiếp trong [L, R].

**Python Solution:**
```python
def solve_prime_distance(L, R):
    """
    Tìm khoảng cách min/max giữa số nguyên tố liên tiếp trong [L, R]
    """
    primes = segmented_sieve(L, R)
    
    if len(primes) < 2:
        return None, None
    
    min_gap = float('inf')
    max_gap = 0
    min_pair = None
    max_pair = None
    
    for i in range(len(primes) - 1):
        gap = primes[i + 1] - primes[i]
        
        if gap < min_gap:
            min_gap = gap
            min_pair = (primes[i], primes[i + 1])
        
        if gap > max_gap:
            max_gap = gap
            max_pair = (primes[i], primes[i + 1])
    
    return min_pair, max_pair

# Test
L, R = 2, 50
min_pair, max_pair = solve_prime_distance(L, R)
print(f"Trong [{L}, {R}]:")
print(f"Khoảng cách nhỏ nhất: {min_pair[1] - min_pair[0]} giữa {min_pair}")
print(f"Khoảng cách lớn nhất: {max_pair[1] - max_pair[0]} giữa {max_pair}")
```

### Bài tập 2: T-primes (Codeforces)
Số T-prime là số có đúng 3 ước (1, √n, n). Tức là n = p² với p nguyên tố.

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
#include <cmath>
using namespace std;

unordered_set<long long> generateTPrimes(long long limit) {
    /*
     * Tạo tất cả T-primes đến limit
     * T-prime = p^2 với p là số nguyên tố
     */
    unordered_set<long long> tprimes;
    
    // Tìm số nguyên tố đến sqrt(limit)
    long long maxP = sqrt(limit) + 1;
    vector<bool> isPrime = sieve(maxP);
    
    for (long long p = 2; p <= maxP; p++) {
        if (isPrime[p]) {
            long long tprime = p * p;
            if (tprime <= limit) {
                tprimes.insert(tprime);
            }
        }
    }
    
    return tprimes;
}

bool isTPrime(long long n) {
    /*
     * Kiểm tra n có phải T-prime không
     * Cách 1: Kiểm tra √n có phải nguyên tố
     */
    if (n < 4) return false;
    
    long long root = sqrt(n);
    if (root * root != n) return false;
    
    return isPrimeOptimized(root);
}

int main() {
    // Precompute T-primes
    unordered_set<long long> tprimes = generateTPrimes(1000000);
    
    vector<long long> testCases = {4, 9, 25, 49, 121, 16, 36, 100};
    
    for (long long n : testCases) {
        cout << n << " là T-prime: " << (tprimes.count(n) > 0) << endl;
        cout << n << " là T-prime (kiểm tra trực tiếp): " << isTPrime(n) << endl;
    }
    
    return 0;
}
```

### Bài tập 3: Almost Prime
Số Almost Prime bậc k là số có đúng k thừa số nguyên tố (tính cả lũy thừa).

**Python:**
```python
def count_prime_factors(n):
    """
    Đếm số lượng thừa số nguyên tố (tính cả lũy thừa)
    Ví dụ: 12 = 2² × 3 → có 3 thừa số nguyên tố
    """
    count = 0
    
    # Xử lý thừa số 2
    while n % 2 == 0:
        count += 1
        n //= 2
    
    # Xử lý thừa số lẻ
    i = 3
    while i * i <= n:
        while n % i == 0:
            count += 1
            n //= i
        i += 2
    
    # Nếu n > 1 thì n là thừa số nguyên tố
    if n > 1:
        count += 1
    
    return count

def find_almost_primes(k, limit):
    """
    Tìm tất cả Almost Prime bậc k đến limit
    """
    almost_primes = []
    
    for n in range(2, limit + 1):
        if count_prime_factors(n) == k:
            almost_primes.append(n)
    
    return almost_primes

# Test
for k in range(1, 5):
    ap = find_almost_primes(k, 30)
    print(f"Almost Prime bậc {k} đến 30: {ap}")

# Bài toán cụ thể: Đếm Almost Prime bậc 2 từ a đến b
def count_almost_prime_2(a, b):
    count = 0
    for n in range(a, b + 1):
        if count_prime_factors(n) == 2:
            count += 1
    return count

print(f"\nSố Almost Prime bậc 2 từ 1 đến 100: {count_almost_prime_2(1, 100)}")
```

---

## 10. Tips và Tricks cho Contest

### 10.1. Fast I/O và Optimization

**C++:**
```cpp
#include <iostream>
#include <vector>
using namespace std;

// Fast I/O
void fastIO() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
}

// Precompute cho multiple test cases
const int MAXN = 1000000;
vector<bool> is_prime(MAXN + 1, true);
vector<int> primes;

void precompute() {
    is_prime[0] = is_prime[1] = false;
    
    for (int i = 2; i <= MAXN; i++) {
        if (is_prime[i]) {
            primes.push_back(i);
            if ((long long)i * i <= MAXN) {
                for (int j = i * i; j <= MAXN; j += i) {
                    is_prime[j] = false;
                }
            }
        }
    }
}

int main() {
    fastIO();
    precompute();
    
    int t;
    cin >> t;
    
    while (t--) {
        int n;
        cin >> n;
        
        // Xử lý truy vấn với dữ liệu đã precompute
        cout << (n <= MAXN ? is_prime[n] : millerRabin(n)) << "\n";
    }
    
    return 0;
}
```

### 10.2. Memory Optimization

**Python:**
```python
# Sử dụng bitset để tiết kiệm bộ nhớ
def memory_efficient_sieve(n):
    """
    Sàng Eratosthenes tiết kiệm bộ nhớ
    Chỉ lưu số lẻ và sử dụng bit manipulation
    """
    if n < 2:
        return []
    if n == 2:
        return [2]
    
    # Chỉ lưu số lẻ từ 3
    size = (n - 1) // 2
    sieve = [True] * size  # sieve[i] tương ứng với số 2*i + 3
    
    # Sàng
    for i in range((int(n**0.5) - 1) // 2):
        if sieve[i]:
            num = 2 * i + 3
            # Bắt đầu từ num^2
            for j in range((num * num - 3) // 2, size, num):
                sieve[j] = False
    
    # Tạo kết quả
    primes = [2]
    for i in range(size):
        if sieve[i]:
            primes.append(2 * i + 3)
    
    return primes

# Test hiệu suất
import time

start = time.time()
primes = memory_efficient_sieve(1000000)
end = time.time()

print(f"Tìm {len(primes)} số nguyên tố đến 10^6 trong {end - start:.3f}s")
```

### 10.3. Common Patterns

**Template cho bài số nguyên tố:**

```python
def solve():
    # Pattern 1: Kiểm tra 1 số
    n = int(input())
    print("YES" if is_prime_optimized(n) else "NO")

def solve_multiple():
    # Pattern 2: Nhiều truy vấn - precompute
    MAXN = 10**6
    primes, is_prime = sieve_of_eratosthenes(MAXN)
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n <= MAXN:
            print("YES" if is_prime[n] else "NO")
        else:
            print("YES" if miller_rabin(n) else "NO")

def solve_range():
    # Pattern 3: Khoảng [L, R]
    L, R = map(int, input().split())
    primes = segmented_sieve(L, R)
    print(len(primes))

def solve_factorization():
    # Pattern 4: Phân tích thừa số
    n = int(input())
    factors = prime_factorization(n)
    for prime, power in factors:
        print(f"{prime}^{power}", end=" ")
    print()
```

---

## 11. Debugging và Testing

### 11.1. Test Cases Generator

**Python:**
```python
import random

def generate_test_cases():
    """
    Tạo test cases cho bài toán số nguyên tố
    """
    test_cases = []
    
    # Small primes
    small_primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]
    test_cases.extend(small_primes)
    
    # Small composites
    small_composites = [4, 6, 8, 9, 10, 12, 14, 15, 16, 18, 20]
    test_cases.extend(small_composites)
    
    # Edge cases
    edge_cases = [1, 2, 10**9, 10**9 + 7, 2**31 - 1]
    test_cases.extend(edge_cases)
    
    # Random large numbers
    for _ in range(10):
        test_cases.append(random.randint(10**8, 10**9))
    
    return test_cases

def verify_implementations():
    """
    So sánh các implementation khác nhau
    """
    test_cases = generate_test_cases()
    
    for n in test_cases:
        if n <= 10**6:  # Chỉ test với số không quá lớn cho naive
            naive_result = is_prime_naive(n) if n <= 10000 else None
            optimized_result = is_prime_optimized(n)
            miller_result = miller_rabin(n)
            
            print(f"n = {n}")
            if naive_result is not None:
                print(f"  Naive: {naive_result}")
            print(f"  Optimized: {optimized_result}")
            print(f"  Miller-Rabin: {miller_result}")
            
            # Kiểm tra consistency
            if naive_result is not None and naive_result != optimized_result:
                print(f"  ❌ MISMATCH: Naive vs Optimized")
            if optimized_result != miller_result:
                print(f"  ⚠️  POSSIBLE MISMATCH: Optimized vs Miller-Rabin")
            print()

# Chạy verification
verify_implementations()
```

### 11.2. Performance Benchmarking

**C++:**
```cpp
#include <iostream>
#include <chrono>
#include <vector>
using namespace std;
using namespace chrono;

void benchmark_prime_checking() {
    vector<int> test_numbers;
    
    // Tạo test data
    for (int i = 1000000; i < 1000100; i++) {
        test_numbers.push_back(i);
    }
    
    // Benchmark naive approach
    auto start = high_resolution_clock::now();
    int naive_count = 0;
    for (int n : test_numbers) {
        if (isPrimeNaive(n)) naive_count++;
    }
    auto end = high_resolution_clock::now();
    auto naive_time = duration_cast<milliseconds>(end - start);
    
    // Benchmark optimized approach
    start = high_resolution_clock::now();
    int optimized_count = 0;
    for (int n : test_numbers) {
        if (isPrimeOptimized(n)) optimized_count++;
    }
    end = high_resolution_clock::now();
    auto optimized_time = duration_cast<milliseconds>(end - start);
    
    // Benchmark sieve approach
    start = high_resolution_clock::now();
    vector<bool> is_prime = sieve(1000100);
    int sieve_count = 0;
    for (int n : test_numbers) {
        if (is_prime[n]) sieve_count++;
    }
    end = high_resolution_clock::now();
    auto sieve_time = duration_cast<milliseconds>(end - start);
    
    cout << "Performance Benchmark (100 numbers around 10^6):" << endl;
    cout << "Naive: " << naive_time.count() << "ms, found " << naive_count << " primes" << endl;
    cout << "Optimized: " << optimized_time.count() << "ms, found " << optimized_count << " primes" << endl;
    cout << "Sieve: " << sieve_time.count() << "ms, found " << sieve_count << " primes" << endl;
}

int main() {
    benchmark_prime_checking();
    return 0;
}
```

---

## 12. Cheat Sheet cho Contest

### Quick Reference

```cpp
// Kiểm tra nhanh số nguyên tố
bool isPrime(long long n) {
    if (n < 2) return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;
    for (long long i = 3; i * i <= n; i += 2)
        if (n % i == 0) return false;
    return true;
}

// Sàng nhanh
vector<int> sieve(int n) {
    vector<bool> is_prime(n + 1, true);
    vector<int> primes;
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) {
            primes.push_back(i);
            for (long long j = (long long)i * i; j <= n; j += i)
                is_prime[j] = false;
        }
    }
    return primes;
}

// Phân tích thừa số nhanh
vector<pair<int, int>> factorize(int n) {
    vector<pair<int, int>> factors;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            int cnt = 0;
            while (n % i == 0) {
                cnt++;
                n /= i;
            }
            factors.push_back({i, cnt});
        }
    }
    if (n > 1) factors.push_back({n, 1});
    return factors;
}

// GCD/LCM
long long gcd(long long a, long long b) {
    return b ? gcd(b, a % b) : a;
}

long long lcm(long long a, long long b) {
    return a / gcd(a, b) * b;
}
```

### Common Prime Patterns

1. **Số nguyên tố < 10⁶**: Dùng sàng Eratosthenes
2. **Số nguyên tố < 10¹²**: Dùng √n optimization  
3. **Số nguyên tố > 10¹²**: Dùng Miller-Rabin
4. **Nhiều truy vấn**: Precompute bằng sàng
5. **Khoảng [L, R] lớn**: Sàng phân đoạn
6. **Phân tích thừa số**: Trial division + Pollard's Rho

### Memory Limits

- **Sàng đến 10⁶**: ~1MB
- **Sàng đến 10⁷**: ~10MB  
- **Sàng đến 10⁸**: ~100MB
- **Bitset optimization**: Giảm 8 lần bộ nhớ

---

## Kết luận

Chuyên đề này đã trình bày đầy đủ các thuật toán về số nguyên tố từ cơ bản đến nâng cao, phù hợp cho các cuộc thi lập trình như APIO, IOI, CEOI và các contest online.

### Những điểm quan trọng cần nhớ:

1. **Lựa chọn thuật toán phù hợp** với giới hạn bài toán
2. **Tối ưu hóa** cho multiple test cases bằng precomputation
3. **Xử lý overflow** khi làm việc với số lớn
4. **Test kỹ lưỡng** với các edge cases
5. **Hiểu rõ độ phức tạp** của từng thuật toán

### Lộ trình học tập đề xuất:

1. **Tuần 1**: Làm chủ kiểm tra số nguyên tố cơ bản và sàng Eratosthenes
2. **Tuần 2**: Thực hành sàng phân đoạn và các bài toán ứng dụng
3. **Tuần 3**: Học Miller-Rabin và các thuật toán cho số lớn
4. **Tuần 4**: Giải các bài tập Olympic/ICPC và tối ưu code

**Chúc các bạn thành công trong các cuộc thi lập trình!** 🏆