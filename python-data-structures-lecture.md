# BÀI GIẢNG: CẤU TRÚC DỮ LIỆU SETS, DICTIONARIES VÀ TUPLES TRONG PYTHON

## PHẦN 1: SETS (TẬP HỢP)

### 1.1. Khái niệm và Đặc điểm

**Set** là một cấu trúc dữ liệu trong Python dùng để lưu trữ tập hợp các phần tử **không có thứ tự**, **không trùng lặp** và **có thể thay đổi** (mutable).

**Đặc điểm chính:**
- Các phần tử trong set là duy nhất (không trùng lặp)
- Không có thứ tự (unordered) - không thể truy cập bằng chỉ số
- Có thể thay đổi (mutable) - thêm/xóa phần tử
- Các phần tử phải là immutable (số, string, tuple)
- Độ phức tạp tìm kiếm: $O(1)$ trung bình

### 1.2. Cú pháp và Các phép toán cơ bản

#### Khởi tạo Set:
```python
# Cach 1: Su dung dau ngoac nhon
s1 = {1, 2, 3, 4, 5}

# Cach 2: Su dung ham set()
s2 = set([1, 2, 3, 4, 5])

# Set rong
s3 = set()  # Khong dung {} vi do la dictionary rong
```

#### Các phép toán cơ bản:
```python
# Them phan tu
s = {1, 2, 3}
s.add(4)           # Them 1 phan tu
s.update([5, 6])   # Them nhieu phan tu

# Xoa phan tu
s.remove(3)        # Xoa phan tu 3, loi neu khong ton tai
s.discard(7)       # Xoa phan tu 7, khong loi neu khong ton tai
s.pop()            # Xoa phan tu ngau nhien

# Kiem tra phan tu
if 2 in s:
    print("2 co trong set")

# Do dai
n = len(s)
```

#### Các phép toán tập hợp:
```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

# Hop (Union): A ∪ B
hop = A | B  # hoac A.union(B)
# Ket qua: {1, 2, 3, 4, 5, 6}

# Giao (Intersection): A ∩ B  
giao = A & B  # hoac A.intersection(B)
# Ket qua: {3, 4}

# Hieu (Difference): A - B
hieu = A - B  # hoac A.difference(B)
# Ket qua: {1, 2}

# Hieu doi xung (Symmetric Difference): A △ B
hieu_doi_xung = A ^ B  # hoac A.symmetric_difference(B)
# Ket qua: {1, 2, 5, 6}

# Kiem tra tap con
is_subset = A <= B  # A co la tap con cua B?
is_superset = A >= B  # A co chua B?
```

### 1.3. Ví dụ minh họa

```python
# Vi du: Tim cac phan tu xuat hien trong it nhat 2 trong 3 mang
def tim_phan_tu_chung():
    # Du lieu dau vao
    arr1 = [1, 2, 3, 4, 5]
    arr2 = [3, 4, 5, 6, 7]
    arr3 = [5, 6, 7, 8, 9]
    
    # Chuyen thanh set
    s1 = set(arr1)
    s2 = set(arr2)
    s3 = set(arr3)
    
    # Tim phan tu xuat hien it nhat 2 lan
    result = (s1 & s2) | (s2 & s3) | (s1 & s3)
    
    print("Phan tu xuat hien >= 2 lan:", sorted(result))
    # Ket qua: [3, 4, 5, 6, 7]

tim_phan_tu_chung()
```

### 1.4. Bài tập vận dụng

#### Bài tập 1 (Cơ bản): Đếm số phần tử khác nhau
```python
def dem_phan_tu_khac_nhau(arr):
    """
    Dem so phan tu khac nhau trong mang
    Input: arr - mang cac so nguyen
    Output: so luong phan tu khac nhau
    """
    # Su dung set de loai bo trung lap
    unique_elements = set(arr)
    return len(unique_elements)

# Test
arr = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
print(f"So phan tu khac nhau: {dem_phan_tu_khac_nhau(arr)}")  # Output: 4
```

#### Bài tập 2 (Trung bình): Tìm phần tử chỉ xuất hiện trong một mảng
```python
def tim_phan_tu_rieng(arrays):
    """
    Tim cac phan tu chi xuat hien trong dung 1 mang
    Input: arrays - danh sach cac mang
    Output: set cac phan tu chi xuat hien 1 lan
    """
    # Dem so lan xuat hien cua moi phan tu
    all_elements = []
    for arr in arrays:
        all_elements.extend(set(arr))
    
    # Dem tan suat
    from collections import Counter
    counter = Counter(all_elements)
    
    # Loc phan tu xuat hien 1 lan
    unique_only = {x for x, count in counter.items() if count == 1}
    return unique_only

# Test
arrays = [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
print(f"Phan tu rieng: {tim_phan_tu_rieng(arrays)}")  # Output: {1, 5}
```

