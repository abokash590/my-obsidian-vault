---

## tags: [cp/block-cancellation, cp/sorted-array, cp/combinatorics, cp/gap-argument] status: solved-with-help difficulty: Div2-C date: 2026-07-17 source: Codeforces Round 1108 (Div. 2) link: https://codeforces.com/contest/2246/problem/C confidence: 🟡

# C. 0mar and Alternating Sums

## Problem (সংক্ষেপে)

Non-decreasing array $a$ ($n \le 2\times10^5$), প্রতিটা element $-1$ অথবা positive integer ($1$ থেকে $10^9$)। কতগুলো subsequence (empty সহ) আছে যাদের alternating sum ($b_1-b_2+b_3-\dots$) $=0$? Answer mod $10^9+7$।

## এক লাইনে কী miss করেছিলাম

প্রথমে $n \le 2\times10^5$ আর value $\le 10^9$ দেখে ভয় পেয়ে গেছিলাম, ভেবেছিলাম DP লাগবে। আসলে দরকার ছিল **একই value-র copy গুলো একে অপরকে cancel করে** — এই observation-টা প্রথমে চোখে পড়েনি।

## Trigger phrase (পরের বার এই lines দেখলে এই note মনে করবো)

- "non-decreasing array" + "alternating sum = 0" গোনার কথা বলছে
- array-তে একটা special value (এখানে $-1$) বাকি সব থেকে আলাদা category-র (sign আলাদা)
- $n$ বড় কিন্তু value $10^9$ পর্যন্ত, তাও $O(n\log n)$ চাওয়া হচ্ছে → মানে DP-over-value সম্ভব না, closed-form/combinatorics খুঁজতে হবে

## যেই trick টা কাজে লাগলো

**Step 1 — Block cancellation:** Non-decreasing array-কে equal-value block-এ ভাগ করো। একটা block-এ $c_i$ টা element থাকলে: $$#(\text{odd সংখ্যক বাছার way}) = #(\text{even সংখ্যক বাছার way}) = 2^{c_i-1}$$ **Proof (bijection):** একটা fixed element toggle করলে (add/remove) subset-এর size-এর parity flip হয়ে যায়, আর এটা reversible — তাই odd আর even subset-এর মধ্যে perfect one-to-one pairing।

**Step 2 — কেন এটা matter করে:** একই block-এর ভেতরের odd/even elements নিজেদের মধ্যেই cancel হয়ে যায় ($+v-v=0$ pattern), তাই final alternating sum শুধু নির্ভর করে "কোন কোন block-এ odd সংখ্যক নিয়েছি" তার উপর — block-এর ভেতরে ঠিক কোনগুলো নিয়েছি সেটা matter করে না।

যেহেতু active (odd) আর inactive (even) দুটোর way-ই সমান ($2^{c_i-1}$), তাই: $$\text{Answer} = 2^{,n-r}\times N$$ যেখানে $r$ = distinct block সংখ্যা ($-1$ সহ), $N$ = "distinct values দিয়ে alternating-sum-zero pattern" সংখ্যা।

**Step 3 — reduced problem ($N$ বের করা):** Sorted distinct values (একটামাত্র $-1$ + strictly increasing positives) থেকে zero-sum subset গোনা।

- **even length ($k\ge2$):** পর পর জোড়ায় ভাগ করলে প্রতিটা জোড়া strictly negative (কারণ sorted, বড় সংখ্যা বিয়োগ হচ্ছে) → sum কখনো 0 হয় না। শুধু $k=0$ কাজ করে।
- **length 1:** কোনো value 0 না, তাই কাজ করে না।
- **length 3 ($a,b,c$):** sum $=a+(c-b)$। $(c-b)>0$ সবসময়, তাই $a$ কে অবশ্যই negative হতে হবে — মানে $a=-1$, আর $c-b=1$ (consecutive integer pair)।
- **length $\ge5$:** ২টা বা বেশি positive "gap" লাগে (প্রতিটা $\ge1$), কিন্তু debt মাত্র $1$ ($-1$ থেকে) — তাই always overshoot, কখনো 0 হয় না।

