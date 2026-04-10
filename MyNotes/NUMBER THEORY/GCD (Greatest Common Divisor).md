**Related Topics:** `[[Number Theory]]`, `[[LCM]]`, `[[Extended Euclidean Algorithm]]`, `[[Modular Inverse]]`

GCD (গরিষ্ঠ সাধারণ গুণনীয়ক) হলো দুই বা ততোধিক সংখ্যার মধ্যে সবচেয়ে বড় সেই সংখ্যা যা দিয়ে প্রতিটি সংখ্যাকে নিঃশেষে ভাগ করা যায়।

---

### 🚀 1. The Euclidean Algorithm (The Foundation)

এটি GCD বের করার সবচেয়ে প্রাচীন এবং দ্রুততম পদ্ধতি। এর মূল কথা হলো:

$$\gcd(a, b) = \gcd(b, a \pmod b)$$

যেখানে $b = 0$ না হওয়া পর্যন্ত আমরা ভাগ করতে থাকি।

#### **C++ Implementation:**


```
// Recursive (সবচেয়ে জনপ্রিয়)
long long gcd(long long a, long long b) {
    return b == 0 ? a : gcd(b, a % b);
}

// STL Built-in (C++17 and above)
#include <numeric>
long long res = std::gcd(a, b);
```

---

### 🛠 2. Important Properties (The CP Toolbox)

সিপিতে প্রবলেম সলভ করার জন্য এই রুলগুলো আপনার মাথায় থাকতে হবে:

1. **$\gcd(a, b) = \gcd(a-b, b)$**: এটি ডাইনামিক প্রোগ্রামিং বা বড় সংখ্যার ক্ষেত্রে কাজে লাগে।
2. **$\gcd(k \cdot a, k \cdot b) = k \cdot \gcd(a, b)$**: ডিস্ট্রিবিউটিভ প্রোপার্টি।
3. **$\gcd(a, b, c) = \gcd(a, \gcd(b, c))$**: একাধিক সংখ্যার GCD বের করার নিয়ম।
4. **$\gcd(a, 0) = a$**: এটি বেইজ কেস হিসেবে কাজ করে।
5. **$\gcd(1, a) = 1$**: ১ এর সাথে যেকোনো সংখ্যার GCD সবসময় ১।

---

### 📐 3. Relationship with LCM

GCD এবং LCM একে অপরের সাথে নিবিড়ভাবে যুক্ত:

$$a \times b = \gcd(a, b) \times \text{lcm}(a, b)$$

তাই LCM বের করার সহজ উপায় হলো: `lcm(a, b) = (a * b) / gcd(a, b)`।

_(সতর্কতা: বড় সংখ্যার ক্ষেত্রে ওভারফ্লো এড়াতে `(a / gcd(a, b)) * b` লিখুন।)_

---

### 🎯 4. CP Insights (Target 1400 Rating)

১৪০০ রেটিং পর্যন্ত প্রবলেমগুলোতে নিচের ৩টি প্যাটার্ন সবচেয়ে বেশি দেখা যায়:
#### **A. GCD and Array Difference Trick**
যদি একটি অ্যারের সব এলিমেন্টকে কোনো সংখ্যা $d$ দিয়ে ভাগ করলে একই ভাগশেষ (Remainder) থাকে, তবে $d$ হবে ওই অ্যারের এলিমেন্টগুলোর পার্থক্যের (Differences) GCD।
- **Property:** $\gcd(a, b, c) = \gcd(a, b-a, c-b)$
- **Application:** "Find a number $x > 1$ such that $a_i \equiv a_j \pmod x$ for all $i, j$."
#### **B. Consecutive Numbers**
- **Property:** $\gcd(x, x+1) = 1$। পরপর দুটি সংখ্যার GCD সবসময় ১ হয়।
- **Application:** এটি কন্ট্রাডিকশন প্রুফ করতে বা "No Solution" কেস হ্যান্ডেল করতে খুব কাজে লাগে।

#### **C. GCD of Subsequences**
একটি অ্যারের কোনো সাব-সিকোয়েন্সের GCD সবসময় ওই অ্যারের ক্ষুদ্রতম উপাদানের সমান অথবা তার ছোট হবে। এটি সার্চ স্পেস কমাতে সাহায্য করে।

---

### ⚡ 5. Extended Euclidean Algorithm

এটি সাধারণ GCD-এর একটি উন্নত সংস্করণ যা $ax + by = \gcd(a, b)$ সমীকরণ থেকে $x$ এবং $y$ এর মান বের করতে সাহায্য করে। এটি **Modular Multiplicative Inverse** বের করার জন্য অপরিহার্য।

#### **Code Snippet:**

```
long long extended_gcd(long long a, long long b, long long &x, long long &y) {
    if (b == 0) {
        x = 1; y = 0;
        return a;
    }
    long long x1, y1;
    long long d = extended_gcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}
```

---
# Tricks
- [[Circular Traversal & GCD]]
- 