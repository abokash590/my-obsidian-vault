## Basic Indexing & Slicing

|**Syntax**|**Description**|
|---|---|
|`arr[i]`|1D array-এর i-তম element|
|`arr[-1]`|শেষ element (negative index)|
|`arr2d[i, j]`|2D array-এ row i, column j-এর element (efficient স্টাইল)|
|`arr2d[i][j]`|একই কাজ, কিন্তু আলাদাভাবে দুইবার index করে (কম efficient)|
|`arr[start:stop:step]`|Slicing — `stop` exclusive|
|`arr[::-1]`|Reverse|
|`arr2d[r_slice, c_slice]`|Row ও column আলাদাভাবে slice করা|
|`arr2d[:, i]`|সব row-এর i-নম্বর column|
|`arr2d[i, :]`|i-নম্বর সম্পূর্ণ row|
|`arr[i] = x`|নির্দিষ্ট index-এ value বসানো|
|`arr[a:b] = [x, y]`|Slice-এ একসাথে একাধিক value বসানো|

## Boolean & Fancy Indexing

|**Syntax**|**Description**|
|---|---|
|`arr > 20`|প্রতিটা element-এর জন্য `True`/`False` mask array রিটার্ন করে|
|`arr[arr > 20]`|শর্ত মেনে চলা element গুলোই বের করে (masking/filtering)|
|`arr[(arr > 15) & (arr < 30)]`|একাধিক শর্ত একসাথে (`&` = and, `\|` = or)|
|`arr[arr > 100] = 0`|Boolean mask দিয়ে সরাসরি value পরিবর্তন করা|
|`arr[[0, 2, 4]]`|Fancy indexing — নির্দিষ্ট কিছু index একসাথে বের করা|
|`arr2d[[0, 2]]`|নির্দিষ্ট row-গুলো (0 এবং 2 নম্বর) বের করা|

## View vs Copy

⚠️ NumPy-তে সবচেয়ে বেশি ভুল হয় এমন একটা জায়গা।

|**Indexing Type**|**View নাকি Copy?**|
|---|---|
|Basic slicing `arr[1:4]`|**View** — মূল array বদলে যায়|
|Fancy indexing `arr[[0, 2]]`|**Copy** — মূল array অপরিবর্তিত থাকে|
|Boolean indexing `arr[arr > 5]`|**Copy** — মূল array অপরিবর্তিত থাকে|
|`arr.view()`|Shallow copy — নতুন object, একই মেমোরি|
|`arr.copy()`|Deep copy — সম্পূর্ণ আলাদা, স্বাধীন মেমোরি|

|**Syntax**|**Description**|
|---|---|
|`sliced = arr[1:4]`|View তৈরি হয় — `sliced` বদলালে `arr`-ও বদলে যাবে|
|`safe_copy = arr[1:4].copy()`|Slice-এর উপর `.copy()` চাপিয়ে সম্পূর্ণ আলাদা array বানানো|

## মূল কথা

- Python list-এর মতোই index 0 থেকে শুরু, negative index শেষ থেকে গোনে। `arr2d[1, 2]` লেখা `arr2d[1][2]`-এর চেয়ে বেশি efficient — কারণ `[1][2]` প্রথমে intermediate 1D array বানায়, তারপর তার উপর আবার index করে।
- একাধিক condition একসাথে লেখার সময় Python-এর `and`/`or` কাজ করে না — বাধ্যতামূলকভাবে `&`/`|` ব্যবহার করতে হয়, প্রতিটা condition আলাদা bracket `()`-এ রাখতে হয়। Boolean indexing ডেটা ক্লিনিং/EDA-তে খুব বেশি ব্যবহার হয়।
- **Slicing** সবসময় original data-এর **view** রিটার্ন করে (memory শেয়ার করে) — sliced অংশ বদলালে মূল array-ও বদলে যায়, এজন্যই slicing দ্রুত। **Fancy** ও **Boolean indexing** সবসময় নতুন **copy** রিটার্ন করে। মূল array যেন ভুলে না বদলে যায় সেটা নিশ্চিত করতে `.copy()` ব্যবহার করা নিরাপদ অভ্যাস।