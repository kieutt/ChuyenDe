# BÀI GIẢNG: SỐ NGUYÊN TỐ VÀ ỨNG DỤNG TRONG LẬP TRÌNH

## I. KHÁI NIỆM CƠ BẢN

### 1.1. Định nghĩa số nguyên tố

**Số nguyên tố** là số tự nhiên lớn hơn 1 và chỉ có đúng hai ước số dương: 1 và chính nó.

**Định nghĩa toán học:**
Một số p được gọi là số nguyên tố nếu:
- p là số tự nhiên (p ≥ 2)
- p không có ước số nào khác ngoài 1 và p
- Nếu p = a × b thì hoặc a = 1, b = p hoặc a = p, b = 1

**Lưu ý quan trọng:**
- Số 1 KHÔNG phải số nguyên tố (cũng không phải hợp số)
- Số 2 là số nguyên tố chẵn duy nhất
- Mọi số nguyên tố khác đều là số lẻ

### 1.2. Danh sách số nguyên tố từ 1 đến 100

Có tổng cộng 25 số nguyên tố trong khoảng [1, 100]:

```
2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
73, 79, 83, 89, 97
```

### 1.3. So sánh số nguyên tố và hợp số

| Số nguyên tố | Hợp số |
|--------------|--------|
| Chỉ có 2 ước: 1 và chính nó | Có nhiều hơn 2 ước số |
| Tất cả đều lẻ (trừ số 2) | Có thể chẵn hoặc lẻ |
| Ví dụ: 2, 3, 5, 7, 11... | Ví dụ: 4, 6, 8, 9, 10... |

## II. TÍNH CHẤT QUAN TRỌNG

### 2.1. Tính chất cơ bản
- Có vô hạn số nguyên tố (định lý Euclid, 300 TCN)
- Không tồn tại số nguyên tố lớn nhất
- Số nguyên tố nhỏ nhất là 2

### 2.2. Định lý cơ bản của số học
Mọi số nguyên dương lớn hơn 1 đều có thể phân tích duy nhất thành tích các thừa số nguyên tố (không kể thứ tự).

**Ví dụ phân tích thừa số nguyên tố:**
- 60 = 2² × 3 × 5
- 72 = 2³ × 3²
- 84 = 2² × 3 × 7

### 2.3. Số nguyên tố cùng nhau (Coprime)
Hai số được gọi là nguyên tố cùng nhau nếu ƯCLN của chúng bằng 1.

**Ví dụ:** 21 và 22 là nguyên tố cùng nhau vì:
- Ước của 21: {1, 3, 7, 21}
- Ước của 22: {1, 2, 11, 22}
- Ước chung duy nhất: 1

## III. THUẬT TOÁN KIỂM TRA SỐ NGUYÊN TỐ

### 3.1. Thuật toán cơ bản - O(n)

```python
def is_prime_basic(n):
    if n < 2:
        return False
    for i in range(2, n):
        if n % i == 0:
            return False
    return True
```

### 3.2. Thuật toán tối ưu - O(√n)

```python
import math

def is_prime_optimized(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    
    # Chỉ kiểm tra đến √n
    for i in range(3, int(math.sqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    return True
```

**Giải thích tối ưu:**
- Nếu n có ước a > √n thì phải có ước b = n/a < √n
- Vậy chỉ cần kiểm tra đến √n là đủ
- Bỏ qua các số chẵn (trừ 2) vì chúng không thể là nguyên tố

### 3.3. Kiểm tra với dạng 6k±1

```python
def is_prime_6k(n):
    if n <= 1:
        return False
    if n <= 3:
        return True
    if n % 2 == 0 or n % 3 == 0:
        return False
    
    # Mọi số nguyên tố > 3 có dạng 6k±1
    i = 5
    while i * i <= n:
        if n % i == 0 or n % (i + 2) == 0:
            return False
        i += 6
    return True
```

## IV. THUẬT TOÁN TÌM TẤT CẢ SỐ NGUYÊN TỐ

### 4.1. Sàng Eratosthenes - O(n log log n)

```python
def sieve_of_eratosthenes(limit):
    """Tìm tất cả số nguyên tố từ 2 đến limit"""
    if limit < 2:
        return []
    
    # Khởi tạo mảng đánh dấu
    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    
    # Sàng
    for i in range(2, int(limit**0.5) + 1):
        if is_prime[i]:
            # Đánh dấu bội của i không phải nguyên tố
            for j in range(i*i, limit + 1, i):
                is_prime[j] = False
    
    # Thu thập kết quả
    primes = [i for i in range(2, limit + 1) if is_prime[i]]
    return primes
```

### 4.2. Sàng phân đoạn (Segmented Sieve)

```python
def segmented_sieve(n):
    """Tìm số nguyên tố đến n với bộ nhớ O(√n)"""
    import math
    
    limit = int(math.sqrt(n)) + 1
    base_primes = sieve_of_eratosthenes(limit)
    
    primes = base_primes.copy()
    
    # Xử lý từng đoạn
    segment_size = max(limit, 32768)
    
    for low in range(limit, n + 1, segment_size):
        high = min(low + segment_size - 1, n)
        is_prime_segment = [True] * (high - low + 1)
        
        # Sàng với các số nguyên tố cơ sở
        for p in base_primes:
            start = max(p * p, (low + p - 1) // p * p)
            for j in range(start, high + 1, p):
                is_prime_segment[j - low] = False
        
        # Thu thập kết quả
        for i in range(high - low + 1):
            if is_prime_segment[i]:
                primes.append(low + i)
    
    return primes
```

