# CHUYÊN ĐỀ: CẤU TRÚC DỮ LIỆU VÀ GIẢI THUẬT
## Vector 1 Chiều và Vector 2 Chiều trong C++

> **Đối tượng:** Học sinh lớp 9 mới học lập trình C++  
> **Độ khó:** Cơ bản  
> **Ngôn ngữ:** C++ (STL Vector)

---

## MỤC LỤC

1. [Giới thiệu về STL Vector](#1-giới-thiệu-về-stl-vector)
2. [Vector 1 Chiều](#2-vector-1-chiều)
3. [Vector 2 Chiều](#3-vector-2-chiều)
4. [Bài toán ứng dụng](#4-bài-toán-ứng-dụng)
5. [Bài tập thực hành](#5-bài-tập-thực-hành)

---

## 1. GIỚI THIỆU VỀ STL VECTOR

### 1.1. STL là gì?

**STL (Standard Template Library)** là thư viện chuẩn của C++, cung cấp sẵn nhiều cấu trúc dữ liệu và giải thuật hữu ích để lập trình viên dùng ngay mà không cần tự xây dựng lại từ đầu.

Trong STL, **`vector`** là một trong những cấu trúc dữ liệu được dùng phổ biến nhất — có thể hiểu đơn giản là một **mảng động** (mảng có thể thay đổi kích thước trong khi chương trình đang chạy).

### 1.2. Tại sao dùng Vector thay vì mảng thông thường?

| Đặc điểm | Mảng thông thường (`int a[100]`) | Vector (`vector<int> v`) |
|---|---|---|
| Kích thước | Cố định, phải khai báo trước | Linh hoạt, tự mở rộng |
| Thêm phần tử | Không hỗ trợ trực tiếp | Dễ dàng với `push_back()` |
| Xóa phần tử | Phức tạp | Dễ dàng với `pop_back()`, `erase()` |
| Biết số phần tử | Phải tự quản lý | Dùng `.size()` |
| An toàn | Dễ truy cập ngoài biên | Có thể kiểm tra với `.at()` |

### 1.3. Cách khai báo sử dụng Vector

Để dùng vector, bạn cần thêm thư viện `<vector>` vào đầu chương trình:

```cpp
#include <iostream>
#include <vector>
using namespace std;
```

---

## 2. VECTOR 1 CHIỀU

### 2.1. Định nghĩa

**Vector 1 chiều** là một dãy các phần tử có cùng kiểu dữ liệu, được lưu trữ liên tiếp trong bộ nhớ, có thể truy cập thông qua chỉ số (index) bắt đầu từ `0`.

```
Chỉ số:  [0]  [1]  [2]  [3]  [4]
Giá trị:  10   20   30   40   50
```

### 2.2. Cú pháp khai báo

```cpp
// Khai báo vector rỗng kiểu int
vector<int> v;

// Khai báo vector có 5 phần tử, tất cả bằng 0
vector<int> v(5);

// Khai báo vector có 5 phần tử, tất cả bằng 7
vector<int> v(5, 7);

// Khai báo và khởi tạo trực tiếp
vector<int> v = {10, 20, 30, 40, 50};

// Vector kiểu khác
vector<double> diem = {8.5, 9.0, 7.5};
vector<string> ten = {"An", "Binh", "Cuong"};
```

### 2.3. Các thao tác cơ bản

#### a) Thêm phần tử vào cuối: `push_back()`

```cpp
vector<int> v;
v.push_back(10);  // v = {10}
v.push_back(20);  // v = {10, 20}
v.push_back(30);  // v = {10, 20, 30}
```

#### b) Xóa phần tử cuối: `pop_back()`

```cpp
vector<int> v = {10, 20, 30};
v.pop_back();  // v = {10, 20}
```

#### c) Truy cập phần tử: `v[i]` hoặc `v.at(i)`

```cpp
vector<int> v = {10, 20, 30, 40, 50};
cout << v[0];      // In ra: 10
cout << v[2];      // In ra: 30
cout << v.at(4);   // In ra: 50

// Sửa giá trị
v[1] = 99;         // v = {10, 99, 30, 40, 50}
```

#### d) Kích thước vector: `size()`

```cpp
vector<int> v = {10, 20, 30, 40};
cout << v.size();  // In ra: 4
```

#### e) Kiểm tra rỗng: `empty()`

```cpp
vector<int> v;
if (v.empty()) {
    cout << "Vector rong!";
}
```

#### f) Xóa toàn bộ: `clear()`

```cpp
vector<int> v = {1, 2, 3};
v.clear();         // v = {}
cout << v.size();  // In ra: 0
```

#### g) Phần tử đầu và cuối: `front()`, `back()`

```cpp
vector<int> v = {10, 20, 30, 40};
cout << v.front();  // In ra: 10
cout << v.back();   // In ra: 40
```

#### h) Thay đổi kích thước: `resize()`

```cpp
vector<int> v = {1, 2, 3};
v.resize(5);        // v = {1, 2, 3, 0, 0}
v.resize(5, 9);     // Nếu tăng kích thước, điền giá trị 9
```

### 2.4. Duyệt qua các phần tử của Vector

#### Cách 1: Dùng vòng lặp `for` với chỉ số

```cpp
vector<int> v = {10, 20, 30, 40, 50};
for (int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}
// Kết quả: 10 20 30 40 50
```

#### Cách 2: Dùng vòng lặp `for` theo phạm vi (range-based for)

```cpp
vector<int> v = {10, 20, 30, 40, 50};
for (int x : v) {
    cout << x << " ";
}
// Kết quả: 10 20 30 40 50
```

#### Cách 3: Dùng `for` ngược

```cpp
vector<int> v = {10, 20, 30, 40, 50};
for (int i = v.size() - 1; i >= 0; i--) {
    cout << v[i] << " ";
}
// Kết quả: 50 40 30 20 10
```

### 2.5. Nhập/Xuất Vector từ bàn phím

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cout << "Nhap so luong phan tu: ";
    cin >> n;

    vector<int> v(n);
    cout << "Nhap cac phan tu:\n";
    for (int i = 0; i < n; i++) {
        cout << "v[" << i << "] = ";
        cin >> v[i];
    }

    cout << "Vector vua nhap: ";
    for (int x : v) {
        cout << x << " ";
    }

    return 0;
}
```

### 2.6. Bảng tổng hợp các hàm thường dùng

| Hàm/Toán tử | Công dụng | Ví dụ |
|---|---|---|
| `v.push_back(x)` | Thêm `x` vào cuối | `v.push_back(5)` |
| `v.pop_back()` | Xóa phần tử cuối | `v.pop_back()` |
| `v[i]` | Truy cập phần tử thứ `i` | `v[0]` |
| `v.at(i)` | Truy cập có kiểm tra biên | `v.at(2)` |
| `v.size()` | Số phần tử hiện tại | `v.size()` |
| `v.empty()` | Kiểm tra vector rỗng | `v.empty()` |
| `v.clear()` | Xóa tất cả phần tử | `v.clear()` |
| `v.front()` | Phần tử đầu tiên | `v.front()` |
| `v.back()` | Phần tử cuối cùng | `v.back()` |
| `v.resize(n)` | Thay đổi kích thước | `v.resize(10)` |

---

## 3. VECTOR 2 CHIỀU

### 3.1. Định nghĩa

**Vector 2 chiều** là một vector mà mỗi phần tử của nó lại là một vector khác — tức là **vector của các vector**. Đây là cách biểu diễn **ma trận** (bảng số có hàng và cột) trong C++.

```
         Cột 0  Cột 1  Cột 2
Hàng 0: [  1  ][  2  ][  3  ]
Hàng 1: [  4  ][  5  ][  6  ]
Hàng 2: [  7  ][  8  ][  9  ]
```

Truy cập phần tử hàng `i`, cột `j`: `v[i][j]`

### 3.2. Cú pháp khai báo

```cpp
// Khai báo vector 2 chiều rỗng
vector<vector<int>> v;

// Khai báo ma trận 3 hàng, 4 cột, tất cả bằng 0
vector<vector<int>> v(3, vector<int>(4, 0));

// Khai báo và khởi tạo trực tiếp
vector<vector<int>> v = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

### 3.3. Truy cập và thay đổi phần tử

```cpp
vector<vector<int>> v = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

cout << v[0][0];  // In ra: 1 (hàng 0, cột 0)
cout << v[1][2];  // In ra: 6 (hàng 1, cột 2)
cout << v[2][1];  // In ra: 8 (hàng 2, cột 1)

v[0][1] = 99;     // Sửa phần tử hàng 0, cột 1
```

### 3.4. Số hàng và số cột

```cpp
vector<vector<int>> v = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int soHang = v.size();       // Số hàng = 3
int soCot = v[0].size();     // Số cột = 3 (cột của hàng 0)
```

### 3.5. Duyệt qua ma trận

#### Cách 1: Dùng vòng lặp lồng nhau với chỉ số

```cpp
vector<vector<int>> v = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < v.size(); i++) {
    for (int j = 0; j < v[i].size(); j++) {
        cout << v[i][j] << "\t";
    }
    cout << "\n";
}
```

Kết quả:
```
1    2    3
4    5    6
7    8    9
```

#### Cách 2: Dùng range-based for

```cpp
for (auto& hang : v) {
    for (int x : hang) {
        cout << x << "\t";
    }
    cout << "\n";
}
```

### 3.6. Nhập/Xuất Vector 2 chiều từ bàn phím

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int m, n;
    cout << "Nhap so hang: ";  cin >> m;
    cout << "Nhap so cot: ";   cin >> n;

    // Tao ma tran m hang n cot, gia tri ban dau = 0
    vector<vector<int>> v(m, vector<int>(n));

    cout << "Nhap cac phan tu:\n";
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cout << "v[" << i << "][" << j << "] = ";
            cin >> v[i][j];
        }
    }

    cout << "\nMa tran vua nhap:\n";
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cout << v[i][j] << "\t";
        }
        cout << "\n";
    }

    return 0;
}
```

### 3.7. Vector 2 chiều không đều (Jagged Vector)

Vector 2 chiều không bắt buộc mỗi hàng phải có cùng số cột:

```cpp
vector<vector<int>> v;

