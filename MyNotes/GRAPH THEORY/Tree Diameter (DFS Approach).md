**Definition:** একটি ট্রি-র যেকোনো দুটি নোডের মধ্যে সর্বোচ্চ দূরত্বকে ওই ট্রি-র **Diameter** বলা হয়।

### 📌 Approach (Two-DFS Method)

১. যেকোনো একটি র‍্যান্ডম নোড `u` থেকে DFS/BFS চালিয়ে সবচেয়ে দূরের নোড `x` খুঁজে বের করো।

২. এবার সেই `x` নোড থেকে পুনরায় আরেকটি DFS/BFS চালিয়ে সবচেয়ে দূরের নোড `y` খুঁজে বের করো।

৩. `x` এবং `y` এর মধ্যবর্তী দূরত্বই হলো ওই ট্রি-র **Diameter**।

> [!NOTE]
> 
> এই পদ্ধতিটি কেবল **Trees** (acyclic graphs) এর জন্য কাজ করে। যদি গ্রাফে সাইকেল থাকে বা নেগেটিভ এজ থাকে, তবে এটি কাজ করবে না।

---

### 💻 C++ Implementation (Using Your Template)


```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define pb push_back
#define nl '\n'
#define all(x) x.begin(), x.end()

const int N = 2e5 + 5;
vector<int> adj[N];
int dist[N];

void dfs(int u, int p, int d) {
    dist[u] = d;
    for (auto v : adj[u]) {
        if (v != p) {
            dfs(v, u, d + 1);
        }
    }
}

void solve(int tc) {
    int n;
    cin >> n;
    
    // Clear adjacency list for multiple test cases
    for (int i = 0; i <= n; i++) adj[i].clear();

    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].pb(v);
        adj[v].pb(u);
    }

    if (n == 1) {
        cout << 0 << nl;
        return;
    }

    // First DFS to find one end of the diameter
    dfs(1, -1, 0);
    int far_node = 1;
    for (int i = 1; i <= n; i++) {
        if (dist[i] > dist[far_node]) {
            far_node = i;
        }
    }

    // Second DFS from far_node to find the actual diameter
    dfs(far_node, -1, 0);
    int diameter = 0;
    for (int i = 1; i <= n; i++) {
        diameter = max(diameter, dist[i]);
    }

    cout << diameter << nl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int t = 1;
    // cin >> t; // Uncomment if there are multiple test cases
    for (int i = 1; i <= t; i++) {
        solve(i);
    }
    return 0;
}
```

---

### 📝 Key Observations for Competitive Programming:

- **Time Complexity:** $O(V + E)$, যেহেতু আমরা মাত্র দুবার ট্রি ট্রাভার্স করছি।
    
- **Tree Center:** ডায়ামিটারের মধ্যবর্তী নোডটিই হলো ট্রির সেন্টার। ডায়ামিটার যদি $D$ হয়, তবে রেডিয়াস (Radius) হবে $\lceil D/2 \rceil$।
    
- **DP Approach:** ডায়ামিটার বের করার আরেকটি পদ্ধতি হলো **Tree DP**, যা বিশেষ করে যখন এজ ওয়েট (Edge Weight) দেওয়া থাকে বা ডাইনামিকালি কিছু ক্যালকুলেট করতে হয় তখন কাজে লাগে।
    

---

### 🔗 Related Topics:

- [[All-Nodes Maximum Distance]]

---

_Created on: 2026-04-25_