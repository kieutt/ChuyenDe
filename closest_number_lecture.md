# BÀI GIẢNG: TÌM SỐ GẦN NHẤT CHIA HẾT CHO M

**Dành cho học sinh lớp 10 chuyên Tin học**

---

## 1. GIỚI THIỆU BÀI TOÁN

### Phát biểu bài toán:
Cho hai số nguyên `n` và `m` (với `m ≠ 0`). Hãy tìm số gần nhất với `n` và chia hết cho `m`.

**Lưu ý đặc biệt**: Nếu có nhiều số có cùng khoảng cách đến `n`, thì chọn số có **giá trị tuyệt đối lớn hơn**.

### Ví dụ minh họa:

**Ví dụ 1:**
- Input: `n = 13, m = 4`
- Output: `12`
- Giải thích: 12 là số gần nhất với 13 và chia hết cho 4

**Ví dụ 2:**
- Input: `n = -15, m = 6`
- Output: `-18`
- Giải thích: Cả -12 và -18 đều cách -15 một khoảng 3, nhưng |-18| = 18 > |-12| = 12, nên chọn -18

---

## 2. PHÂN TÍCH BÀI TOÁN

### Cách suy nghĩ:
1. **Tìm thương**: `q = n ÷ m` (phép chia lấy phần nguyên)
2. **Tìm hai số ứng viên**:
   - `n1 = m × q` (bội của m gần nhất không vượt quá n)
   - `n2` = bội tiếp theo hoặc trước đó của m
3. **So sánh khoảng cách**: Chọn số có khoảng cách nhỏ hơn đến n
4. **Xử lý trường hợp đặc biệt**: Nếu khoảng cách bằng nhau, chọn số có giá trị tuyệt đối lớn hơn

### Tại sao cần xét dấu của n và m?
- Nếu `n` và `m` **cùng dấu**: `n2 = m × (q + 1)` (tiến về phía n)
- Nếu `n` và `m` **khác dấu**: `n2 = m × (q - 1)` (lùi về phía n)

---

## 3. CODE MINH HỌA

### 3.1. Code C++ - Giải pháp tối ưu O(1)

```cpp
#include <iostream>
#include <cmath>    // Thư viện để sử dụng hàm abs()
using namespace std;

/**
 * Hàm tìm số gần nhất với n và chia hết cho m
 * @param n: Số cần tìm số gần nhất
 * @param m: Số chia (m ≠ 0)
 * @return: Số gần nhất với n và chia hết cho m
 */
int closestNumber(int n, int m) {
    // Bước 1: Tính thương khi chia n cho m
    int q = n / m;
    
    // Bước 2: Tìm số ứng viên thứ nhất
    // n1 là bội của m gần nhất không vượt quá n
    int n1 = m * q;
    
    // Bước 3: Tìm số ứng viên thứ hai
    int n2;
    if ((n * m) > 0) {
        // n và m cùng dấu: tiến về phía n
        n2 = m * (q + 1);
    } else {
        // n và m khác dấu: lùi về phía n  
        n2 = m * (q - 1);
    }
    
    // Bước 4: So sánh khoảng cách và trả về kết quả
    int distance1 = abs(n - n1);  // Khoảng cách từ n đến n1
    int distance2 = abs(n - n2);  // Khoảng cách từ n đến n2
    
    if (distance1 < distance2) {
        return n1;  // n1 gần hơn
    } else if (distance1 > distance2) {
        return n2;  // n2 gần hơn
    } else {
        // Khoảng cách bằng nhau: chọn số có giá trị tuyệt đối lớn hơn
        return (abs(n1) > abs(n2)) ? n1 : n2;
    }
}

// Hàm in chi tiết quá trình giải
void printDetailedSolution(int n, int m) {
    cout << "\n=== CHI TIẾT GIẢI BÀI TOÁN ===" << endl;
    cout << "Input: n = " << n << ", m = " << m << endl;
    
    int q = n / m;
    cout << "Thương q = n/m = " << n << "/" << m << " = " << q << endl;
    
    int n1 = m * q;
    cout << "Ứng viên 1: n1 = m*q = " << m << "*" << q << " = " << n1 << endl;
    
    int n2 = (n * m) > 0 ? (m * (q + 1)) : (m * (q - 1));
    cout << "Ứng viên 2: n2 = ";
    if ((n * m) > 0) {
        cout << m << "*(" << q << "+1) = " << n2 << " (cùng dấu)" << endl;
    } else {
        cout << m << "*(" << q << "-1) = " << n2 << " (khác dấu)" << endl;
    }
    
    int dist1 = abs(n - n1);
    int dist2 = abs(n - n2);
    cout << "Khoảng cách: |" << n << "-" << n1 << "| = " << dist1 << endl;
    cout << "Khoảng cách: |" << n << "-" << n2 << "| = " << dist2 << endl;
    
    int result = closestNumber(n, m);
    cout << "Kết quả: " << result << endl;
    cout << "=========================" << endl;
}

int main() {
    // Test các trường hợp cơ bản
    cout << "=== TEST CÁC TRƯỜNG HỢP KHÁC NHAU ===" << endl;
    
    printDetailedSolution(13, 4);   // Trường hợp cơ bản
    printDetailedSolution(-15, 6);  // Trường hợp có số âm
    printDetailedSolution(5, 2);    // Trường hợp khoảng cách bằng nhau
    printDetailedSolution(12, 4);   // Trường hợp n chia hết cho m
    
    return 0;
}
```

