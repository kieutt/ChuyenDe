# BÀI GIẢNG: VECTOR C++ VÀ LIST PYPY3
## TỪ CƠ BẢN ĐẾN NÂNG CAO - PHONG CÁCH IOI

---

## PHẦN I: VECTOR C++ - KIẾN THỨC CƠ BẢN

### 1. Vector là gì?

Vector là một mảng động (dynamic array) trong C++. Nó tự động cấp phát bộ nhớ khi cần và giải phóng khi không cần.

**Tại sao vector mạnh?**
- Tự động mở rộng kích thước
- Truy cập O(1) bất kỳ vị trí nào
- Có iterator support
- Là STL container chuẩn

### 2. Cú Pháp Khai Báo Vector

```cpp
#include<vector>
using namespace std;

// Cách 1: Khai báo trống
vector<int> v;                      // Vector rỗng, type int

// Cách 2: Khai báo với kích thước
vector<int> v(10);                  // 10 phần tử, mặc định = 0
vector<int> v(10, 5);               // 10 phần tử, tất cả = 5

// Cách 3: Khởi tạo với giá trị
vector<int> v = {1, 2, 3, 4, 5};    // Initialization list

// Cách 4: Vector 2 chiều
vector<vector<int>> matrix(3, vector<int>(4, 0));  // 3×4 ma trận = 0

// Cách 5: Vector của cấu trúc
struct Point {
    int x, y;
};
vector<Point> points;
```

### 3. Các Thao Tác Cơ Bản

#### a) Thêm Phần Tử

```cpp
vector<int> v;

// Thêm cuối - O(1) amortized
v.push_back(10);                    // [10]
v.push_back(20);                    // [10, 20]

// Thêm tại vị trí - O(n)
v.insert(v.begin() + 1, 15);        // [10, 15, 20]

// Thêm nhiều phần tử
vector<int> v2 = {1, 2, 3};
v.insert(v.end(), v2.begin(), v2.end());  // [10, 15, 20, 1, 2, 3]
```

#### b) Xóa Phần Tử

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Xóa cuối - O(1)
v.pop_back();                       // [1, 2, 3, 4]

// Xóa tại vị trí - O(n)
v.erase(v.begin() + 2);             // [1, 2, 4]

// Xóa dãy
v.erase(v.begin() + 1, v.begin() + 3);  // [1, 4]

// Xóa tất cả
v.clear();                          // []
```

#### c) Truy Cập Phần Tử

```cpp
vector<int> v = {10, 20, 30, 40, 50};

// Truy cập bằng index - O(1)
v[0];                               // 10
v[2];                               // 30

// Truy cập an toàn (kiểm tra ranh giới)
v.at(0);                            // 10 (nếu index ngoài, throw exception)

// Truy cập đầu/cuối - O(1)
v.front();                          // 10 (phần tử đầu)
v.back();                           // 50 (phần tử cuối)

// Iterator
auto it = v.begin();                // Con trỏ tới phần tử đầu
auto it = v.end();                  // Con trỏ tới vị trí sau phần tử cuối
```

#### d) Thông Tin Vector

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Kích thước - số phần tử hiện tại
v.size();                           // 5

// Dung lượng - số phần tử có thể chứa (cấp phát sẵn)
v.capacity();                       // Thường ≥ 5

// Kiểm tra rỗng
v.empty();                          // false

// Cấp phát tối thiểu
v.reserve(100);                     // Cấp phát ít nhất 100 phần tử

// Giải phóng dung lượng dư
v.shrink_to_fit();                  // Giải phóng dung lượng không dùng
```

### 4. Lặp Qua Vector

#### Cách 1: Range-based For Loop (C++11)

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Lặp đơn giản
for(int x : v) {
    cout << x << " ";               // 1 2 3 4 5
}

// Lặp với reference (có thể sửa)
for(int& x : v) {
    x = x * 2;                      // Nhân đôi tất cả
}

// Lặp const (không thể sửa)
for(const int& x : v) {
    cout << x << " ";
}
```

#### Cách 2: For Loop Truyền Thống

```cpp
for(int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}

// Lặp ngược
for(int i = v.size() - 1; i >= 0; i--) {
    cout << v[i] << " ";
}
```

#### Cách 3: Iterator

```cpp
// Lặp với iterator
for(auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";             // Dereference: *it là giá trị
}

// Lặp ngược
for(auto it = v.rbegin(); it != v.rend(); it++) {
    cout << *it << " ";
}
```

### 5. Các Hàm Hữu Ích

```cpp
#include<algorithm>

vector<int> v = {3, 1, 4, 1, 5, 9, 2, 6};

// Sắp xếp
sort(v.begin(), v.end());           // [1, 1, 2, 3, 4, 5, 6, 9]

