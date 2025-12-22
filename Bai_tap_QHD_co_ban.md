# Bài tập Quy Hoạch Động (QHD) Cơ bản — 90 phút (6 bài)

> Mục tiêu: luyện 6 “mẫu hình” QHD cơ bản: **1D đếm cách**, **1D tối ưu**, **unbounded knapsack (min coins)**, **0/1 knapsack**, **LIS**, **2D grid DP**.  
> Gợi ý khi làm: luôn trả lời 3 câu hỏi: **Trạng thái là gì? Chuyển trạng thái thế nào? Điều kiện đầu/đáp án ở đâu?**

---

## Bài 1. Ếch nhảy bậc thang (Fibonacci)

### {Đề bài}
Có một con ếch đang ở bậc 0 và muốn lên bậc **n**. Mỗi lần ếch có thể nhảy **1** hoặc **2** bậc.  
Hãy tính số cách khác nhau để ếch lên đúng bậc **n**. Vì kết quả có thể rất lớn, hãy in ra **mod \(10^9+7\)**.

**Input**
- Một số nguyên \(n\) \((0 \le n \le 10^7)\)

**Output**
- Số cách (mod \(10^9+7\))

**Ví dụ**
- Input: `4`  
- Output: `5`  
(Giải thích: 1111, 112, 121, 211, 22)

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(n)\) là số cách lên bậc \(n\).  
Bước cuối cùng để tới \(n\) chỉ có thể từ:
- \(n-1\) (nhảy 1 bậc)
- \(n-2\) (nhảy 2 bậc)

Do đó:
\[
f(n)=f(n-1)+f(n-2)
\]
Điều kiện đầu:
\[
f(0)=1,\quad f(1)=1
\]
Đáp án là \(f(n)\).

---

### {Mô hình hóa}
- Trạng thái: `dp[i] = số cách lên bậc i (mod M)`
- Chuyển: `dp[i] = dp[i-1] + dp[i-2] (mod M)`
- Có thể tối ưu bộ nhớ chỉ dùng 2 biến.

---

### {Ý tưởng}
Đây là bài “đếm số cách” kinh điển. Ta duyệt i tăng dần, mỗi trạng thái phụ thuộc 2 trạng thái trước.

---

### {Giải thuật}
1. Đọc \(n\), đặt \(M=10^9+7\).
2. Nếu \(n \le 1\) in `1`.
3. Dùng 2 biến:
   - `a = f(0) = 1`, `b = f(1) = 1`
   - Lặp `i=2..n`: `c=(a+b)%M`, rồi `a=b`, `b=c`
4. In `b`.

**Độ phức tạp**: \(O(n)\) thời gian, \(O(1)\) bộ nhớ.

---

### {Code minh họa}

---

## Bài 2. Cầu thang có bậc hỏng (đếm cách với trạng thái cấm)

### {Đề bài}
Một cầu thang có các bậc từ \(1\) đến \(n\). Có \(k\) bậc bị hỏng, ếch **không được đứng** lên các bậc này.  
Ếch xuất phát ở bậc 0 (không hỏng), mỗi lần nhảy **1** hoặc **2** bậc.  
Hãy đếm số cách để lên đúng bậc \(n\) (bậc \(n\) đảm bảo **không hỏng**) theo modulo \(10^9+7\).

**Input**
- Dòng 1: \(n, k\) \((1 \le n \le 10^7,\ 0 \le k \le 2\cdot 10^5)\)
- Dòng 2: \(k\) số nguyên là các bậc hỏng (tăng/không tăng đều được)

**Output**
- Số cách (mod \(10^9+7\))

**Ví dụ**
- Input  
  ```
  7 2
  2 5
  ```
- Output: `3`

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(i)\) là số cách đứng ở bậc \(i\) mà **không vi phạm bậc hỏng**.  
Nếu bậc \(i\) hỏng thì \(f(i)=0\). Nếu không hỏng:
\[
f(i)=f(i-1)+f(i-2)
\]
với \(f(0)=1\).  
Đáp án: \(f(n)\).