### 3.2. Code Python - Giải pháp tối ưu O(1)

```python
def closest_number(n, m):
    """
    Tìm số gần nhất với n và chia hết cho m
    
    Args:
        n (int): Số cần tìm số gần nhất
        m (int): Số chia (m ≠ 0)
    
    Returns:
        int: Số gần nhất với n và chia hết cho m
    """
    # Bước 1: Tính thương khi chia n cho m
    q = n // m  # Phép chia lấy phần nguyên
    
    # Bước 2: Tìm số ứng viên thứ nhất
    # n1 là bội của m gần nhất không vượt quá n
    n1 = m * q
    
    # Bước 3: Tìm số ứng viên thứ hai
    if (n * m) > 0:
        # n và m cùng dấu: tiến về phía n
        n2 = m * (q + 1)
    else:
        # n và m khác dấu: lùi về phía n
        n2 = m * (q - 1)
    
    # Bước 4: So sánh khoảng cách và trả về kết quả
    distance1 = abs(n - n1)  # Khoảng cách từ n đến n1
    distance2 = abs(n - n2)  # Khoảng cách từ n đến n2
    
    if distance1 < distance2:
        return n1  # n1 gần hơn
    elif distance1 > distance2:
        return n2  # n2 gần hơn
    else:
        # Khoảng cách bằng nhau: chọn số có giá trị tuyệt đối lớn hơn
        return n1 if abs(n1) > abs(n2) else n2

def print_detailed_solution(n, m):
    """In chi tiết quá trình giải bài toán"""
    print("\n=== CHI TIẾT GIẢI BÀI TOÁN ===")
    print(f"Input: n = {n}, m = {m}")
    
    q = n // m
    print(f"Thương q = n//m = {n}//{m} = {q}")
    
    n1 = m * q
    print(f"Ứng viên 1: n1 = m*q = {m}*{q} = {n1}")
    
    if (n * m) > 0:
        n2 = m * (q + 1)
        print(f"Ứng viên 2: n2 = {m}*({q}+1) = {n2} (cùng dấu)")
    else:
        n2 = m * (q - 1)
        print(f"Ứng viên 2: n2 = {m}*({q}-1) = {n2} (khác dấu)")
    
    dist1 = abs(n - n1)
    dist2 = abs(n - n2)
    print(f"Khoảng cách: |{n}-{n1}| = {dist1}")
    print(f"Khoảng cách: |{n}-{n2}| = {dist2}")
    
    result = closest_number(n, m)
    print(f"Kết quả: {result}")
    print("=" * 27)

# Chương trình chính
if __name__ == "__main__":
    print("=== TEST CÁC TRƯỜNG HỢP KHÁC NHAU ===")
    
    # Test các trường hợp khác nhau
    test_cases = [
        (13, 4),    # Trường hợp cơ bản
        (-15, 6),   # Trường hợp có số âm
        (5, 2),     # Trường hợp khoảng cách bằng nhau
        (12, 4),    # Trường hợp n chia hết cho m
        (-7, -3),   # Trường hợp cả hai số đều âm
        (0, 5)      # Trường hợp n = 0
    ]
    
    for n, m in test_cases:
        print_detailed_solution(n, m)
```

