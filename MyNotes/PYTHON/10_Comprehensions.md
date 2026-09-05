
|**Syntax**|**Description**|
|---|---|
|`[x for x in range(n)]`|List comprehension|
|`[x for x in v if x % 2 == 0]`|Filter সহ|
|`[x*x for x in v]`|Transform সহ|
|`[x if x > 0 else 0 for x in v]`|Comprehension-এর ভিতরে if-else (conditional value)|
|`{x for x in v}`|Set comprehension|
|`{x: x*x for x in v}`|Dict comprehension|
|`(x for x in v)`|Generator expression (lazy, memory efficient)|
|`[x for row in matrix for x in row]`|Nested loop comprehension (2D list ফ্ল্যাট করা)|

## মূল কথা

- Comprehension আসলে `for` লুপকেই একলাইনে লেখার একটা compact উপায় — পড়তে ও লিখতে দ্রুত, কিন্তু বেশি জটিল হয়ে গেলে সাধারণ `for` লুপ ব্যবহার করাই ভালো (readability)।
- ML data preprocessing-এ একলাইনে filter/transform করার জন্য (raw list নিয়ে কাজ করার সময়) এটা খুব কমন প্যাটার্ন।