// Sắp xếp giảm dần
sort(v.rbegin(), v.rend());         // [9, 6, 5, 4, 3, 2, 1, 1]

// Sắp xếp tùy chỉnh
sort(v.begin(), v.end(), [](int a, int b) {
    return a > b;                   // Giảm dần
});

// Đảo thứ tự
reverse(v.begin(), v.end());

// Tìm kiếm
auto it = find(v.begin(), v.end(), 5);
if(it != v.end()) {
    cout << "Tìm thấy tại vị trí: " << (it - v.begin());
}

// Tìm max/min
int maxVal = *max_element(v.begin(), v.end());
int minVal = *min_element(v.begin(), v.end());

// Tính tổng
int sum = accumulate(v.begin(), v.end(), 0);  // Cần #include<numeric>

// Tìm kiếm nhị phân (mảng phải sắp xếp)
bool found = binary_search(v.begin(), v.end(), 5);

// Tìm vị trí cận dưới/trên
auto it_lower = lower_bound(v.begin(), v.end(), 5);  // >= 5
auto it_upper = upper_bound(v.begin(), v.end(), 5);  // > 5

// Unique - xóa các phần tử trùng (mảng phải sắp xếp)
v.erase(unique(v.begin(), v.end()), v.end());

// Count - đếm phần tử
int cnt = count(v.begin(), v.end(), 5);
```

### 6. Độ Phức Tạp Thời Gian

```
push_back():    O(1) amortized
pop_back():     O(1)
insert():       O(n)            (phải dịch chuyển phần tử)
erase():        O(n)            (phải dịch chuyển phần tử)
access [i]:     O(1)
find():         O(n)
sort():         O(n log n)
```

**Quy luật vàng:**
- Thêm/xóa CỨA → O(1)
- Thêm/xóa GIỮA → O(n) (tránh nếu có thể)
- Thêm/xóa ĐẦU → O(n) (dùng deque thay thế)

---

## PHẦN II: LIST PYPY3 - KIẾN THỨC CƠ BẢN

### 1. List là gì?

List trong Python là cấu trúc dữ liệu tương tự vector C++. Nó là mảng động, có thể lưu trữ các phần tử kiểu khác nhau.

**Ưu điểm:**
- Dễ sử dụng, cú pháp ngắn gọn
- Linh hoạt (có thể mix kiểu dữ liệu)
- List comprehension mạnh mẽ
- Slice notation tiện lợi

### 2. Cú Pháp Khai Báo List

```python
# Cách 1: Khai báo trống
a = []                              # List rỗng

# Cách 2: Khai báo với phần tử
a = [1, 2, 3, 4, 5]                 # List có 5 phần tử

# Cách 3: Khởi tạo với kích thước
a = [0] * 10                        # 10 phần tử, tất cả = 0
a = [5] * 10                        # 10 phần tử, tất cả = 5

# Cách 4: List comprehension
a = [i for i in range(10)]          # [0, 1, 2, ..., 9]
a = [i*2 for i in range(10)]        # [0, 2, 4, 6, ...]
a = [i for i in range(10) if i % 2 == 0]  # [0, 2, 4, 6, 8]

# Cách 5: List 2 chiều
matrix = [[0] * 4 for _ in range(3)]  # 3×4 ma trận = 0

# Cách 6: Mix kiểu dữ liệu
a = [1, "hello", 3.14, [1, 2, 3]]   # Có thể mix kiểu

# Cách 7: Dùng hàm list()
a = list(range(10))                 # [0, 1, 2, ..., 9]
a = list("hello")                   # ['h', 'e', 'l', 'l', 'o']
```

### 3. Các Thao Tác Cơ Bản

#### a) Thêm Phần Tử

```python
a = [1, 2, 3]

# Thêm cuối - O(1) amortized
a.append(4)                         # [1, 2, 3, 4]
a.append(5)                         # [1, 2, 3, 4, 5]

# Thêm tại vị trí - O(n)
a.insert(2, 99)                     # [1, 2, 99, 3, 4, 5]

# Mở rộng list
a.extend([6, 7, 8])                 # [1, 2, 99, 3, 4, 5, 6, 7, 8]
# Hoặc: a += [6, 7, 8]
```

#### b) Xóa Phần Tử

```python
a = [1, 2, 3, 4, 5]

# Xóa cuối - O(1)
a.pop()                             # [1, 2, 3, 4], trả về 5

# Xóa tại vị trí - O(n)
a.pop(2)                            # [1, 2, 4]

# Xóa theo giá trị - O(n)
a.remove(4)                         # [1, 2]

# Xóa tất cả
a.clear()                           # []

# Xóa bằng del
a = [1, 2, 3, 4, 5]
del a[2]                            # [1, 2, 4, 5]
del a[1:3]                          # [1, 5]
```

#### c) Truy Cập Phần Tử

```python
a = [10, 20, 30, 40, 50]

