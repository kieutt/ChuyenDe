# Duyệt đồ thị BFS, DFS & Ứng dụng

> **Chuyên đề:** Đồ thị — Lập trình thi đấu
> **Đối tượng:** Học sinh lớp 10A4 đã biết C++ và Python
> **Yêu cầu trước:** Đã học Khái niệm đồ thị và Biểu diễn đồ thị bằng danh sách kề

---
---

# Duyệt đồ thị — BFS (Breadth-First Search)

## 1. Đặt vấn đề

Cho đồ thị sau với 6 đỉnh:

```
    1 --- 2 --- 5
    |     |
    3 --- 4 --- 6
```

**Câu hỏi mở:** Nếu xuất phát từ đỉnh 1, có những cách nào để "thăm" tất cả các đỉnh? Thứ tự thăm sẽ ra sao nếu ta ưu tiên thăm các đỉnh **gần trước, xa sau**?

Trả lời trực giác: Từ đỉnh 1, ta thăm các đỉnh cách 1 bước (2, 3), rồi cách 2 bước (4, 5), rồi cách 3 bước (6). Đây chính là ý tưởng cốt lõi của BFS.

---

## 2. Ý tưởng thuật toán BFS

### 2.1. Nguyên lý "lan toả từng lớp"

BFS duyệt đồ thị theo **chiều rộng**: từ đỉnh nguồn, ta thăm lần lượt tất cả đỉnh ở khoảng cách 1, rồi khoảng cách 2, rồi khoảng cách 3, ... giống như **gợn sóng lan trên mặt nước** khi ta ném một viên đá.

### 2.2. Cấu trúc dữ liệu then chốt — Hàng đợi (Queue)

BFS sử dụng hàng đợi (FIFO — First In First Out):

- **Bước 1:** Đưa đỉnh nguồn vào hàng đợi, đánh dấu đã thăm.
- **Bước 2:** Lấy đỉnh ở đầu hàng đợi ra (gọi là `u`).
- **Bước 3:** Xét tất cả đỉnh `v` kề với `u`. Nếu `v` chưa thăm → đánh dấu đã thăm, đưa `v` vào cuối hàng đợi.
- **Lặp lại** Bước 2–3 cho đến khi hàng đợi rỗng.

### 2.3. Mô phỏng từng bước trên ví dụ

Đồ thị mẫu (danh sách kề):

```
1: [2, 3]
2: [1, 4, 5]
3: [1, 4]
4: [2, 3, 6]
5: [2]
6: [4]
```

Xuất phát từ đỉnh `1`:

| Bước | Hàng đợi (trước) | Lấy ra | Thêm vào   | Hàng đợi (sau) | visited            |
|------|-------------------|--------|------------|-----------------|--------------------|
| 0    | [1]               | —      | —          | [1]             | {1}                |
| 1    | [1]               | 1      | 2, 3       | [2, 3]          | {1, 2, 3}          |
| 2    | [2, 3]            | 2      | 4, 5       | [3, 4, 5]       | {1, 2, 3, 4, 5}    |
| 3    | [3, 4, 5]         | 3      | (không có) | [4, 5]          | {1, 2, 3, 4, 5}    |
| 4    | [4, 5]            | 4      | 6          | [5, 6]          | {1, 2, 3, 4, 5, 6} |
| 5    | [5, 6]            | 5      | (không có) | [6]             | {1, 2, 3, 4, 5, 6} |
| 6    | [6]               | 6      | (không có) | []              | {1, 2, 3, 4, 5, 6} |

**Thứ tự thăm:** 1 → 2 → 3 → 4 → 5 → 6

**Nhận xét quan trọng:** Các đỉnh được thăm theo thứ tự khoảng cách tăng dần từ đỉnh nguồn. Đây là tính chất cốt lõi giúp BFS tìm đường đi ngắn nhất trên đồ thị không trọng số.

---

## 3. Cài đặt BFS

### 3.1. Cài đặt C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100005;
vector<int> adj[MAXN];  // Danh sách kề
bool visited[MAXN];      // Đánh dấu đã thăm
int dist[MAXN];          // Khoảng cách từ nguồn

