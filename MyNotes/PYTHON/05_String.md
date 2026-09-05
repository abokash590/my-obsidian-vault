|**Syntax**|**Description**|
|---|---|
|`s[i]`|i-তম index এর character|
|`s[a:b]`|Slicing (a থেকে b-1)|
|`s[::-1]`|Reverse|
|`s + s2`|Concatenation|
|`s * n`|n বার repeat|
|`len(s)`|Length|
|`s.lower()` / `s.upper()`|Case convert|
|`s.strip()` / `.lstrip()` / `.rstrip()`|Whitespace/character trim|
|`s.split(delim)`|Delimiter দিয়ে split → list|
|`delim.join(list)`|List of string জোড়া দেওয়া|
|`s.replace(old, new)`|Substring replace|
|`s.find(sub)`|Index রিটার্ন, না পেলে `-1`|
|`s.count(sub)`|Occurrence count|
|`s.startswith(x)` / `s.endswith(x)`|Prefix/suffix চেক|
|`s.isdigit()` / `.isalpha()` / `.isalnum()`|Character type চেক|
|`ord(c)` / `chr(x)`|Character ↔ ASCII কোড রূপান্তর|

## মূল কথা

- পাইথনে string **immutable** — `s[0] = 'a'` করা যায় না, নতুন string বানাতে হয়।
- Data cleaning-এ (ML preprocessing) `.strip()`, `.lower()`, `.split()`, `.replace()` — এই চারটা সবচেয়ে বেশি লাগবে।

### Content

![1788596566027_image.png](/api/d673b7e7-6628-477d-8218-aa80f6d27efb/files/c17ca437-4953-460b-9b1f-a0c8c5d6d44b/preview)

![1788597136202_image.png](blob:https://claude.ai/145992a9-5ef9-42c8-aaf3-28fb1cf20c05)

### ☑ 1. Declaration & Initialization | **Syntax** | **Description** | **Time Complexity** | | ------------------------- | --------------------------------- | ------------------- | | `vector<int> v;` | Empty vector | $O(1)$

pasted