# Truy cập bằng index - O(1)
a[0]                                # 10
a[2]                                # 30

# Index âm (từ cuối)
a[-1]                               # 50 (phần tử cuối)
a[-2]                               # 40 (phần tử thứ 2 từ cuối)

# Truy cập đầu/cuối (không hàm, dùng index)
a[0]                                # 10 (đầu)
a[-1]                               # 50 (cuối)
```

#### d) Slice - Cắt List

```python
a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Cắt từ index i đến j-1
a[2:5]                              # [2, 3, 4]
a[2:]                               # [2, 3, 4, 5, 6, 7, 8, 9]
a[:5]                               # [0, 1, 2, 3, 4]

# Cắt với bước
a[::2]                              # [0, 2, 4, 6, 8] (mỗi 2 phần tử)
a[1::2]                             # [1, 3, 5, 7, 9] (lẻ)

# Đảo list
a[::-1]                             # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# Modify slice
a[2:5] = [20, 30, 40]               # Sửa phần tử từ 2 đến 4
```

#### e) Thông Tin List

```python
a = [1, 2, 3, 4, 5]

# Kích thước
len(a)                              # 5

# Kiểm tra rỗng
len(a) == 0                         # False
if not a: pass                      # Cách kiểm tra rỗng

# Đếm phần tử có giá trị x
a.count(3)                          # 1

# Tìm index của phần tử
a.index(3)                          # 2 (index của 3)

# Tìm trong range
a.index(3, 0, 3)                    # Tìm trong a[0:3]
```

### 4. Lặp Qua List

#### Cách 1: For Loop Đơn Giản

```python
a = [1, 2, 3, 4, 5]

for x in a:
    print(x)                        # 1 2 3 4 5
```

#### Cách 2: For Loop Với Index

```python
for i in range(len(a)):
    print(i, a[i])                  # 0 1 / 1 2 / 2 3 / ...
```

#### Cách 3: Enumerate

```python
for i, x in enumerate(a):
    print(i, x)                     # 0 1 / 1 2 / 2 3 / ...
```

#### Cách 4: Lặp Ngược

```python
# Cách 1
for i in range(len(a)-1, -1, -1):
    print(a[i])

# Cách 2
for x in reversed(a):
    print(x)

# Cách 3
for x in a[::-1]:
    print(x)
```

### 5. Các Hàm Hữu Ích

```python
a = [3, 1, 4, 1, 5, 9, 2, 6]

# Sắp xếp
a.sort()                            # [1, 1, 2, 3, 4, 5, 6, 9]
b = sorted(a)                       # Return new list

# Sắp xếp giảm dần
a.sort(reverse=True)                # [9, 6, 5, 4, 3, 2, 1, 1]

# Sắp xếp tùy chỉnh
a.sort(key=lambda x: -x)            # Giảm dần
a.sort(key=lambda x: abs(x))        # Theo giá trị tuyệt đối

# Đảo thứ tự
a.reverse()

# Tìm kiếm
if 5 in a:                          # O(n)
    print("Tìm thấy")

# Index của phần tử
try:
    idx = a.index(5)
except ValueError:
    idx = -1                        # Không tìm thấy

# Đếm phần tử
cnt = a.count(1)

# Min/Max
min(a)                              # 1
max(a)                              # 9

# Tổng
sum(a)                              # 29

# List comprehension filter
b = [x for x in a if x > 4]         # [5, 9, 6]

