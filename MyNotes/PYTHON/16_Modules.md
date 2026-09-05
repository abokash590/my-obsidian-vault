
ML-এর জন্য দরকারি হতে পারে এমন কিছু বেসিক built-in module (NumPy/Pandas এখানে নেই)।

### `math`

|**Syntax**|**Description**|
|---|---|
|`math.sqrt(x)`|Square root|
|`math.ceil(x)` / `math.floor(x)`|Ceiling/floor|
|`math.log(x, base)`|Logarithm|
|`math.exp(x)`|$$e^x$$|
|`math.pi` / `math.e`|ধ্রুবক (constant)|
|`math.inf` / `-math.inf`|Infinity|

### `random`

|**Syntax**|**Description**|
|---|---|
|`random.random()`|0 থেকে 1 এর মধ্যে random float|
|`random.randint(a, b)`|a থেকে b (inclusive) random integer|
|`random.choice(v)`|List থেকে random element বাছাই|
|`random.shuffle(v)`|In-place shuffle|
|`random.seed(x)`|Reproducibility-র জন্য seed fix করা|
|`random.sample(v, k)`|Repetition ছাড়া k টা random element|

### `os`

|**Syntax**|**Description**|
|---|---|
|`os.getcwd()`|বর্তমান working directory|
|`os.listdir(path)`|ফোল্ডারের সব ফাইল/ফোল্ডারের list|
|`os.path.join(a, b)`|Cross-platform path জোড়া দেওয়া|
|`os.path.exists(path)`|Path আছে কিনা চেক|
|`os.makedirs(path)`|নতুন ফোল্ডার তৈরি (nested হলেও)|

### `datetime`

|**Syntax**|**Description**|
|---|---|
|`datetime.now()`|বর্তমান তারিখ ও সময়|
|`datetime.strptime(s, fmt)`|String থেকে datetime object বানানো|
|`date.strftime(fmt)`|Datetime কে নির্দিষ্ট ফরম্যাটে string বানানো|

## মূল কথা

- `random.seed()` ML experiment-এ reproducibility (একই result বারবার পাওয়া) নিশ্চিত করতে গুরুত্বপূর্ণ।
- `os.path` মডিউল dataset file path handle করার সময় (বিশেষত নিজের কম্পিউটার আর কারো shared notebook-এ path আলাদা হলে) কাজে লাগে।