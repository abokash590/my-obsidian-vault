### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `deque<int> dq;` | Declares an empty deque | $O(1)$ |
| `deque<int> dq(n, val);` | Initializes deque with $n$ elements of value `val` | $O(n)$ |
| `deque<int> dq = {1, 2, 3};` | Initializes with initializer list | $O(n)$ |
| `deque<int> dq2(dq1);` | Creates a copy of another deque | $O(n)$ |
### 2. Core Operations (Push & Pop)
| Task | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Push Back** | `dq.push_back(x);` | Adds element at the end | $O(1)$ |
| **Push Front** | `dq.push_front(x);` | Adds element at the beginning | $O(1)$ |
| **Pop Back** | `dq.pop_back();` | Removes the last element | $O(1)$ |
| **Pop Front** | `dq.pop_front();` | Removes the first element | $O(1)$ |

### 3. Access & Capacity
| Task | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Random Access** | `dq[i]` or `dq.at(i)` | Accesses the $i$-th element | $O(1)$ |
| **Front Access** | `dq.front();` | Returns the first element | $O(1)$ |
| **Back Access** | `dq.back();` | Returns the last element | $O(1)$ |
| **Size** | `dq.size();` | Returns the number of elements | $O(1)$ |
| **Clear** | `dq.clear();` | Removes all elements | $O(n)$ |
### 4. Advanced Operations
| Operation  | Syntax              | Description                          | Time Complexity |
| :--------- | :------------------ | :----------------------------------- | :-------------- |
| **Insert** | `dq.insert(it, x);` | Inserts $x$ at iterator position     | $O(n)$          |
| **Erase**  | `dq.erase(it);`     | Deletes element at iterator position | $O(n)$          |
| **Resize** | `dq.resize(n);`     | Changes the size to $n$              | $O(n)$          |
### 5. Standard STL Algorithms on Deque
| Algorithm | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Sort** | `sort(dq.begin(), dq.end());` | Sorts elements in ascending order | $O(N \log N)$ |
| **Reverse** | `reverse(dq.begin(), dq.end());` | Reverses the entire deque | $O(N)$ |
| **Random Shuffle**| `shuffle(dq.begin(), dq.end(), rng);`| Shuffles the elements randomly | $O(N)$ |
| **Binary Search** | `binary_search(dq.begin(), dq.end(), x);`| Returns true if $x$ is present (must be sorted) | $O(\log N)$ |
| **Lower Bound** | `lower_bound(dq.begin(), dq.end(), x);`| Returns iterator to first element $\ge x$ | $O(\log N)$ |
| **Upper Bound** | `upper_bound(dq.begin(), dq.end(), x);`| Returns iterator to first element $> x$ | $O(\log N)$ |
### 6. Non-Modifying Operations
| Operation | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Find** | `find(dq.begin(), dq.end(), x);` | Returns iterator to first occurrence of $x$ | $O(N)$ |
| **Count** | `count(dq.begin(), dq.end(), x);` | Counts occurrences of value $x$ | $O(N)$ |
| **Min/Max** | `*max_element(dq.begin(), dq.end());` | Finds the maximum value in deque | $O(N)$ |
| **Accumulate** | `accumulate(dq.begin(), dq.end(), 0);` | Sum of all elements (requires `<numeric>`) | $O(N)$ |
### 7. Modifying Operations
| Operation | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Fill** | `fill(dq.begin(), dq.end(), val);` | Fills entire deque with `val` | $O(N)$ |
| **Rotate** | `rotate(dq.begin(), dq.begin() + k, dq.end());` | Rotates elements left by $k$ positions | $O(N)$ |
| **Replace** | `replace(dq.begin(), dq.end(), old, new);` | Replaces all `old` values with `new` | $O(N)$ |
| **Unique** | `dq.erase(unique(all(dq)), dq.end());` | Removes consecutive duplicates (needs sorting) | $O(N)$ |

---

### **💡 Important Patterns & Tips (CP-এর জন্য)**

১. **Vector vs Deque:**

Vector-এর শুরুতে (front) ডেটা ইনসার্ট করতে $O(n)$ সময় লাগে, কিন্তু Deque-এ মাত্র **$O(1)$** সময় লাগে। যদি আপনার এমন কোনো প্রবলেম থাকে যেখানে সামনে এবং পিছনে—দুই দিক থেকেই বারবার ডেটা যোগ বা ডিলিট করতে হয়, তবে Deque-ই সেরা।

২. **Sliding Window Maximum:**

কম্পিটিটিভ প্রোগ্রামিংয়ে "Monotonic Deque" প্যাটার্নটি খুব জনপ্রিয়। এটি দিয়ে একটি স্লাইডিং উইন্ডোর ভেতর ম্যাক্সিমাম বা মিনিমাম ভ্যালু লিনিয়ার টাইমে ($O(N)$) বের করা যায়।

৩. **Random Access:**

Stack বা Queue-এর মতো নয়, Deque-এ আপনি Vector-এর মতো করেই `dq[i]` দিয়ে যেকোনো ইনডেক্সের ডেটা সরাসরি অ্যাক্সেস করতে পারবেন।

৪. **Memory Management:**

Deque ভেক্টরের মতো মেমোরিতে একটি সিঙ্গেল কন্টিনিউয়াস ব্লক ব্যবহার করে না, বরং ছোট ছোট চাঙ্কে (Chunks) মেমোরি রাখে। তাই বিশাল ডেটার ক্ষেত্রে এটি ভেক্টরের চেয়ে সামান্য স্লো হতে পারে, তবে ফ্লেক্সিবিলিটির দিক থেকে এটি অপ্রতিদ্বন্দ্বী।