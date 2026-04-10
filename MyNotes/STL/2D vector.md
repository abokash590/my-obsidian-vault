### ☑ 2D Vector: Declaration & Operations
| Syntax                                           | Description                                            | Time Complexity |
| :----------------------------------------------- | :----------------------------------------------------- | :-------------- |
| `vector<vector<int>> v;`                         | Declares an empty 2D vector                            | $O(1)$          |
| `vector<vector<int>> v(n, vector<int>(m));`      | Declares an $n \times m$ matrix (initialized to 0)     | $O(n \times m)$ |
| `vector<vector<int>> v(n, vector<int>(m, val));` | Declares an $n \times m$ matrix initialized with `val` | $O(n \times m)$ |
| `v[i][j]`                                        | Accessing element at $i$-th row and $j$-th column      | $O(1)$          |
| `v.size()`                                       | Returns the number of rows                             | $O(1)$          |
| `v[i].size()`                                    | Returns the number of columns in the $i$-th row        | $O(1)$          |
| `v.push_back(vector<int>(m, val));`              | Adds a new row of size $m$ at the end                  | $O(m)$          |
### **☑ 2D Vector: Practical Operations**

| Operation | Syntax | Time Complexity |
| :--- | :--- | :--- |
| **Row Sort** | `sort(v[i].begin(), v[i].end());` | $O(m \log m)$ |
| **Entire Sort** | `sort(v.begin(), v.end());` | $O(n \log n \times m)$ |
| **Add Element** | `v[i].push_back(val);` | $O(1)$ (Amortized) |
| **Iterate** | Nested for loops (row and then column) | $O(n \times m)$ |
