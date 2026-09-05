## Conditional

|**Syntax**|**Description**|
|---|---|
|`if cond: ...`|Simple if|
|`if cond: ... else: ...`|If-else|
|`if cond: ... elif cond2: ... else: ...`|Multiple condition|
|`x = a if cond else b`|Ternary (একলাইনের if-else)|

## Loops

|**Syntax**|**Description**|
|---|---|
|`for i in range(n):`|0 থেকে n-1 পর্যন্ত|
|`for i in range(a, b):`|a থেকে b-1 পর্যন্ত|
|`for i in range(a, b, step):`|Step সহ (negative step দিলে reverse হয়)|
|`for x in iterable:`|List/string/dict-এর উপর সরাসরি লুপ|
|`for i, x in enumerate(iterable):`|Index সহ লুপ|
|`while cond:`|While loop|
|`break`|লুপ সম্পূর্ণ থামানো|
|`continue`|বর্তমান iteration স্কিপ করে পরেরটায় যাওয়া|
|`for ... else:`|Loop `break` ছাড়া শেষ হলে `else` ব্লক রান হয়|
|`pass`|কিছু না করার জন্য placeholder|

## মূল কথা

- `range(a, b, step)`-এ `b` exclusive — অর্থাৎ `b` নিজে অন্তর্ভুক্ত হয় না।
- `for...else` পাইথনে একটা special ফিচার — data validation loop-এ (যেমন "কোনো duplicate আছে কিনা খুঁজে বের করা") খুব কাজে লাগে।