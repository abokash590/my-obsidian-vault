
|**Syntax**|**Description**|
|---|---|
|`open("file.txt", "r")`|Read mode-এ file খোলা|
|`open("file.txt", "w")`|Write mode (overwrite করে)|
|`open("file.txt", "a")`|Append mode|
|`with open("file.txt") as f: ...`|Context manager — automatically close হয়|
|`f.read()`|পুরো file একসাথে পড়া|
|`f.readline()`|এক লাইন পড়া|
|`f.readlines()`|সব লাইন list আকারে পড়া|
|`f.write(text)`|Text লেখা|
|`for line in f:`|Line by line iterate করা|

## মূল কথা

- `with` ব্যবহার করা সবসময় ভালো — এতে file নিজে থেকেই close হয়ে যায়, `f.close()` আলাদা করে লেখা লাগে না।
- CSV/JSON এর মতো structured file-এর জন্য পাইথনের built-in `csv`/`json` module আছে, কিন্তু ML কাজে সাধারণত এগুলো Pandas দিয়েই handle করা হয় (তাই এই নোটে বিস্তারিত রাখিনি)।