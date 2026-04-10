### Vector of Pairs: Master Table
| Syntax                                  | Description                                    | Time Complexity |
| :-------------------------------------- | :--------------------------------------------- | :-------------- |
| `vector<pair<int, int>> vp;`            | Declares an empty vector of pairs              | $O(1)$          |
| `vector<pair<int, int>> vp(n, {a, b});` | Initializes $n$ pairs with value $\{a, b\}$    | $O(n)$          |
| `vp.push_back({a, b});`                 | Adds a pair $\{a, b\}$ to the end              | $O(1)$          |
| `vp.emplace_back(a, b);`                | Directly constructs and adds a pair (Faster)   | $O(1)$          |
| `vp[i].first`                           | Accesses the first element of the $i$-th pair  | $O(1)$          |
| `vp[i].second`                          | Accesses the second element of the $i$-th pair | $O(1)$          |
| `sort(vp.begin(), vp.end());`           | Sorts by `first` (asc), then by `second` (asc) | $O(n \log n)$   |
| `sort(vp.rbegin(), vp.rend());`         | Sorts in descending order                      | $O(n \log n)$   |
| `for(auto &[a, b] : vp)`                | Structured binding to access elements (C++17)  | $O(n)$          |
| `vp.pop_back();`                        | Removes the last pair                          | $O(1)$          |
| `vp.clear();`                           | Removes all pairs from the vector              | $O(n)$          |
| `*max_element(vp.begin(), vp.end());`   | Finds the lexicographically largest pair       | $O(n)$          |
|                                         |                                                |                 |

### ১. **Custom Comparator (সরাসরি ফাংশন):** 
যদি ল্যাম্বডা ফাংশন কঠিন লাগে, তবে কোডের শুরুতে এটি লিখে রাখতে পারেন:
```cpp
bool cmp(pair<int, int> a, pair<int, int> b) {
    if(a.second == b.second) return a.first > b.first; // second সমান হলে first বড় থেকে ছোট
    return a.second < b.second; // অন্যথায় second ছোট থেকে বড়
}
```
