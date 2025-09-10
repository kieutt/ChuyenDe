---

## Bài 15: Implement Hash Set (Array + Linked List)

### Đề bài
Tự implement một HashSet đơn giản với các operations:
- add(key): Thêm key vào set
- remove(key): Xóa key khỏi set  
- contains(key): Kiểm tra key có trong set không

**Input:**
- Dòng 1: q (số operations)
- q dòng tiếp theo: operation

**Output:**
Với mỗi contains operation, in ra "YES" hoặc "NO"

### Phân tích thuật toán
- **Ý tưởng:** Dùng hash function + chaining để xử lý collision
- **Độ phức tạp:** O(1) average, O(n) worst case
- **Không gian:** O(n)

### Code Python
```python
class MyHashSet:
    def __init__(self):
        """
        Initialize hash set với 1000 buckets
        Mỗi bucket là một list để handle collision bằng chaining
        """
        self.size = 1000
        self.buckets = [[] for _ in range(self.size)]
    
    def _hash(self, key):
        """Simple hash function"""
        return key % self.size
    
    def add(self, key):
        """Thêm key vào set"""
        bucket_index = self._hash(key)
        bucket = self.buckets[bucket_index]
        
        # Kiểm tra key đã tồn tại chưa
        if key not in bucket:
            bucket.append(key)
    
    def remove(self, key):
        """Xóa key khỏi set"""
        bucket_index = self._hash(key)
        bucket = self.buckets[bucket_index]
        
        if key in bucket:
            bucket.remove(key)
    
    def contains(self, key):
        """Kiểm tra key có trong set không"""
        bucket_index = self._hash(key)
        bucket = self.buckets[bucket_index]
        
        return key in bucket

# Main program
q = int(input())
hash_set = MyHashSet()

for _ in range(q):
    operation = input().split()
    
    if operation[0] == "ADD":
        key = int(operation[1])
        hash_set.add(key)
    
    elif operation[0] == "REMOVE":
        key = int(operation[1])
        hash_set.remove(key)
    
    elif operation[0] == "CONTAINS":
        key = int(operation[1])
        result = hash_set.contains(key)
        print("YES" if result else "NO")
```

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

class MyHashSet {
private:
    static const int SIZE = 1000;
    vector<vector<int>> buckets;
    
    int hash(int key) {
        return key % SIZE;
    }
    
public:
    MyHashSet() {
        // Khởi tạo SIZE buckets, mỗi bucket là một vector
        buckets.resize(SIZE);
    }
    
    void add(int key) {
        int bucketIndex = hash(key);
        vector<int>& bucket = buckets[bucketIndex];
        
        // Kiểm tra key đã tồn tại chưa
        for (int existingKey : bucket) {
            if (existingKey == key) {
                return; // Key đã tồn tại
            }
        }
        
        // Thêm key mới
        bucket.push_back(key);
    }
    
    void remove(int key) {
        int bucketIndex = hash(key);
        vector<int>& bucket = buckets[bucketIndex];
        
        // Tìm và xóa key
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {
            if (*it == key) {
                bucket.erase(it);
                return;
            }
        }
    }
    