# List comprehension map
b = [x*2 for x in a]                # Nhân đôi tất cả
```

### 6. Độ Phức Tạp Thời Gian

```
append():       O(1) amortized
pop():          O(1)
insert():       O(n)
remove():       O(n)
access [i]:     O(1)
slice [i:j]:    O(j-i)
in operator:    O(n)
sort():         O(n log n)
count():        O(n)
index():        O(n)
```

**So sánh với C++:**
- Append: C++ O(1)* = Python O(1)*
- Pop: C++ O(1) = Python O(1)
- Insert: C++ O(n) = Python O(n)
- Access: C++ O(1) = Python O(1)

---

## PHẦN III: SO SÁNH CHI TIẾT VECTOR VÀ LIST

### Bảng So Sánh

| Tính năng | C++ Vector | Python List |
|----------|-----------|-----------|
| **Khai báo** | `vector<int> v;` | `a = []` |
| **Kích thước** | `v.size()` | `len(a)` |
| **Thêm cuối** | `v.push_back(x)` | `a.append(x)` |
| **Xóa cuối** | `v.pop_back()` | `a.pop()` |
| **Thêm vị trí** | `v.insert(it, x)` | `a.insert(i, x)` |
| **Xóa vị trí** | `v.erase(it)` | `a.pop(i)` / `del a[i]` |
| **Xóa tất cả** | `v.clear()` | `a.clear()` |
| **Truy cập** | `v[i]` | `a[i]` |
| **Lặp** | `for(int x : v)` | `for x in a:` |
| **Sắp xếp** | `sort(v.begin(), v.end())` | `a.sort()` |
| **Slice** | Không có | `a[i:j]` |
| **Comprehension** | Không có | `[x*2 for x in a]` |

### Ưu Nhược Điểm

**Vector C++:**
- ✅ Nhanh hơn (compiled)
- ✅ Tiết kiệm bộ nhớ
- ✅ Type-safe
- ❌ Cú pháp phức tạp hơn
- ❌ Lặy pháp tìm kiếm phức tạp

**List Python:**
- ✅ Dễ viết, cú pháp gọn
- ✅ Slice mạnh mẽ
- ✅ Comprehension tiện lợi
- ✅ Tìm kiếm dễ dàng
- ❌ Chậm hơn
- ❌ Dùng nhiều bộ nhớ
- ❌ Không type-safe

---

## PHẦN IV: BÀI TẬP - LEVEL CƠ BẢN

### Bài 1: Đọc và In Dãy Số

**Đề bài:** Đọc n số, lưu vào vector/list, sau đó in ra theo thứ tự lệch.

**Input:**
```
5
1 2 3 4 5
```

**Output:**
```
1 2 3 4 5
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    for(int x : v) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

for x in a:
    print(x, end=" ")
print()
```

---

### Bài 2: Tìm Max và Min

**Đề bài:** Tìm giá trị lớn nhất và nhỏ nhất trong dãy n số.

**Input:**
```
5
3 7 2 9 5
```

**Output:**
```
9 2
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    int maxVal = *max_element(v.begin(), v.end());
    int minVal = *min_element(v.begin(), v.end());
    
    cout << maxVal << " " << minVal << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

print(max(a), min(a))
```

---

### Bài 3: Đảo Mảng

**Đề bài:** Đảo thứ tự các phần tử trong mảng.

**Input:**
```
5
1 2 3 4 5
```

**Output:**
```
5 4 3 2 1
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    reverse(v.begin(), v.end());
    
    for(int x : v) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

a.reverse()
# Hoặc: a = a[::-1]

for x in a:
    print(x, end=" ")
print()
```

---

### Bài 4: Sắp Xếp Mảng

**Đề bài:** Sắp xếp mảng theo thứ tự tăng dần.

**Input:**
```
5
5 2 8 1 9
```

**Output:**
```
1 2 5 8 9
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    sort(v.begin(), v.end());
    
    for(int x : v) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

a.sort()

for x in a:
    print(x, end=" ")
print()
```

---

### Bài 5: Đếm Tần Số

**Đề bài:** Đếm số lần xuất hiện của mỗi phần tử.

**Input:**
```
7
1 2 2 3 1 3 3
```

**Output:**
```
1 2
2 2
3 3
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    map<int, int> freq;
    vector<int> order;
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
        if(freq[v[i]] == 0) {
            order.push_back(v[i]);
        }
        freq[v[i]]++;
    }
    
    for(int x : order) {
        cout << x << " " << freq[x] << "\n";
    }
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
from collections import Counter

n = int(input())
a = list(map(int, input().split()))

freq = {}
order = []

for x in a:
    if x not in freq:
        freq[x] = 0
        order.append(x)
    freq[x] += 1

for x in order:
    print(x, freq[x])

# Hoặc dùng Counter (ngắn hơn):
# freq = Counter(a)
# unique = []
# for x in a:
#     if x not in unique:
#         unique.append(x)
# for x in unique:
#     print(x, freq[x])
```

---

## PHẦN V: BÀI TẬP - LEVEL TRUNG BÌNH

### Bài 6: Xóa Phần Tử Trùng

**Đề bài:** Xóa các phần tử trùng, giữ lại một bản copy.

**Input:**
```
7
1 2 2 3 1 3 3
```

**Output:**
```
1 2 3
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    sort(v.begin(), v.end());
    v.erase(unique(v.begin(), v.end()), v.end());
    
    for(int x : v) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

a = list(dict.fromkeys(a))  # Giữ thứ tự, xóa trùng
# Hoặc: a = list(set(a)) nếu không cần giữ thứ tự

for x in a:
    print(x, end=" ")
