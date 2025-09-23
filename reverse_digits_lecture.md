# CHUYÊN ĐỀ: ĐẢO NGƯỢC CÁC CHỮ SỐ

**Dành cho học sinh lớp 10 chuyên Tin học**

---

## 📋 MỤC LỤC
1. [Giới thiệu bài toán](#1-giới-thiệu-bài-toán)
2. [Phân tích bài toán](#2-phân-tích-bài-toán)
3. [Các phương pháp giải](#3-các-phương-pháp-giải)
4. [Code minh họa](#4-code-minh-họa)
5. [So sánh các phương pháp](#5-so-sánh-các-phương-pháp)
6. [Bài tập thực hành](#6-bài-tập-thực-hành)

---

## 1. GIỚI THIỆU BÀI TOÁN

### 📝 Phát biểu bài toán
Cho một số nguyên dương n, hãy đảo ngược thứ tự các chữ số của n.

### 📊 Ví dụ
```
Input: n = 122
Output: 221
Giải thích: Đảo ngược 122 thành 221

Input: n = 200
Output: 2
Giải thích: Đảo ngược 200 thành 002, tức là 2

Input: n = 12345
Output: 54321
Giải thích: Đảo ngược 12345 thành 54321
```

### ⚠️ Lưu ý đặc biệt
- Số 0 ở cuối sẽ bị mất khi đảo ngược (VD: 200 → 2)
- Chỉ xét số nguyên dương

---

## 2. PHÂN TÍCH BÀI TOÁN

### 🔍 Ý tưởng chính
Để đảo ngược các chữ số, ta cần:
1. **Lấy từng chữ số** từ cuối số ban đầu
2. **Xây dựng số mới** bằng cách thêm chữ số vào cuối

### 🎯 Quá trình xây dựng số đảo ngược
Ví dụ với n = 123:
```
Bước 1: revNum = 0, n = 123
        Lấy 3: revNum = 0*10 + 3 = 3, n = 12

Bước 2: revNum = 3, n = 12
        Lấy 2: revNum = 3*10 + 2 = 32, n = 1

Bước 3: revNum = 32, n = 1
        Lấy 1: revNum = 32*10 + 1 = 321, n = 0

Kết quả: 321
```

### 🔧 Công thức cốt lõi
```
revNum = revNum * 10 + (n % 10)
n = n / 10
```

---

## 3. CÁC PHƯƠNG PHÁP GIẢI

### 3.1. Phương pháp 1: Sử dụng vòng lặp
**Độ phức tạp**: O(log₁₀(n)) thời gian, O(1) không gian

**Ý tưởng**:
- Dùng vòng lặp while cho đến khi n = 0
- Trong mỗi vòng lặp: lấy chữ số cuối và cập nhật số đảo ngược

### 3.2. Phương pháp 2: Sử dụng đệ quy
**Độ phức tạp**: O(log₁₀(n)) thời gian, O(log₁₀(n)) không gian

**Ý tưởng**:
- Sử dụng hàm đệ quy với tham số tích lũy
- **Base case**: n = 0
- **Recursive case**: Xử lý từ các chữ số đầu tiên

### 3.3. Phương pháp 3: Chuyển thành chuỗi
**Độ phức tạp**: O(log₁₀(n)) thời gian, O(log₁₀(n)) không gian

**Ý tưởng**:
- Chuyển số thành chuỗi
- Đảo ngược chuỗi
- Chuyển ngược thành số

---

## 4. CODE MINH HỌA

### 4.1. Code C++

```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

/**
 * Phương pháp 1: Sử dụng vòng lặp
 */
int reverseDigitsLoop(int n) {
    int revNum = 0;
    
    while (n > 0) {
        revNum = revNum * 10 + n % 10;  // Thêm chữ số cuối vào revNum
        n = n / 10;                     // Loại bỏ chữ số cuối khỏi n
    }
    
    return revNum;
}

/**
 * Phương pháp 2: Sử dụng đệ quy
 */
void reverseDigitsRecursive(int n, int &revNum, int &basePos) {
    if (n > 0) {
        // Gọi đệ quy với n/10 trước
        reverseDigitsRecursive(n / 10, revNum, basePos);
        
        // Thêm chữ số hiện tại vào vị trí thích hợp
        revNum += (n % 10) * basePos;
        basePos *= 10;
    }
}

/**
 * Phương pháp 3: Sử dụng chuỗi
 */
int reverseDigitsString(int n) {
    // Chuyển số thành chuỗi
    string s = to_string(n);
    
    // Đảo ngược chuỗi
    reverse(s.begin(), s.end());
    
    // Chuyển ngược thành số
    return stoi(s);
}

/**
 * Hàm demo với giải thích chi tiết
 */
void demoReverseDigits(int n) {
    cout << "\n=== DEMO: Đảo ngược chữ số của " << n << " ===" << endl;
    
    // Hiển thị quá trình đảo ngược bằng vòng lặp
    cout << "\nPhương pháp 1 - Vòng lặp:" << endl;
    int temp = n;
    int revNum = 0;
    
    while (temp > 0) {
        int digit = temp % 10;
        revNum = revNum * 10 + digit;
        cout << "  Lấy chữ số " << digit 
             << ": revNum = " << (revNum - digit) << " * 10 + " << digit 
             << " = " << revNum 
             << ", còn lại = " << temp / 10 << endl;
        temp /= 10;
    }
    
    cout << "Kết quả: " << revNum << endl;
    
    // So sánh các phương pháp
    cout << "\nSo sánh kết quả:" << endl;
    cout << "  Vòng lặp: " << reverseDigitsLoop(n) << endl;
    
    // Test đệ quy
    int revNumRec = 0, basePos = 1;
    reverseDigitsRecursive(n, revNumRec, basePos);
    cout << "  Đệ quy:   " << revNumRec << endl;
    
    cout << "  Chuỗi:    " << reverseDigitsString(n) << endl;
}

int main() {
    cout << "CHUYÊN ĐỀ: ĐẢO NGƯỢC CÁC CHỮ SỐ" << endl;
    cout << "==================================" << endl;
    
    // Test với các ví dụ từ GeeksforGeeks
    demoReverseDigits(122);
    demoReverseDigits(200);
    demoReverseDigits(12345);
    demoReverseDigits(4562);  // Ví dụ từ code gốc
    
    return 0;
}
```

### 4.2. Code Python

```python
def reverse_digits_loop(n):
    """
    Phương pháp 1: Sử dụng vòng lặp
    """
    rev_num = 0
    
    while n > 0:
        rev_num = rev_num * 10 + n % 10  # Thêm chữ số cuối vào rev_num
        n = n // 10                      # Loại bỏ chữ số cuối khỏi n
    
    return rev_num

def reverse_digits_recursive(n, rev_num, base_pos):
    """
    Phương pháp 2: Sử dụng đệ quy
    """
    if n > 0:
        # Gọi đệ quy với n//10 trước
        reverse_digits_recursive(n // 10, rev_num, base_pos)
        
        # Thêm chữ số hiện tại vào vị trí thích hợp
        rev_num[0] += (n % 10) * base_pos[0]
        base_pos[0] *= 10

def reverse_digits_string(n):
    """
    Phương pháp 3: Chuyển thành chuỗi và đảo ngược
    """
    # Chuyển số thành chuỗi
    s = str(n)
    
    # Đảo ngược chuỗi
    s = s[::-1]
    
    # Chuyển ngược thành số
    return int(s)

def demo_reverse_digits(n):
    """Demo với giải thích chi tiết"""
    print(f"\n=== DEMO: Đảo ngược chữ số của {n} ===")
    
    # Hiển thị quá trình đảo ngược bằng vòng lặp
    print(f"\nPhương pháp 1 - Vòng lặp:")
    temp = n
    rev_num = 0
    
    while temp > 0:
        digit = temp % 10
        old_rev = rev_num
        rev_num = rev_num * 10 + digit
        print(f"  Lấy chữ số {digit}: rev_num = {old_rev} * 10 + {digit} = {rev_num}, còn lại = {temp // 10}")
        temp //= 10
    
    print(f"Kết quả: {rev_num}")
    
    # So sánh các phương pháp
    print(f"\nSo sánh kết quả:")
    print(f"  Vòng lặp: {reverse_digits_loop(n)}")
    
    # Test đệ quy
    rev_num_rec = [0]
    base_pos = [1]
    reverse_digits_recursive(n, rev_num_rec, base_pos)
    print(f"  Đệ quy:   {rev_num_rec[0]}")
    
    print(f"  Chuỗi:    {reverse_digits_string(n)}")

def main():
    print("CHUYÊN ĐỀ: ĐẢO NGƯỢC CÁC CHỮ SỐ")
    print("==================================")
    
    # Test với các ví dụ từ GeeksforGeeks
    demo_reverse_digits(122)
    demo_reverse_digits(200)
    demo_reverse_digits(12345)
    demo_reverse_digits(4562)  # Ví dụ từ code gốc

if __name__ == "__main__":
    main()
```

---

## 5. SO SÁNH CÁC PHƯƠNG PHÁP

### 📊 Bảng so sánh

| Phương pháp | Độ phức tạp thời gian | Độ phức tạp không gian | Ưu điểm | Nhược điểm |
|-------------|----------------------|------------------------|---------|------------|
| **Vòng lặp** | O(log n) | O(1) | Hiệu quả, dễ hiểu | Cần theo dõi vòng lặp |
| **Đệ quy** | O(log n) | O(log n) | Code ngắn gọn | Tốn bộ nhớ stack |
| **Chuỗi** | O(log n) | O(log n) | Đơn giản nhất | Chậm hơn do conversion |

### 🎯 Khi nào dùng phương pháp nào?

- **Vòng lặp**: Phù hợp nhất cho hầu hết trường hợp, hiệu quả về cả thời gian và bộ nhớ
- **Đệ quy**: Khi muốn code ngắn gọn và số không quá lớn
- **Chuỗi**: Khi cần xử lý số rất lớn hoặc muốn code đơn giản

### ⚠️ Lưu ý về số có số 0 ở cuối

```cpp
// Ví dụ với số 1200
int n = 1200;
int reversed = reverseDigitsLoop(n);  // Kết quả: 21 (không phải 0021)

cout << "Gốc: " << n << endl;        // 1200
cout << "Đảo ngược: " << reversed;   // 21
```

---

## 6. BÀI TẬP THỰC HÀNH

### 📝 Bài tập cơ bản

**Bài 1**: Viết chương trình nhập một số và xuất số đảo ngược của nó.

**Bài 2**: Kiểm tra một số có phải là palindrome không (số đọc xuôi bằng đọc ngược).
```cpp
// Ví dụ: 121, 1331, 12321 là palindrome
bool isPalindrome(int n) {
    return n == reverseDigitsLoop(n);
}
```

**Bài 3**: Tìm số lớn nhất có thể tạo được từ các chữ số của một số cho trước.

### 🔥 Bài tập nâng cao

**Bài 4**: **Xử lý số âm**: Mở rộng thuật toán để xử lý cả số âm.
```cpp
// Ví dụ: -123 → -321
int reverseWithSign(int n) {
    bool isNegative = (n < 0);
    n = abs(n);
    int reversed = reverseDigitsLoop(n);
    return isNegative ? -reversed : reversed;
}
```

**Bài 5**: **Kiểm tra overflow**: Kiểm tra kết quả có bị tràn số không (với kiểu int 32-bit).

**Bài 6**: **Đảo ngược mảng số**: Cho một mảng các số, đảo ngược từng số trong mảng.

### 💡 Bài tập ứng dụng

**Bài 7**: **Tạo số mới**: Từ hai số a và b, tạo số mới bằng cách nối đảo ngược của a với b.
```
Ví dụ: a = 123, b = 456
Đảo ngược a = 321
Kết quả = 321456
```

**Bài 8**: **Tìm cặp số**: Tìm tất cả các cặp số (a, b) trong khoảng [1, 1000] sao cho đảo ngược của a bằng b.

**Bài 9**: **Số Armstrong đảo ngược**: Kiểm tra một số có phải là số Armstrong sau khi đảo ngược không.

### 🚀 Bài tập thách thức

**Bài 10**: **Tối ưu bộ nhớ**: Viết hàm đảo ngược số mà không sử dụng biến phụ nào (chỉ dùng phép toán).

**Bài 11**: **Xử lý số lớn**: Đảo ngược số có hàng chục chữ số (sử dụng string hoặc big integer).

**Bài 12**: **Pattern matching**: Tìm tất cả số n chữ số mà khi đảo ngược được số có cùng tổng chữ số.

---

## 7. TÓM TẮT

### 🎯 Những điều cần nhớ

1. **Công thức cốt lõi**: `revNum = revNum * 10 + (n % 10)` và `n = n / 10`
2. **Ba phương pháp chính**: Vòng lặp (tối ưu), đệ quy (ngắn gọn), chuỗi (đơn giản)
3. **Xử lý số 0**: Số 0 ở cuối sẽ bị mất trong kết quả
4. **Độ phức tạp**: O(log₁₀(n)) - phụ thuộc vào số chữ số

### 🚀 Kỹ năng đã học

- Thao tác với các chữ số của số nguyên
- Sử dụng phép chia lấy dư (%) và chia nguyên (/)
- Hiểu về đệ quy và tham số tích lũy
- So sánh hiệu quả của các thuật toán khác nhau

### 📈 Ứng dụng thực tế

- **Kiểm tra palindrome**: Số đọc xuôi bằng đọc ngược
- **Mã hóa đơn giản**: Đảo ngược số để che giấu thông tin
- **Xử lý dữ liệu số**: Chuẩn hóa và biến đổi dữ liệu
- **Games và puzzles**: Tạo các trò chơi liên quan đến số

### 📚 Bước tiếp theo

- Thực hành các bài tập trong chuyên đề
- Áp dụng vào bài toán palindrome và Armstrong numbers
- Tìm hiểu về xử lý overflow và big integer
- Kết hợp với các thuật toán khác (sorting, searching)

---

**🎓 Chúc các em học tốt và thành thạo các kỹ thuật xử lý số!**