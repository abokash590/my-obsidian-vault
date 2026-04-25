**Definition:** একটি ট্রির যেকোনো দুটি নোড $a$ এবং $b$ এর মধ্যবর্তী পথের সকল এজের (Edge) XOR সাম নির্ণয় করার দ্রুততম পদ্ধতি।

### 📌 The Concept (Property)

XOR অপারেশনের একটি বিশেষ ধর্ম হলো: $x \oplus x = 0$। যদি $xr[u]$ হয় রুট (node 1) থেকে নোড $u$ পর্যন্ত পাথ XOR, তবে:

$$PathXOR(a, b) = xr[a] \oplus xr[b]$$

এখানে $LCA(a, b)$ পর্যন্ত পাথটি দুইবার XOR হওয়ায় ভ্যানিশ হয়ে যায়, ফলে শুধু $a$ থেকে $b$ এর পথের XOR অবশিষ্ট থাকে।

---

### 💻 Implementation (Using Your Template)

C++

```cpp
#include <bits/stdc++.h>
using namespace std;

#define N 200005

// Important: adj stores {destination, weight}
vector<pair<int, int>> adj[N];
vector<int> xr(N);

// DFS to calculate prefix XOR from root (node 1)
void dfs(int u, int p) {
    for(auto c : adj[u]) {
        int v = c.first, w = c.second;
        if(v != p) {
            xr[v] = xr[u] ^ w; // The Core Logic
            dfs(v, u);
        }
    }
}

void solve() {
    int n; cin >> n;
    // Clearing for multiple test cases
    for(int i = 0; i <= n; i++) adj[i].clear();

    for(int i = 1; i < n; i++) {
        int u, v, w; cin >> u >> v >> w;
        adj[u].emplace_back(v, w);
        adj[v].emplace_back(u, w);
    }

    // Preprocessing
    xr[1] = 0;
    dfs(1, -1);

    int a, b; cin >> a >> b;
    // Querying: O(1)
    cout << (xr[a] ^ xr[b]) << endl;
}
```

---

### 📝 Key Insights for Obsidian:

- **Preprocessing:** $O(N)$ - DFS এর মাধ্যমে একবারই সব নোডের প্রিফিক্স XOR ক্যালকুলেট করা হয়।
    
- **Querying:** $O(1)$ - যেকোনো দুটি নোডের জন্য রেজাল্ট শুধু `xr[a] ^ xr[b]`।
    
- **Important Note:** এই লজিকটি কেবল **ট্রির (Tree)** ক্ষেত্রে কাজ করবে, কারণ ট্রিতে যেকোনো দুটি নোডের মধ্যে ইউনিক (Unique) পাথ থাকে। গ্রাফে সাইকেল থাকলে এটি কাজ করবে না।
    

### 🔗 Related Topics:

- [[LCA (Lowest Common Ancestor)]]
    
- [[Prefix Sum Array]]
    
- [[Bitwise Operators]]
    

---

_Created on: 2026-04-25__Tags: #Graph #Tree #XOR_