print()
```

---

### Bài 7: Tổng Dãy Con Liên Tiếp Có Tổng Lớn Nhất (Kadane)

**Đề bài:** Tìm dãy con liên tiếp có tổng lớn nhất.

**Input:**
```
6
-2 1 -3 4 -1 2
```

**Output:**
```
5
```

Giải thích: Dãy [4, -1, 2] có tổng = 5 là lớn nhất.

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    long long maxSum = v[0];
    long long currentSum = v[0];
    
    for(int i = 1; i < n; i++) {
        currentSum = max((long long)v[i], currentSum + v[i]);
        maxSum = max(maxSum, currentSum);
    }
    
    cout << maxSum << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = list(map(int, input().split()))

max_sum = a[0]
current_sum = a[0]

for i in range(1, n):
    current_sum = max(a[i], current_sum + a[i])
    max_sum = max(max_sum, current_sum)

print(max_sum)
```

**Giải thích:**
- Tại mỗi vị trí, ta chọn: tiếp tục từ trước hay bắt đầu lại từ đây
- `currentSum = max(a[i], currentSum + a[i])`
- Lưu lại max tổng gặp được

---

### Bài 8: Two Pointers - Tìm Hai Số Có Tổng = S

**Đề bài:** Cho mảng sắp xếp, tìm hai phần tử có tổng = S.

**Input:**
```
5 12
1 2 4 7 8
```

**Output:**
```
4 8
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, S;
    cin >> n >> S;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    int left = 0, right = n - 1;
    
    while(left < right) {
        int sum = v[left] + v[right];
        if(sum == S) {
            cout << v[left] << " " << v[right] << "\n";
            return 0;
        } else if(sum < S) {
            left++;
        } else {
            right--;
        }
    }
    
    cout << "Không tìm thấy\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n, S = map(int, input().split())
a = list(map(int, input().split()))

left = 0
right = n - 1

while left < right:
    s = a[left] + a[right]
    if s == S:
        print(a[left], a[right])
        exit()
    elif s < S:
        left += 1
    else:
        right -= 1

print("Không tìm thấy")
```

**Giải thích:**
- Dùng hai con trỏ từ hai đầu mảng
- Nếu tổng nhỏ → tăng left
- Nếu tổng lớn → giảm right
- Độ phức tạp O(n)

---

### Bài 9: Prefix Sum - Truy Vấn Tổng Đoạn

**Đề bài:** Xây dựng prefix sum để truy vấn nhanh tổng đoạn [L, R].

**Input:**
```
5 3
1 2 3 4 5
1 3
2 5
1 5
```

**Output:**
```
6
14
15
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, q;
    cin >> n >> q;
    
    vector<long long> prefix(n + 1, 0);
    
    for(int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        prefix[i] = prefix[i-1] + x;
    }
    
    for(int i = 0; i < q; i++) {
        int L, R;
        cin >> L >> R;
        cout << prefix[R] - prefix[L-1] << "\n";
    }
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n, q = map(int, input().split())
a = list(map(int, input().split()))

# Xây dựng prefix sum
prefix = [0] * (n + 1)
for i in range(n):
    prefix[i+1] = prefix[i] + a[i]

# Truy vấn
for _ in range(q):
    L, R = map(int, input().split())
    print(prefix[R] - prefix[L-1])
```

**Giải thích:**
- `prefix[i]` = tổng a[0] + a[1] + ... + a[i-1]
- Tổng đoạn [L, R] = prefix[R] - prefix[L-1]
- Truy vấn O(1)

---

## PHẦN VI: BÀI TẬP - LEVEL NÂNG CAO

### Bài 10: Xoay Ma Trận 90 Độ

**Đề bài:** Xoay ma trận n×n 90 độ theo chiều kim đồng hồ.

**Input:**
```
3
1 2 3
4 5 6
7 8 9
```

**Output:**
```
7 4 1
8 5 2
9 6 3
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<vector<int>> a(n, vector<int>(n));
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cin >> a[i][j];
        }
    }
    
    // Bước 1: Chuyển vị (transpose)
    for(int i = 0; i < n; i++) {
        for(int j = i + 1; j < n; j++) {
            swap(a[i][j], a[j][i]);
        }
    }
    
    // Bước 2: Đảo từng hàng
    for(int i = 0; i < n; i++) {
        reverse(a[i].begin(), a[i].end());
    }
    
    // In ra
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cout << a[i][j];
            if(j < n-1) cout << " ";
        }
        cout << "\n";
    }
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n = int(input())
a = []
for _ in range(n):
    row = list(map(int, input().split()))
    a.append(row)

# Bước 1: Chuyển vị
for i in range(n):
    for j in range(i+1, n):
        a[i][j], a[j][i] = a[j][i], a[i][j]

# Bước 2: Đảo từng hàng
for i in range(n):
    a[i].reverse()

# In ra
for i in range(n):
    print(' '.join(map(str, a[i])))
```

**Giải thích:**
- Xoay = Chuyển vị + Đảo hàng
- Chuyển vị: a[i][j] ↔ a[j][i]
- Đảo hàng: Đảo mỗi hàng

