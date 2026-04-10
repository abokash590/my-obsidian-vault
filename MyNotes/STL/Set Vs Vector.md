## Set vs Vector
| **কাজের ধরণ**            | **কোনটি ব্যবহার করবেন?** | **কেন?**                       |
| ------------------------ | ------------------------ | ------------------------------ |
| **Search an element**    | **Set**                  | $O(\log N)$ vs $O(N)$          |
| **Delete an element**    | **Set**                  | $O(\log N)$ vs $O(N)$          |
| **Random Access** (v[i]) | **Vector**               | $O(1)$ vs $O(N)$ (Iterate)     |
| **Memory usage**         | **Vector**               | Much lower than Set            |
| **Automatic Sorting**    | **Set**                  | Always sorted                  |
| **Sorting at once**      | **Vector**               | `sort()` function is very fast |