v.push_back({1});           // Hàng 0: 1 phần tử
v.push_back({2, 3});        // Hàng 1: 2 phần tử
v.push_back({4, 5, 6});     // Hàng 2: 3 phần tử

for (auto& hang : v) {
    for (int x : hang) cout << x << " ";
    cout << "\n";
}
// Kết quả:
// 1
// 2 3
// 4 5 6
```

---

## 4. BÀI TOÁN ỨNG DỤNG

### 4.1. Bài toán 1: Tìm giá trị lớn nhất và nhỏ nhất trong vector

**Bài toán:** Nhập vào một dãy số nguyên, tìm và in ra giá trị lớn nhất, nhỏ nhất.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cout << "Nhap so luong phan tu: ";
    cin >> n;

    vector<int> v(n);
    for (int i = 0; i < n; i++) {
        cout << "Nhap v[" << i << "]: ";
        cin >> v[i];
    }

    int maxVal = v[0], minVal = v[0];
    for (int i = 1; i < n; i++) {
        if (v[i] > maxVal) maxVal = v[i];
        if (v[i] < minVal) minVal = v[i];
    }

    cout << "Gia tri lon nhat: " << maxVal << "\n";
    cout << "Gia tri nho nhat: " << minVal << "\n";

    return 0;
}
```

