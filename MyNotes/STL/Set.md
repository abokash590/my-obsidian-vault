### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `set<int> s;` | Declares an empty set (Sorted ascending) | $O(1)$ |
| `set<int> s = {1, 5, 2};` | Initializes set with values (Removes duplicates) | $O(N \log N)$ |
| `set<int, greater<int>> s;` | Declares a set that stores values in descending order | $O(1)$ |
| `unordered_set<int> us;` | Hash-based set (Unsorted, unique elements) | $O(1)$ Average |
| `multiset<int> ms;` | Stores multiple instances of the same value (Sorted) | $O(1)$ |
### 2. Input & Output (Insertion & Traversal)
| Task | Syntax | Time Complexity |
| :--- | :--- | :--- |
| **Insert** | `s.insert(x);` | $O(\log N)$ |
| **Traversal** | `for(auto x : s) cout << x << " ";` | $O(N)$ |
| **Iterator Traversal**| `for(auto it = s.begin(); it != s.end(); ++it)` | $O(N)$ |
| **Check Existence** | `if(s.count(x))` or `if(s.find(x) != s.end())` | $O(\log N)$ |
### 3. Access & Search (Properties)
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `s.size()` | Returns the number of unique elements | $O(1)$ |
| `s.empty()` | Returns true if the set is empty | $O(1)$ |
| `*s.begin()` | Accesses the smallest element | $O(1)$ |
| `*s.rbegin()` | Accesses the largest element | $O(1)$ |
### 4. Push, Pop & Insert (Modification)
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `s.emplace(x);` | Inserts element directly (Faster than insert) | $O(\log N)$ |
| `s.erase(x);` | Removes the specific value $x$ | $O(\log N)$ |
| `s.erase(it);` | Removes element at the iterator's position | $O(1)$ Amortized |
| `s.clear();` | Removes all elements from the set | $O(N)$ |
### 5. Sorting & Range Search
| Operation       | Syntax              | Time Complexity                           |             |
| :-------------- | :------------------ | :---------------------------------------- | ----------- |
| **Lower Bound** | `s.lower_bound(x);` | Returns iterator to first element $\ge x$ | $O(\log N)$ |
| **Upper Bound** | `s.upper_bound(x);` | Returns iterator to first element $> x$   | $O(\log N)$ |
| **Equal Range** | `s.equal_range(x);` | Returns pair of lower and upper bound     | $O(\log N)$ |


---

### **💡 Important Patterns & Tips (My Recommendations)**

১. **Set vs Multiset Erase:**

`multiset`-এ যদি আপনি সরাসরি ভ্যালু দিয়ে `ms.erase(x)` করেন, তবে ওই ভ্যালুর **সবগুলো** কপি ডিলিট হয়ে যাবে। যদি শুধু একটি কপি ডিলিট করতে চান, তবে ইটারেটর ব্যবহার করুন:

```cpp
ms.erase(ms.find(x)); // শুধুমাত্র একটি 'x' ডিলিট হবে
```

২. **Binary Search in Set:**

গ্লোবাল `binary_search(s.begin(), s.end(), x)` কখনো সেটে ব্যবহার করবেন না, কারণ এটি $O(N)$ এ কাজ করে। সবসময় সেটের মেম্বার ফাংশন `s.find(x)` বা `s.lower_bound(x)` ব্যবহার করবেন যা **$O(\log N)$** এ কাজ করে।

৩. **Unique Elements from Vector:**

আপনার আগের নোটের সেই ট্রিকটি এখানেও মনে করিয়ে দিচ্ছি, ভেক্টর থেকে ইউনিক এলিমেন্ট পেতে এটি সবচেয়ে ফাস্ট:

```cpp
set<int> s(v.begin(), v.end());
```

৪. **Finding Successor and Predecessor:**

সেটে কোনো এলিমেন্টের ঠিক আগের বা পরের এলিমেন্টটি খুঁজে পাওয়া খুব সহজ:

```cpp
auto it = s.find(x);
if(it != s.begin()) auto prev_val = *(--it); // Predecessor
if(next(it) != s.end()) auto next_val = *(++it); // Successor
```
[[Set Vs Vector]]
[[Policy Based Data Structure (PBDS)]]