---

### Bài 11: Sliding Window - Max của Mỗi Cửa Sổ

**Đề bài:** Tìm max trong mỗi cửa sổ kích thước k trượt qua mảng.

**Input:**
```
9 3
1 3 -1 -3 5 3 6 7 4
```

**Output:**
```
3 3 5 5 6 7
```

**Lời giải C++ (dùng deque):**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, k;
    cin >> n >> k;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    deque<int> dq;  // Lưu index
    
    for(int i = 0; i < n; i++) {
        // Xóa phần tử ngoài cửa sổ
        while(!dq.empty() && dq.front() < i - k + 1) {
            dq.pop_front();
        }
        
        // Xóa phần tử nhỏ hơn phần tử hiện tại
        while(!dq.empty() && v[dq.back()] < v[i]) {
            dq.pop_back();
        }
        
        dq.push_back(i);
        
        // Khi đủ k phần tử, lưu max
        if(i >= k - 1) {
            cout << v[dq.front()];
            if(i < n-1) cout << " ";
        }
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
from collections import deque

n, k = map(int, input().split())
a = list(map(int, input().split()))

dq = deque()

for i in range(n):
    # Xóa phần tử ngoài cửa sổ
    while dq and dq[0] < i - k + 1:
        dq.popleft()
    
    # Xóa phần tử nhỏ hơn
    while dq and a[dq[-1]] < a[i]:
        dq.pop()
    
    dq.append(i)
    
    # Lưu max
    if i >= k - 1:
        print(a[dq[0]], end=" " if i < n-1 else "\n")
```

**Giải thích:**
- Deque lưu index các phần tử "ứng viên" là max
- Deque luôn sorted giảm dần
- Front là max của cửa sổ hiện tại

---

### Bài 12: LIS - Dãy Con Tăng Dần Dài Nhất (Binary Search)

**Đề bài:** Tìm độ dài dãy con tăng dần dài nhất (không nhất thiết liên tiếp).

**Input:**
```
8
10 9 2 5 3 7 101 18
```

**Output:**
```
4
```

Giải thích: LIS là [2, 3, 7, 101] hoặc [2, 3, 7, 18], độ dài = 4

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    vector<int> dp;  // dp[i] = phần tử nhỏ nhất cuối dãy tăng độ dài i+1
    
    for(int i = 0; i < n; i++) {
        auto it = lower_bound(dp.begin(), dp.end(), v[i]);
        if(it == dp.end()) {
            dp.push_back(v[i]);
        } else {
            *it = v[i];
        }
    }
    
    cout << dp.size() << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
import bisect

n = int(input())
a = list(map(int, input().split()))

dp = []

for x in a:
    it = bisect.bisect_left(dp, x)
    if it == len(dp):
        dp.append(x)
    else:
        dp[it] = x

print(len(dp))
```

**Giải thích:**
- `dp[i]` = phần tử nhỏ nhất cuối dãy tăng độ dài i+1
- Dùng binary search để tìm vị trí chèn
- Độ phức tạp: O(n log n)

---

### Bài 13: Merge Sorted Arrays

**Đề bài:** Merge hai mảng đã sắp xếp thành một mảng sắp xếp.

**Input:**
```
3 4
1 3 5
2 4 6 8
```

**Output:**
```
1 2 3 4 5 6 8
```

**Lời giải C++:**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    
    vector<int> v1(n), v2(m);
    
    for(int i = 0; i < n; i++) cin >> v1[i];
    for(int i = 0; i < m; i++) cin >> v2[i];
    
    vector<int> result;
    int i = 0, j = 0;
    
    while(i < n && j < m) {
        if(v1[i] < v2[j]) {
            result.push_back(v1[i++]);
        } else {
            result.push_back(v2[j++]);
        }
    }
    
    while(i < n) result.push_back(v1[i++]);
    while(j < m) result.push_back(v2[j++]);
    
    for(int x : result) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

**Lời giải PyPy3:**
```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

result = []
i = j = 0

while i < n and j < m:
    if a[i] < b[j]:
        result.append(a[i])
        i += 1
    else:
        result.append(b[j])
        j += 1

result.extend(a[i:])
result.extend(b[j:])

for x in result:
    print(x, end=" ")
print()
```

**Giải thích:**
- Dùng two pointers
- So sánh phần tử trước, thêm phần tử nhỏ hơn
- Thêm phần tử còn lại
- Độ phức tạp: O(n + m)

---

### Bài 14: Inversion Count - Đếm Cặp Đảo Ngược

**Đề bài:** Đếm số cặp (i, j) với i < j nhưng a[i] > a[j].

**Input:**
```
5
3 1 4 1 5
```

**Output:**
```
3
```

Giải thích: Cặp (3,1), (3,1), (4,1) → 3 inversions

**Lời giải C++ (dùng Merge Sort):**
```cpp
#include<bits/stdc++.h>
using namespace std;

long long mergeCount(vector<int>& v, int left, int mid, int right) {
    vector<int> temp;
    int i = left, j = mid + 1;
    long long inv = 0;
    
    while(i <= mid && j <= right) {
        if(v[i] <= v[j]) {
            temp.push_back(v[i++]);
        } else {
            temp.push_back(v[j++]);
            inv += (mid - i + 1);
        }
    }
    
    while(i <= mid) temp.push_back(v[i++]);
    while(j <= right) temp.push_back(v[j++]);
    
    for(int i = left; i <= right; i++) {
        v[i] = temp[i - left];
    }
    
    return inv;
}

long long mergeSortCount(vector<int>& v, int left, int right) {
    if(left >= right) return 0;
    
    int mid = (left + right) / 2;
    long long inv = 0;
    
    inv += mergeSortCount(v, left, mid);
    inv += mergeSortCount(v, mid + 1, right);
    inv += mergeCount(v, left, mid, right);
    
    return inv;
}

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }
    
    cout << mergeSortCount(v, 0, n-1) << "\n";
    
    return 0;
}
```

**Lời giải PyPy3 (Brute Force):**
```python
n = int(input())
a = list(map(int, input().split()))

count = 0
for i in range(n):
    for j in range(i+1, n):
        if a[i] > a[j]:
            count += 1

print(count)
```

**Giải thích:**
- Brute force: O(n²)
- Merge sort: O(n log n)
- Khi merge, nếu v[i] > v[j], có (mid - i + 1) inversions

---

### Bài 15: Largest Rectangle in Histogram

**Đề bài:** Tìm hình chữ nhật lớn nhất trong histogram.

**Input:**
```
6
2 1 5 6 2 3
```

**Output:**
```
10
```

Giải thích: Hình chữ nhật từ chiều cao 5 và 6, rộng 2, diện tích = 5*2 = 10

**Lời giải C++ (dùng Stack):**
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> h(n);
    
    for(int i = 0; i < n; i++) {
        cin >> h[i];
    }
    
    stack<int> st;
    long long maxArea = 0;
    int i = 0;
    
    while(i < n) {
        if(st.empty() || h[i] >= h[st.top()]) {
            st.push(i);
            i++;
        } else {
            int idx = st.top();
            st.pop();
            long long area = h[idx] * (i - (st.empty() ? 0 : st.top() + 1));
            maxArea = max(maxArea, area);
        }
    }
    
    while(!st.empty()) {
        int idx = st.top();
        st.pop();
        long long area = h[idx] * (n - (st.empty() ? 0 : st.top() + 1));
        maxArea = max(maxArea, area);
    }
    
    cout << maxArea << "\n";
    
    return 0;
}
```

**Lời giải PyPy3 (Brute Force):**
```python
n = int(input())
h = list(map(int, input().split()))

