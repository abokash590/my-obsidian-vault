# Bitset

**Related Topics:** `[[C++ STL]]`, `[[Bit Manipulation]]`, `[[Optimization]]`, `[[Knapsack Optimization]]`

`std::bitset` হলো একটি বিশেষ কন্টেইনার যা বিট লেভেলে ডেটা স্টোর করে। এটি `vector<bool>` বা `bool` অ্যারের চেয়ে অনেক বেশি মেমোরি সাশ্রয়ী এবং দ্রুত।

---

### 🚀 Why Bitset? (CP Advantages)

১. **Memory:** ১টি বিট স্টোর করতে এটি মাত্র ১ বিট জায়গা নেয় (যেখানে `bool` ১ বাইট বা ৮ বিট নেয়)।

২. **Speed:** বিটওয়াইজ অপারেশনগুলো (AND, OR, XOR) এটি একবারে ৩২ বা ৬৪ বিটের ওপর করতে পারে। এটি সাধারণ লুপের চেয়ে প্রায় **৬৪ গুণ দ্রুত**।

৩. **Set Theory:** বড় রেঞ্জের সেটের ইন্টারসেকশন বা ইউনিয়ন বের করতে এটি অত্যন্ত কার্যকরী।

---

### 🛠️ Initialization & Syntax

C++

```
#include <bitset>

const int N = 8;
bitset<N> b;           // ০ দিয়ে ইনিশিয়ালাইজ হবে (00000000)
bitset<N> b1(7);       // ডেসিমাল থেকে বাইনারি (00000111)
bitset<N> b2("1010");  // স্ট্রিং থেকে বাইনারি (00001010)
```

_দ্রষ্টব্য:_ বিটসেটের সাইজ অবশ্যই কম্পাইল টাইমে ফিক্সড হতে হবে (যেমন `const int` বা `#define`)।

---

### 🔍 All Member Functions

|**Function**|**Description**|
|---|---|
|`b.set()`|সব বিটকে `1` করে।|
|`b.set(pos, val)`|`pos` পজিশনে `val` (0 বা 1) বসায়।|
|`b.reset()`|সব বিটকে `0` করে।|
|`b.reset(pos)`|`pos` পজিশনের বিটকে `0` করে।|
|`b.flip()`|সব বিটকে ইনভার্ট/উল্টে দেয়।|
|`b.flip(pos)`|নির্দিষ্ট পজিশনের বিটকে উল্টে দেয়।|
|`b.count()`|কতগুলো বিট `1` (Set bit) আছে তা গুনে দেয়।|
|`b.size()`|বিটসেটের মোট সাইজ কত তা রিটার্ন করে।|
|`b.test(pos)`|নির্দিষ্ট বিটটি `1` কি না তা চেক করে (Error safe)।|
|`b.any()`|অন্তত একটি বিট `1` আছে কি না।|
|`b.none()`|সব বিট `0` কি না।|
|`b.all()`|সব বিট `1` কি না।|
|`b.to_string()`|বিটসেটকে স্ট্রিংয়ে রূপান্তর করে।|
|`b.to_ulong()`|`unsigned long` এ রূপান্তর করে।|

---

### 💻 Bitwise Operations (The Magic)

আপনি বিটসেটের ওপর সরাসরি সব বিটওয়াইজ অপারেটর চালাতে পারেন:

C++

```
bitset<100> a, b, c;
c = a & b;  // AND
c = a | b;  // OR
c = a ^ b;  // XOR
c = ~a;     // NOT
c = a << 2; // Left Shift
```

---

### 🎯 CP Use Case: Knapsack Optimization

যদি আপনার কাছে $N$ টি আইটেম থাকে এবং টার্গেট সাম $W$ হয়, তবে সাধারণ DP-তে $O(N \cdot W)$ সময় লাগে। Bitset ব্যবহার করে আপনি এটি $O(\frac{N \cdot W}{64})$ এ নামিয়ে আনতে পারেন।

C++

```
bitset<100005> dp;
dp[0] = 1;
for (int w : weights) {
    dp |= (dp << w); // এক লাইনে সব পসিবল সাম আপডেট
}
if (dp[target]) cout << "Possible!";
```

---

### ⚠️ Important Notes

- **Index Order:** `b[0]` হলো একদম ডানদিকের বিট (LSB)।
- **Binary Format:** `cout << b` দিলে এটি বাইনারি স্ট্রিং হিসেবে প্রিন্ট হয়।