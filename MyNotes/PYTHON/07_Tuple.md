Tuple হলো **immutable** list — একবার তৈরি হলে পরিবর্তন করা যায় না।

|**Syntax**|**Description**|
|---|---|
|`t = ()`|Empty tuple|
|`t = (1,)`|Single-element tuple (কমা দেওয়া জরুরি)|
|`t = (1, 2, 3)`|Tuple তৈরি|
|`t[i]`|Index access|
|`t[a:b]`|Slicing|
|`a, b = t`|Unpacking|
|`t.count(x)`|Occurrence count|
|`t.index(x)`|প্রথম occurrence-এর index|
|`t + t2`|Concatenation|
|`len(t)`|Length|

## মূল কথা

- Tuple ব্যবহার হয় যখন data পরিবর্তন হওয়ার দরকার নেই — যেমন function থেকে multiple value return করা (`return x, y`), dictionary-এর key হিসেবে, বা fixed configuration রাখা।
- List-এর চেয়ে tuple সামান্য বেশি memory-efficient এবং দ্রুত, কারণ immutable।