    bool contains(int key) {
        int bucketIndex = hash(key);
        vector<int>& bucket = buckets[bucketIndex];
        
        // Tìm key trong bucket
        for (int existingKey : bucket) {
            if (existingKey == key) {
                return true;
            }
        }
        
        return false;
    }
};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int q;
    cin >> q;
    
    MyHashSet hashSet;
    
    for (int i = 0; i < q; i++) {
        string operation;
        cin >> operation;
        
        if (operation == "ADD") {
            int key;
            cin >> key;
            hashSet.add(key);
        }
        else if (operation == "REMOVE") {
            int key;
            cin >> key;
            hashSet.remove(key);
        }
        else if (operation == "CONTAINS") {
            int key;
            cin >> key;
            bool result = hashSet.contains(key);
            cout << (result ? "YES" : "NO") << "\n";
        }
    }
    
    return 0;
}
```

---

## Bài 16: Find All Duplicates (Array as Hash Map)

### Đề bài
Cho một mảng gồm n số nguyên trong khoảng [1, n]. Một số phần tử xuất hiện 1 lần, một số xuất hiện 2 lần. Tìm tất cả phần tử xuất hiện đúng 2 lần.

**Điều kiện:** Không được dùng extra space (không tính output)

**Input:**
- Dòng 1: n
- Dòng 2: n số nguyên

**Output:**
Các số xuất hiện đúng 2 lần (thứ tự bất kỳ)

### Phân tích thuật toán
- **Ý tưởng:** Dùng chính mảng như hash map, đánh dấu bằng dấu âm
- **Độ phức tạp:** O(n)
- **Không gian:** O(1) không tính output

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    
    vector<int> duplicates;
    
    for (int i = 0; i < n; i++) {
        // Lấy absolute value để tránh bị ảnh hưởng bởi dấu âm
        int index = abs(arr[i]) - 1; // Convert to 0-based index
        
        if (arr[index] < 0) {
            // Đã thấy số này rồi -> duplicate
            duplicates.push_back(abs(arr[i]));
        } else {
            // Lần đầu thấy -> đánh dấu bằng cách làm âm
            arr[index] = -arr[index];
        }
    }
    
    // In kết quả
    for (int i = 0; i < duplicates.size(); i++) {
        cout << duplicates[i];
        if (i < duplicates.size() - 1) cout << " ";
    }
    if (!duplicates.empty()) cout << "\n";
    
    return 0;
}
```

### Code Python
```python
n = int(input())
arr = list(map(int, input().split()))

duplicates = []

for i in range(n):
    # Lấy absolute value
    index = abs(arr[i]) - 1  # Convert to 0-based index
    
    if arr[index] < 0:
        # Đã thấy số này rồi -> duplicate
        duplicates.append(abs(arr[i]))
    else:
        # Lần đầu thấy -> đánh dấu bằng dấu âm
        arr[index] = -arr[index]

if duplicates:
    print(' '.join(map(str, duplicates)))
```

---

## Bài 17: Largest Rectangle in Histogram (Stack + Array)

### Đề bài
Cho một histogram với n thanh có chiều cao khác nhau. Tìm diện tích hình chữ nhật lớn nhất có thể tạo ra.

**Input:**
- Dòng 1: n
- Dòng 2: n số nguyên (chiều cao các thanh)

**Output:**
Diện tích lớn nhất

### Phân tích thuật toán
- **Ý tưởng:** Dùng stack để track các thanh theo thứ tự tăng dần
- **Độ phức tạp:** O(n)
- **Không gian:** O(n)

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    vector<long long> heights(n);
    for (int i = 0; i < n; i++) {
        cin >> heights[i];
    }
    
    stack<int> st; // Lưu index của các thanh
    long long maxArea = 0;
    
    for (int i = 0; i <= n; i++) {
        // Thêm thanh có chiều cao 0 ở cuối để force process tất cả thanh
        long long currentHeight = (i == n) ? 0 : heights[i];
        
        // Process các thanh có chiều cao >= currentHeight
        while (!st.empty() && heights[st.top()] >= currentHeight) {
            long long height = heights[st.top()];
            st.pop();
            
            // Tính width: từ thanh sau thanh trước đó đến thanh hiện tại
            long long width = st.empty() ? i : (i - st.top() - 1);
            
            // Cập nhật max area
            maxArea = max(maxArea, height * width);
        }
        
        // Thêm thanh hiện tại vào stack
        st.push(i);
    }
    
    cout << maxArea << "\n";
    
    return 0;
}
```

### Code Python
```python
n = int(input())
heights = list(map(int, input().split()))

stack = []  # Lưu index của các thanh
max_area = 0

# Thêm 0 ở cuối để force process tất cả thanh
heights.append(0)

for i in range(n + 1):
    current_height = heights[i]
    
    # Process các thanh có chiều cao >= current_height
    while stack and heights[stack[-1]] >= current_height:
        height = heights[stack.pop()]
        
        # Tính width
        width = i if not stack else (i - stack[-1] - 1)
        
        # Cập nhật max area
        max_area = max(max_area, height * width)
    
    # Thêm thanh hiện tại
    stack.append(i)

