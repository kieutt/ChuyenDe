# Thuật toán Tarjan - Tìm cầu và khớp trong đồ thị

## 📚 Mục lục
1. [Lý thuyết cơ bản](#1-lý-thuyết-cơ-bản)
2. [Thuật toán Tarjan tìm cầu](#2-thuật-toán-tarjan-tìm-cầu)
3. [Thuật toán Tarjan tìm khớp](#3-thuật-toán-tarjan-tìm-khớp)
4. [Ví dụ minh họa](#4-ví-dụ-minh-họa)
5. [Phân tích độ phức tạp](#5-phân-tích-độ-phức-tạp)
6. [Code template](#6-code-template)
7. [Mở rộng ứng dụng](#7-mở-rộng-ứng-dụng)
8. [Bài tập thực hành](#8-bài-tập-thực-hành)

---

## 1. Lý thuyết cơ bản

### 1.1. Định nghĩa

**Cầu (Bridge)**: Là cạnh mà khi loại bỏ sẽ làm tăng số thành phần liên thông của đồ thị.

**Khớp (Articulation Point/Cut Vertex)**: Là đỉnh mà khi loại bỏ (cùng với tất cả cạnh liền kề) sẽ làm tăng số thành phần liên thông của đồ thị.

### 1.2. Ý nghĩa thực tế

- **Cầu**: Đường giao thông quan trọng, nếu bị phá hủy sẽ chia cắt khu vực
- **Khớp**: Thành phố trung tâm, nếu bị tấn công sẽ chia cắt mạng lưới
- **Ứng dụng**: Phân tích độ tin cậy mạng, tối ưu hóa hạ tầng

### 1.3. Thuật toán trước Tarjan

**Cách naive**: Với mỗi cạnh/đỉnh, loại bỏ rồi kiểm tra tính liên thông
- **Độ phức tạp**: O(E × (V + E)) = O(E²) cho cầu
- **Vấn đề**: Quá chậm với đồ thị lớn

---

## 2. Thuật toán Tarjan tìm cầu

### 2.1. Ý tưởng chính

Robert Tarjan (1974) phát minh thuật toán sử dụng **DFS Tree** với hai mảng:

- **tin[u]**: Thời điểm phát hiện đỉnh u trong DFS
- **low[u]**: Thời điểm nhỏ nhất có thể đến được từ cây con gốc u

### 2.2. Công thức tính low[]

```
low[u] = min(
    tin[u],                           // Chính nó
    min(tin[v] | v là con của u và (u,v) là back edge),
    min(low[v] | v là con của u và (u,v) là tree edge)
)
```

### 2.3. Điều kiện nhận biết cầu

Cạnh (u, v) là cầu ⟺ **low[v] > tin[u]** (v là con của u trong DFS tree)

**Giải thích**: Nếu low[v] > tin[u], nghĩa là từ cây con gốc v không thể quay lại được u hoặc tổ tiên của u → Cạnh (u,v) là cầu nối duy nhất.

### 2.4. Code cơ bản

```cpp
vector<vector<int>> adj;
vector<int> tin, low;
vector<bool> visited;
vector<pair<int,int>> bridges;
int timer;

void dfs(int u, int p = -1) {
    visited[u] = true;
    tin[u] = low[u] = timer++;
    
    for (int v : adj[u]) {
        if (v == p) continue; // Không quay lại cha
        
        if (visited[v]) {
            // Back edge
            low[u] = min(low[u], tin[v]);
        } else {
            // Tree edge
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            
            // Kiểm tra cầu
            if (low[v] > tin[u]) {
                bridges.push_back({u, v});
            }
        }
    }
}

void find_bridges() {
    timer = 0;
    visited.assign(n, false);
    tin.assign(n, -1);
    low.assign(n, -1);
    
    for (int i = 0; i < n; ++i) {
        if (!visited[i]) {
            dfs(i);
        }
    }
}
```

---

## 3. Thuật toán Tarjan tìm khớp

### 3.1. Điều kiện nhận biết khớp

Đỉnh u là khớp nếu:

1. **u là gốc** của DFS tree và có **≥ 2 con**
2. **u không là gốc** và tồn tại con v sao cho **low[v] ≥ tin[u]**

### 3.2. Code tìm khớp

```cpp
vector<bool> cut_point;

void dfs(int u, int p = -1) {
    visited[u] = true;
    tin[u] = low[u] = timer++;
    int children = 0;
    
    for (int v : adj[u]) {
        if (v == p) continue;
        
        if (visited[v]) {
            low[u] = min(low[u], tin[v]);
        } else {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            
            // Kiểm tra khớp
            if (p == -1 && children > 1) {
                cut_point[u] = true; // Gốc có nhiều con
            }
            if (p != -1 && low[v] >= tin[u]) {
                cut_point[u] = true; // Không thể bypass qua u
            }
            children++;
        }
    }
}
```

---

## 4. Ví dụ minh họa

### 4.1. Ví dụ tìm cầu

```
Đồ thị:
1---2---3
|   |   |
4---5---6
    |
    7

DFS bắt đầu từ đỉnh 1:
```

**Bước thực hiện**:
1. DFS(1): tin[1] = 0, low[1] = 0
2. DFS(2): tin[2] = 1, low[2] = 0 (có back edge tới 1)
3. DFS(3): tin[3] = 2, low[3] = 1 (có back edge tới 2)
4. DFS(6): tin[6] = 3, low[6] = 2 (có back edge tới 3)
5. DFS(5): tin[5] = 4, low[5] = 1 (có back edge tới 2)
6. DFS(7): tin[7] = 5, low[7] = 4 (chỉ có thể về 5)

**Cầu tìm được**: (5,7) vì low[7] = 4 > tin[5] = 4

### 4.2. Ví dụ tìm khớp

Trong cùng đồ thị trên:
- Đỉnh 5 là khớp vì low[7] = 4 ≥ tin[5] = 4
- Loại bỏ đỉnh 5 sẽ tách đỉnh 7 ra khỏi đồ thị

---

## 5. Phân tích độ phức tạp

### 5.1. Thời gian
- **DFS**: O(V + E)
- **Mỗi cạnh được duyệt đúng 2 lần**: một lần từ mỗi đầu
- **Tổng**: O(V + E)

### 5.2. Không gian
- **Mảng tin, low, visited**: O(V)
- **Stack đệ quy**: O(V) trong trường hợp xấu nhất
- **Tổng**: O(V)

### 5.3. So sánh với naive approach
- **Naive**: O(E × (V + E)) = O(E²)
- **Tarjan**: O(V + E)
- **Cải thiện**: Từ O(E²) xuống O(E) - cải thiện đáng kể!

---

## 6. Code template

### 6.1. Template tìm cầu

```cpp
class BridgeFinder {
private:
    int n, timer;
    vector<vector<int>> adj;
    vector<int> tin, low;
    vector<bool> visited;
    vector<pair<int,int>> bridges;
    
    void dfs(int u, int p = -1) {
        visited[u] = true;
        tin[u] = low[u] = timer++;
        
        for (int v : adj[u]) {
            if (v == p) continue;
            if (visited[v]) {
                low[u] = min(low[u], tin[v]);
            } else {
                dfs(v, u);
                low[u] = min(low[u], low[v]);
                if (low[v] > tin[u]) {
                    bridges.push_back({min(u,v), max(u,v)});
                }
            }
        }
    }
    
public:
    BridgeFinder(int n) : n(n), adj(n) {}
    
    void add_edge(int u, int v) {
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    
    vector<pair<int,int>> find_bridges() {
        timer = 0;
        tin.assign(n, -1);
        low.assign(n, -1);
        visited.assign(n, false);
        bridges.clear();
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i);
            }
        }
        
        sort(bridges.begin(), bridges.end());
        return bridges;
    }
};
```

### 6.2. Template tìm khớp

```cpp
class ArticulationFinder {
private:
    int n, timer;
    vector<vector<int>> adj;
    vector<int> tin, low;
    vector<bool> visited, is_cutpoint;
    
    void dfs(int u, int p = -1) {
        visited[u] = true;
        tin[u] = low[u] = timer++;
        int children = 0;
        
        for (int v : adj[u]) {
            if (v == p) continue;
            if (visited[v]) {
                low[u] = min(low[u], tin[v]);
            } else {
                dfs(v, u);
                low[u] = min(low[u], low[v]);
                
                if (p == -1 && children > 1) {
                    is_cutpoint[u] = true;
                }
                if (p != -1 && low[v] >= tin[u]) {
                    is_cutpoint[u] = true;
                }
                children++;
            }
        }
    }
    
public:
    ArticulationFinder(int n) : n(n), adj(n) {}
    
    void add_edge(int u, int v) {
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    
    vector<int> find_articulations() {
        timer = 0;
        tin.assign(n, -1);
        low.assign(n, -1);
        visited.assign(n, false);
        is_cutpoint.assign(n, false);
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i);
            }
        }
        
        vector<int> result;
        for (int i = 0; i < n; i++) {
            if (is_cutpoint[i]) {
                result.push_back(i);
            }
        }
        return result;
    }
};
```

---

## 7. Mở rộng ứng dụng

### 7.1. Strongly Connected Components (SCC)

Tarjan cũng có thuật toán tìm SCC trong đồ thị có hướng:

```cpp
// Sử dụng stack để theo dõi các đỉnh trong SCC hiện tại
vector<int> ids, low, on_stack;
stack<int> st;
vector<vector<int>> sccs;

void dfs(int u) {
    ids[u] = low[u] = timer++;
    st.push(u);
    on_stack[u] = true;
    
    for (int v : adj[u]) {
        if (ids[v] == -1) {
            dfs(v);
        }
        if (on_stack[v]) {
            low[u] = min(low[u], low[v]);
        }
    }
    
    // u là gốc của một SCC
    if (ids[u] == low[u]) {
        vector<int> component;
        while (true) {
            int v = st.top(); st.pop();
            on_stack[v] = false;
            component.push_back(v);
            if (v == u) break;
        }
        sccs.push_back(component);
    }
}
```

### 7.2. 2-Edge-Connected Components

Tìm các thành phần liên thông không chứa cầu:

```cpp
// Sau khi tìm được các cầu, loại bỏ chúng và tìm connected components
void find_2_edge_connected_components() {
    auto bridges = find_bridges();
    set<pair<int,int>> bridge_set(bridges.begin(), bridges.end());
    
    // Xây dựng đồ thị không có cầu
    vector<vector<int>> new_adj(n);
    for (int u = 0; u < n; u++) {
        for (int v : adj[u]) {
            if (bridge_set.find({min(u,v), max(u,v)}) == bridge_set.end()) {
                new_adj[u].push_back(v);
            }
        }
    }
    
    // Tìm connected components trong đồ thị mới
    // ...
}
```

### 7.3. Block-Cut Tree

Cây biểu diễn mối quan hệ giữa khớp và block (thành phần 2-connected):

```cpp
// Xây dựng block-cut tree từ đồ thị gốc
// Mỗi khớp và mỗi block là một node trong cây
// Có cạnh nối giữa khớp và các block chứa nó
```

---

## 8. Bài tập thực hành

### 8.1. Beginner Level

**1. Critical Links (UVA 796)**
- **Link**: [UVA 796](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=737)
- **Mô tả**: Tìm tất cả các cầu trong mạng máy tính
- **Độ khó**: ⭐⭐☆☆☆

**2. Articulation Points (SPOJ SUBMERGE)**
- **Link**: [SPOJ SUBMERGE](https://www.spoj.com/problems/SUBMERGE/)
- **Mô tả**: Đếm số lượng khớp trong đồ thị
- **Độ khó**: ⭐⭐☆☆☆

### 8.2. Intermediate Level

**3. Codeforces 118E - Berland Roads**
- **Link**: [CF 118E](https://codeforces.com/problemset/problem/118/E)
- **Mô tả**: Định hướng các cạnh để tạo đồ thị liên thông mạnh
- **Độ khó**: ⭐⭐⭐☆☆
- **Kỹ thuật**: Tarjan bridges + graph orientation

**4. AtCoder ABC334 F - Christmas Present 2**
- **Link**: [AtCoder ABC334F](https://atcoder.jp/contests/abc334/tasks/abc334_f)
- **Mô tả**: Tối ưu hóa với ràng buộc về cầu
- **Độ khó**: ⭐⭐⭐☆☆

**5. LeetCode 1192 - Critical Connections**
- **Link**: [LeetCode 1192](https://leetcode.com/problems/critical-connections-in-a-network/)
- **Mô tả**: Tìm các kết nối quan trọng trong mạng
- **Độ khó**: ⭐⭐⭐☆☆

### 8.3. Advanced Level

**6. Codeforces 700C - Break Up**
- **Link**: [CF 700C](https://codeforces.com/problemset/problem/700/C)
- **Mô tả**: Tìm chi phí tối thiểu để phá vỡ tính liên thông
- **Độ khó**: ⭐⭐⭐⭐☆
- **Kỹ thuật**: Tarjan + shortest path + binary search

**7. Codeforces 555E - Case of Computer Network**
- **Link**: [CF 555E](https://codeforces.com/problemset/problem/555/E)
- **Mô tả**: Kiểm tra tính khả thi của định hướng đồ thị
- **Độ khó**: ⭐⭐⭐⭐☆
- **Kỹ thuật**: Bridge tree + LCA

**8. AtCoder ARC 039C - 幼稚園児高橋君**
- **Link**: [AtCoder ARC039C](https://atcoder.jp/contests/arc039/tasks/arc039_c)
- **Mô tả**: Đếm số cách đi qua tất cả cầu
- **Độ khó**: ⭐⭐⭐⭐⭐
- **Kỹ thuật**: Bridge decomposition + DP

### 8.4. Expert Level

**9. Codeforces 231E - Cactus**
- **Link**: [CF 231E](https://codeforces.com/problemset/problem/231/E)
- **Mô tả**: Xử lý trên đồ thị cactus (mỗi cạnh thuộc tối đa 1 cycle)
- **Độ khó**: ⭐⭐⭐⭐⭐
- **Kỹ thuật**: Modified Tarjan + tree DP

**10. ICPC World Finals - Pollution Solution**
- **Mô tả**: Tối ưu hóa network flow với ràng buộc về articulation points
- **Độ khó**: ⭐⭐⭐⭐⭐

---

## 9. Tips & Tricks

### 9.1. Common Pitfalls

1. **Multiple edges**: Cần xử lý cẩn thận khi có nhiều cạnh giữa 2 đỉnh
2. **Self loops**: Thường bỏ qua vì không ảnh hưởng đến tính liên thông
3. **0-indexed vs 1-indexed**: Chú ý convert đúng
4. **Parent tracking**: Dùng edge ID thay vì vertex để tránh nhầm lẫn

### 9.2. Optimization Techniques

1. **Fast I/O**: Sử dụng `ios::sync_with_stdio(false)`
2. **Memory pre-allocation**: Reserve vector size trước
3. **Iterative DFS**: Tránh stack overflow với đồ thị lớn

### 9.3. Debug Strategies

1. **Visualize**: Vẽ đồ thị nhỏ để hiểu thuật toán
2. **Print intermediate states**: In tin[], low[] để debug
3. **Test edge cases**: Đồ thị rỗng, cây, complete graph

---

## 10. Kết luận

Thuật toán Tarjan là một trong những thuật toán đồ thị quan trọng nhất, với ứng dụng rộng rãi từ network analysis đến competitive programming. Việc hiểu sâu về:

- **DFS tree structure**
- **Back edge vs tree edge**  
- **Ý nghĩa của tin[] và low[]**

sẽ giúp bạn không chỉ implement correctly mà còn áp dụng vào các bài toán phức tạp khác.

**Next steps**:
1. Implement và test trên các bài cơ bản
2. Mở rộng sang SCC và block-cut tree
3. Áp dụng vào các contest problems
4. Kết hợp với các thuật toán khác (LCA, DP, Flow)

**Remember**: Practice makes perfect! 🚀