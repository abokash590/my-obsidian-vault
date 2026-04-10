### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `vector<tuple<int, int, int>> vt;` | Declares an empty vector of tuples | $O(1)$ |
| `vector<tuple<int, int, int>> vt(n, {0, 0, 0});` | Initializes $n$ tuples with value $\{0, 0, 0\}$ | $O(n)$ |
| `vt.push_back(make_tuple(1, 2, 3));` | Adds a tuple using `make_tuple` | $O(1)$ |
| `vt.push_back({1, 2, 3});` | Adds a tuple using initializer list | $O(1)$ |
### 2. Access & Traversal
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `get<0>(vt[i])` | Accesses the 1st element of the $i$-th tuple | $O(1)$ |
| `get<1>(vt[i])` | Accesses the 2nd element of the $i$-th tuple | $O(1)$ |
| `for(auto &[a, b, c] : vt)` | Structured binding to access all elements (C++17) | $O(n)$ |
### 3. Input & Output
| Task | Syntax | Time Complexity |
| :--- | :--- | :--- |
| **Input** | `for(auto &[a, b, c] : vt) cin >> a >> b >> c;` | $O(n)$ |
| **Output** | `for(auto [a, b, c] : vt) cout << a << ' ' << b << ' ' << c << endl;` | $O(n)$ |
### 4. Push, Pop & Insert
| Syntax                      | Description                                        | Time Complexity |
| :-------------------------- | :------------------------------------------------- | :-------------- |
| `vt.emplace_back(a, b, c);` | Directly constructs and adds a tuple (Recommended) | $O(1)$          |
| `vt.pop_back();`            | Removes the last tuple                             | $O(1)$          |
| `vt.erase(vt.begin() + i);` | Deletes tuple at $i$-th index                      | $O(n)$          |
### 5. Sort, Reverse & Custom Comparator
| Operation | Syntax | Time Complexity |
| :--- | :--- | :--- |
| **Default Sort** | `sort(vt.begin(), vt.end());` | Sorts by 1st, then 2nd, then 3rd element | $O(n \log n)$ |
| **Reverse** | `reverse(vt.begin(), vt.end());` | Reverses the vector order | $O(n)$ |
| **Custom Sort** | `sort(vt.begin(), vt.end(), cmp);` | Sorts based on a specific element/logic | $O(n \log n)$ |
১. **Custom Comparator (সরাসরি ৩য় ভ্যালু অনুযায়ী সর্ট করতে):** যদি আপনার ৩য় এলিমেন্টের ওপর ভিত্তি করে সর্ট করার প্রয়োজন হয়, তবে এই ফাংশনটি ব্যবহার করুন:
```cpp
bool cmp(const tuple<int, int, int>& a, const tuple<int, int, int>& b) {
    return get<2>(a) < get<2>(b); // 3rd element অনুযায়ী ছোট থেকে বড়
}
```