void bfs(int source) {
    queue<int> q;

    // Khởi tạo
    memset(visited, false, sizeof(visited));
    memset(dist, -1, sizeof(dist));

    // Đưa đỉnh nguồn vào hàng đợi
    q.push(source);
    visited[source] = true;
    dist[source] = 0;

    while (!q.empty()) {
        int u = q.front();   // Lấy đỉnh đầu hàng đợi
        q.pop();

        // Duyệt tất cả đỉnh kề với u
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int n, m;  // n đỉnh, m cạnh
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);  // Đồ thị vô hướng
    }

    bfs(1);  // BFS từ đỉnh 1

    // In khoảng cách từ đỉnh 1 đến tất cả đỉnh
    for (int i = 1; i <= n; i++) {
        cout << "dist[" << i << "] = " << dist[i] << "\n";
    }

    return 0;
}
```

**Input mẫu:**

```
6 7
1 2
1 3
2 4
2 5
3 4
4 6
```

**Output:**

```
dist[1] = 0
dist[2] = 1
dist[3] = 1
dist[4] = 2
dist[5] = 2
dist[6] = 3
```

### 3.2. Cài đặt Python

```python
from collections import deque

def bfs(adj, source, n):
    visited = [False] * (n + 1)
    dist = [-1] * (n + 1)

    q = deque()
    q.append(source)
    visited[source] = True
    dist[source] = 0

    while q:
        u = q.popleft()          # Lấy đỉnh đầu hàng đợi
        for v in adj[u]:
            if not visited[v]:
                visited[v] = True
                dist[v] = dist[u] + 1
                q.append(v)

    return dist

# Đọc input
n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

dist = bfs(adj, 1, n)

for i in range(1, n + 1):
    print(f"dist[{i}] = {dist[i]}")
```

> **Lưu ý cho học sinh:** Trong Python, dùng `deque` thay vì `list` làm hàng đợi. Thao tác `list.pop(0)` có độ phức tạp O(n), trong khi `deque.popleft()` chỉ O(1).

---

## 4. Phân tích độ phức tạp

- **Thời gian:** O(V + E), trong đó V là số đỉnh, E là số cạnh. Mỗi đỉnh được đưa vào hàng đợi đúng 1 lần, mỗi cạnh được xét đúng 2 lần (vô hướng) hoặc 1 lần (có hướng).
- **Bộ nhớ:** O(V) cho mảng `visited`, `dist` và hàng đợi.

Đây là thuật toán rất hiệu quả — với đồ thị 10⁵ đỉnh và 10⁵ cạnh, BFS chạy gần như tức thì.

---

## 5. Tính chất quan trọng của BFS

**Định lý:** Trên đồ thị không trọng số (hoặc tất cả cạnh trọng số bằng nhau), BFS từ đỉnh `s` sẽ tìm được **đường đi ngắn nhất** (tính theo số cạnh) từ `s` đến mọi đỉnh khác.

**Tại sao?** Vì BFS thăm theo từng "lớp" khoảng cách. Khi đỉnh `v` được thăm lần đầu tiên, giá trị `dist[v]` chính xác bằng số cạnh ít nhất cần đi từ `s` đến `v`. Không thể có đường đi nào ngắn hơn, bởi tất cả đường đi ngắn hơn đã được khám phá ở các lớp trước.

---

## 6. Tổng kết tiết 3

**Ghi nhớ:**

- BFS = Duyệt theo chiều rộng = Hàng đợi (Queue).
- Thăm theo từng lớp khoảng cách.
- Tìm đường đi ngắn nhất trên đồ thị không trọng số.
- Độ phức tạp O(V + E).

---
---

# Duyệt đồ thị — DFS (Depth-First Search)

## 1. Đặt vấn đề

Quay lại đồ thị cũ:

```
    1 --- 2 --- 5
    |     |
    3 --- 4 --- 6
