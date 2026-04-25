**Definition:** যে গ্রাফের প্রতিটি এজ (Edge) বা সংযোগের সাথে একটি নির্দিষ্ট মান বা ওজন (Weight) যুক্ত থাকে, তাকে **Weighted Graph** বলা হয়। এই ওজন সাধারণত দূরত্ব (Distance), খরচ (Cost), বা সময় (Time) নির্দেশ করে।

### 📌 Key Concept: Adjacency List with Weights

Weighted Graph-এর ক্ষেত্রে `vector<int> adj[N]` এর বদলে আমাদের এমন কিছু ব্যবহার করতে হয় যা একই সাথে গন্তব্য নোড (`v`) এবং ওজন (`w`) ধরে রাখতে পারে। এর জন্য `vector<pair<int, int>> adj[N]` সবচেয়ে আদর্শ।

---

### 💻 Implementation (Using Your Template)

C++

```cpp
vector<pair<int,int>>adj[N];
vector<int>dis(N);
void dfs(int u,int p) {
    for(auto c:adj[u]){
        int v=c.first,w=c.second;
        if(v!=p){
            dis[v]=dis[u]+w;
            dfs(v,u);
        }
    }
}
```

---

### 📝 Key Insights for Obsidian:

- **Weighted Representation:** `vector<pair<int, int>> adj[N]` ব্যবহার করা হয় যেখানে `first` হলো নোড এবং `second` হলো ওয়েট।
    
- **Complexity:** DFS বা BFS-এর মাধ্যমে ট্রাভার্স করলে টাইম কমপ্লেক্সিটি $O(V + E)$ থাকে।
    
- **Constraint Note:** যদি গ্রাফে সাইকেল থাকে বা শর্টেস্ট পাথ বের করতে হয়, তবে সাধারণ DFS-এর বদলে **Dijkstra's Algorithm** ব্যবহার করা নিরাপদ।
    

---

### 🔗 Related Topics:
- [[XOR Path Query in Trees]]
- 
- 

---

_Created on: 2026-04-25_