maxArea = 0

for i in range(n):
    minHeight = h[i]
    for j in range(i, n):
        minHeight = min(minHeight, h[j])
        area = minHeight * (j - i + 1)
        maxArea = max(maxArea, area)

print(maxArea)
```

**Giải thích:**
- Brute force: O(n²)
- Stack approach: O(n)
- Dùng stack để track chiều cao

---

## PHẦN VII: MẸO VÀ KINH NGHIỆM

### 1. Performance Tips cho C++ Vector

```cpp
// ✅ TỐT: Reserve trước nếu biết kích thước
vector<int> v;
v.reserve(1000000);  // Tránh reallocation

// ❌ CHẬM: Không reserve
vector<int> v;
for(int i = 0; i < 1000000; i++) {
    v.push_back(i);  // Có thể reallocation nhiều lần
}

// ✅ TỐT: Range-based for (C++11)
for(int x : v) { /* ... */ }

// ✅ TỐT: Dùng reference để tránh copy
for(const int& x : v) { /* ... */ }

// ❌ CHẬM: Copy từng phần tử
for(int x : v) { /* x là copy */ }

// ✅ TỐT: Dùng iterator
auto it = v.begin();

// ✅ TỐT: Find + erase cho một phần tử
v.erase(remove(v.begin(), v.end(), 5), v.end());

// ❌ CHẬM: Erase từng cái một
for(auto it = v.begin(); it != v.end(); ) {
    if(*it == 5) {
        it = v.erase(it);  // O(n) mỗi lần
    } else {
        it++;
    }
}
```

### 2. Performance Tips cho Python List

```python
# ✅ TỐT: List comprehension
a = [x*2 for x in range(1000000)]

# ❌ CHẬM: Loop + append
a = []
for i in range(1000000):
    a.append(i*2)