print(max_area)
```

---

## Bài 18: Design Twitter (Dict + List + Heap)

### Đề bài
Thiết kế một Twitter đơn giản với các chức năng:
- postTweet(userId, tweetId): User đăng tweet
- getNewsFeed(userId): Lấy 10 tweet mới nhất từ user và những người user follow
- follow(followerId, followeeId): Follow user khác
- unfollow(followerId, followeeId): Unfollow user

**Input:**
- Dòng 1: q (số operations)
- q dòng tiếp theo: operations

**Output:**
Với mỗi getNewsFeed, in ra list tweet IDs

### Code Python
```python
import heapq
from collections import defaultdict

class Twitter:
    def __init__(self):
        self.time = 0  # Global timestamp để track thứ tự tweet
        self.tweets = defaultdict(list)  # userId -> [(time, tweetId), ...]
        self.following = defaultdict(set)  # userId -> set of followeeIds
    
    def postTweet(self, userId, tweetId):
        """User đăng tweet mới"""
        self.tweets[userId].append((self.time, tweetId))
        self.time += 1
    
    def getNewsFeed(self, userId):
        """Lấy 10 tweet mới nhất từ user và người user follow"""
        # Tập hợp tất cả users cần lấy tweets (bao gồm chính user)
        users = self.following[userId] | {userId}
        
        # Dùng max heap để lấy 10 tweet mới nhất
        # Python heapq là min heap, nên dùng negative time
        heap = []
        
        for user in users:
            if user in self.tweets and self.tweets[user]:
                # Lấy tweet mới nhất của user này
                time, tweetId = self.tweets[user][-1]
                heapq.heappush(heap, (-time, tweetId, user, len(self.tweets[user]) - 1))
        
        result = []
        
        # Lấy tối đa 10 tweets
        while heap and len(result) < 10:
            neg_time, tweetId, user, tweet_index = heapq.heappop(heap)
            result.append(tweetId)
            
            # Nếu user này còn tweet cũ hơn, thêm vào heap
            if tweet_index > 0:
                time, tweetId = self.tweets[user][tweet_index - 1]
                heapq.heappush(heap, (-time, tweetId, user, tweet_index - 1))
        
        return result
    
    def follow(self, followerId, followeeId):
        """Follow user khác"""
        if followerId != followeeId:  # Không thể follow chính mình
            self.following[followerId].add(followeeId)
    
    def unfollow(self, followerId, followeeId):
        """Unfollow user"""
        self.following[followerId].discard(followeeId)

# Main program
q = int(input())
twitter = Twitter()

for _ in range(q):
    operation = input().split()
    
    if operation[0] == "POST":
        userId = int(operation[1])
        tweetId = int(operation[2])
        twitter.postTweet(userId, tweetId)
    
    elif operation[0] == "FEED":
        userId = int(operation[1])
        feed = twitter.getNewsFeed(userId)
        if feed:
            print(' '.join(map(str, feed)))
        else:
            print("EMPTY")
    
    elif operation[0] == "FOLLOW":
        followerId = int(operation[1])
        followeeId = int(operation[2])
        twitter.follow(followerId, followeeId)
    
    elif operation[0] == "UNFOLLOW":
        followerId = int(operation[1])
        followeeId = int(operation[2])
        twitter.unfollow(followerId, followeeId)
