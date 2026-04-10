### ☑ 1. Declaration & Initialization

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`vector<int> v;`|Empty vector|$O(1)$|
|`vector<int> v(n);`|Size $n$, default initialized (0)|$O(n)$|
|`vector<int> v(n, val);`|Size $n$, each element = `val`|$O(n)$|
|`vector<int> v{1, 2, 3};`|Initializer list|$O(n)$|
|`vector<int> v2(v1);`|Copy constructor|$O(n)$|
|`v = v2;`|Assignment (copying all elements)|$O(n)$|
### ☑ 2. Insertion

| **Syntax**                            | **Description**                           | **Time Complexity** |
| ------------------------------------- | ----------------------------------------- | ------------------- |
| `v.push_back(x);`                     | Insert at the end                         | $O(1)$ (Amortized)  |
| `v.insert(it, x);`                    | Insert `x` at iterator position `it`      | $O(n)$              |
| `v.insert(it, n, x);`                 | Insert `n` copies of `x` at position `it` | $O(n)$              |
| `v.insert(it, v2.begin(), v2.end());` | Insert a range of elements                | $O(n)$              |

### ☑ 3. Deletion

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`v.pop_back();`|Remove last element|$O(1)$|
|`v.erase(it);`|Remove element at position `it`|$O(n)$|
|`v.erase(it1, it2);`|Remove a range of elements|$O(n)$|
|`v.clear();`|Remove all elements|$O(n)$|
### ☑ 4. Element Access

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`v[i]` / `v.at(i)`|Access $i$-th element|$O(1)$|
|`v.front();`|Access first element|$O(1)$|
|`v.back();`|Access last element|$O(1)$|
|`v.data();`|Raw pointer to the underlying array|$O(1)$|

### ☑ 5. Capacity

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`v.size();`|Number of elements|$O(1)$|
|`v.empty();`|Check if vector is empty|$O(1)$|
|`v.capacity();`|Currently allocated storage|$O(1)$|
|`v.resize(n);`|Resize to $n$ elements|$O(n)$|
|`v.reserve(n);`|Pre-allocate capacity for $n$ elements|$O(n)$|
|`v.shrink_to_fit();`|Reduce capacity to match size|$O(n)$|

### ☑ 6. Iterators

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`v.begin();`|Iterator to the first element|$O(1)$|
|`v.end();`|Iterator to the element past the last|$O(1)$|
|`v.rbegin();`|Reverse iterator to the last element|$O(1)$|
|`v.rend();`|Reverse iterator to before the first|$O(1)$|
### ☑ 7. Modifiers & Utilities

এগুলো মূলত ভেক্টরের ডেটা পরিবর্তন বা ম্যানেজমেন্টের জন্য ব্যবহৃত হয়।

| **Syntax**              | **Description**                                        | **Time Complexity** |
| ----------------------- | ------------------------------------------------------ | ------------------- |
| `v.assign(n, val);`     | Replaces content with $n$ copies of `val`              | $O(n)$              |
| `v.assign(it1, it2);`   | Replaces content with elements from range `[it1, it2)` | $O(n)$              |
| `v.swap(v2);`           | Swaps the contents of two vectors                      | $O(1)$              |
| `swap(v1, v2);`         | Non-member swap function                               | $O(1)$              |
| `v.emplace_back(args);` | Constructs element in-place at the end                 | $O(1)$ (Amortized)  |
| `v.emplace(it, args);`  | Constructs element in-place at position `it`           | $O(n)$              |
### ☑ 8. Algorithms / Operations

এগুলো `algorithm` হেডার ফাইল থেকে আসে যা ভেক্টরের উপর বিভিন্ন অপারেশন চালাতে সাহায্য করে।

| **Syntax**                            | **Description**                                    | **Time Complexity** |
| ------------------------------------- | -------------------------------------------------- | ------------------- |
| `sort(v.begin(), v.end());`           | Sorts elements in ascending order                  | $O(n \log n)$       |
| `sort(v.rbegin(), v.rend());`         | Sorts elements in descending order                 | $O(n \log n)$       |
| `reverse(v.begin(), v.end());`        | Reverses the order of elements                     | $O(n)$              |
| `*max_element(v.begin(), v.end());`   | Finds the maximum element                          | $O(n)$              |
| `*min_element(v.begin(), v.end());`   | Finds the minimum element                          | $O(n)$              |
| `accumulate(v.begin(), v.end(), 0);`  | Calculates the sum of all elements                 | $O(n)$              |
| `count(v.begin(), v.end(), x);`       | Counts occurrences of value `x`                    | $O(n)$              |
| `find(v.begin(), v.end(), x);`        | Returns iterator to the first occurrence of `x`    | $O(n)$              |
| `lower_bound(v.begin(), v.end(), x);` | Returns iterator to first element $\ge x$ (Sorted) | $O(\log n)$         |
| `upper_bound(v.begin(), v.end(), x);` | Returns iterator to first element $> x$ (Sorted)   | $O(\log n)$         |
| `unique(v.begin(), v.end());`         | Removes consecutive duplicates                     | $O(n)$              |
### ☑ 9. Vector-Set-Vector

| Syntax                            | Description                   | Time Complexity |
| :-------------------------------- | :---------------------------- | :-------------- |
| `set<int> s(v.begin(), v.end());` | Remove duplicates             | $O(n \log n)$   |
| `v.assign(s.begin(), s.end());`   | Back to set (Unique Elements) | $O(n)$          |
### 10. Vector Set Operations

_Note: Vectors must be sorted before using these functions._

![set operations Venn diagram union intersection difference, AI generated](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcQcls98FZAjNRlLLS2jrV7dW-vhs0djSsC7KtTPNWPKnK7-VRmOAjuq3WPPL6Y-thJIHar3egcsLeHir8qvg-B3u9rWPEDfmkVTNkUCs5jabNTy0sg)

| Syntax                                                                              | Description                    | Time Complexity |
| :---------------------------------------------------------------------------------- | :----------------------------- | :-------------- |
| `set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), back_inserter(res));`        | Union of two sorted vectors    | $O(N_1 + N_2)$  |
| `set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), back_inserter(res));` | Common elements in both        | $O(N_1 + N_2)$  |
| `set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), back_inserter(res));`   | Elements in V1 but not in V2   | $O(N_1 + N_2)$  |
| `includes(v1.begin(), v1.end(), v2.begin(), v2.end());`                             | Checks if V2 is a subset of V1 | $O(N_1 + N_2)$  |
### ☑ 11. Important Tricks for CP

| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `v.resize(unique(v.begin(), v.end()) - v.begin());` | Removes duplicates and resizes the vector | $O(N)$ |
| `lower_bound(v.begin(), v.end(), x) - v.begin();` | Gets the index of the first element $\ge x$ | $O(\log N)$ |
| `iota(v.begin(), v.end(), 1);` | Fills vector with increasing values (1, 2, 3...) | $O(N)$ |
| `is_sorted(v.begin(), v.end());` | Checks if the vector is sorted (Returns bool) | $O(N)$ |