---

### {Mô hình hóa}
- Mảng đánh dấu hỏng: `bad[i]` (bool)
- `dp[i]` là số cách đến bậc i.
- Tối ưu: chỉ cần 2 biến `f(i-2), f(i-1)` + kiểm tra `bad[i]`.

---

### {Ý tưởng}
Giống Bài 1 nhưng có “trạng thái cấm”: nếu bậc i hỏng thì đặt 0, coi như không tồn tại.

---

### {Giải thuật}
1. Đọc \(n,k\), đánh dấu các bậc hỏng.
2. Khởi tạo `f0 = 1` (bậc 0), `f1` cho bậc 1:
   - nếu bậc 1 hỏng thì `f1 = 0` else `f1 = 1`
3. Với `i=2..n`:
   - nếu `bad[i]`: `fi = 0`
   - else `fi = (f0 + f1) % MOD`
   - cập nhật `f0=f1, f1=fi`
4. In `f1` (ứng với bậc n).

**Độ phức tạp**: \(O(n+k)\) thời gian, \(O(n)\) bộ nhớ nếu dùng mảng `bad` (bool).  
> Nếu cần tiết kiệm bộ nhớ hơn, có thể lưu danh sách bậc hỏng trong `unordered_set`/`set` (nhưng chậm hơn) hoặc bitset.

---

### {Code minh họa}

---

## Bài 3. Đổi tiền ít nhất (Unbounded DP - Min coins)

### {Đề bài}
Bạn có \(m\) loại tiền xu, mệnh giá lần lượt là \(a_1, a_2, \dots, a_m\).  
Mỗi loại có thể dùng **không giới hạn**. Hãy tìm **số xu ít nhất** để đổi được đúng số tiền \(S\).  
Nếu không thể đổi đúng, in ra `-1`.

**Input**
- Dòng 1: \(m, S\) \((1 \le m \le 100,\ 0 \le S \le 10^6)\)
- Dòng 2: \(m\) số nguyên dương \(a_i\) \((1 \le a_i \le 10^6)\)

**Output**
- Số xu ít nhất, hoặc `-1`

**Ví dụ**
- Input  
  ```
  3 11
  1 5 7
  ```
- Output: `3`  
(Giải thích: 5 + 5 + 1)

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(x)\) = số xu ít nhất để đổi đúng \(x\).  
Để tạo \(x\), bước cuối cùng chọn một đồng \(a_i\) và trước đó đã tạo \(x-a_i\):
\[
f(x)=\min_{i: a_i \le x} \left(f(x-a_i)+1\right)
\]
Điều kiện đầu:
\[
f(0)=0,\quad f(x)=+\infty\ \text{(khởi tạo)}
\]
Đáp án: nếu \(f(S)=+\infty\) in `-1`, ngược lại in \(f(S)\).

---

### {Mô hình hóa}
- `dp[x]` (0..S) lưu giá trị nhỏ nhất.
- Khởi tạo `dp[0]=0`, còn lại INF.
- Với unbounded, có thể duyệt `x` từ 1..S, thử tất cả coin.

---

### {Ý tưởng}
Đây là DP tối ưu 1D. INF thể hiện “chưa đổi được”. Mỗi `dp[x]` chọn đồng cuối cùng tốt nhất.

---

### {Giải thuật}
1. Đọc `m, S`, danh sách coin.
2. `dp = [INF]*(S+1)`, `dp[0]=0`.
3. For `x=1..S`:
   - For each `coin`:
     - nếu `coin<=x`: `dp[x] = min(dp[x], dp[x-coin]+1)`
4. Nếu `dp[S] >= INF/2` in `-1`, else in `dp[S]`.

**Độ phức tạp**: \(O(mS)\) thời gian, \(O(S)\) bộ nhớ.

---

### {Code minh họa}

---

## Bài 4. Ba lô 0/1 (0/1 Knapsack)

