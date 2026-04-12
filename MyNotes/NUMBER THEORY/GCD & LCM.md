# 📌 Number Theory: GCD & LCM

**Tags:** #cp #math #number-theory

### 1. Basic Definition

$GCD(a, b)$ হলো সেই বৃহত্তম সংখ্যা যা $a$ এবং $b$ উভয়কেই নিঃশেষে ভাগ করে।

- **Property:** $GCD(a, b) \times LCM(a, b) = |a \times b|$
    
- **Example:** $GCD(12, 18) = 6$ এবং $LCM(12, 18) = 36$। এখানে $6 \times 36 = 12 \times 18 = 216$।
    

### 2. Key Properties for CP

- **GCD of Multiple Numbers:** $GCD(a, b, c) = GCD(a, GCD(b, c))$। (এটি Associative)।
    
- **GCD & Addition:** $GCD(a, b) = GCD(a, a \pm b) = GCD(a, b \pm a)$।
    
- **Scaling:** $GCD(k \cdot a, k \cdot b) = k \cdot GCD(a, b)$।
    
- **Subarray GCD:** কোনো অ্যারের সাব-অ্যারের দৈর্ঘ্য বাড়লে GCD সবসময় **কমে** অথবা **একই থাকে**। এটি কখনো বাড়ে না।
    

### 3. GCD Stability (The LCM Rule) 🚀

যদি $a_i$ কে বদলে $m$ করতে হয় এবং প্রতিবেশীদের সাথে GCD ($g_L, g_R$) ঠিক রাখতে হয়:

- **Condition:** $m$ কে অবশ্যই $LCM(g_L, g_R)$ এর গুণিতক হতে হবে।
    
- **Why:** কারণ $m$ কে একই সাথে $g_L$ এবং $g_R$ দিয়ে বিভাজ্য হতে হবে। $LCM$ হলো সেই ক্ষুদ্রতম সাধারণ ভিত্তি।
    

### 4. Euclidean Algorithm (C++)

![Euclidean algorithm flow chart, AI generated|203](https://encrypted-tbn0.gstatic.com/licensed-image?q=tbn:ANd9GcSPaJQX2EsnjDNBkToDUdVp4wX9UrLcbwM-xcKebdIT3CNgl6LPPhpKq0QzGbnpidwhWyq3-IYebztW_S9gMeRNBX2ymcy-0Nsc2n5TqdX8yDpnqlM)

Shutterstock

C++

```cpp
// Standard GCD
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}

// Built-in (Fastest)
__gcd(a, b); // For older C++
std::gcd(a, b); // For C++17+
```

### 5. Common Examples & Tricks

- **Relatively Prime:** যদি $GCD(a, b) = 1$ হয়, তবে তাদের **Co-prime** বলা হয়।
    
- **Difference Trick:** অনেক সময় $GCD(a, b)$ বের করার চেয়ে $GCD(a, |a-b|)$ বের করা সহজ হয়।
    
- **GCD on Trees:** ট্রি-র কোনো পাথের GCD বের করতে হলে **Sparse Table** বা **Segment Tree** এর সাথে **LCA** ব্যবহার করা হয়।