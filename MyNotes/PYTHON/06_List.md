
| **Syntax**             | **Description**                                  | **Time Complexity** |
| ---------------------- | ------------------------------------------------ | ------------------- |
| `v = []` / `list()`    | Empty list                                       | $O(1)$              |
| `v = [0] * n`          | Size n, সব 0 দিয়ে initialize                    | $O(n)$              |
| `v = [1, 2, 3]`        | Initializer                                      | $O(n)$              |
| `v.append(x)`          | শেষে element যোগ                                 | $O(1)$ (Amortized)  |
| `v.insert(i, x)`       | i index-এ element বসানো                          | $O(n)$              |
| `v.extend(v2)`         | আরেকটা list জোড়া দেওয়া                         | $O(k)$              |
| `v.pop()`              | শেষ element রিমুভ ও রিটার্ন                      | $O(1)$              |
| `v.pop(i)`             | i-তম element রিমুভ                               | $O(n)$              |
| `v.remove(x)`          | প্রথম matching value রিমুভ                       | $O(n)$              |
| `v.clear()`            | সব element মুছে ফেলা                             | $O(n)$              |
| `v[i]`                 | Index access                                     | $O(1)$              |
| `v[a:b]`               | Slicing                                          | $O(k)$              |
| `v[::-1]`              | Reverse                                          | $O(n)$              |
| `len(v)`               | Length                                           | $O(1)$              |
| `x in v`               | Membership চেক                                   | $O(n)$              |
| `v.sort()`             | In-place ascending sort                          | $O(n \log n)$       |
| `v.sort(reverse=True)` | Descending sort                                  | $O(n \log n)$       |
| `sorted(v)`            | নতুন sorted list রিটার্ন করে (original ঠিক থাকে) | $O(n \log n)$       |
| `max(v)` / `min(v)`    | Maximum/minimum                                  | $O(n)$              |
| `sum(v)`               | যোগফল                                            | $O(n)$              |
| `v.copy()`             | Shallow copy                                     | $O(n)$              |

## মূল কথা

- List **mutable** — item change করা যায়, তাই ML-এ raw data রাখার সময় সাবধান, একটা list অন্য variable-এ assign করলে reference কপি হয় (`v2 = v1` মানে দুটো একই list-কে point করে)। আসল copy পেতে `v.copy()` বা `list(v)` ব্যবহার করো।
- List ভিতরে ভিন্ন type mix করা যায় (`[1, "a", True]`), কিন্তু ML/data কাজে সাধারণত একই type রাখাই ভালো অভ্যাস (পরে NumPy array বানানোর সময় সুবিধা হয়)।