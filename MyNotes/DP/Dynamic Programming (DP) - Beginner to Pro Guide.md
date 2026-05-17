**মূল দর্শন (The Core Mantra):** > "একই Calculation আমরা বারবার করবো না!"

> কোনো একটা প্রবলেম সলভ করার সময় যদি একই সাব-প্রবলেম (Sub-problem) বারবার সামনে আসে, তবে আমরা প্রথমবার তার উত্তর বের করে মেমরিতে সেভ করে রাখবো, যাতে পরে লাগলে সরাসরি ব্যবহার করা যায়.

---

### 🚀 Problem Solving Strategy (কীভাবে চিন্তা করবে?)

যেকোনো DP প্রবলেম দেখলে নিচের ৩টি গোল্ডেন স্টেপ অনুসরণ করো:

1. **Problem টা ভালো করে পড়া:** কন্ডিশন এবং ইনপুট-আউটপুট বোঝা.
    
2. **Brute Force ওয়েতে চিন্তা করা:** নরমাল রিকার্সন (Recursion) বা লুপ দিয়ে চিন্তা করা এবং খেয়াল করা—_একই স্টেট (State) বারবার আসছে কি না_।
    
3. **মেমোরিতে Save/Memorize করা:** উত্তর একবার বের হলে সেটাকে ক্যাশে রেখে দেওয়া.
    

---

### 🧩 Memoization কী?

$$\text{Memoization} = \text{Recursion (নরমাল ব্যাকট্র্যাকিং)} + \text{Caching (Array বা Map দিয়ে স্টোরেজ)} \text{}$$

---

### 📊 Basic Types of DP Problems

সাধারণত প্রাথমিক লেভেলের DP প্রবলেমগুলো দুই ধরণের হয়:

1. **Counting:** মোট কত উপায়ে (Number of ways) একটি কাজ করা যায়।
    
2. **Optimization:** কোনো কিছুর সর্বনিম্ন (Minimization) বা সর্বোচ্চ (Maximization) ভ্যালু বের করা।
    

---

### 💻 Live Simulation: Counting (Number of Ways)

ধরো, তোমাকে $N$ তম সিঁড়িতে উঠতে হবে। তুমি প্রতি পদক্ষেপে ১ থেকে ৬ পর্যন্ত যেকোনো সংখ্যক সিঁড়ি লাফাতে পারো (যেমন একটি ছক্কার চাল)। মোট কত উপায়ে $N$-এ পৌঁছানো যাবে?

#### 🛠️ DP Steps & Code Structure:

C++

```
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;

// ১. Memoization Storage (শুরুতে সব ০ দিয়ে ফিল করা থাকে)
int dp[1000005]; //

int ways(int n) {
    // ২. Base-Case (নেগেটিভ সিঁড়িতে যাওয়া অসম্ভব)
    if (n < 0) return 0; //
    
    // ৩. Memoization Check (যদি আগে থেকেই উত্তর জানা থাকে, তবে সরাসরি রিটার্ন)
    if (dp[n]) return dp[n]; //
    
    // ৪. আরেকটি Base-Case (০ নম্বর সিঁড়িতে থাকার উপায় ১টি - চুপচাপ দাঁড়িয়ে থাকা)
    if (n == 0) return 1; //
    
    int res = 0; //
    
    // ৫. Recursion Process (১ থেকে ৬ ধাপের সব উপায়ের যোগফল)
    for (int i = 1; i <= 6; i++) {
        res += ways(n - i); //
        res %= MOD;         //
    }
    
    // ৬. Memoization Checking (ভবিষ্যতের জন্য উত্তরটি সেভ করে রাখা)
    dp[n] = res; //
    
    // ৭. Return Value
    return res; //
}
```

---

### 🎨 Why this works? (The Visualized Magic)

যদি তুমি `ways(5)` কল করো, তবে সেটি ভেতরের লুপের জন্য `ways(4)`, `ways(3)` ইত্যাদি কল করবে। আবার যখন `ways(4)` কল হবে, সেও কিন্তু `ways(3)` কে কল করবে।

- **নরমাল রিকার্সনে:** `ways(3)` এর ক্যালকুলেশন দুইবার করা হতো।
    
- **DP তে:** প্রথমবার `ways(3)` এর মান বের হয়ে `dp[3]` এ সেভ হয়ে যাবে। দ্বিতীয়বার যখন রিকার্সন সেখানে আসবে, সে `if (dp[n])` কন্ডিশন সত্য পেয়ে এক পলকেই রিটার্ন করে দেবে! ফলে এক্সপোনেনশিয়াল টাইম কমপ্লেক্সিটি $O(6^N)$ থেকে সোজা **$O(N)$** এ নেমে আসে।
    

---

### 📝 Metadata & Tags

_Created on: 2026-05-18_

_Tags: #Dynamic_Programming #Basic_DP #Memoization #Algorithms #C++ #ObsidianNotes_