### {Đề bài}
Có \(n\) đồ vật, vật thứ \(i\) có:
- khối lượng \(w_i\)
- giá trị \(v_i\)

Bạn có chiếc balo sức chứa tối đa \(W\). Mỗi đồ vật **chọn hoặc không chọn** (0/1).  
Hãy tính **tổng giá trị lớn nhất** có thể mang theo mà tổng khối lượng không vượt quá \(W\).

**Input**
- Dòng 1: \(n, W\) \((1 \le n \le 2000,\ 1 \le W \le 2\cdot 10^5)\)
- Dòng 2..n+1: mỗi dòng \(w_i, v_i\) \((1 \le w_i \le W,\ 0 \le v_i \le 10^9)\)

**Output**
- Giá trị lớn nhất

**Ví dụ**
- Input  
  ```
  3 7
  3 4
  4 5
  2 3
  ```
- Output: `9`  
(Giải thích: chọn vật 1 và 2)

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(i, c)\) là giá trị lớn nhất khi xét **i vật đầu**, với sức chứa còn/đang dùng tối đa là \(c\).  
Xét vật i (1-indexed), có 2 lựa chọn:
- Không lấy: \(f(i,c)=f(i-1,c)\)
- Lấy (nếu \(w_i\le c\)): \(f(i,c)=f(i-1,c-w_i)+v_i\)

Nên:
\[
f(i,c)=\max\left(f(i-1,c),\ f(i-1,c-w_i)+v_i\right)
\]
Cơ sở: \(f(0,c)=0\).

Đáp án: \(f(n,W)\).

---

### {Mô hình hóa}
- DP 2D theo i và c, nhưng tối ưu về 1D:
  - `dp[c]` là đáp án tốt nhất với sức chứa c sau khi xử lý một số vật.
- Với 0/1, phải duyệt `c` **giảm dần** để tránh dùng 1 vật nhiều lần.

---

### {Ý tưởng}
“0/1” nghĩa là mỗi vật dùng tối đa 1 lần → vòng lặp sức chứa phải đi từ lớn xuống nhỏ.

---

### {Giải thuật}
1. Đọc `n, W`.
2. `dp[0..W]=0`.
3. Với mỗi vật `(w, v)`:
   - For `c=W..w`:
     - `dp[c] = max(dp[c], dp[c-w] + v)`
4. In `dp[W]`.

**Độ phức tạp**: \(O(nW)\) thời gian, \(O(W)\) bộ nhớ.

---

### {Code minh họa}

---

## Bài 5. Dãy tăng dài nhất (LIS \(O(n^2)\))

### {Đề bài}
Cho dãy \(a_1, a_2, \dots, a_n\). Hãy tìm **độ dài** của dãy con tăng **nghiêm ngặt** dài nhất (LIS).

**Input**
- Dòng 1: \(n\) \((1 \le n \le 5000)\)
- Dòng 2: \(n\) số nguyên \(a_i\) \((|a_i| \le 10^9)\)

**Output**
- Một số nguyên: độ dài LIS

**Ví dụ**
- Input  
  ```
  8
  3 1 2 1 8 5 6 2
  ```
- Output: `4`  
(Một LIS: 1 2 5 6)

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(i)\) là độ dài LIS **kết thúc tại vị trí i** (bắt buộc chọn \(a_i\)).  
Khi đó:
\[
f(i)=1+\max_{j<i,\ a_j<a_i} f(j)
\]
Nếu không có j thỏa, \(f(i)=1\).  
Đáp án:
\[
\max_{1\le i\le n} f(i)
\]

---

### {Mô hình hóa}
- `dp[i] = LIS kết thúc ở i`
- Duyệt i từ 1..n, với mỗi i duyệt j < i để cập nhật.

---

### {Ý tưởng}
Mỗi phần tử đóng vai trò “phần tử cuối” của một dãy tăng; ta nối từ các dãy kết thúc trước đó có giá trị nhỏ hơn.

---

