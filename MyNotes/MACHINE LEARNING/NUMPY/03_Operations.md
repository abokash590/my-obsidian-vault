## Arithmetic Operations & Broadcasting

|**Syntax**|**Description**|
|---|---|
|`a + b` / `a - b`|Element-wise যোগ/বিয়োগ|
|`a * b`|Element-wise গুণ (matrix multiplication **না**)|
|`a / b`|Element-wise ভাগ|
|`a ** 2`|প্রতিটা element-কে power করা|
|`a % 2`|Element-wise modulo|
|`arr + scalar`|Broadcasting — একটা single value পুরো array-তে apply হয়|
|`arr2d + row_1d`|Broadcasting — ছোট shape-এর array বড় array-এর shape-এ ফিট হয়|

## Matrix Operations

|**Syntax**|**Description**|
|---|---|
|`A * B`|Element-wise multiplication (matrix multiplication **না**)|
|`A @ B`|আসল Matrix multiplication (dot product)|
|`np.dot(A, B)` / `np.matmul(A, B)`|`@`-এর সমতুল্য, ফাংশন আকারে|
|`A.T`|Transpose (row-column অদল-বদল)|
|`np.linalg.det(A)`|Determinant|
|`np.linalg.inv(A)`|Inverse matrix|

## Methods & Aggregation

|**Syntax**|**Description**|
|---|---|
|`arr.sum()`|সব element-এর যোগফল|
|`arr.sum(axis=0)`|প্রতিটা column-এর যোগফল|
|`arr.sum(axis=1)`|প্রতিটা row-এর যোগফল|
|`arr.min()` / `arr.max()`|সর্বনিম্ন / সর্বোচ্চ মান|
|`arr.mean()`|গড়|
|`arr.std()`|Standard deviation|
|`arr.var()`|Variance|
|`arr.sort()`|In-place sort|
|`np.sort(arr)`|নতুন sorted array রিটার্ন করে (original অপরিবর্তিত)|
|`arr.argmax()` / `.argmin()`|সর্বোচ্চ/সর্বনিম্ন মানের index|
|`np.any(arr)` / `np.all(arr)`|কোনো একটা / সবগুলো element `True` কিনা|

## Universal Functions & Sorting

|**Syntax**|**Description**|**Time Complexity**|
|---|---|---|
|`np.sqrt(arr)`|প্রতিটা element-এর square root|$$O(n)$$|
|`np.exp(arr)`|প্রতিটা element-এ $$e^x$$|$$O(n)$$|
|`np.log(arr)`|Natural log|$$O(n)$$|
|`np.abs(arr)`|Absolute value|$$O(n)$$|
|`np.unique(arr)`|ডুপ্লিকেট বাদ দিয়ে sorted unique value|$$O(n \log n)$$|
|`np.intersect1d(x, y)` / `np.union1d(x, y)`|দুই array-এর common element / union|$$O(n \log n)$$|
|`np.argsort(arr)`|Sort করলে কোন element কোন index থেকে আসছে সেই index-গুলো দেয়|$$O(n \log n)$$|

## মূল কথা

- সব arithmetic operation **element-wise**। Python list-এ `+` করলে concatenate হয়, NumPy array-এ `+` করলে যোগ হয় — এই পার্থক্য মনে রাখা জরুরি। **Broadcasting**-এর নিয়ম: shape দুটো ডান দিক থেকে তুলনা হয় — প্রতিটা dimension সমান হতে হবে, নয়তো একটার সাইজ 1 হতে হবে। এটাই **Vectorization**-এর ভিত্তি, NumPy-এর গতির মূল রহস্য।
- `*` element-wise multiplication, কিন্তু `@`/`np.dot()`/`np.matmul()` হলো real matrix multiplication — এই দুইটা গুলিয়ে ফেলা খুব common ভুল। Neural network/linear regression-এর হিসাব মূলত matrix multiplication দিয়েই চলে।
- `axis=0` মানে "column বরাবর নিচের দিকে" (row মিলিয়ে ফেলা), `axis=1` মানে "row বরাবর পাশাপাশি" (column মিলিয়ে ফেলা) — এই axis-এর দিক গুলিয়ে ফেলা common ভুল। `arr.sort()` in-place, `np.sort(arr)` নতুন copy দেয় — Python list-এর `.sort()` vs `sorted()`-এর মতোই।
- `np.sqrt`, `np.exp`, `np.log` ইত্যাদি **universal functions (ufuncs)** — element-wise, লুপ ছাড়াই পুরো array-তে apply হয়। `np.unique()` EDA-তে distinct value বের করতে কাজে লাগে; `np.argsort()` দরকার হয় যখন sorted value-র আসল index জানা লাগে।