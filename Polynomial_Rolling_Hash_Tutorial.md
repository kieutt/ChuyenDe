# Chuyên đề: Polynomial Rolling Hash

## Mục lục

1. [Giới thiệu](#1-giới-thiệu)
2. [Hàm băm đa thức](#2-hàm-băm-đa-thức)
3. [Lựa chọn tham số](#3-lựa-chọn-tham-số)
4. [Cài đặt cơ bản](#4-cài-đặt-cơ-bản)
5. [Vấn đề va chạm và Double Hashing](#5-vấn-đề-va-chạm-và-double-hashing)
6. [Tính hash xâu con trong O(1)](#6-tính-hash-xâu-con-trong-o1)
7. [Các kỹ thuật nâng cao](#7-các-kỹ-thuật-nâng-cao)
8. [Ứng dụng](#8-ứng-dụng)
9. [Bài tập thực hành](#9-bài-tập-thực-hành)
10. [Lỗi thường gặp](#10-lỗi-thường-gặp)

---

## 1. Giới thiệu

### 1.1. Hàm băm là gì?

**Hàm băm (Hash Function)** là hàm ánh xạ dữ liệu có kích thước tùy ý sang giá trị có kích thước cố định. Giá trị trả về được gọi là **giá trị băm (hash value)** hoặc **digest**.

**Tính chất quan trọng:**
- Nếu hai xâu **bằng nhau**, hash của chúng **chắc chắn bằng nhau**
- Nếu hai xâu **khác nhau**, hash của chúng **có thể bằng nhau** (va chạm - collision)

### 1.2. Tại sao cần String Hashing?

| Thao tác | Không dùng Hash | Dùng Hash |
|----------|-----------------|-----------|
| So sánh 2 xâu độ dài n | O(n) | O(1)* |
| Tìm xâu con (pattern matching) | O(n×m) | O(n+m) |
| Đếm xâu con phân biệt | O(n²×k) | O(n²) |

*Giả sử hash đã được tính trước

### 1.3. Ứng dụng phổ biến

- **Tìm kiếm xâu con** (Rabin-Karp Algorithm)
- **So sánh xâu con** trong O(1)
- **Tìm Longest Common Substring/Prefix**
- **Đếm số xâu con phân biệt**
- **Palindrome checking**
- **So khớp chuỗi trong các bài toán DP**

---

## 2. Hàm băm đa thức

### 2.1. Công thức

**Polynomial Rolling Hash** sử dụng công thức:

$$\text{hash}(s) = \sum_{i=0}^{n-1} s[i] \cdot p^i \mod m$$

Hoặc viết đầy đủ:

$$\text{hash}(s) = s[0] \cdot p^0 + s[1] \cdot p^1 + s[2] \cdot p^2 + \ldots + s[n-1] \cdot p^{n-1} \mod m$$

Trong đó:
- $s[i]$ là giá trị số của ký tự thứ $i$ (thường là `s[i] - 'a' + 1`)
- $p$ là **cơ số (base)**, thường là số nguyên tố
- $m$ là **modulo**, thường là số nguyên tố lớn
- $n$ là độ dài xâu

### 2.2. Biến thể ngược

Một số tài liệu sử dụng công thức ngược (lũy thừa giảm dần):

$$\text{hash}(s) = s[0] \cdot p^{n-1} + s[1] \cdot p^{n-2} + \ldots + s[n-1] \cdot p^0 \mod m$$

**Ưu điểm:** Dễ tính hash xâu con hơn (không cần modular inverse)

**Công thức này tương đương với việc coi xâu như một số trong hệ cơ số $p$.**

### 2.3. Ví dụ minh họa

Tính hash của xâu `"abc"` với $p = 31$, $m = 10^9 + 7$:

```
s[0] = 'a' - 'a' + 1 = 1
s[1] = 'b' - 'a' + 1 = 2
s[2] = 'c' - 'a' + 1 = 3

hash = 1×31⁰ + 2×31¹ + 3×31² 
     = 1×1 + 2×31 + 3×961
     = 1 + 62 + 2883
     = 2946
```

---

## 3. Lựa chọn tham số

### 3.1. Chọn cơ số p

| Loại ký tự | Giá trị p đề xuất |
|------------|-------------------|
| Chỉ chữ thường (a-z) | 31, 37 |
| Chữ thường + hoa (a-z, A-Z) | 53, 59 |
| Mọi ký tự ASCII | 131, 137 |

**Nguyên tắc:** $p$ nên là số nguyên tố và lớn hơn số ký tự khác nhau trong bảng chữ cái.

**Trong competitive programming**, các giá trị phổ biến:
- `p = 31` hoặc `p = 37` (cho chữ thường)
- `p = 131` hoặc `p = 13331` (cho mọi ký tự)

### 3.2. Chọn modulo m

**Yêu cầu:**
- $m$ phải là số nguyên tố lớn
- $m$ nên vừa với kiểu `long long` (< 2⁶³)

**Giá trị phổ biến:**
- $m = 10^9 + 7$ (1000000007)
- $m = 10^9 + 9$ (1000000009)
- $m = 2^{61} - 1$ (Mersenne prime, cho phép dùng `__int128`)

### 3.3. Xác suất va chạm

Xác suất hai xâu **khác nhau** có cùng hash:

$$P(\text{collision}) \approx \frac{1}{m}$$

Với $m = 10^9 + 7$, xác suất va chạm khoảng $10^{-9}$.

**Birthday Paradox:** Nếu có $k$ xâu, xác suất có ít nhất một cặp va chạm:

$$P \approx 1 - e^{-\frac{k^2}{2m}}$$

Với $k = 10^5$ và $m = 10^9$, xác suất va chạm khoảng **0.5%** — đủ để bị hack!

---

## 4. Cài đặt cơ bản

### 4.1. Tính hash của một xâu

```cpp
typedef long long ll;
const ll MOD = 1e9 + 7;
const ll BASE = 31;

ll computeHash(const string& s) {
    ll hash_value = 0;
    ll p_pow = 1;
    
    for (char c : s) {
        hash_value = (hash_value + (c - 'a' + 1) * p_pow) % MOD;
        p_pow = (p_pow * BASE) % MOD;
    }
    
    return hash_value;
}
```

### 4.2. Tiền xử lý hash tiền tố

Để tính hash xâu con trong O(1), ta cần lưu hash của tất cả tiền tố:

```cpp
const int MAXN = 1e6 + 5;
ll hash_prefix[MAXN];  // hash_prefix[i] = hash của s[0..i-1]
ll p_pow[MAXN];        // p_pow[i] = BASE^i mod MOD

void precompute(const string& s) {
    int n = s.size();
    
    // Tính lũy thừa của BASE
    p_pow[0] = 1;
    for (int i = 1; i <= n; i++) {
        p_pow[i] = (p_pow[i-1] * BASE) % MOD;
    }
    
    // Tính hash tiền tố
    hash_prefix[0] = 0;
    for (int i = 0; i < n; i++) {
        hash_prefix[i+1] = (hash_prefix[i] + (s[i] - 'a' + 1) * p_pow[i]) % MOD;
    }
}
```

### 4.3. Độ phức tạp

| Thao tác | Thời gian | Bộ nhớ |
|----------|-----------|--------|
| Tính hash một xâu | O(n) | O(1) |
| Tiền xử lý | O(n) | O(n) |
| Tính hash xâu con | O(1) | - |

---

## 5. Vấn đề va chạm và Double Hashing

### 5.1. Ví dụ va chạm thực tế

Với $p = 31$ và $m = 10^9 + 7$:
- `"countermand"` và `"furnace"` có **cùng hash**

Với $p = 37$ và $m = 10^9 + 9$:
- `"answers"` và `"stead"` có **cùng hash**

### 5.2. Tại sao va chạm nguy hiểm?

Trong competitive programming, **anti-hash test** có thể được tạo để làm sai lời giải dùng single hash. Kẻ tấn công có thể:
1. Tìm các cặp xâu có cùng hash
2. Tạo test case khiến thuật toán cho kết quả sai

### 5.3. Double Hashing

**Ý tưởng:** Dùng **hai hàm hash** với các tham số khác nhau. Hai xâu chỉ được coi là bằng nhau khi **cả hai hash đều bằng nhau**.

```cpp
struct DoubleHash {
    ll h1, h2;
    
    bool operator==(const DoubleHash& other) const {
        return h1 == other.h1 && h2 == other.h2;
    }
};

const ll MOD1 = 1e9 + 7, MOD2 = 1e9 + 9;
const ll BASE1 = 31, BASE2 = 37;

DoubleHash computeDoubleHash(const string& s) {
    ll hash1 = 0, hash2 = 0;
    ll p1 = 1, p2 = 1;
    
    for (char c : s) {
        int val = c - 'a' + 1;
        hash1 = (hash1 + val * p1) % MOD1;
        hash2 = (hash2 + val * p2) % MOD2;
        p1 = (p1 * BASE1) % MOD1;
        p2 = (p2 * BASE2) % MOD2;
    }
    
    return {hash1, hash2};
}
```

### 5.4. Xác suất va chạm với Double Hash

$$P(\text{collision}) \approx \frac{1}{m_1} \times \frac{1}{m_2} \approx 10^{-18}$$

Xác suất này **cực kỳ nhỏ** — gần như không thể xảy ra trong thực tế.

### 5.5. Natural Overflow (Không dùng MOD)

Một kỹ thuật phổ biến trong competitive programming là để tràn số tự nhiên trong `unsigned long long`:

```cpp
typedef unsigned long long ull;
const ull BASE = 131;

ull computeHash(const string& s) {
    ull hash_value = 0;
    for (char c : s) {
        hash_value = hash_value * BASE + c;
    }
    return hash_value;
}
```

**Ưu điểm:**
- Nhanh hơn (không có phép chia lấy dư)
- Modulo tự động là $2^{64}$

**Nhược điểm:**
- Dễ bị hack hơn nếu chỉ dùng single hash
- Nên kết hợp với double hash

---

## 6. Tính hash xâu con trong O(1)

### 6.1. Công thức (dạng lũy thừa tăng dần)

Với hash tiền tố được tính theo công thức:
$$H[i] = s[0] \cdot p^0 + s[1] \cdot p^1 + \ldots + s[i-1] \cdot p^{i-1}$$

Hash của xâu con $s[l \ldots r]$ (0-indexed):

$$\text{hash}(s[l..r]) = \frac{H[r+1] - H[l]}{p^l} \mod m$$

Cần dùng **modular inverse** để chia:

```cpp
// Tính nghịch đảo modular bằng Fermat nhỏ
ll mod_inverse(ll a, ll m) {
    return power(a, m - 2, m);  // a^(m-2) mod m
}

ll getSubstringHash(int l, int r) {
    ll diff = (hash_prefix[r+1] - hash_prefix[l] + MOD) % MOD;
    ll inv = mod_inverse(p_pow[l], MOD);
    return (diff * inv) % MOD;
}
```

### 6.2. Công thức (dạng lũy thừa giảm dần) — Đơn giản hơn

Nếu hash tiền tố được tính theo công thức:
$$H[i] = s[0] \cdot p^{i-1} + s[1] \cdot p^{i-2} + \ldots + s[i-1] \cdot p^0$$

Hash của xâu con $s[l \ldots r]$:

$$\text{hash}(s[l..r]) = H[r+1] - H[l] \cdot p^{r-l+1} \mod m$$

**Không cần modular inverse!**

```cpp
void precompute(const string& s) {
    int n = s.size();
    
    p_pow[0] = 1;
    for (int i = 1; i <= n; i++) {
        p_pow[i] = (p_pow[i-1] * BASE) % MOD;
    }
    
    hash_prefix[0] = 0;
    for (int i = 0; i < n; i++) {
        hash_prefix[i+1] = (hash_prefix[i] * BASE + (s[i] - 'a' + 1)) % MOD;
    }
}

ll getSubstringHash(int l, int r) {
    // Hash của s[l..r], 0-indexed
    int len = r - l + 1;
    ll result = (hash_prefix[r+1] - hash_prefix[l] * p_pow[len] % MOD + MOD) % MOD;
    return result;
}
```

### 6.3. So sánh hai phương pháp

| Tiêu chí | Lũy thừa tăng dần | Lũy thừa giảm dần |
|----------|-------------------|-------------------|
| Công thức hash xâu con | Cần chia (modular inverse) | Chỉ cần trừ và nhân |
| Tốc độ | Chậm hơn | Nhanh hơn |
| Tiền xử lý | Cần tính inverse | Không cần |
| Phổ biến trong CP | Ít hơn | Phổ biến hơn |

**Khuyến nghị:** Dùng **lũy thừa giảm dần** cho đơn giản và nhanh.

---

## 7. Các kỹ thuật nâng cao

### 7.1. Cập nhật hash khi thay đổi một ký tự

Khi thay đổi $s[i]$ từ $c_1$ sang $c_2$:

$$\text{hash\_new} = \text{hash\_old} - c_1 \cdot p^i + c_2 \cdot p^i \mod m$$

```cpp
ll updateHash(ll old_hash, int pos, char old_char, char new_char) {
    ll diff = (new_char - old_char) * p_pow[pos] % MOD;
    return (old_hash + diff + MOD) % MOD;
}
```

### 7.2. Hash của xâu đảo ngược

Để kiểm tra palindrome, ta cần so sánh hash của xâu gốc và xâu đảo ngược.

```cpp
// Tiền xử lý hash từ phải sang trái
void precomputeReverse(const string& s) {
    int n = s.size();
    
    hash_suffix[n] = 0;
    for (int i = n - 1; i >= 0; i--) {
        hash_suffix[i] = (hash_suffix[i+1] * BASE + (s[i] - 'a' + 1)) % MOD;
    }
}

// Kiểm tra s[l..r] có phải palindrome không
bool isPalindrome(int l, int r) {
    ll forward_hash = getSubstringHash(l, r);
    ll backward_hash = getReverseSubstringHash(l, r);
    return forward_hash == backward_hash;
}
```

### 7.3. Ghép nối hash

Khi ghép hai xâu $A$ và $B$ có độ dài $|A|$ và $|B|$:

$$\text{hash}(A + B) = \text{hash}(A) \cdot p^{|B|} + \text{hash}(B) \mod m$$

```cpp
ll concatHash(ll hash_A, int len_A, ll hash_B, int len_B) {
    return (hash_A * p_pow[len_B] + hash_B) % MOD;
}
```

### 7.4. Template hoàn chỉnh (C++)

```cpp
struct StringHash {
    vector<ll> h1, h2, pw1, pw2;
    static const ll MOD1 = 1e9 + 7, MOD2 = 1e9 + 9;
    static const ll BASE1 = 31, BASE2 = 37;
    int n;
    
    StringHash(const string& s) : n(s.size()) {
        h1.resize(n + 1); h2.resize(n + 1);
        pw1.resize(n + 1); pw2.resize(n + 1);
        
        pw1[0] = pw2[0] = 1;
        h1[0] = h2[0] = 0;
        
        for (int i = 0; i < n; i++) {
            pw1[i+1] = pw1[i] * BASE1 % MOD1;
            pw2[i+1] = pw2[i] * BASE2 % MOD2;
            h1[i+1] = (h1[i] * BASE1 + s[i]) % MOD1;
            h2[i+1] = (h2[i] * BASE2 + s[i]) % MOD2;
        }
    }
    
    // Hash của s[l..r], 0-indexed
    pair<ll, ll> get(int l, int r) {
        int len = r - l + 1;
        ll hash1 = (h1[r+1] - h1[l] * pw1[len] % MOD1 + MOD1) % MOD1;
        ll hash2 = (h2[r+1] - h2[l] * pw2[len] % MOD2 + MOD2) % MOD2;
        return {hash1, hash2};
    }
    
    // So sánh hai xâu con
    bool equal(int l1, int r1, int l2, int r2) {
        return get(l1, r1) == get(l2, r2);
    }
};
```

---

## 8. Ứng dụng

### 8.1. Rabin-Karp: Tìm kiếm xâu con

**Bài toán:** Tìm tất cả vị trí xuất hiện của pattern $P$ trong text $T$.

**Thuật toán:**
1. Tính hash của $P$
2. Duyệt qua tất cả xâu con độ dài $|P|$ của $T$
3. So sánh hash

```cpp
vector<int> rabinKarp(const string& text, const string& pattern) {
    int n = text.size(), m = pattern.size();
    if (m > n) return {};
    
    StringHash ht(text), hp(pattern);
    auto pattern_hash = hp.get(0, m - 1);
    
    vector<int> result;
    for (int i = 0; i + m - 1 < n; i++) {
        if (ht.get(i, i + m - 1) == pattern_hash) {
            result.push_back(i);
        }
    }
    return result;
}
```

**Độ phức tạp:** O(n + m)

### 8.2. Longest Common Prefix với Binary Search

**Bài toán:** Cho hai xâu, tìm độ dài LCP.

```cpp
int longestCommonPrefix(StringHash& h1, StringHash& h2, int i, int j) {
    int lo = 0, hi = min(h1.n - i, h2.n - j);
    int result = 0;
    
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        if (h1.get(i, i + mid - 1) == h2.get(j, j + mid - 1)) {
            result = mid;
            lo = mid + 1;
        } else {
            hi = mid - 1;
        }
    }
    return result;
}
```

**Độ phức tạp:** O(log n) mỗi truy vấn

### 8.3. Đếm số xâu con phân biệt

**Bài toán:** Đếm số xâu con phân biệt của xâu $S$.

```cpp
ll countDistinctSubstrings(const string& s) {
    int n = s.size();
    StringHash h(s);
    
    set<pair<ll, ll>> distinct;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            distinct.insert(h.get(i, j));
        }
    }
    return distinct.size();
}
```

**Độ phức tạp:** O(n² log n)

### 8.4. Kiểm tra Palindrome

```cpp
struct PalindromeHash {
    StringHash forward, backward;
    int n;
    
    PalindromeHash(const string& s) : n(s.size()), forward(s) {
        string rev = s;
        reverse(rev.begin(), rev.end());
        backward = StringHash(rev);
    }
    
    bool isPalindrome(int l, int r) {
        auto h1 = forward.get(l, r);
        auto h2 = backward.get(n - 1 - r, n - 1 - l);
        return h1 == h2;
    }
};
```

---

## 9. Bài tập thực hành

### 9.1. Cơ bản

| Bài | Nguồn | Kỹ thuật |
|-----|-------|----------|
| [String Hashing](https://www.geeksforgeeks.org/problems/challenge-by-nikitasha3208/1) | GFG | Hash cơ bản |
| [Pattern Searching](https://cses.fi/problemset/task/1753) | CSES | Rabin-Karp |
| [Finding Periods](https://cses.fi/problemset/task/1733) | CSES | Hash + số học |

### 9.2. Trung bình

| Bài | Nguồn | Kỹ thuật |
|-----|-------|----------|
| [Distinct Substrings](https://cses.fi/problemset/task/2105) | CSES | Hash + set |
| [Longest Palindrome](https://cses.fi/problemset/task/1111) | CSES | Hash + binary search |
| [String Matching](https://cses.fi/problemset/task/1753) | CSES | Double hash |

### 9.3. Nâng cao

| Bài | Nguồn | Kỹ thuật |
|-----|-------|----------|
| [P14363 Thay thế xâu](https://www.luogu.com.cn/problem/P14363) | Luogu | Hash + brute force |
| [Minimal Rotation](https://cses.fi/problemset/task/1110) | CSES | Hash + so sánh |
| [Palindrome Queries](https://cses.fi/problemset/task/2420) | CSES | Hash + Segment Tree |

---

## 10. Lỗi thường gặp

### 10.1. Overflow

```cpp
// SAI: Tràn số trước khi mod
ll hash = (hash + s[i] * p_pow[i]) % MOD;

// ĐÚNG: Ép kiểu hoặc mod từng bước
ll hash = (hash + (ll)s[i] * p_pow[i] % MOD) % MOD;
```

### 10.2. Số âm khi trừ

```cpp
// SAI: Kết quả có thể âm
ll result = (h[r+1] - h[l] * pw[len]) % MOD;

// ĐÚNG: Cộng MOD trước khi mod
ll result = (h[r+1] - h[l] * pw[len] % MOD + MOD) % MOD;
```

### 10.3. Off-by-one trong chỉ số

```cpp
// Cẩn thận với 0-indexed vs 1-indexed
// h[i] thường là hash của s[0..i-1]
// Xâu con s[l..r] có độ dài len = r - l + 1
```

### 10.4. Quên tiền xử lý lũy thừa

```cpp
// Luôn tiền xử lý pw[] với kích thước đủ lớn
// pw[0] = 1, không phải pw[0] = BASE
```

### 10.5. Dùng single hash trong contest quan trọng

```cpp
// NGUY HIỂM: Có thể bị hack
ll hash = computeHash(s);

// AN TOÀN: Dùng double hash
pair<ll, ll> hash = computeDoubleHash(s);
```

---

## Tổng kết

| Khía cạnh | Khuyến nghị |
|-----------|-------------|
| **Base** | 31 hoặc 37 (chữ thường), 131 (mọi ký tự) |
| **Modulo** | $10^9 + 7$ và $10^9 + 9$ |
| **Số lượng hash** | Double hash để tránh collision |
| **Công thức** | Lũy thừa giảm dần (không cần inverse) |
| **Tối ưu tốc độ** | Natural overflow với `unsigned long long` |

**Polynomial Rolling Hash** là kỹ thuật mạnh mẽ và linh hoạt, được sử dụng rộng rãi trong competitive programming và các ứng dụng xử lý chuỗi. Việc nắm vững kỹ thuật này sẽ giúp bạn giải quyết nhiều bài toán string một cách hiệu quả.

---

*Tài liệu tham khảo:*
- [GeeksforGeeks - String hashing using Polynomial rolling hash function](https://www.geeksforgeeks.org/string-hashing-using-polynomial-rolling-hash-function/)
- [CP-Algorithms - String Hashing](https://cp-algorithms.com/string/string-hashing.html)
- [CSES Problem Set - String Algorithms](https://cses.fi/problemset/)
