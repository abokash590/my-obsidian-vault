
| **Syntax**      | **Description**                         | **Time Complexity** |
| --------------- | --------------------------------------- | ------------------- |
| `s = set()`     | Empty set (`{}` দিয়ে হয় না, ওটা dict) | $O(1)$              |
| `s = {1, 2, 3}` | Initializer                             | $O(n)$              |
| `s.add(x)`      | Element যোগ                             | $O(1) (Avg)$        |
| `s.remove(x)`   | Element রিমুভ, না থাকলে exception       | $O(1) (Avg)$        |
| `s.discard(x)`  | Element রিমুভ, না থাকলে কিছু হয় না     | $O(1) (Avg)$        |
| `x in s`        | Membership চেক                          | $O(1) (Avg)$        |
| `len(s)`        | Size                                    | $O(1)$              |
| `s1 \| s2`      | Union                                   | $O(n+m)$            |
| `s1 & s2`       | Intersection                            | $O(n+m)$            |
| `s1 - s2`       | Difference                              | $O(n+m)$            |
| `s1 ^ s2`       | Symmetric difference                    | $O(n+m)$            |

## মূল কথা

- Set-এ duplicate থাকে না — কোনো list থেকে duplicate সরাতে `set(v)` করলেই হয়ে যায় (কিন্তু order নষ্ট হয়ে যায়)।
- Data cleaning-এ unique value বের করা বা membership চেক দ্রুত করার জন্য (list-এর $O(n)$ এর বদলে set-এর $O(1)$) খুব কাজে লাগে।