
| **Syntax**               | **Description**                 | **Time Complexity** |
| ------------------------ | ------------------------------- | ------------------- |
| `d = {}` / `dict()`      | Empty dictionary                | $O(1)$              |
| `d = {"a": 1, "b": 2}`   | Initializer                     | $O(n)$              |
| `d[key] = value`         | Insert বা update                | $O(1)(Avg)$         |
| `d[key]`                 | Access, key না থাকলে `KeyError` | $O(1) (Avg)$        |
| `d.get(key)`             | Access, না থাকলে `None`         | $O(1) (Avg)$        |
| `d.get(key, default)`    | Default value সহ access         | $O(1) (Avg)$        |
| `key in d`               | Key আছে কিনা চেক                | $O(1) (Avg)$        |
| `del d[key]`             | Key মুছে ফেলা                   | $O(1) (Avg)$        |
| `d.pop(key)`             | Key মুছে value রিটার্ন করে      | $O(1) (Avg)$        |
| `d.keys()`               | সব key                          | $O(1)$              |
| `d.values()`             | সব value                        | $O(1)$              |
| `d.items()`              | (key, value) pair               | $O(1)$              |
| `for k, v in d.items():` | Key-value pair দিয়ে iterate    | $O(n)$              |
| `d.update(d2)`           | আরেকটা dict merge করা           | $O(k)$              |

## মূল কথা

- Dictionary key অবশ্যই **immutable** টাইপ হতে হবে (string, number, tuple) — list বা dict নিজে key হতে পারে না।
- Python 3.7+ থেকে dict insertion order মনে রাখে — অর্থাৎ যেই order-এ item add করেছো, `for` লুপে সেই order-ই পাবে।
- ML preprocessing-এ label encoding, mapping (যেমন `{"cat": 0, "dog": 1}`) বানাতে dictionary সবচেয়ে বেশি ব্যবহৃত হয়।