**Kết quả mẫu:**
```
Nhap so luong phan tu: 5
Nhap v[0]: 3
Nhap v[1]: 7
Nhap v[2]: 1
Nhap v[3]: 9
Nhap v[4]: 4
Gia tri lon nhat: 9
Gia tri nho nhat: 1
```

---

### 4.2. Bài toán 2: Tính tổng và trung bình cộng

**Bài toán:** Nhập điểm của học sinh, tính tổng điểm và điểm trung bình.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cout << "Nhap so hoc sinh: ";
    cin >> n;

    vector<double> diem(n);
    for (int i = 0; i < n; i++) {
        cout << "Diem hoc sinh " << i + 1 << ": ";
        cin >> diem[i];
    }

    double tong = 0;
    for (double d : diem) {
        tong += d;
    }

    double trungBinh = tong / n;
    cout << "Tong diem: " << tong << "\n";
    cout << "Diem trung binh: " << trungBinh << "\n";

    return 0;
}
```

---

### 4.3. Bài toán 3: Lọc số chẵn từ danh sách

**Bài toán:** Nhập dãy số nguyên, lọc ra các số chẵn và lưu vào vector mới.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cout << "Nhap so luong phan tu: ";
    cin >> n;

    vector<int> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i];
    }

    vector<int> soChans;   // Vector lưu các số chẵn
    for (int x : v) {
        if (x % 2 == 0) {
            soChans.push_back(x);
        }
    }

    cout << "Cac so chan: ";
    for (int x : soChans) {
        cout << x << " ";
    }
    cout << "\nSo luong so chan: " << soChans.size() << "\n";

    return 0;
}
```

