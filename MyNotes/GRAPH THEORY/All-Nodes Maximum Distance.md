# 🌳 All-Nodes Maximum Distance

**Definition:** একটি ট্রির প্রতিটি নোড $u$ এর জন্য, ওই নোড থেকে সবচেয়ে দূরে অবস্থিত নোডের দূরত্ব বের করা।

### 📌 The Concept (Diameter-based Approach)

একটি ট্রির যেকোনো নোড থেকে সবচেয়ে দূরের নোডটি অবশ্যই ওই ট্রির **Diameter** এর যেকোনো একটি প্রান্তবিন্দু (Endpoint) হবে।

১. ট্রির ডায়ামিটারের দুটি প্রান্তবিন্দু $x$ এবং $y$ খুঁজে বের করো।

২. প্রতিটি নোড $u$ এর জন্য:

$$\text{Max Distance}(u) = \max(\text{dist}(u, x), \text{dist}(u, y))$$

---

### 💻 Implementation (Using Your Style)


```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define nl '\n'
#define N 200005

vector<int> adj[N];
int lvl[N];

// তোমার প্র্যাকটিস করা DFS ফাংশন
void dfs_lvl(int u, int par, int d) {
    lvl[u] = d;
    for (int v : adj[u]) {
        if (v != par) dfs_lvl(v, u, d + 1);
    }
}

void solve(int tc) {
    int n; 
    if(!(cin >> n)) return;

    // ক্লিয়ার অ্যাডজাসেন্সি লিস্ট (For Multiple Test Cases)
    for(int i = 0; i <= n; i++) adj[i].clear();

    for(int i = 1; i < n; i++) {
        int u, v; cin >> u >> v;
        adj[u].emplace_back(v);
        adj[v].emplace_back(u);
    }

    if(n == 1) {
        cout << 0 << nl;
        return;
    }

    // ১. যেকোনো নোড থেকে DFS চালিয়ে ১ম এন্ডপয়েন্ট (mxi1) বের করা
    dfs_lvl(1, -1, 0);
    int mxi1 = 1, mx = -1;
    for(int i = 1; i <= n; i++) {
        if(lvl[i] > mx) {
            mx = lvl[i];
            mxi1 = i;
        }
    }

    // ২. ১ম এন্ডপয়েন্ট থেকে DFS চালিয়ে ২য় এন্ডপয়েন্ট (mxi2) বের করা এবং ডিস্ট্যান্স সেভ করা
    dfs_lvl(mxi1, -1, 0);
    vector<int> dist_from_x(n + 1);
    int mxi2 = 1; mx = -1;
    for(int i = 1; i <= n; i++) {
        dist_from_x[i] = lvl[i];
        if(lvl[i] > mx) {
            mx = lvl[i];
            mxi2 = i;
        }
    }

    // ৩. ২য় এন্ডপয়েন্ট থেকে DFS চালিয়ে ডিস্ট্যান্স বের করা
    dfs_lvl(mxi2, -1, 0);

    // ৪. রেজাল্ট প্রিন্ট: max(dist_from_x, dist_from_y)
    for(int i = 1; i <= n; i++) {
        cout << max(dist_from_x[i], lvl[i]) << (i == n ? "" : " ");
    }
    cout << nl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    int t = 1;
    // cin >> t;
    while(t--) solve(t);
    return 0;
}
```

---

### 📝 Key Insights for Obsidian:

- **Time Complexity:** $O(3 \times (V+E)) \approx O(V+E)$।
    
- **Space Complexity:** $O(V)$ একটি এডিশনাল ভেক্টর এবং রিকারশন স্ট্যাকের জন্য।
    
- **Property:** ট্রির যেকোনো নোড থেকে ফারদলেস্ট নোডটি সবসময় একটি লিফ নোড (Leaf Node) হবে।
    

---

_Created on: 2026-04-25_