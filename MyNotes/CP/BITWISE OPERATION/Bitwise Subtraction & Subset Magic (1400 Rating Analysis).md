---
Title: Bitwise Subtraction & Subset Magic (1400 Rating Analysis)
Created: 2026-05-22
Category: Competitive Programming
Topic: Bit Manipulation, Number Theory
Tags: #CP #Bit_Manipulation #XOR #Truth_Table #Codeforces #ObsidianNotes
---

# 🧠 Bitwise Subtraction & Subset Magic (1400 Rating)

## 📌 Problem Statement
Given three integers $b, c,$ and $d$. We need to find if there exists a valid integer $a$ such that it satisfies the following equation:
$$(a \mid b) - (a \ \& \ c) = d$$
If yes, output any valid $a$. Otherwise, output `-1` (or `NO`).

---

## 🚀 Key Learning 1: Eliminating Subtraction via Subset Property

Bitwise operations-এ মাইনাস ($-$) চিহ্ন থাকলে প্রবলেম অ্যানালাইসিস করা কঠিন। কিন্তু একটি ক্লাসিক আইডেন্টিটি ব্যবহার করে একে সহজে দূর করা সম্ভব:

> [!important] **The Subset XOR Identity**
> For any two integers $X$ and $Y$, if $Y$ is a strict **subset** of $X$ (i.e., wherever $Y$ has a set bit `1`, $X$ must also have a set bit `1`), then:
> $$X - Y = X \oplus Y$$

### 🔍 Proof for our Equation:
Let $X = (a \mid b)$ and $Y = (a \ \& \ c)$.
1. ধরি, কোনো একটি নির্দিষ্ট বিটে $Y = (a \ \& \ c) = 1$ হয়েছে।
2. AND (`&`) অপারেটরের নিয়ম অনুযায়ী, এটি তখনই সম্ভব যখন ওই বিটে **$a = 1$** এবং **$c = 1$** হবে।
3. এবার $X = (a \mid b)$ এর দিকে তাকালে দেখা যায়, যেহেতু আমরা নিশ্চিত যে $a = 1$, তাই $b$-এর মান যা-ই হোক না কেন, OR (`|`) অপারেটরের নিয়মে **$X = 1$ হতে বাধ্য**।

যেহেতু $Y$-এর প্রতিটা সেট বিট $X$-এ অন থাকেই ($Y \subseteq X$), তাই আমরা বিয়োগফল সরিয়ে সরাসরি **XOR (`^`)** বসাতে পারি। 



**Simplified Equation:**
$$(a \mid b) \oplus (a \ \& \ c) = d$$

---

## 📊 Key Learning 2: Bit-by-Bit Independence & Truth Table

Bitwise প্রবলেমে ৩২ বা ৬৪-বিটের পুরো সংখ্যা নিয়ে একসাথে ভাবার প্রয়োজন নেই। প্রতিটি বিট পজিশন একে অপরের থেকে সম্পূর্ণ স্বাধীন। 

ইনপুট $b_i, c_i, d_i$ এর সব কম্বিনেশনের জন্য $a_i$ এর সম্ভাব্য মান বের করার সত্যক সারণী (Truth Table):

| $b_i$ | $c_i$ | $d_i$ | Equation: $(a \mid b) \oplus (a \ \& \ c) == d$ | Possible $a_i$ | Action in Code |
| :---: | :---: | :---: | :--- | :---: | :--- |
| **0** | **0** | **0** | $(a \mid 0) \oplus (a \ \& \ 0) \to a \oplus 0 == 0 \implies a = 0$ | **0** | Do nothing (keep 0) |
| **0** | **0** | **1** | $(a \mid 0) \oplus (a \ \& \ 0) \to a \oplus 0 == 1 \implies a = 1$ | **1** | `ans += (1LL << i)` |
| **0** | **1** | **0** | $(a \mid 0) \oplus (a \ \& \ 1) \to a \oplus a == 0 \implies 0 == 0$ | **0 or 1** | Pick **0** (for minimum $a$) |
| **0** | **1** | **1** | $(a \mid 0) \oplus (a \ \& \ 1) \to a \oplus a == 1 \implies 0 == 1$ | **None** | ⚠️ **Impossible! Flag = OK = 0** |
| **1** | **0** | **0** | $(1 \mid 0) \oplus (1 \ \& \ 0) \to 1 \oplus 0 == 0 \implies 1 == 0$ | **None** | ⚠️ **Impossible! Flag = OK = 0** |
| **1** | **0** | **1** | $(1 \mid 0) \oplus (1 \ \& \ 0) \to 1 \oplus 0 == 1 \implies 1 == 1$ | **0 or 1** | Pick **0** (for minimum $a$) |
| **1** | **1** | **0** | $(1 \mid 1) \oplus (1 \ \& \ 1) \to 1 \oplus 1 == 0 \implies 0 == 0$ | **1** | `ans += (1LL << i)` |
| **1** | **1** | **1** | $(1 \mid 1) \oplus (1 \ \& \ 1) \to 1 \oplus 1 == 1 \implies 0 == 1$ | **0** | Do nothing (keep 0) |

---

## ⚠️ Key Learning 3: Critical CP Pitfalls & Bugs

> [!danger] **1. The Bit-Checking Trap (`&` operator behavior)**
> সিপ্লাসপ্লাসে `if (c & (1LL << i))` কন্ডিশনটি সত্য হলে কিন্তু `1` রিটার্ন করে না, বরং এটি **$2^i$** রিটার্ন করে। তাই একাধিক অ্যান্ড-অপারেশন একসাথে চেক করতে গেলে এটি মারাত্মক লজিক্যাল বাগ তৈরি করে।
> * **Safe Fix:** সবসময় রাইট শিফট ট্রিক ব্যবহার করবে $\to$ `((c >> i) & 1LL)`. এটি সবসময় পিওর `0` অথবা `1` রিটার্ন করে।

> [!warning] **2. Integer Overflow (`1LL` Usage)**
> প্রবলেমের লিমিট যখনই $10^9$ পার হয়ে যাবে, তখন ইনপুট ভ্যারিয়েবল (`b, c, d`), অ্যানসার ভ্যারিয়েবল (`ans`) এবং শিফটিং মাস্ক (`1LL << i`) সব জায়গায় **`long long`** ডিফাইন করা বাধ্যতামূলক। 

---

## 💻 Clean & Optimized C++ Template

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve_1400_bitmask() {
    long long b, c, d; 
    if (!(cin >> b >> c >> d)) return;
    
    bool ok = true;
    long long ans = 0LL; 
    
    // 62-bit পর্যন্ত লুপ চালানো নিরাপদ (long long range)
    for(int i = 0; i < 62; i++){
        // i-th bit এক্সট্র্যাক্ট করার সবচেয়ে বাগ-ফ্রি উপায়
        long long bi = (b >> i) & 1LL;
        long long ci = (c >> i) & 1LL;
        long long di = (d >> i) & 1LL;
        
        if (bi == 0) {
            if (ci == 0 && di == 1) {
                ans += (1LL << i); // a_i must be 1
            }
            else if (ci == 1 && di == 1) {
                ok = false; // Truth table অনুযায়ী অসম্ভব কেস
                break;
            }
        }
        else { // bi == 1
            if (ci == 0 && di == 0) {
                ok = false; // Truth table অনুযায়ী অসম্ভব কেস
                break;
            }
            else if (ci == 1 && di == 0) {
                ans += (1LL << i); // a_i must be 1
            }
        }
    }
    
    if (ok) cout << ans << "\n";
    else cout << -1 << "\n";
}