---

### 4.4. Bài toán 4: Tính tổng các hàng và cột của ma trận (Vector 2D)

**Bài toán:** Nhập ma trận m×n, tính tổng từng hàng và từng cột.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int m, n;
    cout << "Nhap so hang: "; cin >> m;
    cout << "Nhap so cot: ";  cin >> n;

    vector<vector<int>> a(m, vector<int>(n));

    cout << "Nhap ma tran:\n";
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            cout << "a[" << i << "][" << j << "] = ";
            cin >> a[i][j];
        }

    // Tính tổng từng hàng
    cout << "\nTong tung hang:\n";
    for (int i = 0; i < m; i++) {
        int tongHang = 0;
        for (int j = 0; j < n; j++) tongHang += a[i][j];
        cout << "Hang " << i << ": " << tongHang << "\n";
    }

    // Tính tổng từng cột
    cout << "\nTong tung cot:\n";
    for (int j = 0; j < n; j++) {
        int tongCot = 0;
        for (int i = 0; i < m; i++) tongCot += a[i][j];
        cout << "Cot " << j << ": " << tongCot << "\n";
    }

    return 0;
}
```

---

### 4.5. Bài toán 5: Đếm số lần xuất hiện phần tử

**Bài toán:** Nhập dãy số nguyên và một số `k`, đếm xem `k` xuất hiện bao nhiêu lần trong dãy.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n, k;
    cout << "Nhap so luong phan tu: ";
    cin >> n;

    vector<int> v(n);
    for (int i = 0; i < n; i++) cin >> v[i];

    cout << "Nhap gia tri can dem: ";
    cin >> k;

    int dem = 0;
    for (int x : v) {
        if (x == k) dem++;
    }

    cout << "So " << k << " xuat hien " << dem << " lan.\n";

    return 0;
}
```

---

## 5. BÀI TẬP THỰC HÀNH

> **Hướng dẫn chung:** Mỗi bài tập, bạn hãy:
> 1. Đọc kỹ đề bài
> 2. Xác định cần dùng vector 1D hay 2D
> 3. Viết code và kiểm tra với ví dụ mẫu

---

### Bài 1: Nhập và in ngược dãy số ⭐

**Đề bài:**  
Nhập vào `n` số nguyên, lưu vào vector. In ra dãy số theo thứ tự **ngược lại**.

**Ví dụ:**
```
Nhap n: 5
Nhap cac so: 1 2 3 4 5
Ket qua: 5 4 3 2 1
```

**Gợi ý:** Dùng vòng lặp `for` từ `v.size() - 1` về `0`.

---

### Bài 2: Tính tổng các số dương ⭐

**Đề bài:**  
Nhập vào `n` số nguyên (có thể âm hoặc dương). Tính và in tổng của các số **dương**.

**Ví dụ:**
```
Nhap n: 6
Nhap cac so: 3 -1 5 -2 7 -4
Tong cac so duong: 15
```

**Gợi ý:** Duyệt qua từng phần tử, kiểm tra `x > 0` thì cộng vào tổng.

---

### Bài 3: Đếm số nguyên tố trong dãy ⭐⭐

**Đề bài:**  
Nhập vào `n` số nguyên dương. Đếm và in ra bao nhiêu số nguyên tố có trong dãy.

**Ví dụ:**
```
Nhap n: 7
Nhap cac so: 2 3 4 5 6 7 8
So luong so nguyen to: 4 (la cac so: 2, 3, 5, 7)
```

**Gợi ý:** Viết hàm `bool laNguyenTo(int x)` kiểm tra số nguyên tố, sau đó áp dụng cho từng phần tử.

---

### Bài 4: Tìm phần tử thứ hai lớn nhất ⭐⭐

**Đề bài:**  
Nhập vào `n` số nguyên (n ≥ 2). Tìm phần tử **lớn thứ hai** trong dãy (không phải lớn nhất).

**Ví dụ:**
```
Nhap n: 5
Nhap cac so: 3 7 1 9 5
Phan tu lon nhat: 9
Phan tu lon thu hai: 7
```

**Gợi ý:** Dùng hai biến `max1` và `max2`. Duyệt từng phần tử và cập nhật.

---

### Bài 5: Xóa các số trùng lặp ⭐⭐

**Đề bài:**  
Nhập vào `n` số nguyên. Tạo ra một vector mới chỉ chứa các số **xuất hiện đúng một lần** (loại bỏ các số bị trùng).