```

**Câu hỏi mở:** Thay vì thăm "gần trước, xa sau" như BFS, nếu ta đi theo chiến lược **"lao sâu nhất có thể rồi mới quay lui"** thì thứ tự thăm sẽ ra sao?

Trả lời trực giác: Từ đỉnh 1, đi sâu: 1 → 2 → 4 → 3 (quay lui vì 1 đã thăm) → 6 → (quay lui) → 5. Đây là ý tưởng của DFS.

---

## 2. Ý tưởng thuật toán DFS

### 2.1. Nguyên lý "lao sâu rồi quay lui"

DFS duyệt theo **chiều sâu**: từ đỉnh hiện tại, ta chọn một đỉnh kề chưa thăm và đi tiếp vào đó. Khi không còn đỉnh kề nào chưa thăm, ta **quay lui** (backtrack) về đỉnh trước đó và thử hướng khác. Giống như đi trong mê cung: ta cứ rẽ vào một ngã cho đến khi đi vào ngõ cụt, rồi quay lại và thử ngã khác.

### 2.2. Hai cách cài đặt

- **Đệ quy (Recursion):** Gọi hàm DFS lồng nhau — hệ thống tự quản lý ngăn xếp gọi hàm. Cách này ngắn gọn, trực quan.
- **Ngăn xếp (Stack):** Dùng stack tường minh, tương tự BFS nhưng thay queue bằng stack. Cách này tránh bị tràn ngăn xếp (stack overflow) khi đồ thị rất lớn.

### 2.3. Mô phỏng DFS đệ quy trên ví dụ

Danh sách kề (đã sắp xếp tăng dần cho nhất quán):

```
1: [2, 3]
2: [1, 4, 5]
3: [1, 4]
4: [2, 3, 6]
5: [2]
6: [4]
```

Xuất phát từ đỉnh `1`:

```
DFS(1)
  → visited = {1}
  → Xét 2 (chưa thăm) → gọi DFS(2)
      → visited = {1, 2}
      → Xét 1 (đã thăm, bỏ qua)
      → Xét 4 (chưa thăm) → gọi DFS(4)
          → visited = {1, 2, 4}
          → Xét 2 (đã thăm, bỏ qua)
          → Xét 3 (chưa thăm) → gọi DFS(3)
              → visited = {1, 2, 4, 3}
              → Xét 1 (đã thăm, bỏ qua)
              → Xét 4 (đã thăm, bỏ qua)
              → ← quay lui về DFS(4)
          → Xét 6 (chưa thăm) → gọi DFS(6)
              → visited = {1, 2, 4, 3, 6}
              → Xét 4 (đã thăm, bỏ qua)
              → ← quay lui về DFS(4)
          → ← quay lui về DFS(2)
      → Xét 5 (chưa thăm) → gọi DFS(5)
          → visited = {1, 2, 4, 3, 6, 5}
          → Xét 2 (đã thăm, bỏ qua)
          → ← quay lui về DFS(2)
      → ← quay lui về DFS(1)
  → Xét 3 (đã thăm, bỏ qua)
  → Kết thúc
```

**Thứ tự thăm:** 1 → 2 → 4 → 3 → 6 → 5

> **So sánh:** BFS cho thứ tự 1 → 2 → 3 → 4 → 5 → 6 (theo lớp), DFS cho thứ tự 1 → 2 → 4 → 3 → 6 → 5 (lao sâu).

### 2.4. Thời gian vào – ra (Discovery & Finish Time)

Trong quá trình DFS, ta ghi lại hai mốc thời gian cho mỗi đỉnh:

- `tin[u]` (time in): thời điểm bắt đầu thăm đỉnh `u`.
- `tout[u]` (time out): thời điểm kết thúc thăm đỉnh `u` (đã xét hết mọi đỉnh kề).

Mô phỏng (dùng biến `timer` tăng dần):

| Đỉnh | tin | tout |
|------|-----|------|
| 1    | 1   | 12   |
| 2    | 2   | 11   |
| 4    | 3   | 8    |
| 3    | 4   | 5    |
| 6    | 6   | 7    |
| 5    | 9   | 10   |

**Tính chất quan trọng:** Đỉnh `u` là tổ tiên của đỉnh `v` trong cây DFS khi và chỉ khi `tin[u] < tin[v]` và `tout[u] > tout[v]` (khoảng thời gian của `v` nằm hoàn toàn trong khoảng thời gian của `u`). Tính chất này rất hữu ích cho nhiều bài toán nâng cao.

---

## 3. Cài đặt DFS

### 3.1. DFS đệ quy — C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100005;
vector<int> adj[MAXN];
bool visited[MAXN];
int tin[MAXN], tout[MAXN];
int timer_counter = 0;

void dfs(int u) {
    visited[u] = true;
    tin[u] = ++timer_counter;       // Ghi thời gian vào

    for (int v : adj[u]) {
        if (!visited[v]) {
            dfs(v);                  // Đệ quy xuống đỉnh con
        }
    }

    tout[u] = ++timer_counter;      // Ghi thời gian ra
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int n, m;
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    // DFS từ đỉnh 1
    dfs(1);

    for (int i = 1; i <= n; i++) {
        cout << "Dinh " << i
             << ": tin = " << tin[i]
             << ", tout = " << tout[i] << "\n";
    }

    return 0;
}
```

