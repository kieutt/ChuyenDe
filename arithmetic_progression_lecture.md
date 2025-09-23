# CHUYÊN ĐỀ: CẤP SỐ CỘNG (ARITHMETIC PROGRESSION)
## Tìm Số Hạng Thứ N từ Hai Số Hạng Đầu

**Dành cho học sinh lớp chuyên Tin học**

---

## 📋 MỤC LỤC
1. [Giới thiệu về Cấp số cộng](#1-giới-thiệu-về-cấp-số-cộng)
2. [Bài toán cơ bản](#2-bài-toán-cơ-bản)
3. [Phân tích toán học](#3-phân-tích-toán-học)
4. [Các phương pháp giải](#4-các-phương-pháp-giải)
5. [Code minh họa](#5-code-minh-họa)
6. [Bài toán mở rộng](#6-bài-toán-mở-rộng)
7. [Ứng dụng thực tế](#7-ứng-dụng-thực-tế)
8. [Bài tập thực hành](#8-bài-tập-thực-hành)

---

## 1. GIỚI THIỆU VỀ CẤP SỐ CỘNG

### 📖 Định nghĩa
**Cấp số cộng** (Arithmetic Progression - AP) là một dãy số mà hiệu của hai số hạng liên tiếp luôn không đổi.

Hiệu không đổi này được gọi là **công sai** (common difference), ký hiệu là **d**.

### 🔢 Dạng tổng quát
```
a₁, a₂, a₃, a₄, ..., aₙ

Trong đó: a₂ - a₁ = a₃ - a₂ = a₄ - a₃ = ... = d
```

### 📚 Ví dụ minh họa
```
Ví dụ 1: 2, 5, 8, 11, 14, ...
→ d = 5 - 2 = 3

Ví dụ 2: 10, 7, 4, 1, -2, ...
→ d = 7 - 10 = -3

Ví dụ 3: 1, 1, 1, 1, 1, ...
→ d = 1 - 1 = 0
```

### 🎯 Các thuật ngữ quan trọng
- **a₁**: Số hạng đầu tiên
- **d**: Công sai
- **aₙ**: Số hạng thứ n
- **Sₙ**: Tổng n số hạng đầu

---

## 2. BÀI TOÁN CƠ BẢN

### 📝 Phát biểu
Cho hai số nguyên a₁ và a₂ lần lượt là số hạng thứ nhất và thứ hai của một cấp số cộng. Hãy tìm số hạng thứ n của cấp số cộng này.

### 📊 Input/Output
```
Input: a₁ = 2, a₂ = 3, n = 4
Output: 5
Giải thích: Dãy là 2, 3, 4, 5, 6, ... → số hạng thứ 4 là 5

Input: a₁ = 1, a₂ = 3, n = 10  
Output: 19
Giải thích: Dãy là 1, 3, 5, 7, 9, 11, 13, 15, 17, 19, ... → số hạng thứ 10 là 19
```

---

## 3. PHÂN TÍCH TOÁN HỌC

### 🔍 Tìm công sai
Từ định nghĩa cấp số cộng:
```
d = a₂ - a₁
```

### 📐 Công thức tổng quát
Số hạng thứ n của cấp số cộng:
```
aₙ = a₁ + (n - 1) × d
```

### 📝 Chứng minh công thức
```
a₁ = a₁
a₂ = a₁ + d
a₃ = a₁ + 2d = a₁ + (3-1)d
a₄ = a₁ + 3d = a₁ + (4-1)d
...
aₙ = a₁ + (n-1)d
```

### 🎯 Thay d = a₂ - a₁
```
aₙ = a₁ + (n - 1) × (a₂ - a₁)
```

---

## 4. CÁC PHƯƠNG PHÁP GIẢI

### 4.1. Phương pháp Lặp (Naive)
**Độ phức tạp**: O(n)  
**Ý tưởng**: Tính từng số hạng một cách tuần tự

**Ưu điểm**:
- Dễ hiểu, trực quan
- Có thể in ra toàn bộ dãy số

**Nhược điểm**:
- Chậm với n lớn
- Không hiệu quả về mặt tính toán

### 4.2. Phương pháp Công thức (Optimal)
**Độ phức tạp**: O(1)  
**Ý tưởng**: Sử dụng công thức toán học trực tiếp

**Ưu điểm**:
- Nhanh chóng, hiệu quả
- Không phụ thuộc vào kích thước n
- Tận dụng được tính chất toán học

**Nhược điểm**:
- Cần hiểu và nhớ công thức

---

## 5. CODE MINH HỌA

### 5.1. Code C++ - Đầy đủ cả hai phương pháp

```cpp
#include <iostream>
#include <vector>
#include <chrono>
#include <iomanip>
using namespace std;
using namespace chrono;

/**
 * Phương pháp Lặp - O(n)
 * Tính số hạng thứ n bằng cách lặp từ a1
 */
long long nthTermNaive(long long a1, long long a2, int n) {
    if (n == 1) return a1;
    if (n == 2) return a2;
    
    long long currentTerm = a1;
    long long d = a2 - a1;  // Tính công sai
    
    // Lặp từ số hạng thứ 1 đến thứ n
    for (int i = 1; i < n; i++) {
        currentTerm += d;
    }
    
    return currentTerm;
}

/**
 * Phương pháp Công thức - O(1)
 * Sử dụng công thức toán học trực tiếp
 */
long long nthTermOptimal(long long a1, long long a2, int n) {
    // Công thức: an = a1 + (n-1) * d
    // Với d = a2 - a1
    return a1 + (long long)(n - 1) * (a2 - a1);
}

/**
 * Hàm tạo và hiển thị cấp số cộng
 */
void displayArithmeticSequence(long long a1, long long a2, int count) {
    cout << "\n🔢 CẤP SỐ CỘNG:" << endl;
    cout << "Số hạng đầu (a₁): " << a1 << endl;
    cout << "Số hạng thứ 2 (a₂): " << a2 << endl;
    cout << "Công sai (d): " << (a2 - a1) << endl;
    
    cout << "\nDãy số (" << count << " số hạng đầu): ";
    for (int i = 1; i <= count; i++) {
        cout << nthTermOptimal(a1, a2, i);
        if (i < count) cout << ", ";
    }
    cout << endl;
}

/**
 * Hàm so sánh hiệu suất hai phương pháp
 */
void comparePerformance(long long a1, long long a2, int n) {
    cout << "\n⏱️ SO SÁNH HIỆU SUẤT:" << endl;
    
    // Test phương pháp Naive
    auto start = high_resolution_clock::now();
    long long result1 = nthTermNaive(a1, a2, n);
    auto end = high_resolution_clock::now();
    auto duration1 = duration_cast<microseconds>(end - start);
    
    // Test phương pháp Optimal
    start = high_resolution_clock::now();
    long long result2 = nthTermOptimal(a1, a2, n);
    end = high_resolution_clock::now();
    auto duration2 = duration_cast<microseconds>(end - start);
    
    cout << "Phương pháp Lặp (O(n)):" << endl;
    cout << "  → Kết quả: " << result1 << endl;
    cout << "  → Thời gian: " << duration1.count() << " μs" << endl;
    
    cout << "Phương pháp Công thức (O(1)):" << endl;
    cout << "  → Kết quả: " << result2 << endl;
    cout << "  → Thời gian: " << duration2.count() << " μs" << endl;
    
    cout << "✓ Kết quả khớp: " << (result1 == result2 ? "Có" : "Không") << endl;
    
    if (duration1.count() > 0) {
        double speedup = (double)duration1.count() / duration2.count();
        cout << "⚡ Tăng tốc: " << fixed << setprecision(2) << speedup << "x" << endl;
    }
}

/**
 * Hàm demo chi tiết với giải thích
 */
void detailedDemo(long long a1, long long a2, int n) {
    cout << "\n📖 DEMO CHI TIẾT:" << endl;
    cout << "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" << endl;
    cout << "Input: a₁ = " << a1 << ", a₂ = " << a2 << ", n = " << n << endl;
    
    // Bước 1: Tính công sai
    long long d = a2 - a1;
    cout << "\nBước 1: Tính công sai" << endl;
    cout << "  d = a₂ - a₁ = " << a2 << " - " << a1 << " = " << d << endl;
    
    // Bước 2: Áp dụng công thức
    cout << "\nBước 2: Áp dụng công thức" << endl;
    cout << "  aₙ = a₁ + (n-1) × d" << endl;
    cout << "  a₍" << n << "₎ = " << a1 << " + (" << n << "-1) × " << d << endl;
    cout << "  a₍" << n << "₎ = " << a1 << " + " << (n-1) << " × " << d << endl;
    cout << "  a₍" << n << "₎ = " << a1 << " + " << ((n-1) * d) << endl;
    
    long long result = nthTermOptimal(a1, a2, n);
    cout << "  a₍" << n << "₎ = " << result << endl;
    
    // Bước 3: Kiểm tra bằng phương pháp lặp
    cout << "\nBước 3: Kiểm tra bằng phương pháp lặp" << endl;
    cout << "  Dãy số: ";
    
    int displayCount = min(n, 10);  // Hiển thị tối đa 10 số hạng
    for (int i = 1; i <= displayCount; i++) {
        cout << nthTermOptimal(a1, a2, i);
        if (i < displayCount && i < n) cout << ", ";
        else if (i == displayCount && n > displayCount) cout << ", ...";
    }
    
    if (n > displayCount) {
        cout << ", " << result;
    }
    cout << endl;
    
    cout << "\n✓ Kết quả: Số hạng thứ " << n << " là " << result << endl;
}

/**
 * Class quản lý cấp số cộng
 */
class ArithmeticProgression {
private:
    long long first_term;   // a₁
    long long second_term;  // a₂  
    long long common_diff;  // d
    
public:
    // Constructor
    ArithmeticProgression(long long a1, long long a2) 
        : first_term(a1), second_term(a2), common_diff(a2 - a1) {}
    
    // Getter methods
    long long getFirstTerm() const { return first_term; }
    long long getSecondTerm() const { return second_term; }
    long long getCommonDiff() const { return common_diff; }
    
    // Tính số hạng thứ n
    long long getNthTerm(int n) const {
        if (n <= 0) throw invalid_argument("n phải lớn hơn 0");
        return first_term + (long long)(n - 1) * common_diff;
    }
    
    // Tính tổng n số hạng đầu
    long long getSumOfNTerms(int n) const {
        if (n <= 0) throw invalid_argument("n phải lớn hơn 0");
        // Sn = n/2 * [2a₁ + (n-1)d]
        return (long long)n * (2 * first_term + (long long)(n - 1) * common_diff) / 2;
    }
    
    // Tìm vị trí của một số trong cấp số cộng
    int findPosition(long long value) const {
        // value = a₁ + (n-1)d
        // n = (value - a₁)/d + 1
        if (common_diff == 0) {
            return (value == first_term) ? 1 : -1;
        }
        
        if ((value - first_term) % common_diff != 0) {
            return -1;  // Không thuộc cấp số cộng
        }
        
        int position = (value - first_term) / common_diff + 1;
        return (position > 0) ? position : -1;
    }
    
    // Tạo vector chứa n số hạng đầu
    vector<long long> generateSequence(int n) const {
        vector<long long> sequence;
        for (int i = 1; i <= n; i++) {
            sequence.push_back(getNthTerm(i));
        }
        return sequence;
    }
    
    // Hiển thị thông tin cấp số cộng
    void displayInfo() const {
        cout << "📊 THÔNG TIN CẤP SỐ CỘNG:" << endl;
        cout << "  Số hạng đầu (a₁): " << first_term << endl;
        cout << "  Số hạng thứ 2 (a₂): " << second_term << endl;
        cout << "  Công sai (d): " << common_diff << endl;
        cout << "  Công thức tổng quát: aₙ = " << first_term;
        if (common_diff > 0) cout << " + ";
        if (common_diff != 0) cout << common_diff << "(n-1)" << endl;
        else cout << endl;
    }
};

int main() {
    cout << "🎓 CHUYÊN ĐỀ: CẤP SỐ CỘNG 🎓" << endl;
    cout << "=================================" << endl;
    
    // Test case 1: Cấp số cộng với công sai dương
    cout << "\n1️⃣ TEST CASE 1: Công sai dương" << endl;
    detailedDemo(2, 3, 4);
    displayArithmeticSequence(2, 3, 8);
    
    // Test case 2: Cấp số cộng với công sai âm
    cout << "\n2️⃣ TEST CASE 2: Công sai âm" << endl;
    detailedDemo(10, 7, 5);
    displayArithmeticSequence(10, 7, 8);
    
    // Test case 3: Cấp số cộng với công sai bằng 0
    cout << "\n3️⃣ TEST CASE 3: Công sai bằng 0" << endl;
    detailedDemo(5, 5, 10);
    
    // Test case 4: Số lớn
    cout << "\n4️⃣ TEST CASE 4: Số lớn" << endl;
    detailedDemo(1, 3, 1000);
    comparePerformance(1, 3, 1000);
    
    // Demo sử dụng class
    cout << "\n5️⃣ DEMO SỬ DỤNG CLASS:" << endl;
    cout << "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━" << endl;
    
    ArithmeticProgression ap(1, 3);
    ap.displayInfo();
    
    cout << "\nCác tính năng:" << endl;
    cout << "  → Số hạng thứ 10: " << ap.getNthTerm(10) << endl;
    cout << "  → Tổng 10 số hạng đầu: " << ap.getSumOfNTerms(10) << endl;
    cout << "  → Vị trí của số 19: " << ap.findPosition(19) << endl;
    cout << "  → Vị trí của số 20: " << ap.findPosition(20) << endl;
    
    cout << "\n✅ Hoàn thành demo!" << endl;
    
    return 0;
}
```

### 5.2. Code Python - Phiên bản đầy đủ

```python
import time
import math
from typing import List, Tuple, Optional

def nth_term_naive(a1: int, a2: int, n: int) -> int:
    """
    Phương pháp Lặp - O(n)
    Tính số hạng thứ n bằng cách lặp từ a1
    
    Args:
        a1 (int): Số hạng đầu tiên
        a2 (int): Số hạng thứ hai
        n (int): Vị trí cần tìm
        
    Returns:
        int: Số hạng thứ n
    """
    if n == 1:
        return a1
    if n == 2:
        return a2
    
    current_term = a1
    d = a2 - a1  # Tính công sai
    
    # Lặp từ số hạng thứ 1 đến thứ n
    for i in range(1, n):
        current_term += d
    
    return current_term

def nth_term_optimal(a1: int, a2: int, n: int) -> int:
    """
    Phương pháp Công thức - O(1)
    Sử dụng công thức toán học trực tiếp
    
    Args:
        a1 (int): Số hạng đầu tiên
        a2 (int): Số hạng thứ hai
        n (int): Vị trí cần tìm
        
    Returns:
        int: Số hạng thứ n
    """
    # Công thức: an = a1 + (n-1) * d
    # Với d = a2 - a1
    return a1 + (n - 1) * (a2 - a1)

def display_arithmetic_sequence(a1: int, a2: int, count: int) -> None:
    """Hiển thị cấp số cộng"""
    print(f"\n🔢 CẤP SỐ CỘNG:")
    print(f"Số hạng đầu (a₁): {a1}")
    print(f"Số hạng thứ 2 (a₂): {a2}")
    print(f"Công sai (d): {a2 - a1}")
    
    sequence = [nth_term_optimal(a1, a2, i) for i in range(1, count + 1)]
    print(f"\nDãy số ({count} số hạng đầu): {', '.join(map(str, sequence))}")

def compare_performance(a1: int, a2: int, n: int) -> None:
    """So sánh hiệu suất hai phương pháp"""
    print(f"\n⏱️ SO SÁNH HIỆU SUẤT:")
    
    # Test phương pháp Naive
    start_time = time.perf_counter()
    result1 = nth_term_naive(a1, a2, n)
    end_time = time.perf_counter()
    time1 = (end_time - start_time) * 1_000_000  # Convert to microseconds
    
    # Test phương pháp Optimal
    start_time = time.perf_counter()
    result2 = nth_term_optimal(a1, a2, n)
    end_time = time.perf_counter()
    time2 = (end_time - start_time) * 1_000_000  # Convert to microseconds
    
    print(f"Phương pháp Lặp (O(n)):")
    print(f"  → Kết quả: {result1}")
    print(f"  → Thời gian: {time1:.2f} μs")
    
    print(f"Phương pháp Công thức (O(1)):")
    print(f"  → Kết quả: {result2}")
    print(f"  → Thời gian: {time2:.2f} μs")
    
    print(f"✓ Kết quả khớp: {'Có' if result1 == result2 else 'Không'}")
    
    if time1 > 0 and time2 > 0:
        speedup = time1 / time2
        print(f"⚡ Tăng tốc: {speedup:.2f}x")

def detailed_demo(a1: int, a2: int, n: int) -> None:
    """Demo chi tiết với giải thích"""
    print(f"\n📖 DEMO CHI TIẾT:")
    print("━" * 50)
    print(f"Input: a₁ = {a1}, a₂ = {a2}, n = {n}")
    
    # Bước 1: Tính công sai
    d = a2 - a1
    print(f"\nBước 1: Tính công sai")
    print(f"  d = a₂ - a₁ = {a2} - {a1} = {d}")
    
    # Bước 2: Áp dụng công thức
    print(f"\nBước 2: Áp dụng công thức")
    print(f"  aₙ = a₁ + (n-1) × d")
    print(f"  a₍{n}₎ = {a1} + ({n}-1) × {d}")
    print(f"  a₍{n}₎ = {a1} + {n-1} × {d}")
    print(f"  a₍{n}₎ = {a1} + {(n-1) * d}")
    
    result = nth_term_optimal(a1, a2, n)
    print(f"  a₍{n}₎ = {result}")
    
    # Bước 3: Kiểm tra bằng phương pháp lặp
    print(f"\nBước 3: Kiểm tra bằng phương pháp lặp")
    display_count = min(n, 10)  # Hiển thị tối đa 10 số hạng
    
    sequence = [nth_term_optimal(a1, a2, i) for i in range(1, display_count + 1)]
    sequence_str = ", ".join(map(str, sequence))
    
    if n > display_count:
        sequence_str += f", ..., {result}"
    
    print(f"  Dãy số: {sequence_str}")
    print(f"\n✓ Kết quả: Số hạng thứ {n} là {result}")

class ArithmeticProgression:
    """Class quản lý cấp số cộng"""
    
    def __init__(self, a1: int, a2: int):
        """
        Khởi tạo cấp số cộng
        
        Args:
            a1 (int): Số hạng đầu tiên
            a2 (int): Số hạng thứ hai
        """
        self.first_term = a1
        self.second_term = a2
        self.common_diff = a2 - a1
    
    def get_nth_term(self, n: int) -> int:
        """
        Tính số hạng thứ n
        
        Args:
            n (int): Vị trí cần tìm (n > 0)
            
        Returns:
            int: Số hạng thứ n
            
        Raises:
            ValueError: Nếu n <= 0
        """
        if n <= 0:
            raise ValueError("n phải lớn hơn 0")
        return self.first_term + (n - 1) * self.common_diff
    
    def get_sum_of_n_terms(self, n: int) -> int:
        """
        Tính tổng n số hạng đầu
        
        Args:
            n (int): Số lượng số hạng cần tính tổng
            
        Returns:
            int: Tổng n số hạng đầu
        """
        if n <= 0:
            raise ValueError("n phải lớn hơn 0")
        # Sn = n/2 * [2a₁ + (n-1)d]
        return n * (2 * self.first_term + (n - 1) * self.common_diff) // 2
    
    def find_position(self, value: int) -> Optional[int]:
        """
        Tìm vị trí của một số trong cấp số cộng
        
        Args:
            value (int): Giá trị cần tìm vị trí
            
        Returns:
            Optional[int]: Vị trí của số (bắt đầu từ 1), None nếu không tìm thấy
        """
        if self.common_diff == 0:
            return 1 if value == self.first_term else None
        
        if (value - self.first_term) % self.common_diff != 0:
            return None  # Không thuộc cấp số cộng
        
        position = (value - self.first_term) // self.common_diff + 1
        return position if position > 0 else None
    
    def generate_sequence(self, n: int) -> List[int]:
        """
        Tạo danh sách chứa n số hạng đầu
        
        Args:
            n (int): Số lượng số hạng cần tạo
            
        Returns:
            List[int]: Danh sách n số hạng đầu
        """
        return [self.get_nth_term(i) for i in range(1, n + 1)]
    
    def is_arithmetic_sequence(self, sequence: List[int]) -> bool:
        """
        Kiểm tra xem một dãy có phải là cấp số cộng không
        
        Args:
            sequence (List[int]): Dãy số cần kiểm tra
            
        Returns:
            bool: True nếu là cấp số cộng, False nếu không
        """
        if len(sequence) < 2:
            return True
        
        diff = sequence[1] - sequence[0]
        for i in range(2, len(sequence)):
            if sequence[i] - sequence[i-1] != diff:
                return False
        return True
    
    def display_info(self) -> None:
        """Hiển thị thông tin cấp số cộng"""
        print("📊 THÔNG TIN CẤP SỐ CỘNG:")
        print(f"  Số hạng đầu (a₁): {self.first_term}")
        print(f"  Số hạng thứ 2 (a₂): {self.second_term}")
        print(f"  Công sai (d): {self.common_diff}")
        formula = f"aₙ = {self.first_term}"
        if self.common_diff > 0:
            formula += f" + {self.common_diff}(n-1)"
        elif self.common_diff < 0:
            formula += f" - {abs(self.common_diff)}(n-1)"
        else:
            formula += " (dãy hằng số)"
        print(f"  Công thức tổng quát: {formula}")

def main():
    """Chương trình chính"""
    print("🎓 CHUYÊN ĐỀ: CẤP SỐ CỘNG 🎓")
    print("=" * 35)
    
    # Test case 1: Cấp số cộng với công sai dương
    print("\n1️⃣ TEST CASE 1: Công sai dương")
    detailed_demo(2, 3, 4)
    display_arithmetic_sequence(2, 3, 8)
    
    # Test case 2: Cấp số cộng với công sai âm
    print("\n2️⃣ TEST CASE 2: Công sai âm")
    detailed_demo(10, 7, 5)
    display_arithmetic_sequence(10, 7, 8)
    
    # Test case 3: Cấp số cộng với công sai bằng 0
    print("\n3️⃣ TEST CASE 3: Công sai bằng 0")
    detailed_demo(5, 5, 10)
    
    # Test case 4: Số lớn
    print("\n4️⃣ TEST CASE 4: Số lớn")
    detailed_demo(1, 3, 1000)
    compare_performance(1, 3, 1000)
    
    # Demo sử dụng class
    print("\n5️⃣ DEMO SỬ DỤNG CLASS:")
    print("━" * 50)
    
    ap = ArithmeticProgression(1, 3)
    ap.display_info()
    
    print("\nCác tính năng:")
    print(f"  → Số hạng thứ 10: {ap.get_nth_term(10)}")
    print(f"  → Tổng 10 số hạng đầu: {ap.get_sum_of_n_terms(10)}")
    print(f"  → Vị trí của số 19: {ap.find_position(19)}")
    print(f"  → Vị trí của số 20: {ap.find_position(20)}")
    
    # Kiểm tra dãy số
    test_sequence = [1, 3, 5, 7, 9]
    print(f"  → Dãy {test_sequence} có phải AP không: {ap.is_arithmetic_sequence(test_sequence)}")
    
    non_ap_sequence = [1, 3, 6, 10, 15]
    print(f"  → Dãy {non_ap_sequence} có phải AP không: {ap.is_arithmetic_sequence(non_ap_sequence)}")
    
    print("\n✅ Hoàn thành demo!")

if __name__ == "__main__":
    main()
```

---

## 6. BÀI TOÁN MỞ RỘNG

### 6.1. Tìm cấp số cộng từ ba số hạng bất kỳ

```cpp
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

/**
 * Kiểm tra ba số có tạo thành cấp số cộng không
 */
bool isArithmeticProgression(int a, int b, int c) {
    return (b - a) == (c - b);
}

/**
 * Tìm số hạng thiếu trong cấp số cộng
 * Cho ba số trong đó có một số bị thiếu (ký hiệu -1)
 */
int findMissingTerm(int a, int b, int c) {
    if (a == -1) {
        // Thiếu số hạng đầu: a = 2b - c
        return 2 * b - c;
    } else if (b == -1) {
        // Thiếu số hạng giữa: b = (a + c) / 2
        return (a + c) / 2;
    } else if (c == -1) {
        // Thiếu số hạng cuối: c = 2b - a
        return 2 * b - a;
    }
    return 0; // Không có số thiếu
}

/**
 * Tạo cấp số cộng từ số hạng đầu, cuối và số lượng số hạng
 */
vector<double> createAPFromFirstLastCount(double first, double last, int count) {
    if (count < 2) return {};
    
    double d = (last - first) / (count - 1);
    vector<double> ap;
    
    for (int i = 0; i < count; i++) {
        ap.push_back(first + i * d);
    }
    
    return ap;
}

int main() {
    cout << "🔢 BÀI TOÁN MỞ RỘNG - CẤP SỐ CỘNG" << endl;
    cout << "=================================" << endl;
    
    // Test 1: Kiểm tra cấp số cộng
    cout << "\n📊 Test 1: Kiểm tra cấp số cộng" << endl;
    vector<vector<int>> testSequences = {
        {2, 4, 6}, {1, 3, 5}, {5, 5, 5}, {1, 2, 4}
    };
    
    for (auto seq : testSequences) {
        bool isAP = isArithmeticProgression(seq[0], seq[1], seq[2]);
        cout << "  " << seq[0] << ", " << seq[1] << ", " << seq[2] 
             << " → " << (isAP ? "✓ Là AP" : "✗ Không phải AP") << endl;
    }
    
    // Test 2: Tìm số hạng thiếu
    cout << "\n🔍 Test 2: Tìm số hạng thiếu" << endl;
    vector<vector<int>> missingTests = {
        {-1, 5, 9},   // Thiếu số đầu
        {2, -1, 8},   // Thiếu số giữa
        {3, 7, -1}    // Thiếu số cuối
    };
    
    for (auto test : missingTests) {
        int missing = findMissingTerm(test[0], test[1], test[2]);
        cout << "  ";
        for (int i = 0; i < 3; i++) {
            if (test[i] == -1) cout << "?, ";
            else cout << test[i] << ", ";
        }
        cout << "→ Số thiếu: " << missing << endl;
    }
    
    // Test 3: Tạo AP từ đầu, cuối và số lượng
    cout << "\n🎯 Test 3: Tạo AP từ số đầu, cuối, số lượng" << endl;
    auto ap = createAPFromFirstLastCount(1.0, 10.0, 5);
    cout << "  Từ 1 đến 10 với 5 số hạng: ";
    for (double x : ap) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 6.2. Cấp số cộng 2D (Ma trận cấp số cộng)

```python
import numpy as np
from typing import List, Tuple

class ArithmeticProgressionMatrix:
    """Ma trận cấp số cộng 2D"""
    
    def __init__(self, rows: int, cols: int, start: float, row_diff: float, col_diff: float):
        """
        Tạo ma trận AP 2D
        
        Args:
            rows (int): Số dòng
            cols (int): Số cột  
            start (float): Giá trị góc trái trên
            row_diff (float): Công sai theo dòng
            col_diff (float): Công sai theo cột
        """
        self.rows = rows
        self.cols = cols
        self.start = start
        self.row_diff = row_diff
        self.col_diff = col_diff
        self.matrix = self._generate_matrix()
    
    def _generate_matrix(self) -> np.ndarray:
        """Tạo ma trận AP"""
        matrix = np.zeros((self.rows, self.cols))
        
        for i in range(self.rows):
            for j in range(self.cols):
                # Công thức: A[i][j] = start + i*row_diff + j*col_diff
                matrix[i][j] = self.start + i * self.row_diff + j * self.col_diff
        
        return matrix
    
    def get_element(self, row: int, col: int) -> float:
        """Lấy phần tử tại vị trí (row, col)"""
        if 0 <= row < self.rows and 0 <= col < self.cols:
            return self.matrix[row][col]
        raise IndexError("Chỉ số ngoài phạm vi")
    
    def get_row_sum(self, row: int) -> float:
        """Tính tổng một dòng"""
        if 0 <= row < self.rows:
            return np.sum(self.matrix[row])
        raise IndexError("Chỉ số dòng ngoài phạm vi")
    
    def get_col_sum(self, col: int) -> float:
        """Tính tổng một cột"""
        if 0 <= col < self.cols:
            return np.sum(self.matrix[:, col])
        raise IndexError("Chỉ số cột ngoài phạm vi")
    
    def get_total_sum(self) -> float:
        """Tính tổng toàn bộ ma trận"""
        return np.sum(self.matrix)
    
    def display(self) -> None:
        """Hiển thị ma trận"""
        print(f"\n🔢 MA TRẬN CẤP SỐ CỘNG {self.rows}x{self.cols}:")
        print(f"Giá trị đầu: {self.start}")
        print(f"Công sai dòng: {self.row_diff}")
        print(f"Công sai cột: {self.col_diff}")
        print("\nMa trận:")
        
        for i in range(self.rows):
            print("  ", end="")
            for j in range(self.cols):
                print(f"{self.matrix[i][j]:6.1f}", end=" ")
            print()

# Demo ma trận AP
def demo_ap_matrix():
    print("🎯 DEMO MA TRẬN CẤP SỐ CỘNG 2D")
    print("=" * 35)
    
    # Tạo ma trận 4x5
    ap_matrix = ArithmeticProgressionMatrix(4, 5, 1.0, 2.0, 3.0)
    ap_matrix.display()
    
    print(f"\nTổng dòng 0: {ap_matrix.get_row_sum(0)}")
    print(f"Tổng cột 1: {ap_matrix.get_col_sum(1)}")
    print(f"Tổng toàn bộ: {ap_matrix.get_total_sum()}")

if __name__ == "__main__":
    demo_ap_matrix()
```

---

## 7. ỨNG DỤNG THỰC TẾ

### 7.1. Tính toán lãi suất đơn

```cpp
#include <iostream>
#include <vector>
#include <iomanip>
using namespace std;

/**
 * Ứng dụng: Tính lãi suất đơn
 * Số tiền sau n kỳ hạn tạo thành cấp số cộng
 */
class SimpleInterestCalculator {
private:
    double principal;      // Số tiền gốc
    double interestRate;   // Lãi suất mỗi kỳ
    
public:
    SimpleInterestCalculator(double p, double rate) 
        : principal(p), interestRate(rate) {}
    
    // Tính số tiền sau n kỳ hạn
    double getAmountAfterPeriods(int n) {
        // Công thức lãi đơn: A = P + P*r*n = P(1 + r*n)
        // Tạo thành AP: P, P(1+r), P(1+2r), P(1+3r), ...
        return principal * (1 + interestRate * n);
    }
    
    // Hiển thị bảng tính lãi
    void displayInterestTable(int periods) {
        cout << "\n💰 BẢNG TÍNH LÃI SUẤT ĐƠN:" << endl;
        cout << "Số tiền gốc: " << fixed << setprecision(2) << principal << endl;
        cout << "Lãi suất: " << (interestRate * 100) << "%/kỳ" << endl;
        cout << "\n" << setw(5) << "Kỳ" << setw(15) << "Số tiền" << setw(15) << "Lãi nhận" << endl;
        cout << string(35, '-') << endl;
        
        for (int i = 0; i <= periods; i++) {
            double amount = getAmountAfterPeriods(i);
            double interest = amount - principal;
            
            cout << setw(5) << i 
                 << setw(15) << fixed << setprecision(2) << amount
                 << setw(15) << fixed << setprecision(2) << interest << endl;
        }
    }
};

int main() {
    // Demo tính lãi suất
    SimpleInterestCalculator calc(1000000, 0.05); // 1 triệu, lãi 5%/kỳ
    calc.displayInterestTable(10);
    
    return 0;
}
```

### 7.2. Lập kế hoạch tiết kiệm

```python
class SavingsPlan:
    """Kế hoạch tiết kiệm theo cấp số cộng"""
    
    def __init__(self, initial_amount: float, monthly_increase: float):
        """
        Khởi tạo kế hoạch tiết kiệm
        
        Args:
            initial_amount (float): Số tiền tiết kiệm tháng đầu
            monthly_increase (float): Số tiền tăng thêm mỗi tháng
        """
        self.initial_amount = initial_amount
        self.monthly_increase = monthly_increase
        
    def get_monthly_saving(self, month: int) -> float:
        """Tính số tiền tiết kiệm tháng thứ n"""
        return self.initial_amount + (month - 1) * self.monthly_increase
    
    def get_total_saved(self, months: int) -> float:
        """Tính tổng số tiền tiết kiệm sau n tháng"""
        # Sử dụng công thức tổng AP: Sn = n/2 * (2a + (n-1)d)
        return months * (2 * self.initial_amount + (months - 1) * self.monthly_increase) / 2
    
    def months_to_reach_goal(self, target: float) -> int:
        """Tính số tháng cần thiết để đạt mục tiêu"""
        # Giải phương trình: target = n/2 * (2a + (n-1)d)
        # Đây là phương trình bậc 2: (d/2)n² + (a - d/2)n - target = 0
        a = self.monthly_increase / 2
        b = self.initial_amount - self.monthly_increase / 2
        c = -target
        
        if a == 0:  # Trường hợp không tăng tiền hàng tháng
            return int(target / self.initial_amount) if self.initial_amount > 0 else -1
        
        discriminant = b * b - 4 * a * c
        if discriminant < 0:
            return -1  # Không thể đạt được
        
        import math
        n1 = (-b + math.sqrt(discriminant)) / (2 * a)
        n2 = (-b - math.sqrt(discriminant)) / (2 * a)
        
        # Chọn nghiệm dương
        n = max(n1, n2) if n1 > 0 and n2 > 0 else (n1 if n1 > 0 else n2)
        return math.ceil(n) if n > 0 else -1
    
    def display_plan(self, months: int) -> None:
        """Hiển thị kế hoạch tiết kiệm"""
        print(f"\n💳 KẾ HOẠCH TIẾT KIỆM:")
        print(f"Tháng đầu: {self.initial_amount:,.0f} VNĐ")
        print(f"Tăng mỗi tháng: {self.monthly_increase:,.0f} VNĐ")
        print(f"\nChi tiết {months} tháng đầu:")
        print(f"{'Tháng':<8} {'Tiết kiệm':<15} {'Tổng tích lũy':<15}")
        print("-" * 40)
        
        for month in range(1, months + 1):
            monthly = self.get_monthly_saving(month)
            total = self.get_total_saved(month)
            print(f"{month:<8} {monthly:,.0f}₫{'':<6} {total:,.0f}₫")
        
        final_total = self.get_total_saved(months)
        print(f"\n🎯 Tổng tiết kiệm sau {months} tháng: {final_total:,.0f} VNĐ")

# Demo kế hoạch tiết kiệm
def demo_savings_plan():
    print("💰 DEMO KẾ HOẠCH TIẾT KIỆM")
    print("=" * 30)
    
    # Kế hoạch: Tháng đầu 1 triệu, tăng 200k mỗi tháng
    plan = SavingsPlan(1_000_000, 200_000)
    plan.display_plan(12)
    
    # Tính thời gian để đạt 100 triệu
    target = 100_000_000
    months_needed = plan.months_to_reach_goal(target)
    print(f"\n⏰ Thời gian để tích lũy {target:,.0f} VNĐ: {months_needed} tháng")
    
    if months_needed > 0:
        actual_total = plan.get_total_saved(months_needed)
        print(f"   Tổng thực tế: {actual_total:,.0f} VNĐ")

if __name__ == "__main__":
    demo_savings_plan()
```

### 7.3. Ứng dụng trong Game Development

```python
class GameLevelProgression:
    """Hệ thống tiến triển level trong game"""
    
    def __init__(self, base_exp: int, exp_increase: int):
        """
        Khởi tạo hệ thống EXP
        
        Args:
            base_exp (int): EXP cần cho level 2
            exp_increase (int): Lượng EXP tăng thêm mỗi level
        """
        self.base_exp = base_exp
        self.exp_increase = exp_increase
    
    def exp_needed_for_level(self, level: int) -> int:
        """Tính EXP cần để lên level n"""
        if level <= 1:
            return 0
        # AP: base_exp, base_exp + exp_increase, base_exp + 2*exp_increase, ...
        return self.base_exp + (level - 2) * self.exp_increase
    
    def total_exp_to_level(self, level: int) -> int:
        """Tính tổng EXP cần để đạt level n"""
        if level <= 1:
            return 0
        
        # Tổng AP từ level 2 đến level n
        n = level - 1  # Số level cần tính (từ 2 đến level)
        return n * (2 * self.base_exp + (n - 1) * self.exp_increase) // 2
    
    def current_level_from_exp(self, total_exp: int) -> Tuple[int, int, int]:
        """
        Tính level hiện tại từ tổng EXP
        
        Returns:
            Tuple[int, int, int]: (level hiện tại, EXP dư, EXP cần cho level tiếp theo)
        """
        if total_exp < self.base_exp:
            return (1, total_exp, self.base_exp - total_exp)
        
        # Tìm level bằng binary search
        left, right = 2, 1000
        current_level = 1
        
        while left <= right:
            mid = (left + right) // 2
            exp_needed = self.total_exp_to_level(mid)
            
            if exp_needed <= total_exp:
                current_level = mid
                left = mid + 1
            else:
                right = mid - 1
        
        # Tính EXP dư và EXP cần cho level tiếp theo
        exp_used = self.total_exp_to_level(current_level)
        exp_remaining = total_exp - exp_used
        exp_needed_next = self.exp_needed_for_level(current_level + 1)
        
        return (current_level, exp_remaining, exp_needed_next - exp_remaining)
    
    def display_level_table(self, max_level: int) -> None:
        """Hiển thị bảng level"""
        print(f"\n🎮 BẢNG TIẾN TRIỂN LEVEL:")
        print(f"{'Level':<8} {'EXP cần':<12} {'Tổng EXP':<12} {'% tăng':<10}")
        print("-" * 45)
        
        prev_total = 0
        for level in range(1, max_level + 1):
            exp_needed = self.exp_needed_for_level(level)
            total_exp = self.total_exp_to_level(level)
            
            if level == 1:
                percent_increase = 0
            else:
                percent_increase = ((total_exp - prev_total) / prev_total * 100) if prev_total > 0 else 0
            
            print(f"{level:<8} {exp_needed:<12} {total_exp:<12} {percent_increase:<9.1f}%")
            prev_total = total_exp

# Demo game progression
def demo_game_progression():
    print("🎮 DEMO HỆ THỐNG LEVEL GAME")
    print("=" * 35)
    
    # Hệ thống: Level 2 cần 100 EXP, tăng 50 EXP mỗi level
    game = GameLevelProgression(100, 50)
    game.display_level_table(20)
    
    # Test với một số lượng EXP cụ thể
    test_exp = [0, 100, 500, 1500, 5000]
    
    print(f"\n📊 PHÂN TÍCH EXP:")
    for exp in test_exp:
        level, remaining, needed = game.current_level_from_exp(exp)
        progress = remaining / (remaining + needed) * 100 if (remaining + needed) > 0 else 0
        
        print(f"EXP {exp:>5}: Level {level} ({remaining}/{remaining + needed} EXP, {progress:.1f}% để level tiếp theo)")

if __name__ == "__main__":
    demo_game_progression()
```

---

## 8. BÀI TẬP THỰC HÀNH

### 📝 Bài tập cơ bản

**Bài 1**: Viết chương trình nhập a₁, a₂, n và xuất số hạng thứ n cùng với 10 số hạng đầu tiên.

**Bài 2**: So sánh thời gian chạy của phương pháp lặp và công thức với n = 10⁶.

**Bài 3**: Viết hàm kiểm tra một dãy số có phải là cấp số cộng không.

### 🔥 Bài tập nâng cao

**Bài 4**: Cho ba số a, b, c tạo thành cấp số cộng. Viết chương trình tìm x sao cho a+x, b+x, c+x cũng tạo thành cấp số cộng.

**Bài 5**: Tìm cấp số cộng có 5 số hạng, biết tổng của chúng là 25 và tích của số hạng đầu và cuối là 9.

**Bài 6**: Viết class `APAnalyzer` có các tính năng:
- Tìm số hạng thiếu trong dãy
- Tính trung bình cộng của n số hạng đầu
- Tìm số hạng âm đầu tiên (nếu có)

### 🚀 Bài tập thách thức

**Bài 7**: **Ma phương cấp số cộng**: Tạo ma trận vuông n×n sao cho:
- Mỗi hàng là một cấp số cộng
- Mỗi cột là một cấp số cộng  
- Các đường chéo cũng là cấp số cộng

**Bài 8**: **Tối ưu hóa ngân sách**: Một công ty có ngân sách tăng theo cấp số cộng qua các năm. Viết chương trình tìm phân bổ ngân sách tối ưu cho n dự án.

**Bài 9**: **Cấp số cộng động**: Viết thuật toán để tìm cấp số cộng dài nhất trong một mảng số nguyên cho trước.

### 💡 Bài tập sáng tạo

**Bài 10**: **Music Generator**: Sử dụng cấp số cộng để tạo ra các nốt nhạc với tần số theo quy luật toán học.

**Bài 11**: **Simulation Game**: Mô phỏng hệ thống kinh tế trong game với dân số, tài nguyên tăng theo cấp số cộng.

**Bài 12**: **Data Compression**: Thiết kế thuật toán nén dữ liệu dựa trên nhận diện các chuỗi cấp số cộng trong dữ liệu.

---

## 9. TÓM TẮT VÀ KẾT LUẬN

### 🎯 Kiến thức cốt lõi

1. **Định nghĩa**: Cấp số cộng là dãy số có hiệu số liên tiếp không đổi
2. **Công thức cơ bản**: `aₙ = a₁ + (n-1)d` với `d = a₂ - a₁`
3. **Tính tổng**: `Sₙ = n/2 × [2a₁ + (n-1)d]`
4. **Tối ưu hóa**: Sử dụng công thức toán học thay vì lặp

### 🚀 Kỹ năng đã phát triển

- **Tư duy toán học**: Nhận ra quy luật và áp dụng công thức
- **Tối ưu hóa thuật toán**: Từ O(n) xuống O(1)
- **Lập trình hướng đối tượng**: Thiết kế class mạnh mẽ
- **Ứng dụng thực tế**: Kết nối lý thuyết với đời sống

### 📈 Mở rộng kiến thức

1. **Cấp số nhân** (Geometric Progression)
2. **Dãy Fibonacci** và các dãy đệ quy
3. **Chuỗi vô hạn** và hội tụ
4. **Ứng dụng trong Machine Learning**

### 🔧 Công cụ và thư viện

- **C++**: STL containers, chrono, iomanip
- **Python**: NumPy, typing, time
- **Toán học**: Giải phương trình bậc 2, tối ưu hóa

### 🎓 Lời khuyên cho học sinh

1. **Thực hành đều đặn**: Làm bài tập từ cơ bản đến nâng cao
2. **Hiểu bản chất**: Nắm vững công thức và cách chứng minh
3. **Ứng dụng thực tế**: Tìm cấp số cộng trong cuộc sống hàng ngày
4. **Code sạch**: Viết code có chú thích và kiểm tra edge cases
5. **Tối ưu hóa**: Luôn suy nghĩ về độ phức tạp thuật toán

### 🏆 Thành tựu sau chuyên đề

Sau khi hoàn thành chuyên đề này, các em đã:
- ✅ Nắm vững lý thuyết cấp số cộng
- ✅ Thành thạo hai phương pháp giải (lặp và công thức)
- ✅ Biết cách tối ưu hóa thuật toán từ O(n) → O(1)
- ✅ Ứng dụng vào các bài toán thực tế
- ✅ Thiết kế class và viết code chuyên nghiệp
- ✅ Phân tích độ phức tạp và hiệu suất

---

## 📚 TÀI LIỆU THAM KHẢO VÀ MỞ RỘNG

### 📖 Sách tham khảo
1. **"Concrete Mathematics"** - Donald Knuth
2. **"Introduction to Algorithms"** - CLRS
3. **"Mathematics for Computer Science"** - MIT

### 🌐 Tài nguyên online
1. **GeeksforGeeks**: Arithmetic Progression problems
2. **Khan Academy**: Sequences and Series
3. **Wolfram MathWorld**: Mathematical sequences

### 🔗 Liên kết với chủ đề khác
- **Cấp số nhân**: Geometric Progression
- **Dãy Fibonacci**: Recursive sequences  
- **Big O Notation**: Algorithm complexity
- **Dynamic Programming**: Optimization techniques

---

## 🎯 CHALLENGES BONUS

### Challenge 1: AP Puzzle Game
Tạo game puzzle giải các bài toán cấp số cộng với giao diện đồ họa.

### Challenge 2: AP Music Generator  
Sử dụng cấp số cộng để tạo nhạc với các tần số theo quy luật toán học.

### Challenge 3: Financial Calculator
Xây dựng máy tính tài chính với các tính năng:
- Lãi suất đơn/kép
- Kế hoạch tiết kiệm
- Phân tích đầu tư

### Challenge 4: Data Pattern Recognition
Viết thuật toán AI nhận diện các mẫu cấp số cộng trong big data.

---

## 🏁 KẾT THÚC CHUYÊN ĐỀ

**Cảm ơn các em đã theo dõi chuyên đề về Cấp số cộng!**

Chuyên đề này đã đưa các em từ những khái niệm cơ bản nhất về cấp số cộng đến những ứng dụng phức tạp trong thực tế. Từ việc tính toán đơn giản đến việc thiết kế hệ thống game, từ tối ưu hóa thuật toán đến ứng dụng trong tài chính - tất cả đều có thể liên kết với kiến thức toán học cơ bản này.

### 🚀 Bước tiếp theo:
1. Thực hành các bài tập trong chuyên đề
2. Tự tạo ra các ứng dụng thú vị với cấp số cộng
3. Tìm hiểu về cấp số nhân và các dãy số khác
4. Áp dụng vào các dự án cá nhân

### 💡 Nhớ rằng:
> *"Mathematics is not about numbers, equations, computations, or algorithms: it is about understanding."*
> - William Paul Thurston

**Chúc các em học tốt và thành công! 🎓✨**

---

*📧 Liên hệ: Nếu có thắc mắc về chuyên đề, hãy thảo luận với giáo viên hoặc bạn bè để hiểu sâu hơn.*

*🔄 Cập nhật: Chuyên đề sẽ được cập nhật thêm các ví dụ và bài tập mới.*