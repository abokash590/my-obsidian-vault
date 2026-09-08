## Reshaping & Resizing

|**Syntax**|**Description**|
|---|---|
|`arr.reshape(r, c)`|Shape বদলায়, মোট element সংখ্যা একই থাকতে হয়|
|`arr.reshape(r, -1)`|`-1` দিলে NumPy নিজে থেকেই বাকি dimension হিসাব করে নেয়|
|`arr.resize(r, c)`|In-place shape পরিবর্তন — দরকার হলে ডেটা repeat/truncate করে|
|`arr.flatten()`|Multi-dimensional কে 1D করে (নতুন copy)|
|`arr.ravel()`|`flatten()`-এর মতোই, কিন্তু যথাসম্ভব original data-এর view রিটার্ন করে|
|`arr.T`|Transpose (row-column অদল-বদল)|

## Stacking & Splitting

|**Syntax**|**Description**|
|---|---|
|`np.vstack((a, b))`|Vertical stack — নতুন row হিসেবে জোড়া|
|`np.hstack((a, b))`|Horizontal stack — পাশাপাশি জোড়া (1D গুলো একটাই লম্বা row হয়ে যায়)|
|`np.column_stack((a, b))`|প্রতিটা 1D array-কে column হিসেবে পাশাপাশি বসায় (2D তৈরি হয়)|
|`np.concatenate((a, b), axis=...)`|নির্দিষ্ট axis বরাবর সাধারণভাবে জোড়া দেওয়া|
|`np.split(arr, n)`|সমান n ভাগে ভাগ করা (অসমান হলে error)|
|`np.array_split(arr, n)`|অসমান ভাগেও কাজ করে|
|`np.vsplit(arr2d, n)` / `np.hsplit(arr2d, n)`|Row / Column অনুযায়ী ভাগ করা|

## Extras — Type Conversion, Conditional Selection, NaN, Save/Load

|**Syntax**|**Description**|
|---|---|
|`arr.astype(float)` / `arr.astype(int)`|dtype পরিবর্তন করে নতুন array রিটার্ন করে|
|`np.where(cond, x, y)`|Condition true হলে `x`, false হলে `y` — element-wise ternary|
|`np.clip(arr, min, max)`|মান একটা range-এর মধ্যে বেঁধে দেয়|
|`np.isnan(arr)`|কোন element `NaN` কিনা বুলিয়ান mask দেয়|
|`arr[~np.isnan(arr)]`|`NaN` বাদ দিয়ে বাকি value বের করা|
|`np.nanmean(arr)` / `np.nansum(arr)`|`NaN` বাদ দিয়ে গড়/যোগফল হিসাব|
|`np.save("file.npy", arr)` / `np.load(...)`|Array-কে বাইনারি ফাইলে সেভ/লোড করা|
|`np.savetxt(...)` / `np.loadtxt(...)`|Array-কে টেক্সট/CSV ফাইলে সেভ/লোড করা|
|`np.dot(v1, v2)`|দুইটা 1D vector-এর dot product (scalar রিটার্ন করে)|

## মূল কথা

- `reshape()` মোট element সংখ্যা একই থাকতে বাধ্য করে, `resize()` দরকার হলে ডেটা বাড়ায়/কমায়। `flatten()` সবসময় নতুন কপি বানায়, `ravel()` যথাসম্ভব original data-এর সাথে memory শেয়ার করে (দ্রুত, কিন্তু ফলাফল বদলালে মূল array-ও বদলে যেতে পারে)।
- `vstack` = নিচে/উপরে জোড়া, `hstack` = পাশাপাশি জোড়া। **⚠️** 1D array-এর ক্ষেত্রে `hstack` আর `column_stack` সম্পূর্ণ ভিন্ন ফলাফল দেয় — `hstack` একটা লম্বা 1D array বানায়, `column_stack` প্রতিটা array-কে column হিসেবে treat করে 2D matrix বানায় (আলাদা feature থেকে dataset বানাতে এটা কাজে লাগে)।
- `astype()`-এ float থেকে int করলে decimal অংশ round না করে truncate (কেটে ফেলা) হয়। Real dataset-এ `NaN` (missing value) খুব common — সাধারণ `.mean()`/`.sum()` NaN থাকলে `nan` রিটার্ন করে, তাই `np.nanmean()`/`np.nansum()`-এর মতো "nan-safe" ফাংশন ব্যবহার করতে হয়।