### 3.2. DFS đệ quy — Python

```python
import sys
sys.setrecursionlimit(200000)  # Tăng giới hạn đệ quy cho đồ thị lớn

timer_counter = 0

def dfs(u, adj, visited, tin, tout):
    global timer_counter
    visited[u] = True
    timer_counter += 1
    tin[u] = timer_counter

    for v in adj[u]:
        if not visited[v]:
            dfs(v, adj, visited, tin, tout)

    timer_counter += 1
    tout[u] = timer_counter

# Đọc input
n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

visited = [False] * (n + 1)
tin = [0] * (n + 1)
tout = [0] * (n + 1)

dfs(1, adj, visited, tin, tout)

for i in range(1, n + 1):
    print(f"Dinh {i}: tin = {tin[i]}, tout = {tout[i]}")
```

> **Lưu ý cho học sinh:** Trong Python, giới hạn đệ quy mặc định là 1000. Với đồ thị lớn, ta cần gọi `sys.setrecursionlimit(...)`. Nếu đồ thị có thể lên đến 10⁵ đỉnh dạng đường thẳng (worst case), nên dùng DFS lặp (stack) thay vì đệ quy.

### 3.3. DFS lặp (dùng ngăn xếp tường minh) — C++

```cpp
void dfs_iterative(int source) {
    stack<int> st;
    st.push(source);

    while (!st.empty()) {
        int u = st.top();
        st.pop();

        if (visited[u]) continue;  // Đỉnh đã thăm thì bỏ qua
        visited[u] = true;

        // Đẩy các đỉnh kề chưa thăm vào stack
        // Duyệt ngược để giữ thứ tự tương tự đệ quy
        for (int i = (int)adj[u].size() - 1; i >= 0; i--) {
            int v = adj[u][i];
            if (!visited[v]) {
                st.push(v);
            }
        }
    }
}
```

> **Khi nào dùng DFS lặp?** Khi N lớn (≥ 10⁵) và đồ thị có thể có chuỗi dài (ví dụ: đồ thị đường thẳng 1–2–3–...–N). DFS đệ quy sẽ gây tràn ngăn xếp hệ thống, còn DFS lặp thì an toàn.

---

## 4. So sánh BFS và DFS

| Tiêu chí               | BFS                              | DFS                               |
|-------------------------|----------------------------------|------------------------------------|
| Cấu trúc dữ liệu       | Hàng đợi (Queue)                 | Ngăn xếp (Stack) / Đệ quy        |
| Thứ tự duyệt           | Theo lớp (gần → xa)             | Lao sâu rồi quay lui              |
| Tìm đường đi ngắn nhất | Có (đồ thị không trọng số)       | Không                              |
| Bộ nhớ (worst case)     | O(V) — có thể lớn nếu tầng rộng | O(V) — có thể lớn nếu chuỗi sâu  |
| Phát hiện chu trình     | Được                             | Được                               |
| Ứng dụng nổi bật        | Khoảng cách ngắn nhất, BFS lưới | Topo sort, SCC, cầu/khớp, quay lui |

**Quy tắc ngón tay cái:**

- Cần **khoảng cách ngắn nhất** → nghĩ đến BFS.
- Cần **duyệt toàn bộ / quay lui / phân loại cạnh** → nghĩ đến DFS.
- Cần **đếm thành phần liên thông** → cả hai đều được.

---

## 5. Ứng dụng cơ bản: Đếm thành phần liên thông

Đồ thị có thể không liên thông (gồm nhiều "mảnh" rời). Để đếm số thành phần liên thông, ta duyệt BFS hoặc DFS từ từng đỉnh chưa thăm:

```cpp
int count_components = 0;
for (int i = 1; i <= n; i++) {
    if (!visited[i]) {
        dfs(i);              // hoặc bfs(i)
        count_components++;
    }
}
cout << count_components << "\n";
```

Mỗi lần gọi `dfs(i)` hoặc `bfs(i)`, toàn bộ thành phần liên thông chứa `i` sẽ được thăm. Số lần gọi chính là số thành phần liên thông.

---

## 6. Tổng kết

**Ghi nhớ:**

- DFS = Duyệt theo chiều sâu = Đệ quy hoặc Stack.
- Lao sâu nhất có thể, rồi quay lui.
- Ghi nhận `tin`, `tout` để biết quan hệ tổ tiên–hậu duệ.
- DFS đệ quy: gọn, dễ hiểu. DFS lặp: an toàn cho đồ thị lớn.
- Cả BFS và DFS đều có thể đếm thành phần liên thông.

