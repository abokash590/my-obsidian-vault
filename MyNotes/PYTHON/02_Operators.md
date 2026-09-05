|**Category**|**Operators**|**নোট**|
|---|---|---|
|Arithmetic|`+ - * / // % **`|`/` সবসময় float রিটার্ন করে, `//` floor division|
|Comparison|`== != > < >= <=`|রেজাল্ট সবসময় `bool`|
|Logical|`and` `or` `not`|C++ এর `&&` `\|` `!`-এর সমতুল্য|
|Bitwise|`& \| ^ ~ << >>`|C++ এর মতোই কাজ করে|
|Assignment|`= += -= *= /= //= %= **=`|Compound assignment|
|Identity|`is` / `is not`|দুইটা variable একই object কিনা (value নয়, memory address)|
|Membership|`in` / `not in`|list/str/set/dict-এ উপাদান আছে কিনা|

## মূল কথা

- `==` value compare করে, `is` object identity compare করে। ছোট integer/string এর জন্য মাঝে মাঝে দুটো একই মনে হলেও, বড় বা mutable object-এর ক্ষেত্রে পার্থক্য গুরুত্বপূর্ণ।
- `**` হলো power operator — `2 ** 3 = 8`।
- Chained comparison সম্ভব: `1 < x < 10` — এটা `1 < x and x < 10`-এর সমান।