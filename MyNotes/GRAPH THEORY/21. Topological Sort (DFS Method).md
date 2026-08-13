
**Definition:** এটি একটি **DAG (Directed Acyclic Graph)**-এর লিনিয়ার অর্ডারিং যেখানে প্রতিটি এজ $u \to v$-এর জন্য $u$ সবসময় $v$-এর আগে থাকে। এটি মূলত গ্রাফের ডিপেন্ডেন্সি বা কাজের ক্রম নির্ধারণে ব্যবহৃত হয়।

### 📌 লজিক ও কার্যপদ্ধতি (Logic & Mechanism)

- **Post-order Traversal:** DFS চলাকালীন একটি নোডের সব নেইবার (neighbors) ভিজিট করা শেষ হলে তবেই সেই নোডটিকে আমাদের লিস্টে (`tps`) যুক্ত করা হয়।
- **Reverse Ordering:** যেহেতু আমরা নোডগুলোকে একদম শেষের দিক থেকে (leaf to root) ট্রাভার্স শেষ করে লিস্টে রাখছি, তাই পুরো লিস্টটিকে শেষে `reverse` করে সঠিক টপোলজিক্যাল অর্ডার পাওয়া যায়।
- **Visited Array:** কোনো নোড যেন একাধিকবার প্রসেস না হয়, সেজন্য একটি `vis` অ্যারে ব্যবহার করা হয়।
---

### 💻 Implementation Snippet (DFS Based)

```cpp
vector<int> adj[N], tps;
vector<int> vis(N);

/**
 * u: current node
 * Mechanism: Post-order insertion and then reverse
 */
void dfs(int u) {
    vis[u] = 1; 
    for (int v : adj[u]) {
        if (!vis[v]) {
            dfs(v);
        }
    }
    // নেইবারদের ভিজিট শেষ হওয়ার পর ভেক্টরে রাখা
    tps.emplace_back(u); 
}

void topological_sort(int n) {
    tps.clear();
    fill(vis.begin(), vis.end(), 0);
    
    // গ্রাফের প্রতিটি বিচ্ছিন্ন অংশ (component) হ্যান্ডেল করার জন্য
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            dfs(i);
        }
    }
    
    // স্ট্যাকের মতো লজিক পূর্ণ করতে রিভার্স করা
    reverse(tps.begin(), tps.end()); 
}
```

---

### 📝 Strategic Insights

- **Simple & Intuitive:** Kahn's Algorithm-এর তুলনায় DFS মেথড কোড করা সহজ এবং ছোট।
- **Indegree 0 Handling:** শুরুতে একাধিক নোডের ইনডিগ্রি $0$ থাকলেও সমস্যা নেই; `for` লুপের মাধ্যমে সেগুলো অটোমেটিক কভার হয়ে যায়।
- **Memory Efficiency:** বাড়তি কোনো কিউ বা প্রায়োরিটি কিউ লাগে না, শুধুমাত্র রিকার্শন স্ট্যাক এবং একটি ভিজিটেড অ্যারে দিয়েই কাজ চলে।
- **Complexity:** $O(V + E)$, যেখানে $V$ হলো নোড এবং $E$ হলো এজ সংখ্যা।

---

_Created on: 2026-05-05_
