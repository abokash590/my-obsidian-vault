| **Syntax**                 | **Description**                            |
| -------------------------- | ------------------------------------------ |
| `input()`                  | Input নেয়, সবসময় `str` হিসেবে আসে        |
| `int(input())`             | Integer input                              |
| `float(input())`           | Float input                                |
| `print(x)`                 | Newline সহ print                           |
| `print(x, end="")`         | Newline ছাড়া print                        |
| `print(a, b, sep=", ")`    | Custom separator দিয়ে print               |
| `print(f"{x} - {y}")`      | f-string formatting (সবচেয়ে বেশি ব্যবহৃত) |
| `print(f"{x:.2f}")`        | 2 দশমিক ঘর পর্যন্ত round করে print         |
| `"{} and {}".format(x, y)` | `.format()` স্টাইল formatting              |

## মূল কথা

- ML/data কাজে সাধারণত file বা DataFrame থেকে data আসে, তাই `input()` কম লাগে — কিন্তু script/CLI টুল লেখার সময় দরকার হয়।
- f-string (`f"..."`) সবচেয়ে readable এবং modern পদ্ধতি — variable আর expression দুটোই `{}`-এর ভিতরে লেখা যায়, যেমন `f"{a+b}"`।