### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `map<key_type, value_type> mp;` | Declares an empty map (Sorted by key) | $O(1)$ |
| `map<int, int> mp = {{1, 10}, {2, 20}};` | Initializes map with specified key-value pairs | $O(N \log N)$ |
| `unordered_map<int, int> ump;` | Hash table based map (Unsorted) | $O(1)$ Average |
### 2. Input & Output (Insertion & Traversal)
| Task | Syntax | Time Complexity |
| :--- | :--- | :--- |
| **Insert/Update** | `mp[key] = value;` | $O(\log N)$ |
| **Insert (Explicit)** | `mp.insert({key, value});` | $O(\log N)$ |
| **Traversal (C++17)** | `for(auto &[k, v] : mp) cout << k << " " << v;` | $O(N)$ |
| **Traversal (Iterator)** | `for(auto it = mp.begin(); it != mp.end(); ++it)` | $O(N)$ |
### 3. Access & Traversal (Searching)
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `mp[key]` | Accesses value (Creates key if not exists) | $O(\log N)$ |
| `mp.at(key)` | Accesses value (Throws error if key not exists) | $O(\log N)$ |
| `mp.count(key)` | Returns 1 if key exists, 0 otherwise | $O(\log N)$ |
| `mp.find(key)` | Returns iterator to the element (or `mp.end()`) | $O(\log N)$ |
### 4. Push, Pop & Insert (Modification)
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `mp.emplace(key, value);` | Inserts if key doesn't exist (Efficient) | $O(\log N)$ |
| `mp.erase(key);` | Removes the element with specified key | $O(\log N)$ |
| `mp.erase(iterator);` | Removes element at specific position | $O(1)$ Amortized |
| `mp.clear();` | Removes all elements from the map | $O(N)$ |
### 5. Sort, Reverse & Custom Comparator
| Operation | Syntax | Description |
| :--- | :--- | :--- |
| **Default Sort** | (Automatic) | Map always keeps keys in ascending order |
| **Descending Sort** | `map<int, int, greater<int>> mp;` | Keys will be stored in descending order |
| **Lower Bound** | `mp.lower_bound(key);` | First element $\ge$ key |
| **Upper Bound** | `mp.upper_bound(key);` | First element $>$ key |

### 💡 Important Patterns & Tips (My Recommendations)

১. **`mp[key]` বনাম `mp.find()`:** যদি আপনি শুধু চেক করতে চান যে কোনো কী (key) ম্যাপে আছে কি না, তবে `mp[key]` ব্যবহার করবেন না। কারণ এটি যদি কী-টি খুঁজে না পায়, তবে মেমরিতে ওই কী-র জন্য একটি ডিফল্ট ভ্যালু (যেমন ০) তৈরি করে ফেলে। চেক করার জন্য সবসময় `mp.count(key)` বা `mp.find() != mp.end()` ব্যবহার করা নিরাপদ।

২. **Frequency Count Pattern (খুবই কমন):**

অ্যারেতে কোনো এলিমেন্ট কতবার আছে তা বের করতে এটি সবচেয়ে সহজ উপায়:

C++

```cpp
for(int x : v) mp[x]++;
```

৩. **Unordered Map এর সতর্কতা:**

`unordered_map` সাধারণত $O(1)$ এ কাজ করে, কিন্তু কিছু বিশেষ টেস্ট কেসে (Worst Case) এটি $O(N)$ হয়ে যেতে পারে এবং TLE (Time Limit Exceeded) দিতে পারে। সেক্ষেত্রে সেফ থাকার জন্য `map` বা কাস্টম হ্যাশ ব্যবহার করা ভালো।

৪. **Map of Vectors:**

মাঝে মাঝে একটি কী-র বিপরীতে অনেকগুলো ভ্যালু রাখার প্রয়োজন হয়:

C++

```cpp
map<int, vector<int>> adj;
adj[1].push_back(10);
```