---
---

# Ứng dụng BFS & DFS (phần 1)

## 1. Ứng dụng 1 — Kiểm tra đồ thị hai phía (Bipartite Check)

### 1.1. Bài toán

Đồ thị hai phía (bipartite graph) là đồ thị mà tập đỉnh có thể chia thành **hai nhóm** sao cho mọi cạnh đều nối một đỉnh nhóm A với một đỉnh nhóm B (không có cạnh nối hai đỉnh cùng nhóm).

**Ứng dụng thực tế:**

- Xếp chỗ ngồi sao cho hai người không ưa nhau không ngồi cùng bàn (với đúng 2 bàn).
- Phân đội trong trò chơi: mỗi cặp "đối thủ" phải ở khác đội.

### 1.2. Điều kiện cần và đủ

**Định lý:** Đồ thị là hai phía khi và chỉ khi nó **không chứa chu trình có độ dài lẻ**.

### 1.3. Thuật toán kiểm tra bằng BFS

Ý tưởng: tô màu đồ thị bằng 2 màu. Xuất phát từ đỉnh bất kỳ, tô màu 0. Tất cả đỉnh kề tô màu 1. Tất cả đỉnh kề của chúng tô màu 0. Nếu gặp đỉnh kề đã tô cùng màu → **không phải đồ thị hai phía**.

```
Ví dụ 1 — ĐỒ THỊ HAI PHÍA:

    1 --- 2           Tô màu: 1(A) - 2(B) - 3(A) - 4(B)
    |     |           Nhóm A = {1, 3}
    3 --- 4           Nhóm B = {2, 4}
                      → Hợp lệ ✓

Ví dụ 2 — KHÔNG PHẢI HAI PHÍA:

    1 --- 2           Tô màu: 1(A) - 2(B) - 3(A)
    |     |           Nhưng cạnh 1—3: cả hai đều màu A
    3 ----+           → Mâu thuẫn ✗ (chu trình lẻ 1-2-3)
    |
    + --- 1
```

### 1.4. Cài đặt C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100005;
vector<int> adj[MAXN];
int color[MAXN];  // -1: chưa tô, 0: màu A, 1: màu B

bool bfs_bipartite(int source) {
    queue<int> q;
    q.push(source);
    color[source] = 0;

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        for (int v : adj[u]) {
            if (color[v] == -1) {
                // Chưa tô → tô màu ngược với u
                color[v] = 1 - color[u];
                q.push(v);
            } else if (color[v] == color[u]) {
                // Đã tô cùng màu → không phải hai phía
                return false;
            }
        }
    }
    return true;
}

int main() {
    int n, m;
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    memset(color, -1, sizeof(color));

    bool is_bipartite = true;
    for (int i = 1; i <= n; i++) {
        if (color[i] == -1) {
            if (!bfs_bipartite(i)) {
                is_bipartite = false;
                break;
            }
        }
    }

    cout << (is_bipartite ? "YES" : "NO") << "\n";
    return 0;
}
```

### 1.5. Cài đặt Python

```python
from collections import deque

def bfs_bipartite(source, adj, color):
    q = deque()
    q.append(source)
    color[source] = 0

    while q:
        u = q.popleft()
        for v in adj[u]:
            if color[v] == -1:
                color[v] = 1 - color[u]
                q.append(v)
            elif color[v] == color[u]:
                return False
    return True

n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

color = [-1] * (n + 1)
is_bipartite = True

for i in range(1, n + 1):
    if color[i] == -1:
        if not bfs_bipartite(i, adj, color):
            is_bipartite = False
            break