#### Bài tập 3 (Nâng cao): Kiểm tra hai tập có giao nhau k phần tử
```python
def kiem_tra_giao_k_phan_tu(A, B, k):
    """
    Kiem tra hai tap co giao dung k phan tu
    Input: A, B - hai tap hop; k - so phan tu giao
    Output: True neu |A ∩ B| = k, nguoc lai False
    Thoi gian: O(min(|A|, |B|))
    """
    # Tinh tap giao
    giao = set(A) & set(B)
    
    # Kiem tra so luong
    return len(giao) == k

# Test
A = [1, 2, 3, 4, 5]
B = [3, 4, 5, 6, 7]
k = 3
print(f"A va B co giao {k} phan tu: {kiem_tra_giao_k_phan_tu(A, B, k)}")  # True
```

### 1.5. Bài tập từ các trang thi đấu

1. **[Codeforces - Distinct Numbers](https://codeforces.com/problemset/problem/1742/A)** - Cơ bản
2. **[AtCoder - Unique Elements](https://atcoder.jp/contests/abc268/tasks/abc268_b)** - Trung bình  
3. **[Codeforces - Set Union](https://codeforces.com/problemset/problem/1097/B)** - Nâng cao

---

## PHẦN 2: DICTIONARIES (TỪ ĐIỂN)

### 2.1. Khái niệm và Đặc điểm

**Dictionary** là cấu trúc dữ liệu lưu trữ các cặp **key-value** (khóa-giá trị), cho phép truy cập nhanh thông qua key.

**Đặc điểm chính:**
- Lưu trữ theo cặp key-value
- Keys phải là immutable và duy nhất
- Values có thể là bất kỳ kiểu dữ liệu nào
- Có thứ tự (từ Python 3.7+)
- Độ phức tạp truy cập: $O(1)$ trung bình

### 2.2. Cú pháp và Các phép toán cơ bản

#### Khởi tạo Dictionary:
```python
# Cach 1: Su dung dau ngoac nhon
d1 = {'a': 1, 'b': 2, 'c': 3}

# Cach 2: Su dung ham dict()
d2 = dict(a=1, b=2, c=3)

# Cach 3: Tu list cac cap
d3 = dict([('a', 1), ('b', 2), ('c', 3)])

# Dictionary rong
d4 = {}  # hoac dict()
```

#### Các thao tác cơ bản:
```python
d = {'a': 1, 'b': 2, 'c': 3}

# Truy cap gia tri
val1 = d['a']           # Loi neu key khong ton tai
val2 = d.get('d', 0)    # Tra ve 0 neu 'd' khong ton tai

# Them/Cap nhat
d['d'] = 4              # Them key moi
d['a'] = 10            # Cap nhat gia tri
d.update({'e': 5, 'f': 6})  # Cap nhat nhieu cap

# Xoa
del d['b']             # Xoa key 'b'
val = d.pop('c', None) # Xoa va tra ve gia tri

# Kiem tra key
if 'a' in d:
    print("Key 'a' ton tai")

# Lay danh sach keys, values, items
keys = list(d.keys())
values = list(d.values())
items = list(d.items())

# Duyet dictionary
for key in d:
    print(f"{key}: {d[key]}")

for key, value in d.items():
    print(f"{key}: {value}")
```

### 2.3. Dictionary nâng cao

```python
from collections import defaultdict, Counter

# defaultdict - tu dong khoi tao gia tri mac dinh
dd = defaultdict(int)  # Mac dinh la 0
dd['a'] += 1  # Khong can kiem tra ton tai

dd_list = defaultdict(list)  # Mac dinh la []
dd_list['key'].append(1)

# Counter - dem tan suat
arr = [1, 2, 2, 3, 3, 3]
counter = Counter(arr)
print(counter)  # Counter({3: 3, 2: 2, 1: 1})

# Lay k phan tu xuat hien nhieu nhat
top_k = counter.most_common(2)  # [(3, 3), (2, 2)]
```

### 2.4. Ví dụ minh họa

```python
# Vi du: Gom nhom hoc sinh theo diem
def gom_nhom_theo_diem(students):
    """
    Gom nhom hoc sinh theo diem so
    Input: students - list cac tuple (ten, diem)
    Output: dictionary {diem: [danh sach ten]}
    """
    from collections import defaultdict
    
    # Su dung defaultdict de tu dong tao list rong
    nhom_diem = defaultdict(list)
    
    # Gom nhom
    for ten, diem in students:
        nhom_diem[diem].append(ten)
    
    # Sap xep theo diem giam dan
    result = dict(sorted(nhom_diem.items(), reverse=True))
    return result

# Test
students = [
    ("An", 8), ("Binh", 9), ("Cuong", 8),
    ("Dung", 10), ("Em", 9), ("Phuong", 10)
]
nhom = gom_nhom_theo_diem(students)
for diem, ds_ten in nhom.items():
    print(f"Diem {diem}: {', '.join(ds_ten)}")
```

### 2.5. Bài tập vận dụng

#### Bài tập 1 (Cơ bản): Đếm tần suất ký tự
```python
def dem_tan_suat_ky_tu(s):
    """
    Dem tan suat xuat hien cua tung ky tu
    Input: s - chuoi ky tu
    Output: dictionary {ky_tu: so_lan}
    """
    # Cach 1: Su dung Counter
    from collections import Counter
    return dict(Counter(s))
    
    # Cach 2: Tu lam
    # freq = {}
    # for char in s:
    #     freq[char] = freq.get(char, 0) + 1
    # return freq

# Test
s = "programming"
freq = dem_tan_suat_ky_tu(s)
print(f"Tan suat: {freq}")
# Output: {'p': 1, 'r': 2, 'o': 1, 'g': 2, 'a': 1, 'm': 2, 'i': 1, 'n': 1}
```

#### Bài tập 2 (Trung bình): Tìm hai số có tổng bằng target
```python
def tim_cap_tong_target(arr, target):
    """
    Tim hai so trong mang co tong bang target
    Input: arr - mang so nguyen, target - tong can tim
    Output: tuple (i, j) neu tim thay, None neu khong
    Thoi gian: O(n)
    """
    # Luu vi tri cua tung phan tu
    vi_tri = {}
    
    for i, num in enumerate(arr):
        # Can tim: target - num
        can_tim = target - num
        
        # Kiem tra da gap truoc do chua
        if can_tim in vi_tri:
            return (vi_tri[can_tim], i)
        
        # Luu vi tri hien tai
        vi_tri[num] = i
    
    return None

# Test
arr = [2, 7, 11, 15]
target = 9
result = tim_cap_tong_target(arr, target)
if result:
    i, j = result
    print(f"Cap ({i}, {j}): {arr[i]} + {arr[j]} = {target}")
```

#### Bài tập 3 (Nâng cao): Anagram Groups
```python
def gom_nhom_anagram(words):
    """
    Gom nhom cac tu la anagram cua nhau
    Input: words - danh sach cac tu
    Output: danh sach cac nhom anagram
    """
    from collections import defaultdict
    
    # Gom nhom theo chu cai sau khi sap xep
    nhom = defaultdict(list)
    
    for word in words:
        # Sap xep ky tu lam key
        key = ''.join(sorted(word))
        nhom[key].append(word)
    
    # Tra ve cac nhom co >= 2 tu
    return [group for group in nhom.values() if len(group) >= 2]

# Test
words = ["eat", "tea", "tan", "ate", "nat", "bat"]
groups = gom_nhom_anagram(words)
print("Cac nhom anagram:")
for group in groups:
    print(f"  {group}")
# Output: [['eat', 'tea', 'ate'], ['tan', 'nat']]
```

### 2.6. Bài tập từ các trang thi đấu

1. **[Codeforces - Registration System](https://codeforces.com/problemset/problem/4/C)** - Cơ bản
2. **[AtCoder - Count Pairs](https://atcoder.jp/contests/abc245/tasks/abc245_c)** - Trung bình
3. **[Codeforces - Group Anagrams](https://codeforces.com/problemset/problem/1520/D)** - Nâng cao

---

## PHẦN 3: TUPLES (BỘ)

### 3.1. Khái niệm và Đặc điểm

**Tuple** là cấu trúc dữ liệu lưu trữ một dãy các phần tử **có thứ tự** và **không thể thay đổi** (immutable).

**Đặc điểm chính:**
- Immutable - không thể thay đổi sau khi tạo
- Có thứ tự - truy cập bằng chỉ số
- Cho phép phần tử trùng lặp
- Có thể chứa nhiều kiểu dữ liệu khác nhau
- Có thể dùng làm key trong dictionary
- Hiệu quả về bộ nhớ hơn list

### 3.2. Cú pháp và Các phép toán cơ bản

#### Khởi tạo Tuple:
```python
# Cach 1: Su dung dau ngoac tron
t1 = (1, 2, 3, 4, 5)

# Cach 2: Khong can dau ngoac
t2 = 1, 2, 3, 4, 5

# Tuple 1 phan tu (chu y dau phay)
t3 = (1,)  # hoac: t3 = 1,

# Tuple rong
t4 = ()

# Tu list hoac string
t5 = tuple([1, 2, 3])
t6 = tuple("abc")  # ('a', 'b', 'c')
```

#### Các thao tác cơ bản:
```python
t = (1, 2, 3, 4, 5)

# Truy cap phan tu
first = t[0]     # 1
last = t[-1]     # 5
sub = t[1:4]     # (2, 3, 4)

# Unpacking
a, b, c = (1, 2, 3)
first, *middle, last = (1, 2, 3, 4, 5)
# first=1, middle=[2,3,4], last=5

# Noi tuple
t1 = (1, 2)
t2 = (3, 4)
t3 = t1 + t2  # (1, 2, 3, 4)

# Nhan tuple
t4 = (1, 2) * 3  # (1, 2, 1, 2, 1, 2)

# Kiem tra phan tu
if 3 in t:
    print("3 co trong tuple")

# Dem so lan xuat hien
count = t.count(3)

# Tim vi tri
index = t.index(3)  # Vi tri dau tien cua 3

# Do dai
n = len(t)
```

### 3.3. Tuple nâng cao

```python
# Named Tuple - Tuple co ten truong
from collections import namedtuple

# Dinh nghia kieu Point
Point = namedtuple('Point', ['x', 'y'])

# Tao doi tuong
p1 = Point(3, 4)
p2 = Point(x=1, y=2)

# Truy cap
print(p1.x, p1.y)  # 3 4
print(p1[0], p1[1]) # 3 4

# Su dung tuple lam key trong dictionary
graph = {}
graph[(0, 0)] = "Origin"
graph[(1, 1)] = "Point A"

# Swap gia tri
a, b = 5, 10
a, b = b, a  # Swap su dung tuple unpacking
```

### 3.4. Ví dụ minh họa

```python
# Vi du: Quan ly toa do trong he toa do 2D
def tinh_khoang_cach_manhattan(points):
    """
    Tinh tong khoang cach Manhattan tu goc toa do den tat ca cac diem
    Input: points - list cac tuple (x, y)
    Output: tong khoang cach
    """
    tong_khoang_cach = 0
    
    for x, y in points:  # Unpacking tuple
        # Khoang cach Manhattan: |x| + |y|
        khoang_cach = abs(x) + abs(y)
        tong_khoang_cach += khoang_cach
        print(f"Diem ({x}, {y}): khoang cach = {khoang_cach}")
    
    return tong_khoang_cach

# Test
points = [(1, 2), (3, 4), (-1, 5), (2, -3)]
tong = tinh_khoang_cach_manhattan(points)
print(f"Tong khoang cach: {tong}")
```

### 3.5. Bài tập vận dụng

#### Bài tập 1 (Cơ bản): Sắp xếp danh sách sinh viên
```python
def sap_xep_sinh_vien(students):
    """
    Sap xep sinh vien theo diem giam dan, neu bang nhau thi theo ten
    Input: students - list cac tuple (ten, diem)
    Output: list da sap xep
    """
    # Sap xep theo diem giam dan (-diem), ten tang dan
    return sorted(students, key=lambda x: (-x[1], x[0]))

# Test
students = [
    ("An", 8),
    ("Binh", 9),
    ("Cuong", 8),
    ("Dung", 9),
    ("Em", 7)
]
result = sap_xep_sinh_vien(students)
print("Danh sach sau khi sap xep:")
for ten, diem in result:
    print(f"  {ten}: {diem}")
```

#### Bài tập 2 (Trung bình): Tìm cặp điểm gần nhất
```python
def tim_cap_diem_gan_nhat(points):
    """
    Tim hai diem gan nhau nhat theo khoang cach Euclidean
    Input: points - list cac tuple (x, y)
    Output: tuple (diem1, diem2, khoang_cach)
    Thoi gian: O(n²)
    """
    import math
    
    n = len(points)
    if n < 2:
        return None
    
    min_dist = float('inf')
    cap_gan_nhat = None
    
    # Duyet tat ca cac cap
    for i in range(n):
        for j in range(i + 1, n):
            x1, y1 = points[i]
            x2, y2 = points[j]
            
            # Tinh khoang cach Euclidean
            dist = math.sqrt((x2 - x1)**2 + (y2 - y1)**2)
            
            if dist < min_dist:
                min_dist = dist
                cap_gan_nhat = (points[i], points[j], dist)
    
    return cap_gan_nhat

# Test
points = [(0, 0), (1, 1), (3, 3), (1, 0), (2, 2)]
result = tim_cap_diem_gan_nhat(points)
if result:
    p1, p2, dist = result
    print(f"Cap gan nhat: {p1} va {p2}")
    print(f"Khoang cach: {dist:.2f}")
```

#### Bài tập 3 (Nâng cao): Convex Hull (Bao lồi)
```python
def tinh_bao_loi(points):
    """
    Tim bao loi cua tap diem bang thuat toan Graham Scan don gian
    Input: points - list cac tuple (x, y)
    Output: list cac diem tren bao loi theo chieu kim dong ho
    """
    def cross_product(O, A, B):
        """Tinh tich co huong OA x OB"""
        return (A[0] - O[0]) * (B[1] - O[1]) - (A[1] - O[1]) * (B[0] - O[0])
    
    # Sap xep diem theo x, neu bang nhau thi theo y
    points = sorted(set(points))
    
    if len(points) <= 1:
        return points
    
    # Xay dung nua duoi bao loi
    lower = []
    for p in points:
        while len(lower) >= 2 and cross_product(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)
    
    # Xay dung nua tren bao loi
    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross_product(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)
    
    # Loai bo diem trung lap o dau va cuoi
    return lower[:-1] + upper[:-1]

# Test
points = [(0, 0), (1, 1), (2, 0), (0, 2), (2, 2), (1, 0), (1, 2)]
hull = tinh_bao_loi(points)
print("Cac diem tren bao loi:")
for point in hull:
    print(f"  {point}")
```

### 3.6. So sánh Tuple với List

| Đặc điểm | Tuple | List |
|----------|-------|------|
| Mutability | Immutable | Mutable |
| Cú pháp | `()` hoặc không | `[]` |
| Hiệu suất | Nhanh hơn | Chậm hơn |
| Bộ nhớ | Ít hơn | Nhiều hơn |
| Làm key dict | Có thể | Không thể |
| Methods | Ít (count, index) | Nhiều (append, extend, ...) |

### 3.7. Bài tập từ các trang thi đấu

1. **[Codeforces - Coordinate Compression](https://codeforces.com/problemset/problem/1541/B)** - Cơ bản
2. **[AtCoder - Points on Line](https://atcoder.jp/contests/abc268/tasks/abc268_c)** - Trung bình
3. **[Codeforces - Convex Hull](https://codeforces.com/problemset/problem/1195/C)** - Nâng cao

---

## KẾT LUẬN

### Tổng kết các cấu trúc dữ liệu

| Cấu trúc | Khi nào sử dụng | Độ phức tạp |
|----------|-----------------|-------------|
| **Set** | - Loại bỏ trùng lặp<br>- Phép toán tập hợp<br>- Kiểm tra membership nhanh | $O(1)$ average |
| **Dictionary** | - Lưu trữ key-value<br>- Đếm tần suất<br>- Mapping/Lookup table | $O(1)$ average |
| **Tuple** | - Dữ liệu không đổi<br>- Key trong dict<br>- Return nhiều giá trị | $O(1)$ index |

### Lời khuyên cho lập trình thi đấu

1. **Set**: Dùng khi cần kiểm tra sự tồn tại nhanh hoặc loại bỏ trùng lặp
2. **Dictionary**: Dùng cho bài toán đếm, nhóm, hoặc cần tra cứu nhanh
3. **Tuple**: Dùng cho tọa độ, cặp giá trị cố định, hoặc làm key phức tạp

### Một số thư viện hữu ích

```python
from collections import defaultdict, Counter, namedtuple
from itertools import combinations, permutations
import heapq  # Priority queue
import bisect  # Binary search
```

Thực hành thường xuyên với các bài tập trên Codeforces và AtCoder sẽ giúp nắm vững cách áp dụng các cấu trúc dữ liệu này trong các tình huống khác nhau!