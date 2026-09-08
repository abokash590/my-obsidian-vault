`import numpy as np` — সবার আগে এটা লিখতে হয়।
## Array Creation

|**Syntax**|**Description**|
|---|---|
|`np.array([1, 2, 3, 4])`|Python list থেকে 1D array তৈরি|
|`np.array([[1, 2], [3, 4]])`|Nested list থেকে 2D array তৈরি|
|`np.array(v, dtype=float)`|নির্দিষ্ট data type সহ array তৈরি (int, float, complex ইত্যাদি)|
|`arr.tolist()`|NumPy array কে ফিরিয়ে সাধারণ পাইথন list বানানো|

## Array Generation Functions

|**Syntax**|**Description**|
|---|---|
|`np.zeros((r, c))`|সব 0 দিয়ে ভরা array|
|`np.ones((r, c))`|সব 1 দিয়ে ভরা array|
|`np.full((r, c), val)`|নির্দিষ্ট একটা value দিয়ে ভরা array|
|`np.empty((r, c))`|Uninitialized array (দ্রুত, কিন্তু ভিতরে random garbage value)|
|`np.eye(n)`|n×n identity matrix (কর্ণ বরাবর 1, বাকি 0)|
|`np.arange(start, stop, step)`|পাইথনের `range()`-এর মতো, কিন্তু array রিটার্ন করে|
|`np.linspace(start, stop, n)`|start থেকে stop পর্যন্ত সমান দূরত্বের n টা মান|

## Random Generation

|**Syntax**|**Description**|
|---|---|
|`np.random.rand(r, c)`|0 থেকে 1-এর মধ্যে uniform random float|
|`np.random.randn(r, c)`|Standard normal distribution (mean=0, std=1) থেকে random value|
|`np.random.normal(mean, std, size)`|নির্দিষ্ট mean/std সহ normal distribution থেকে random value|
|`np.random.uniform(low, high, size)`|নির্দিষ্ট range-এর মধ্যে uniform random float|
|`np.random.randint(low, high, size)`|নির্দিষ্ট range-এর random integer|
|`np.random.choice(v, size)`|একটা array/list থেকে random element বাছাই|
|`np.random.shuffle(arr)`|In-place shuffle|
|`np.random.seed(x)`|Reproducibility-র জন্য seed fix করা|

## Array Attributes

|**Syntax**|**Description**|
|---|---|
|`arr.shape`|(row, column) — সারি ও কলামের সংখ্যা|
|`arr.ndim`|কয়টা dimension (1D, 2D, 3D...)|
|`arr.size`|মোট element সংখ্যা|
|`arr.dtype`|Array-এর data type (int64, float64...)|
|`arr.itemsize`|প্রতিটা element কত বাইট জায়গা নেয়|
|`arr.nbytes`|মোট memory ব্যবহার (বাইটে) — `size * itemsize`-এর সমান|

## মূল কথা

- `np.array()` দিয়ে পাইথনের list/nested list থেকে array তৈরি হয়; `dtype` না দিলে NumPy নিজে থেকেই ডেটা দেখে টাইপ বুঝে নেয়। সাধারণ Python list-এর চেয়ে NumPy array দ্রুত, কারণ এটা fixed type আর contiguous memory ব্যবহার করে।
- `arange()`-এ `stop` exclusive (বাদ যায়), কিন্তু `linspace()`-এ শেষ মানটা সাধারণত অন্তর্ভুক্ত থাকে — এই পার্থক্যটা মনে রাখা জরুরি। `np.empty()` দ্রুত হলেও ভিতরে garbage value থাকে, তাই ব্যবহারের আগে সব index-এ value বসিয়ে দিতে হবে।
- `np.random.seed()` সেট করলে প্রতিবার একই random sequence পাওয়া যায় — reproducibility-র জন্য প্রায় সবসময় প্রথমে লেখা হয়। `rand()` uniform distribution, `randn()` normal/Gaussian distribution — কোনটা দরকার সেটা বুঝে ব্যবহার করতে হবে।
- Attribute-গুলো (shape, ndim, size ইত্যাদি) ফাংশন না, তাই bracket `()` ছাড়া লেখা হয়। `dtype` জানা গুরুত্বপূর্ণ, কারণ ভুল dtype দিয়ে হিসাব করলে নিঃশব্দে ভুল answer আসতে পারে।