print("YES" if is_bipartite else "NO")
```

**Độ phức tạp:** O(V + E) — giống BFS thông thường.

---

## 2. Ứng dụng 2 — Phát hiện chu trình trong đồ thị vô hướng

### 2.1. Bài toán

Cho đồ thị vô hướng. Hỏi đồ thị có chứa chu trình hay không?

**Ứng dụng thực tế:** Kiểm tra mạng lưới có vòng lặp, kiểm tra cây (cây = đồ thị liên thông, không chu trình).

### 2.2. Ý tưởng bằng DFS

Khi DFS, nếu ta gặp một đỉnh kề `v` **đã thăm** mà `v` **không phải cha trực tiếp** của đỉnh hiện tại `u` trong cây DFS → tồn tại **chu trình**.

Tại sao cần loại trừ cha? Vì đồ thị vô hướng: cạnh u–v có cả hai chiều. Khi DFS từ `u` (mà `u` được gọi từ `v`), ta sẽ luôn gặp lại `v` — nhưng đó chỉ là cạnh quay về cha, không phải chu trình.

```
Ví dụ có chu trình:

    1 --- 2
    |     |
    3 --- 4

    DFS(1) → DFS(2) → DFS(4) → DFS(3)
    Tại DFS(3): gặp đỉnh 1 đã thăm, 1 ≠ cha(3) = 4
    → Phát hiện chu trình: 1 — 2 — 4 — 3 — 1
```

### 2.3. Cài đặt C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100005;
vector<int> adj[MAXN];
bool visited[MAXN];

// Trả về true nếu tìm thấy chu trình
bool dfs_cycle(int u, int parent) {
    visited[u] = true;

    for (int v : adj[u]) {
        if (!visited[v]) {
            // Đi tiếp vào đỉnh chưa thăm
            if (dfs_cycle(v, u)) return true;
        } else if (v != parent) {
            // Gặp đỉnh đã thăm mà không phải cha → chu trình!
            return true;
        }
    }
    return false;
}

int main() {
    int n, m;
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    memset(visited, false, sizeof(visited));

    bool has_cycle = false;
    for (int i = 1; i <= n; i++) {
        if (!visited[i]) {
            if (dfs_cycle(i, -1)) {  // -1 nghĩa là không có cha
                has_cycle = true;
                break;
            }
        }
    }

    cout << (has_cycle ? "YES" : "NO") << "\n";
    return 0;
}
```

### 2.4. Cài đặt Python

```python
import sys
sys.setrecursionlimit(200000)

def dfs_cycle(u, parent, adj, visited):
    visited[u] = True
    for v in adj[u]:
        if not visited[v]:
            if dfs_cycle(v, u, adj, visited):
                return True
        elif v != parent:
            return True
    return False

n, m = map(int, input().split())
adj = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    adj[u].append(v)
    adj[v].append(u)

visited = [False] * (n + 1)
has_cycle = False

for i in range(1, n + 1):
    if not visited[i]:
        if dfs_cycle(i, -1, adj, visited):
            has_cycle = True
            break

print("YES" if has_cycle else "NO")
```

### 2.5. Cách khác: Kiểm tra bằng đếm cạnh

Với đồ thị liên thông, nó là cây khi và chỉ khi có đúng N − 1 cạnh. Nếu E ≥ N → chắc chắn có chu trình.

Tuy nhiên, cách DFS tổng quát hơn (áp dụng cả khi đồ thị không liên thông, và khi cần **chỉ ra cụ thể** chu trình nằm ở đâu).

---

## 3. Tổng hợp

| Ứng dụng                   | Thuật toán | Ý tưởng cốt lõi                              |
|-----------------------------|------------|-----------------------------------------------|
| Kiểm tra đồ thị hai phía   | BFS/DFS    | Tô 2 màu, kiểm tra mâu thuẫn                 |
| Phát hiện chu trình (vô hướng) | DFS    | Gặp đỉnh đã thăm, không phải cha → chu trình |

---

## 4. Tổng kết

**Ghi nhớ:**

- Đồ thị hai phía ↔ không có chu trình lẻ ↔ tô được 2 màu.
- Phát hiện chu trình vô hướng: DFS + kiểm tra đỉnh đã thăm khác cha.
- Cả hai ứng dụng đều chạy O(V + E).

**Chuẩn bị cho tiết sau:** Tiết 6 sẽ học BFS trên lưới 2D (bài toán mê cung, flood fill) và truy vết đường đi.

---
---

# BÀI TẬP: Củng cố BFS, DFS & Ứng dụng

> **Mục tiêu:** Học sinh tự giải được các bài tập từ dễ đến trung bình, áp dụng đúng BFS/DFS, biết kiểm tra hai phía và phát hiện chu trình.
> **Hình thức:** Làm bài trên máy, nộp online (VNOJ, CSES hoặc hệ thống chấm của trường).

---

## Bài 1: Đếm thành phần liên thông (★☆☆ — Dễ)

**Đề bài:**