```

---

## Bài 19: Longest Substring Without Repeating Characters (Sliding Window + Set)

### Đề bài
Cho một chuỗi s, tìm độ dài của substring dài nhất không có ký tự lặp lại.

**Input:**
- Một chuỗi s

**Output:**
Độ dài substring dài nhất

### Phân tích thuật toán
- **Ý tưởng:** Sliding window với set để track characters
- **Độ phức tạp:** O(n)
- **Không gian:** O(min(m, n)) với m là kích thước alphabet

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    string s;
    cin >> s;
    
    int n = s.length();
    unordered_set<char> window; // Characters trong window hiện tại
    
    int left = 0; // Left pointer của sliding window
    int maxLen = 0;
    
    // Right pointer duyệt qua string
    for (int right = 0; right < n; right++) {
        char rightChar = s[right];
        
        // Nếu character đã tồn tại trong window
        // Thu hẹp window từ bên trái cho đến khi loại bỏ duplicate
        while (window.count(rightChar)) {
            window.erase(s[left]);
            left++;
        }
        
        // Thêm character mới vào window
        window.insert(rightChar);
        
        // Cập nhật max length
        maxLen = max(maxLen, right - left + 1);
    }
    
    cout << maxLen << "\n";
    
    return 0;
}
```

### Code Python
```python
s = input().strip()

window = set()  # Characters trong window hiện tại
left = 0  # Left pointer
max_len = 0

# Right pointer duyệt string
for right in range(len(s)):
    right_char = s[right]
    
    # Thu hẹp window từ trái đến khi loại bỏ duplicate
    while right_char in window:
        window.remove(s[left])
        left += 1
    
    # Thêm character mới
    window.add(right_char)
    
    # Cập nhật max length
    max_len = max(max_len, right - left + 1)

print(max_len)
```

---

## Bài 20: Design HashMap (Array + Linked List)

### Đề bài
Implement HashMap đơn giản với operations:
- put(key, value): Thêm/update key-value pair
- get(key): Lấy value của key, return -1 nếu không tồn tại
- remove(key): Xóa key khỏi map

### Code Python
```python
class ListNode:
    """Node cho linked list để handle collision"""
    def __init__(self, key=-1, value=-1):
        self.key = key
        self.value = value
        self.next = None

class MyHashMap:
    def __init__(self):
        """Initialize với 1000 buckets"""
        self.size = 1000
        self.buckets = [ListNode() for _ in range(self.size)]  # Dummy heads
    
    def _hash(self, key):
        """Hash function đơn giản"""
        return key % self.size
    
    def put(self, key, value):
        """Thêm hoặc update key-value pair"""
        bucket_index = self._hash(key)
        head = self.buckets[bucket_index]
        
        # Tìm key trong linked list
        current = head
        while current.next:
            if current.next.key == key:
                # Key đã tồn tại -> update value
                current.next.value = value
                return
            current = current.next
        
        # Key không tồn tại -> thêm node mới
        new_node = ListNode(key, value)
        current.next = new_node
    
    def get(self, key):
        """Lấy value của key"""
        bucket_index = self._hash(key)
        head = self.buckets[bucket_index]
        
        # Tìm key trong linked list
        current = head.next  # Skip dummy head
        while current:
            if current.key == key:
                return current.value
            current = current.next
        
        return -1  # Key không tồn tại
    
    def remove(self, key):
        """Xóa key khỏi map"""
        bucket_index = self._hash(key)
        head = self.buckets[bucket_index]
        
        # Tìm và xóa key
        current = head
        while current.next:
            if current.next.key == key:
                # Xóa node
                current.next = current.next.next
                return
            current = current.next

# Main program
q = int(input())
hash_map = MyHashMap()

for _ in range(q):
    operation = input().split()
    
    if operation[0] == "PUT":
        key = int(operation[1])
        value = int(operation[2])
        hash_map.put(key, value)
    
    elif operation[0] == "GET":
        key = int(operation[1])
        result = hash_map.get(key)
        print(result)
    
    elif operation[0] == "REMOVE":
        key = int(operation[1])
        hash_map.remove(key)
```

---

## Pattern Recognition và Tips nâng cao

### 🎯 Khi nào dùng cấu trúc dữ liệu nào?

**List/Array:**
- Cần access random: `arr[i]`
- Iterate tuần tự
- Space-efficient
- Cache-friendly

**Set:**
- Loại bỏ duplicates
- Check membership: `x in set`
- Set operations: union, intersection
- Maintain unique elements

**Dict/Map:**
- Key-value mapping
- Counting frequency
- Memoization/caching  
- Group by key

**Stack:**
- LIFO operations
- Parsing expressions
- Undo functionality
- Function call stack

