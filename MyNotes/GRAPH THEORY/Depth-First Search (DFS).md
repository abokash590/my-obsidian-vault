# DFS Introduction

**Related Topics:** `[[Graph Traversal]]`, `[[Recursion]]`, `[[Stack]]`, `[[Connectivity]]`

**DFS (Depth First Search)** হলো গ্রাফ বা ট্রি ট্রাভার্সাল করার একটি অ্যালগরিদম যা "গভীরে যাওয়ার" নীতিতে কাজ করে। এটি একটি নির্দিষ্ট সোর্স নোড থেকে শুরু করে একটি পাথ ধরে যতদূর সম্ভব গভীরে যায় এবং যখন আর কোনো নতুন নেইবার পাওয়া যায় না, তখন এটি ফিরে আসে (**Backtrack**) এবং অন্য পাথ খোঁজে।

---

### 🔍 মূল বৈশিষ্ট্য (Core Logic)

১. **নীতি (Policy):** Go deep as much as possible, then backtrack.
২. **ডাটা স্ট্রাকচার:** এটি ইন্টারনালি **Stack** (রিকার্সন স্ট্যাক) ব্যবহার করে কাজ করে।
৩. **ভিজিটেড অ্যারে:** একই নোড যাতে বারবার ভিজিট করে ইনফিনিট লুপ তৈরি না করে, সেজন্য একটি `vis[]` অ্যারে ব্যবহার করা হয়।

---

### 🚀 Optimized DFS Template

আপনার প্র্যাকটিস করা কোডটি সিপি-র জন্য আরও ক্লিন করে নিচে দেওয়া হলো:


```cpp
#define N  100005
vector<int> adj[N];
vector<int>vis(N),path;
void dfs(int u) {
    vis[u]=1;
    path.push_back(u);
    for (int v:adj[u]) {
        if (!vis[v]) {
            dfs(v);
        }
    }
}
```


---

### ⏱️ Complexity Analysis

- **Time Complexity:** $O(V + E)$
    - $V$ = মোট নোড (Vertices), $E$ = মোট এডজ (Edges)।
    - প্রতিটি নোড এবং এডজ ঠিক একবার করে প্রসেস হয়।
- **Space Complexity:** $O(V)$
    - `vis` অ্যারে এবং রিকার্সন স্ট্যাকের জন্য।

---

### **📂 Obsidian Note Checklist**

আপনার নোটের জন্য এই পয়েন্টগুলো লিখে রাখুন:
1. **Always clear global arrays** in multi-test case problems.
2. **Use `1-based indexing`** for clarity in most competitive programming problems.
3. **Check for disconnected components** using a loop.
4. **Tree identification:** If `edges == n - 1` and the graph is connected (1 component), it's a tree.
---

### 💡 ব্যবহারের ক্ষেত্র (Use Cases)

- **Connectivity Check:** গ্রাফের সবগুলো নোড কানেক্টেড কি না দেখা।
- **Cycle Detection:** গ্রাফে কোনো লুপ বা সাইকেল আছে কি না তা বের করা।
- **Path Finding:** এক নোড থেকে অন্য নোডে যাওয়ার পথ খুঁজে বের করা (তবে এটি শর্টেস্ট পাথ গ্যারান্টি দেয় না)।
- **Topological Sort:** ডিরেক্টেড অ্যাসাইক্লিক গ্রাফ (DAG) সাজানো।

---

### ⚠️ সিপিতে মনে রাখার মতো বিষয় (Best Practices)

- **Resetting:** প্রতিটি টেস্ট কেসের শুরুতে `adj[i].clear()`, `vis[i] = 0` এবং `path.clear()` করতে ভুলবেন না।
- **Disconnected Components:** গ্রাফ যদি বিচ্ছিন্ন হয়, তবে সব নোড কভার করতে `main` এ একটি লুপ চালান:
    ```
    for(int i = 1; i <= n; i++) {
        if(!vis[i]) dfs(i);
    }
    ```

- **Stack Overflow:** যদি গ্রাফের গভীরতা খুব বেশি হয় ($10^5$ এর উপরে), তবে রিকার্সন লিমিট বা ইটারেটিভ DFS ব্যবহার করা নিরাপদ।

---
## Related Note:
- [[DFS on Trees vs Graphs]]
- [[Level of Tree (DFS)]]
- [[Tree Diameter (DFS Approach)]]
- 