Cho đồ thị vô hướng gồm N đỉnh (1 ≤ N ≤ 10⁵) và M cạnh (0 ≤ M ≤ 2×10⁵). Đếm số thành phần liên thông của đồ thị.

**Input:**

- Dòng 1: Hai số nguyên N, M.
- M dòng tiếp theo, mỗi dòng chứa hai số u, v (1 ≤ u, v ≤ N) biểu diễn một cạnh.

**Output:** Một số nguyên — số thành phần liên thông.

**Ví dụ:**

```
Input:                  Output:
6 4                     3
1 2
2 3
4 5
```

**Giải thích:** Ba thành phần: {1, 2, 3}, {4, 5}, {6}.

**Gợi ý:** Duyệt BFS hoặc DFS từ từng đỉnh chưa thăm, mỗi lần gọi = 1 thành phần.

---

## Bài 2: Khoảng cách ngắn nhất từ nguồn (★☆☆ — Dễ)

**Đề bài:**

Cho đồ thị vô hướng không trọng số gồm N đỉnh và M cạnh. Tìm khoảng cách ngắn nhất (theo số cạnh) từ đỉnh 1 đến tất cả các đỉnh còn lại. Nếu không đến được, in -1.

**Input:**

- Dòng 1: Hai số nguyên N, M (1 ≤ N ≤ 10⁵, 0 ≤ M ≤ 2×10⁵).
- M dòng tiếp theo: mỗi dòng chứa u, v.

**Output:** Một dòng gồm N số, số thứ i là khoảng cách từ đỉnh 1 đến đỉnh i.

**Ví dụ:**

```
Input:                  Output:
5 4                     0 1 2 1 -1
1 2
2 3
1 4
```

**Giải thích:** dist = [0, 1, 2, 1, -1]. Đỉnh 5 không liên thông với đỉnh 1.

**Gợi ý:** BFS chuẩn từ đỉnh 1.

---

## Bài 3: Kiểm tra đồ thị hai phía (★★☆ — Trung bình dễ)

**Đề bài:**

Cho đồ thị vô hướng gồm N đỉnh và M cạnh. Kiểm tra xem đồ thị có phải là đồ thị hai phía hay không. Nếu có, in "YES", ngược lại in "NO".

**Input:**

- Dòng 1: N, M (1 ≤ N ≤ 10⁵, 0 ≤ M ≤ 2×10⁵).
- M dòng tiếp theo: mỗi dòng chứa u, v.

**Output:** "YES" hoặc "NO".

**Ví dụ 1:**

```
Input:                  Output:
4 4                     YES
1 2
2 3
3 4
4 1
```

**Ví dụ 2:**

```
Input:                  Output:
3 3                     NO
1 2
2 3
3 1
```

**Gợi ý:** Tô 2 màu bằng BFS. Chú ý đồ thị có thể không liên thông → kiểm tra từng thành phần.

---

## Bài 4: Phát hiện chu trình (★★☆ — Trung bình dễ)

**Đề bài:**

Cho đồ thị vô hướng gồm N đỉnh và M cạnh. Kiểm tra xem đồ thị có chứa chu trình hay không.

**Input:**

- Dòng 1: N, M (1 ≤ N ≤ 10⁵, 0 ≤ M ≤ 2×10⁵).
- M dòng tiếp theo: mỗi dòng chứa u, v.

**Output:** "YES" nếu có chu trình, "NO" nếu không.

**Ví dụ 1:**

```
Input:                  Output:
4 4                     YES
1 2
2 3
3 4
4 1
```

**Ví dụ 2:**

```
Input:                  Output:
4 3                     NO
1 2
2 3
3 4
```

**Gợi ý:** DFS + kiểm tra đỉnh đã thăm khác cha. Hoặc đếm: nếu đồ thị liên thông có N đỉnh và M ≥ N cạnh thì chắc chắn có chu trình.

---

## Bài 5: Đường đi giữa hai đỉnh (★★☆ — Trung bình)

**Đề bài:**

Cho đồ thị vô hướng gồm N đỉnh và M cạnh, cùng hai đỉnh S và T. Tìm đường đi ngắn nhất (theo số cạnh) từ S đến T. Nếu không có đường đi, in "IMPOSSIBLE".

**Input:**

- Dòng 1: N, M (1 ≤ N ≤ 10⁵, 0 ≤ M ≤ 2×10⁵).
- M dòng tiếp theo: mỗi dòng chứa u, v.
- Dòng cuối: hai số S, T.

**Output:**