**Queue/Deque:**
- FIFO operations
- BFS traversal
- Sliding window problems
- Level order processing

**Heap:**
- Priority queue
- Top K problems
- Merge K sorted
- Real-time median

### 🚀 Advanced Patterns

1. **Two Pointers:** Sorted array, palindrome check
2. **Sliding Window:** Substring/subarray với constraints
3. **Prefix Sum:** Range queries
4. **Hash + Prefix:** Subarray sum problems
5. **Monotonic Stack:** Next/previous greater element
6. **Union Find:** Connected components
7. **Trie:** String prefix problems

### ⚡ Optimization Tips

1. **Memory:** Use primitive types when possible
2. **Time:** Choose right data structure for access pattern
3. **Cache:** Locality of reference matters
4. **Space-Time:** Sometimes trade memory for speed
5. **Early termination:** Break loops when possible
6. **Batch operations:** Reduce function call overhead
7. **Avoid string concatenation:** Use StringBuilder/list join# Bài tập Lập trình Thi đấu - List, Set, Dict, Tuple

## Bài 1: Đếm tần suất xuất hiện (Dict/Map)

### Đề bài
Cho một dãy số nguyên gồm n phần tử. Hãy đếm số lần xuất hiện của mỗi phần tử và in ra theo thứ tự tăng dần của giá trị.

**Input:**
- Dòng 1: n (1 ≤ n ≤ 10^5)
- Dòng 2: n số nguyên (-10^9 ≤ ai ≤ 10^9)

**Output:**
In ra mỗi dòng một cặp (giá trị, số lần xuất hiện)

### Phân tích thuật toán
- **Ý tưởng:** Sử dụng dict/map để đếm tần suất
- **Độ phức tạp:** O(n log n) - do phải sắp xếp keys
- **Không gian:** O(n) - lưu trữ tối đa n phần tử khác nhau

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    map<int, int> freq;
    
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        freq[x]++;
    }
    
    for (auto& p : freq) {
        cout << p.first << " " << p.second << "\n";
    }
    
    return 0;
}
```

### Code Python
```python
n = int(input())
arr = list(map(int, input().split()))

freq = {}
for x in arr:
    freq[x] = freq.get(x, 0) + 1

# In theo thứ tự tăng dần
for key in sorted(freq.keys()):
    print(key, freq[key])
```

---

## Bài 2: Tìm phần tử duy nhất (Set)

### Đề bài
Cho một dãy số trong đó mỗi số xuất hiện đúng 2 lần, chỉ có 1 số xuất hiện đúng 1 lần. Tìm số đó.

**Input:**
- Dòng 1: n (1 ≤ n ≤ 10^5, n lẻ)
- Dòng 2: n số nguyên

**Output:**
Số xuất hiện đúng 1 lần

### Phân tích thuật toán
- **Ý tưởng 1:** Dùng set để track các phần tử đã thấy
- **Ý tưởng 2:** Dùng XOR (tối ưu hơn)
- **Độ phức tạp:** O(n)
- **Không gian:** O(n) với set, O(1) với XOR

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    // Cách 1: Dùng set
    set<int> seen;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        if (seen.count(x)) {
            seen.erase(x);
        } else {
            seen.insert(x);
        }
    }
    cout << *seen.begin() << "\n";
    
    // Cách 2: Dùng XOR (uncomment để dùng)
    /*
    int result = 0;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        result ^= x;
    }
    cout << result << "\n";
    */
    
    return 0;
}
```

### Code Python
```python
n = int(input())
arr = list(map(int, input().split()))

# Cách 1: Dùng set
seen = set()
for x in arr:
    if x in seen:
        seen.remove(x)
    else:
        seen.add(x)

print(list(seen)[0])

# Cách 2: Dùng XOR
# result = 0
# for x in arr:
#     result ^= x
# print(result)
```

---

## Bài 3: Tìm cặp số có tổng bằng K (List + Set)

### Đề bài
Cho một dãy số và một số K. Tìm xem có tồn tại cặp số nào trong dãy có tổng bằng K không?