### {Giải thuật}
1. Đọc n và dãy a.
2. `dp[i]=1` cho mọi i.
3. For i=0..n-1:
   - For j=0..i-1:
     - Nếu `a[j] < a[i]`: `dp[i] = max(dp[i], dp[j] + 1)`
4. In `max(dp)`.

**Độ phức tạp**: \(O(n^2)\) thời gian, \(O(n)\) bộ nhớ.

---

### {Code minh họa}

---

## Bài 6. Đường đi chi phí nhỏ nhất trong lưới (2D DP)

### {Đề bài}
Cho ma trận \(n \times m\) gồm các số nguyên không âm \(c_{i,j}\) (chi phí khi đi vào ô đó).  
Bạn bắt đầu ở ô \((1,1)\) và muốn đến \((n,m)\). Mỗi bước chỉ được đi:
- sang phải \((i, j+1)\) hoặc
- xuống dưới \((i+1, j)\)

Tổng chi phí của một đường đi bằng tổng các \(c_{i,j}\) của các ô đi qua (kể cả ô đầu và ô cuối).  
Hãy tìm **tổng chi phí nhỏ nhất**.

**Input**
- Dòng 1: \(n, m\) \((1 \le n,m \le 2000)\)
- Tiếp theo \(n\) dòng, mỗi dòng \(m\) số \(c_{i,j}\) \((0 \le c_{i,j} \le 10^6)\)

**Output**
- Chi phí nhỏ nhất

**Ví dụ**
- Input  
  ```
  3 4
  1 3 1 2
  1 5 1 1
  4 2 1 3
  ```
- Output: `8`

---

### {Phân tích đề theo mô hình toán học}
Gọi \(f(i,j)\) là chi phí nhỏ nhất để đến ô \((i,j)\).  
Để đến \((i,j)\), bước trước đó chỉ có thể từ:
- \((i-1, j)\) hoặc
- \((i, j-1)\)

Vậy:
\[
f(i,j)=c_{i,j}+\min(f(i-1,j),\ f(i,j-1))
\]
Điều kiện biên:
\[
f(1,1)=c_{1,1}
\]
Các ô ở hàng 1 chỉ đi từ trái, cột 1 chỉ đi từ trên.

Đáp án: \(f(n,m)\).

---

### {Mô hình hóa}
- DP 2D, nhưng tối ưu bộ nhớ theo hàng:
  - `dp[j]` là chi phí nhỏ nhất đến ô hiện tại của cột j trong khi quét từng hàng.
- Khi xử lý hàng i:
  - `dp[j] = c[i][j] + min(dp[j] (từ trên), dp[j-1] (từ trái))`

---

### {Ý tưởng}
Bài toán “đường đi tối ưu trên lưới” là mẫu hình DP 2D rất cơ bản; quét theo thứ tự hàng-cột đảm bảo các trạng thái cần thiết đã có.

---

### {Giải thuật}
1. Đọc n, m.
2. Khởi tạo `dp[1..m]` = INF.
3. Với i=1..n:
   - Với j=1..m:
     - Nếu (i,j) = (1,1): dp[j] = c
     - Nếu i==1: dp[j] = dp[j-1] + c
     - Nếu j==1: dp[j] = dp[j] + c   (chỉ từ trên)
     - Ngược lại: dp[j] = min(dp[j], dp[j-1]) + c
4. In `dp[m]`.

**Độ phức tạp**: \(O(nm)\) thời gian, \(O(m)\) bộ nhớ.

---

### {Code minh họa}

---

## Gợi ý phân bố thời gian (90 phút)
- 10' nhắc lại “khung DP” + Bài 1 (đếm cách 1D)
- 10' Bài 2 (thêm trạng thái cấm)
- 15' Bài 3 (tối ưu 1D, INF)
- 20' Bài 4 (0/1 knapsack, duyệt ngược)
- 15' Bài 5 (LIS \(O(n^2)\))
- 20' Bài 6 (DP 2D + tối ưu bộ nhớ)