- Nếu có đường đi: dòng 1 ghi số đỉnh trên đường đi, dòng 2 ghi danh sách các đỉnh.
- Nếu không: in "IMPOSSIBLE".

**Ví dụ:**

```
Input:                  Output:
5 5                     4
1 2                     1 2 4 5
2 3
2 4
3 4
4 5
1 5
```

**Gợi ý:** BFS từ S, lưu mảng `parent[]` để truy vết. Khi đến T, đi ngược `parent` từ T về S rồi in đảo ngược.

**Code mẫu truy vết (C++):**

```cpp
// Sau khi BFS xong, truy vết từ T về S:
if (!visited[T]) {
    cout << "IMPOSSIBLE\n";
} else {
    vector<int> path;
    for (int v = T; v != -1; v = parent[v]) {
        path.push_back(v);
    }
    reverse(path.begin(), path.end());

    cout << path.size() << "\n";
    for (int v : path) cout << v << " ";
    cout << "\n";
}
```

---

## Bài 6: Tô màu thành phần (★★☆ — Trung bình)

**Đề bài:**

Cho đồ thị vô hướng gồm N đỉnh và M cạnh. Gán cho mỗi đỉnh một "nhãn" là số thứ tự thành phần liên thông mà nó thuộc về (đánh số từ 1).

**Input:**

- Dòng 1: N, M.
- M dòng tiếp theo: mỗi dòng chứa u, v.

**Output:** Một dòng gồm N số, số thứ i là nhãn thành phần của đỉnh i.

**Ví dụ:**

```
Input:                  Output:
7 4                     1 1 1 2 2 3 3
1 2
2 3
4 5
6 7
```

**Gợi ý:** Mỗi lần gọi BFS/DFS cho đỉnh chưa thăm, tăng biến đếm thành phần lên 1, gán nhãn cho tất cả đỉnh trong thành phần đó.

---

## Bài 7: Khoảng cách xa nhất (★★☆ — Trung bình)

**Đề bài:**

Cho đồ thị vô hướng liên thông gồm N đỉnh và M cạnh. Tìm đỉnh xa đỉnh 1 nhất (khoảng cách tính theo số cạnh). Nếu có nhiều đỉnh cùng xa nhất, in đỉnh có chỉ số nhỏ nhất.

**Input:**

- Dòng 1: N, M (1 ≤ N ≤ 10⁵, 0 ≤ M ≤ 2×10⁵).
- M dòng tiếp theo: mỗi dòng chứa u, v.

**Output:** Một dòng gồm hai số: chỉ số đỉnh xa nhất và khoảng cách.

**Ví dụ:**

```
Input:                  Output:
6 6                     6 3
1 2
1 3
2 4
3 4
4 5
5 6
```

**Gợi ý:** BFS từ đỉnh 1, tìm đỉnh có `dist[]` lớn nhất.

---

## Bảng tổng hợp bài tập

| Bài | Tên                    | Độ khó | Thuật toán chính  | Kỹ năng rèn luyện            |
|-----|------------------------|--------|-------------------|-------------------------------|
| 1   | Đếm thành phần liên thông | ★☆☆   | BFS hoặc DFS      | Duyệt đồ thị cơ bản          |
| 2   | Khoảng cách từ nguồn   | ★☆☆    | BFS               | Tính dist[]                   |
| 3   | Kiểm tra hai phía      | ★★☆    | BFS + tô 2 màu    | Bipartite check               |
| 4   | Phát hiện chu trình    | ★★☆    | DFS + parent       | Phân loại cạnh                |
| 5   | Đường đi S → T         | ★★☆    | BFS + parent[]     | Truy vết đường đi             |
| 6   | Tô màu thành phần     | ★★☆    | BFS/DFS + nhãn     | Gán nhãn thành phần           |
| 7   | Khoảng cách xa nhất   | ★★☆    | BFS + tìm max dist | Kết hợp BFS với xử lý kết quả |

---

## Bài tập tự luyện thêm (trên các OJ)

| Bài | Nguồn | Link gợi ý |
|-----|-------|-------------|
| Building Roads | CSES | cses.fi/problemset/task/1666 |
| Message Route | CSES | cses.fi/problemset/task/1667 |
| Building Teams | CSES | cses.fi/problemset/task/1668 |
| Round Trip | CSES | cses.fi/problemset/task/1669 |
| NKGUARD | VNOJ | oj.vnoi.info/problem/nkguard |

---