**Input:**
- Dòng 1: n, k
- Dòng 2: n số nguyên

**Output:**
"YES" nếu tồn tại, "NO" nếu không

### Phân tích thuật toán
- **Ý tưởng:** Với mỗi phần tử x, kiểm tra xem (k-x) có trong set không
- **Độ phức tạp:** O(n)
- **Không gian:** O(n)

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n, k;
    cin >> n >> k;
    
    vector<int> arr(n);
    unordered_set<int> seen;
    
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        if (seen.count(k - arr[i])) {
            cout << "YES\n";
            return 0;
        }
        seen.insert(arr[i]);
    }
    
    cout << "NO\n";
    return 0;
}
```

### Code Python
```python
n, k = map(int, input().split())
arr = list(map(int, input().split()))

seen = set()
for x in arr:
    if (k - x) in seen:
        print("YES")
        exit()
    seen.add(x)

print("NO")
```

---

## Bài 4: Quản lý điểm sinh viên (Dict + Tuple)

### Đề bài
Cho thông tin điểm của sinh viên ở nhiều môn. Hãy tính điểm trung bình của mỗi sinh viên.

**Input:**
- Dòng 1: n (số bản ghi)
- n dòng tiếp theo: tên_sv môn_học điểm

**Output:**
In ra tên sinh viên và điểm TB (làm tròn 2 chữ số)

### Phân tích thuật toán
- **Ý tưởng:** Dùng dict lưu tuple (tổng điểm, số môn) cho mỗi sinh viên
- **Độ phức tạp:** O(n)
- **Không gian:** O(số sinh viên)

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    map<string, pair<double, int>> students; // {tổng điểm, số môn}
    
    for (int i = 0; i < n; i++) {
        string name, subject;
        double score;
        cin >> name >> subject >> score;
        
        students[name].first += score;
        students[name].second++;
    }
    
    cout << fixed << setprecision(2);
    for (auto& p : students) {
        double avg = p.second.first / p.second.second;
        cout << p.first << " " << avg << "\n";
    }
    
    return 0;
}
```

### Code Python
```python
n = int(input())

students = {}  # {tên: (tổng_điểm, số_môn)}

for _ in range(n):
    parts = input().split()
    name = parts[0]
    subject = parts[1]
    score = float(parts[2])
    
    if name not in students:
        students[name] = (0.0, 0)
    
    total, count = students[name]
    students[name] = (total + score, count + 1)

# In kết quả
for name in sorted(students.keys()):
    total, count = students[name]
    avg = total / count
    print(f"{name} {avg:.2f}")
```

---

## Bài 5: Tìm dãy con có tổng bằng 0 (List + Dict)

### Đề bài
Cho một dãy số nguyên. Tìm xem có tồn tại dãy con liên tiếp nào có tổng bằng 0 không?

**Input:**
- Dòng 1: n
- Dòng 2: n số nguyên

**Output:**
"YES" và in ra vị trí bắt đầu, kết thúc (1-indexed) nếu có
"NO" nếu không có

### Phân tích thuật toán
- **Ý tưởng:** Dùng prefix sum. Nếu prefix[i] = prefix[j] thì sum(i+1...j) = 0
- **Độ phức tạp:** O(n)
- **Không gian:** O(n)

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int n;
    cin >> n;
    
    vector<int> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    
    unordered_map<long long, int> prefixPos;
    prefixPos[0] = 0; // prefix sum = 0 tại vị trí 0
    
    long long prefixSum = 0;
    
    for (int i = 0; i < n; i++) {
        prefixSum += arr[i];
        
        if (prefixPos.count(prefixSum)) {
            cout << "YES\n";
            cout << prefixPos[prefixSum] + 1 << " " << i + 1 << "\n";
            return 0;
        }
        
        prefixPos[prefixSum] = i + 1;
    }
    
    cout << "NO\n";
    return 0;
}
```

### Code Python
```python
n = int(input())
arr = list(map(int, input().split()))

prefix_pos = {0: 0}  # prefix_sum: position
prefix_sum = 0