---

## 4. PHƯƠNG PHÁP GIẢI NAIVE (CHO HIỂU THÊM)

### Code C++ - Phương pháp duyệt O(m)

```cpp
#include <iostream>
#include <climits>
#include <cmath>
using namespace std;

/**
 * Phương pháp naive: Duyệt tất cả số xung quanh n
 * Độ phức tạp: O(m) - không hiệu quả với m lớn
 */
int closestNumberNaive(int n, int m) {
    int closest = 0;
    int minDistance = INT_MAX;  // Giá trị vô cùng lớn
    
    // Duyệt từ (n - |m|) đến (n + |m|)
    for (int i = n - abs(m); i <= n + abs(m); i++) {
        if (i % m == 0) {  // Nếu i chia hết cho m
            int distance = abs(n - i);
            
            // Cập nhật nếu tìm được số gần hơn hoặc
            // cùng khoảng cách nhưng giá trị tuyệt đối lớn hơn
            if (distance < minDistance || 
                (distance == minDistance && abs(i) > abs(closest))) {
                closest = i;
                minDistance = distance;
            }
        }
    }
    return closest;
}
```

### So sánh hai phương pháp:

| Phương pháp | Độ phức tạp | Ưu điểm | Nhược điểm |
|-------------|-------------|---------|------------|
| **Naive** | O(m) | Dễ hiểu, dễ code | Chậm với m lớn |
| **Optimal** | O(1) | Nhanh, hiệu quả | Cần hiểu logic phức tạp hơn |

---

## 5. CÁC TRƯỜNG HỢP ĐẶC BIỆT

### 5.1. Trường hợp n chia hết cho m
```
n = 12, m = 4
→ q = 3, n1 = 12, n2 = 16
→ Khoảng cách: 0 và 4 → Chọn 12
```

### 5.2. Trường hợp khoảng cách bằng nhau
```
n = 5, m = 2
→ q = 2, n1 = 4, n2 = 6
→ Khoảng cách: |5-4| = 1, |5-6| = 1
→ |4| = 4 < |6| = 6 → Chọn 6
```

### 5.3. Trường hợp số âm
```
n = -15, m = 6
→ q = -3, n1 = -18, n2 = -12
→ Khoảng cách: |-15-(-18)| = 3, |-15-(-12)| = 3
→ |-18| = 18 > |-12| = 12 → Chọn -18
```

---

## 6. BÀI TẬP THỰC HÀNH

### Bài tập 1: Tính tay
Tính kết quả cho các trường hợp sau:
1. `n = 17, m = 5`
2. `n = -8, m = 3` 
3. `n = 7, m = -2`
4. `n = -10, m = -4`

### Bài tập 2: Viết code
Viết chương trình nhập `n` và `m` từ bàn phím, xuất kết quả và giải thích chi tiết quá trình tính.

### Bài tập 3: Mở rộng
Viết hàm tìm **tất cả** các số gần nhất với `n` và chia hết cho `m` (có thể có nhiều số cùng khoảng cách).

---

## 7. TÓM TẮT

### Thuật toán chính:
1. **Tính thương**: `q = n / m`
2. **Tìm hai ứng viên**: `n1 = m*q` và `n2` (phụ thuộc dấu)
3. **So sánh khoảng cách**: Chọn số gần nhất
4. **Xử lý trường hợp đặc biệt**: Chọn số có giá trị tuyệt đối lớn hơn nếu khoảng cách bằng nhau

### Độ phức tạp:
- **Thời gian**: O(1)
- **Không gian**: O(1)

### Điểm quan trọng cần nhớ:
- Phải xét dấu của `n` và `m` để tìm `n2` đúng
- Trường hợp khoảng cách bằng nhau: chọn số có **giá trị tuyệt đối lớn hơn**
- Thuật toán hoạt động với mọi số nguyên (âm, dương, zero)

---

**Chúc các em học tốt!** 🎓