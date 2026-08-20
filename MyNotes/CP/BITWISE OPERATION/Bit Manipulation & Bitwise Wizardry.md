**Agenda:** মেমোরির সবচেয়ে ছোট ইউনিট অর্থাৎ **Bits (0 এবং 1)** নিয়ে সরাসরি কাজ করে $O(1)$ টাইম কমপ্লেক্সিটিতে বিভিন্ন গাণিতিক ও লজিক্যাল প্রবলেম সলভ করা। এটি অত্যন্ত ফাস্ট এবং মেমোরি এফিশিয়েন্ট।

---

### 🧠 1. Core Bitwise Operators

| **Operator** | **Name**        | **Logic**                            | **Rule of Thumb**         |
| ------------ | --------------- | ------------------------------------ | ------------------------- |
| `&`          | **AND**         | দুটি বিট-ই `1` হলে `1`, না হলে `0`   | Clearing / Checking bits  |
| `\|`         | **OR**          | যেকোনো একটি বিট `1` হলেই `1`         | Setting bits              |
| `^`          | **XOR**         | দুটি বিট আলাদা হলে `1`, এক হলে `0`   | Toggling / Odd-Even count |
| `~`          | **NOT**         | `1` থাকলে `0`, `0` থাকলে `1`         | Inverting all bits        |
| `<<`         | **Left Shift**  | বিটগুলোকে বামে সরায়, ডানপাশে `0` বসে | Multiply by $2^k$         |
| `>>`         | **Right Shift** | বিটগুলোকে ডানে সরায়, বামপাশে `0` বসে | Divide by $2^k$           |

---

### ⚡ 2. The $2^k$ Shift Properties (Crucial for CP)

- $1 \ll k = 2^k$
    
- $n \ll k = n \times 2^k$
    
- $n \gg k = \lfloor \frac{n}{2^k} \rfloor$
    

> [!warning] **Integer Overflow Alert!**
> 
> `1 << 40` কন্ডিশনে ওভারফ্লো করবে কারণ স্ট্যান্ডার্ড `1` একটি `int`। $10^9$-এর ওপর শিফট করতে সবসময় **`1LL << k`** ব্যবহার করবে।

---

### 🛠️ 3. Essential Bit Hacks (The "Must-Knows")

#### ক. Check if $i$-th bit is Set (1) or Not (0)

C++

```cpp
bool isSet(int n, int i) {
    return (n & (1LL << i)) != 0;
}
```

#### খ. Set the $i$-th bit (0 কে 1 বানানো, 1 থাকলে 1-ই রাখা)

C++

```cpp
int setBit(int n, int i) {
    return n | (1LL << i);
}
```

#### গ. Clear/Unset the $i$-th bit (1 কে 0 বানানো)

C++

```cpp
int clearBit(int n, int i) {
    return n & ~(1LL << i);
}
```

#### ঘ. Toggle the $i$-th bit (1 থাকলে 0, 0 থাকলে 1 করা)

C++

```cpp
int toggleBit(int n, int i) {
    return n ^ (1LL << i);
}
```

---

### 🚀 4. Advanced Advanced Tricks (Pupil to Specialist)

#### ১. Even/Odd Checking ($O(1)$)

C++

```cpp
if (n & 1) cout << "Odd";
else cout << "Even";
```

#### ২. Check if a number is a Power of 2

যদি কোনো সংখ্যা ২-এর পাওয়ার হয় (যেমন ৪, ৮, ১৬), তবে তার বাইনারিতে মাত্র একটি `1` থাকে।

C++

```cpp
bool isPowerOfTwo(long long n) {
    if (n <= 0) return false;
    return (n & (n - 1)) == 0;
}
```

#### ৩. Clear the lowest set bit (সর্বডানের ১-কে ০ করা)

C++

```cpp
n = n & (n - 1);
```

#### ৪. Multiplying/Dividing by 2

- `n = n << 1;` (Multiply by 2)
    
- `n = n >> 1;` (Divide by 2)
    

---

### 🧮 5. C++ Built-in GCC Functions (Direct Magic)

সবসময় ম্যানুয়াল লুপ না লিখে জিসিসি-র এই বিল্ট-ইন ফাংশনগুলো ব্যবহার করবে, এগুলো হার্ডওয়্যার লেভেলে কাজ করে এবং অত্যন্ত ফাস্ট:

- **`__builtin_popcountll(n)`**: `n`-এর বাইনারিতে মোট কয়টি `1` (Set bit) আছে তা গুনে দেয়।
    
- **`__builtin_clzll(n)`**: `n`-এর সর্ববামে (Count Leading Zeros) কয়টি শূন্য আছে তা জানায়।
    
- **`__builtin_ctzll(n)`**: `n`-এর সর্বডানে (Count Trailing Zeros) কয়টি শূন্য আছে তা জানায়।
    

---

### 💎 6. Master Property of XOR (`^`)

XOR-এর এই প্রপার্টিগুলো কন্টেস্টে লাইফ-সেভার হিসেবে কাজ করে:

1. $A \wedge A = 0$ (যেকোনো সংখ্যাকে নিজের সাথে XOR করলে ০ হয়)
    
2. $A \wedge 0 = A$
    
3. $A \wedge B = B \wedge A$ (Commutative)
    
4. যদি $A \wedge B = C$ হয়, তবে অবশ্যই $A \wedge C = B$ এবং $B \wedge C = A$ হবে।
    

**পপুলার ইন্টারভিউ/সিপি প্রবলেম:** একটি অ্যারেতে সব এলিমেন্ট জোড়ায় জোড়ায় (২ বার করে) আছে, শুধু একটি এলিমেন্ট একবার আছে। সেটি বের করো।

C++

```
int uniqueElement(vector<int>& v) {
    int xor_sum = 0;
    for(int x : v) xor_sum ^= x;
    return xor_sum; // জোড়ার এলিমেন্টগুলো কাটাকাটি হয়ে শুধু ইউনিকটা থাকবে
}
```

---

### 🗺️ 7. Subset Generation using Bitmasking

একটি $N$ সাইজের অ্যারের সব সাবসেট ($2^N$ টি) বের করার ক্ল্যাসিক মেথড:

C++

```cpp
void generateSubsets(vector<int>& v) {
    int n = v.size();
    // মোট সাবসেট সংখ্যা ১ থেকে শুরু করে এন বার লেফট শিফট (২^এন)
    for (int mask = 0; mask < (1 << n); mask++) {
        vector<int> current_subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                current_subset.push_back(v[i]);
            }
        }
        // এখানে current_subset নিয়ে প্রসেস করো
    }
}
```

---

### 📝 Metadata & Tags

_Created on: 2026-05-17_

_Tags: #Bit_Manipulation #Bitmasking #Math #Algorithms #C++ #Codeforces_Prep #Pupil_Level_