**Ví dụ:**
```
Nhap n: 8
Nhap cac so: 1 2 3 2 4 1 5 3
Cac so khong trung: 4 5
```

**Gợi ý:** Với mỗi phần tử `v[i]`, kiểm tra xem nó có xuất hiện ở vị trí khác không. Nếu chỉ xuất hiện 1 lần, thêm vào vector kết quả.

---

### Bài 6: Cộng hai vector ⭐

**Đề bài:**  
Nhập hai vector `A` và `B`, mỗi vector có `n` phần tử. Tính và in vector `C = A + B` (cộng từng phần tử tương ứng).

**Ví dụ:**
```
Nhap n: 4
Nhap vector A: 1 2 3 4
Nhap vector B: 5 6 7 8
Vector C = A + B: 6 8 10 12
```

**Gợi ý:** Tạo vector `C(n)`, sau đó `C[i] = A[i] + B[i]`.

---

### Bài 7: Tính tổng đường chéo chính của ma trận ⭐⭐

**Đề bài:**  
Nhập vào ma trận vuông `n×n`. Tính tổng các phần tử nằm trên **đường chéo chính** (các phần tử có `i == j`).

**Ví dụ:**
```
Nhap n: 3
Nhap ma tran:
1 2 3
4 5 6
7 8 9
Tong duong cheo chinh: 1 + 5 + 9 = 15
```

**Gợi ý:** Duyệt `i` từ `0` đến `n-1`, cộng `v[i][i]` vào tổng.

---

### Bài 8: Chuyển vị ma trận ⭐⭐

**Đề bài:**  
Nhập vào ma trận `m×n`. In ra **ma trận chuyển vị** (ma trận `n×m`, trong đó hàng và cột được hoán đổi: phần tử `[i][j]` trở thành `[j][i]`).

**Ví dụ:**
```
Ma tran goc (2x3):    Ma tran chuyen vi (3x2):
1  2  3               1  4
4  5  6               2  5
                      3  6
```

**Gợi ý:** Tạo vector 2D `b(n, vector<int>(m))`, sau đó `b[j][i] = a[i][j]`.

---

### Bài 9: Tìm hàng có tổng lớn nhất ⭐⭐

**Đề bài:**  
Nhập vào ma trận `m×n`. Tìm hàng có **tổng các phần tử lớn nhất** và in ra chỉ số hàng đó cùng giá trị tổng.

**Ví dụ:**
```
Nhap ma tran 3x3:
1  2  3
8  1  2
4  5  0
Hang co tong lon nhat: Hang 1 voi tong = 11
```

**Gợi ý:** Tính tổng từng hàng, lưu vào vector `tongHang`. Tìm max trong vector đó.

---

### Bài 10: Xây dựng bảng cửu chương bằng vector 2D ⭐

**Đề bài:**  
Dùng vector 2 chiều để xây dựng và in ra bảng cửu chương từ 1 đến 9 (ma trận 9×9, phần tử `[i][j]` = `(i+1) * (j+1)`).

**Ví dụ đầu ra (một phần):**
```
Bang cuu chuong:
 1  2  3  4  5  6  7  8  9
 2  4  6  8 10 12 14 16 18
 3  6  9 12 15 18 21 24 27
...
```

**Gợi ý:** Dùng hai vòng lặp lồng nhau: `v[i][j] = (i + 1) * (j + 1)`.

---

## TỔNG KẾT

| Chủ đề | Điểm mấu chốt |
|---|---|
| **Vector 1D** | Mảng động, truy cập bằng `v[i]`, thêm xóa bằng `push_back`/`pop_back` |
| **Vector 2D** | Vector của vector, truy cập bằng `v[i][j]`, biểu diễn ma trận |
| **Kích thước** | `v.size()` cho 1D; `v.size()` × `v[0].size()` cho 2D |
| **Duyệt phần tử** | Dùng `for` với chỉ số hoặc range-based `for` |
| **Khai báo thư viện** | Luôn thêm `#include <vector>` vào đầu chương trình |

---

> 💡 **Lời khuyên cho học sinh:** Hãy thực hành bằng cách tự gõ code, đừng copy-paste. Khi gặp lỗi, đọc kỹ thông báo lỗi và thử suy nghĩ nguyên nhân trước khi hỏi thầy/cô. Chúc các bạn học tốt! 🎉