for i in range(n):
    prefix_sum += arr[i]
    
    if prefix_sum in prefix_pos:
        print("YES")
        print(prefix_pos[prefix_sum] + 1, i + 1)
        exit()
    
    prefix_pos[prefix_sum] = i + 1

print("NO")
```

---

## Bài 6: Hợp và giao của nhiều tập hợp (Set Operations)

### Đề bài
Cho k tập hợp số nguyên. Tìm giao và hợp của tất cả các tập hợp.

**Input:**
- Dòng 1: k (số tập hợp)
- k nhóm tiếp theo, mỗi nhóm:
  - Dòng đầu: ni (số phần tử tập i)
  - Dòng sau: ni số nguyên

**Output:**
- Dòng 1: Các phần tử trong giao (tăng dần)
- Dòng 2: Các phần tử trong hợp (tăng dần)

### Phân tích thuật toán
- **Ý tưởng:** Dùng set intersection và union
- **Độ phức tạp:** O(∑ni × log(max_elements))
- **Không gian:** O(∑ni)

### Code C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int k;
    cin >> k;
    
    vector<set<int>> sets(k);
    set<int> unionSet, intersectionSet;
    
    for (int i = 0; i < k; i++) {
        int n;
        cin >> n;
        for (int j = 0; j < n; j++) {
            int x;
            cin >> x;
            sets[i].insert(x);
            unionSet.insert(x);
        }
    }
    
    // Tìm giao - bắt đầu với tập đầu tiên
    if (k > 0) {
        intersectionSet = sets[0];
        for (int i = 1; i < k; i++) {
            set<int> temp;
            set_intersection(intersectionSet.begin(), intersectionSet.end(),
                           sets[i].begin(), sets[i].end(),
                           inserter(temp, temp.begin()));
            intersectionSet = temp;
        }
    }
    
    // In giao
    if (intersectionSet.empty()) {
        cout << "EMPTY\n";
    } else {
        for (int x : intersectionSet) {
            cout << x << " ";
        }
        cout << "\n";
    }
    
    // In hợp
    for (int x : unionSet) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

### Code Python
```python
k = int(input())

sets_list = []
union_set = set()

for i in range(k):
    n = int(input())
    current_set = set(map(int, input().split()))
    sets_list.append(current_set)
    union_set.update(current_set)

# Tìm giao
if k > 0:
    intersection_set = sets_list[0]
    for i in range(1, k):
        intersection_set = intersection_set.intersection(sets_list[i])
else:
    intersection_set = set()

# In giao
if not intersection_set:
    print("EMPTY")
else:
    print(" ".join(map(str, sorted(intersection_set))))

# In hợp
print(" ".join(map(str, sorted(union_set))))
```

---

## Lưu ý khi sử dụng các cấu trúc dữ liệu

### List/Vector
- **Ưu điểm:** Truy cập random O(1), cache-friendly
- **Nhược điểm:** Insert/delete giữa dãy O(n)
- **Khi nào dùng:** Cần truy cập theo index, dữ liệu có thứ tự

### Set
- **Ưu điểm:** Insert/delete/search O(log n), tự động sắp xếp, không trùng lặp
- **Nhược điểm:** Không truy cập random
- **Khi nào dùng:** Cần duy trì tính duy nhất, tìm kiếm nhanh

### Dict/Map
- **Ưu điểm:** Key-value mapping, search O(1) average cho unordered
- **Nhược điểm:** Overhead bộ nhớ
- **Khi nào dùng:** Cần mapping, đếm tần suất, memoization

### Tuple
- **Ưu điểm:** Immutable, có thể làm key của dict, nhóm dữ liệu related
- **Nhược điểm:** Không thể thay đổi
- **Khi nào dùng:** Trả về nhiều giá trị, coordinate, immutable data

## Tips tối ưu
1. Dùng `unordered_set`/`unordered_map` khi không cần thứ tự
2. Reserve memory cho vector khi biết trước kích thước
3. Dùng reference trong range-based for loop
4. Cân nhắc trade-off giữa thời gian và bộ nhớ
5. Test với dữ liệu edge case (empty, single element, max size)