## V. ỨNG DỤNG TRONG TIN HỌC

### 5.1. Mã hóa RSA

```python
def simple_rsa_demo():
    """Minh họa nguyên lý RSA đơn giản"""
    # Chọn 2 số nguyên tố
    p, q = 61, 53
    n = p * q  # 3233
    
    # Hàm Euler
    phi = (p - 1) * (q - 1)  # 3120
    
    # Chọn e (thường là 65537)
    e = 17  # Nguyên tố cùng nhau với phi
    
    # Tính d (nghịch đảo modular của e)
    d = pow(e, -1, phi)  # Python 3.8+
    
    print(f"Public key: (n={n}, e={e})")
    print(f"Private key: (n={n}, d={d})")
    
    # Mã hóa
    message = 123
    encrypted = pow(message, e, n)
    
    # Giải mã
    decrypted = pow(encrypted, d, n)
    
    print(f"Original: {message}")
    print(f"Encrypted: {encrypted}")
    print(f"Decrypted: {decrypted}")
```

### 5.2. Hàm băm (Hash Functions)

```python
def simple_hash_with_prime(data, table_size=101):
    """Hàm băm sử dụng số nguyên tố"""
    # Sử dụng số nguyên tố làm kích thước bảng
    # giúp giảm collision
    hash_value = 0
    prime = 31  # Số nguyên tố nhỏ
    
    for char in str(data):
        hash_value = (hash_value * prime + ord(char)) % table_size
    
    return hash_value
```

### 5.3. Kiểm tra tính nguyên thủy (Miller-Rabin)

```python
import random

def miller_rabin(n, k=5):
    """Test Miller-Rabin với độ chính xác cao"""
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False
    
    # Viết n-1 = 2^r * d
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2
    
    # Test k lần
    for _ in range(k):
        a = random.randrange(2, n - 1)
        x = pow(a, d, n)
        
        if x == 1 or x == n - 1:
            continue
        
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False
    
    return True
```

## VI. BÀI TẬP THỰC HÀNH

### Bài tập cơ bản

1. **Kiểm tra số nguyên tố**: Viết hàm kiểm tra n có phải số nguyên tố không với độ phức tạp O(√n)

2. **Đếm số nguyên tố**: Đếm có bao nhiêu số nguyên tố trong đoạn [L, R]

3. **Tổng số nguyên tố**: Tính tổng n số nguyên tố đầu tiên

### Bài tập nâng cao

4. **Số nguyên tố sinh đôi**: Tìm tất cả cặp số nguyên tố sinh đôi (p, p+2) trong khoảng [1, n]

5. **Phân tích thừa số**: Phân tích n thành tích các thừa số nguyên tố

6. **Goldbach Conjecture**: Kiểm tra mọi số chẵn > 2 có thể viết thành tổng 2 số nguyên tố

### Bài tập ứng dụng

7. **Mã hóa đơn giản**: Implement thuật toán mã hóa RSA đơn giản với p, q < 100

8. **Hash Table**: Xây dựng hash table với kích thước là số nguyên tố để tối ưu collision

9. **Lucky Numbers**: Số may mắn là số chỉ chứa các chữ số nguyên tố (2,3,5,7). Đếm số lượng số may mắn ≤ n

## VII. LỜI GIẢI MẪU

### Giải bài tập 1: Kiểm tra số nguyên tố

```python
def is_prime(n):
    """Kiểm tra số nguyên tố với O(√n)"""
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    
    i = 3
    while i * i <= n:
        if n % i == 0:
            return False
        i += 2
    return True

# Test
test_cases = [2, 17, 25, 29, 37, 41, 97]
for num in test_cases:
    print(f"{num}: {'Nguyên tố' if is_prime(num) else 'Không nguyên tố'}")
```

### Giải bài tập 4: Số nguyên tố sinh đôi

```python
def find_twin_primes(n):
    """Tìm cặp số nguyên tố sinh đôi đến n"""
    primes = sieve_of_eratosthenes(n)
    prime_set = set(primes)
    
    twin_primes = []
    for p in primes:
        if p + 2 in prime_set:
            twin_primes.append((p, p + 2))
    
    return twin_primes

# Test: Tìm sinh đôi từ 1-100
twins = find_twin_primes(100)
print("Số nguyên tố sinh đôi từ 1-100:")
for pair in twins:
    print(pair)
```

## VIII. TỔNG KẾT

Số nguyên tố không chỉ là khái niệm toán học cơ bản mà còn là nền tảng cho nhiều ứng dụng quan trọng trong tin học:

1. **An ninh mạng**: RSA, Diffie-Hellman, ECC
2. **Cấu trúc dữ liệu**: Hash tables, Bloom filters
3. **Thuật toán**: Random number generation, Error correction codes
4. **Lý thuyết số tính toán**: Factorization, Primality testing

Việc nắm vững các thuật toán liên quan đến số nguyên tố sẽ giúp các em giải quyết hiệu quả nhiều bài toán trong lập trình thi đấu và ứng dụng thực tế.

## IX. TÀI LIỆU THAM KHẢO

1. Introduction to Algorithms - Cormen, Leiserson, Rivest, Stein
2. The Art of Computer Programming Vol. 2 - Donald Knuth
3. Competitive Programming Handbook - Antti Laaksonen
4. Number Theory for Computing - Song Y. Yan