# ✅ TỐT: Slice
b = a[100:200]

# ✅ TỐT: Extend (thêm nhiều)
a.extend([1, 2, 3])

# ❌ CHẬM: Thêm một cái một
for x in [1, 2, 3]:
    a.append(x)

# ✅ TỐT: Unpacking
a = [1, 2] + [3, 4]

# ✅ TỐT: sorted() nếu không cần in-place
b = sorted(a)

# ✅ TỐT: in operator (nhanh với list nhỏ)
if 5 in a:
    pass

# ❌ CHẬM với danh sách lớn: in operator
# Dùng set thay thế:
s = set(a)
if 5 in s:  # O(1)
    pass
```

### 3. Debugging Tips

**C++:**
```cpp
// Kiểm tra bounds
if(i < 0 || i >= v.size()) {
    cerr << "Out of bounds\n";
}

// Print vector
void print(vector<int> v) {
    for(int x : v) cout << x << " ";
    cout << "\n";
}

// Kiểm tra capacity vs size
cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << "\n";
```

**Python:**
```python
# Print list
print(a)

# Debug trênngành
import pdb; pdb.set_trace()

# Print với index
for i, x in enumerate(a):
    print(f"{i}: {x}")

# Kiểm tra type
print(type(a))
```

---

## PHẦN VIII: CHECKLIST - TRƯỚC KHI NỘP BÀI

### Cho C++

- [ ] Bao gồm `#include<bits/stdc++.h>`?
- [ ] Có `using namespace std;`?
- [ ] Có `ios_base::sync_with_stdio(false);` để tối ưu I/O?
- [ ] Dùng `long long` khi cần (để tránh overflow)?
- [ ] Kiểm tra ranh giới mảng (0 ≤ i < n)?
- [ ] Dùng `.end()` đúng cách (vị trí sau phần tử cuối)?
- [ ] Có iterator / range-based for nếu cần?
- [ ] Có xử lý cạnh biên (n=0, n=1)?
- [ ] Test với ví dụ trong đề?
- [ ] Code không có vòng lặp vô hạn?

### Cho PyPy3

- [ ] Input/output đúng format?
- [ ] Dùng `int(input())` cho số nguyên?
- [ ] Dùng `list(map(int, input().split()))` cho dãy?
- [ ] Kiểm tra index (0-indexed vs 1-indexed)?
- [ ] Không dùng `import` quá nhiều (nặng)?
- [ ] Dùng `sys.exit()` hoặc `exit()` để dừng sớm?
- [ ] List comprehension tối ưu?
- [ ] Không bị TypeError (kiểu dữ liệu)?
- [ ] Test trên máy local trước?
- [ ] Code không quá dài (>1000 dòng)?

---

## PHẦN IX: CÂU HỎI THƯỜNG GẶP

### Q1: Vector hay List?

**A:** Trong Olympic:
- **C++ Vector**: Nhanh, tiết kiệm bộ nhớ, chuẩn quốc tế
- **PyPy3 List**: Dễ, nhanh viết, debug dễ

Chọn cái mà bạn thạo nhất!

### Q2: Khi nào dùng insert()?

**A:** Tránh insert() ở giữa vì O(n). Nếu phải:
- Dùng push_back() rồi sắp xếp
- Hoặc dùng deque nếu cần thêm đầu

### Q3: Reserve bao nhiêu?

**A:** 
- Biết trước: `reserve(n)`
- Không biết: `reserve(1000000)` (an toàn)

### Q4: Vector 2D hay 1D?

**A:**
- 2D: `vector<vector<int>>` (ma trận)
- 1D: `vector<int>` rồi xử lý index (nhanh hơn)

### Q5: List hay Dict?

**A:**
- List: Duyệt theo thứ tự, truy cập index
- Dict: Tìm kiếm nhanh O(1), không có thứ tự

---

## TÓM TẮT

### Vector C++

**Ưu điểm:**
- ✅ Nhanh (compiled)
- ✅ Tiết kiệm bộ nhớ
- ✅ Chuẩn quốc tế IOI
- ✅ Iterator mạnh

**Nhược điểm:**
- ❌ Cú pháp phức tạp
- ❌ Lỗi dễ
- ❌ Chậm viết

### List Python

**Ưu điểm:**
- ✅ Dễ viết
- ✅ Slice powerful
- ✅ Comprehension
- ✅ Debug dễ

**Nhược điểm:**
- ❌ Chậm
- ❌ Dùng nhiều bộ nhớ
- ❌ Không type-safe

---

**Tài liệu được biên soạn theo tiêu chuẩn IOI - Hãy luyện tập thường xuyên! 🏆**

**Công thức thành công: 30% lý thuyết + 70% luyện tập = Gold Medal! 🥇**