$$N = 1 + (\text{কতগুলো } v \text{ আছে যেন } v+1\text{-ও present})$$

## Alternative approach (case-split on $-1$ এর parity) — এটাই বেশি natural লেগেছিলো

- $-1$ even বার নিলে → net contribution 0 → positive অংশ থেকেও 0 লাগবে
- $-1$ odd বার নিলে → net contribution $-1$ → positive অংশ থেকে ঠিক $+1$ লাগবে (শুধু consecutive pair $(v,v+1)$ দিয়েই সম্ভব: $-v+(v+1)=1$)
- Answer $=2^{m-1}\cdot2^{N_{pos}-k}\cdot1 ;+; 2^{m-1}\cdot2^{N_{pos}-k}\cdot A = 2^{n-r}(A+1)$ — একই formula।

## আমার ভুল ধারণা কী ছিল

Length 5, 7 ইত্যাদি odd length-এও হয়তো solution থাকতে পারে ভেবেছিলাম। আসল কারণ যেটা miss করেছিলাম: প্রতিটা "gap" (দুই consecutive positive value-এর মাঝের ব্যবধান) কমপক্ষে $1$, আর $-1$ থেকে পাওয়া debt-ও ঠিক $1$ — তাই ১টার বেশি gap মানেই debt-এর চেয়ে বেশি "ফেরত" চলে আসে, যা কখনো ঠিক 0 বানাতে পারে না। এই "debt vs gap≥1" argument-টা আগে থেকে মাথায় না রাখলে পরের বার একই জায়গায় আটকাবো।

## Final Algorithm (O(n log n) per test case)

1. Array থেকে blocks বানাও ($m$ = $-1$ এর count, positive distinct values sorted পাওয়া যায় বিনা extra কাজে যেহেতু array আগে থেকেই sorted)
2. $r$ = distinct block সংখ্যা
3. $N = 1$; যদি $m>0$, sorted positive distinct values-এর adjacent pair check করো ($v_{i+1}=v_i+1$ হলে $N{+}{+}$)
4. Answer $= N \times 2^{n-r} \bmod (10^9+7)$

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const long long MOD = 1e9 + 7;

long long power(long long b, long long e, long long m) {
    b %= m; long long r = 1;
    while (e > 0) {
        if (e & 1) r = r * b % m;
        b = b * b % m;
        e >>= 1;
    }
    return r;
}

int main() {
    int t; scanf("%d", &t);
    while (t--) {
        int n; scanf("%d", &n);
        vector<long long> a(n);
        for (auto &x : a) scanf("%lld", &x);

        long long m = 0; int idx = 0;
        while (idx < n && a[idx] == -1) { m++; idx++; }

        vector<long long> distinctVals;
        int r = (m > 0 ? 1 : 0);
        while (idx < n) {
            long long v = a[idx];
            distinctVals.push_back(v);
            r++;
            while (idx < n && a[idx] == v) idx++;
        }

        long long N = 1;
        if (m > 0) {
            for (size_t i = 0; i + 1 < distinctVals.size(); i++)
                if (distinctVals[i + 1] == distinctVals[i] + 1) N++;
        }

        long long exponent = n - r;
        long long ans = N % MOD * power(2, exponent, MOD) % MOD;
        printf("%lld\n", ans);
    }
    return 0;
}
```

## Related problems

[[block-cancellation-trick]] [[sorted-array-subsequence-sum]]

## Confidence

🟡 — trick আর algorithm মনে আছে, কিন্তু "gap≥1 overshoot" argument-টা নিজে থেকে scratch থেকে আবার derive করতে গেলে হয়তো আটকাবো। ৩ দিন পর Day-3 revision-এ শুধু title দেখে trick মনে করার চেষ্টা করবো।