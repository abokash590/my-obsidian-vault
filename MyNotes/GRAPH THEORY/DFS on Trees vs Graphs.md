**Related Topics:** `[[DFS]]`, `[[Tree Traversal]]`, `[[Level Calculation]]`, `[[Cycle Detection]]`

ট্রি (Tree) হলো একটি বিশেষ ধরনের গ্রাফ যেখানে কোনো **Cycle** থাকে না এবং সব নোড কানেক্টেড থাকে। এই বৈশিষ্ট্যের কারণে ট্রিতে DFS চালানো সাধারণ গ্রাফের চেয়ে কিছুটা সহজ।

---

### 🚀 DFS Implementation on a Tree

একটি ট্রিতে কোনো সাইকেল থাকে না বলে আমাদের `vis[]` অ্যারে ব্যবহার না করলেও চলে। আমরা শুধু বর্তমান নোডের **Parent** কে তা মনে রাখলেই ইনফিনিট লুপ এড়াতে পারি।

```cpp
#define N  100005
vector<int> adj[N];
void dfs(int u,int par) {
    for (int v:adj[u]) {
        if(v!=par)dfs(v,u);
    }
}
```

---

### 📏 Level Calculation: Tree vs Non-Tree Graph

একটি খুবই গুরুত্বপূর্ণ কনসেপ্ট হলো DFS দিয়ে **Level** বা **Shortest Distance** বের করা।

#### **১. ট্রির ক্ষেত্রে (When it's a Tree):**

ট্রিতে যেকোনো দুটি নোডের মধ্যে **একটি মাত্র ইউনিক পাথ (Unique Path)** থাকে। তাই আপনি DFS দিয়ে যে পথেই যান না কেন, সেটিই হবে ওই নোডের শর্টেস্ট পাথ বা লেভেল।

- **সিদ্ধান্ত:** DFS দিয়ে ট্রির লেভেল বের করা সম্ভব এবং সঠিক।
#### **২. সাধারণ গ্রাফের ক্ষেত্রে (When it's a Non-Tree):**

সাধারণ গ্রাফে (যেখানে সাইকেল থাকতে পারে) একটি নোডে যাওয়ার একাধিক পথ থাকতে পারে।

- **সমস্যা:** DFS একটি পাথ ধরে একদম শেষ পর্যন্ত চলে যায়। হতে পারে সে অনেক ঘুরে কোনো নোডে পৌঁছালো, অথচ তার পাশেই হয়তো একটি ছোট পথ ছিল।
- **কেন কাজ করে না:** DFS শর্টেস্ট পাথ গ্যারান্টি দেয় না। এটি শুধু একটি পাথ খুঁজে পায়।
- **সমাধান:** নন-ট্রি বা সাধারণ গ্রাফে লেভেল বা শর্টেস্ট ডিস্টেন্স বের করার জন্য সব সময় **BFS** বা **Dijkstra** ব্যবহার করতে হয়।

---

### 📊 Summary Table for Obsidian

|**Feature**|**DFS on Tree**|**DFS on Non-Tree Graph**|
|---|---|---|
|**Visited Array**|Not mandatory (Parent check is enough)|**Mandatory** (To avoid infinite loops)|
|**Level/Distance**|Accurate Shortest Distance|**Inaccurate** (Does not guarantee shortest path)|
|**Best For**|Path finding, Subtree problems|Connectivity, Cycle Detection|

---
## Related Note:
- [[Level of Tree (DFS)]]
- [[Subtree Size in a Tree]]
- 