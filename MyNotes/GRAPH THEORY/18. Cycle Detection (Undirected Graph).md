**Definition:** একটি গ্রাফে সাইকেল আছে বলা হয় যদি কোনো একটি নোড থেকে যাত্রা শুরু করে পুনরায় সেই নোডেই ফিরে আসা যায় (একই এজ দুইবার ব্যবহার না করে)।

### 📌 Key Observations

১. **Back-Edge Concept:** DFS চলাকালীন যদি এমন কোনো নোড `v` পাওয়া যায় যা অলরেডি ভিজিটেড (`vis[v] == 1`) এবং সেটি বর্তমান নোডের প্যারেন্ট বা `prev` নয়, তবেই সেখানে একটি সাইকেল বিদ্যমান।
২. **Tree vs Graph:** একটি কানেক্টেড গ্রাফে যদি $N$ টি নোড এবং $N-1$ টি এজ থাকে, তবে সেটি একটি **Tree** (কোনো সাইকেল নেই)। যদি এজের সংখ্যা $\ge N$ হয়, তবে অবশ্যই অন্তত একটি সাইকেল আছে।
৩. **Parent Constraint:** আনডাইরেক্টেড গ্রাফে সাইকেল চেক করার সময় `if(v != prev)` কন্ডিশনটি অত্যন্ত গুরুত্বপূর্ণ। কারণ সরাসরি প্যারেন্টে ফিরে যাওয়া সাইকেল হিসেবে গণ্য হয় না।
৪. **Disconnected Components:** গ্রাফ যদি ডিসকানেক্টেড হয়, তবে প্রতিটি কম্পোনেন্টের জন্য আলাদাভাবে DFS চালাতে হবে।
৫. **DFS state:** সাইকেল ডিটেকশনের পর চাইলে দ্রুত `return` করে দেওয়া ভালো যেন অপ্রয়োজনীয় ট্রাভার্সাল না হয়।

---

### 💻 Implementation (Your Practiced Style)


```cpp
vector<int> adj[N];
vector<int>vis(N);
bool ok=0;
void dfs(int u,int prev){
    if(vis[u]){
        ok=1;return;
    }vis[u]=1;
    for (int v:adj[u]){
        if(v!=prev)dfs(v,u);
    }
}
```

## ✨(If there are multi connected graph)

**Use this in code :**

```cpp
for(int i=1;i<=n;i++)if(!vis[i])dfs(i,1);
```

---

### 📝 Strategic Tips for Competitive Programming:

- **Early Exit:** তোমার কোডে `if(ok) return;` লুপের ভেতরে যোগ করলে বড় গ্রাফে সময় বাঁচবে।
- **Directed Graph Difference:** মনে রেখো, ডাইরেক্টেড গ্রাফের সাইকেল ডিটেকশন পদ্ধতি আলাদা (সেখানে ৩টি স্টেট—Unvisited, Visiting, Visited—প্রয়োজন হয়)। তোমার এই কোডটি শুধু **Undirected Graph**-এর জন্য।
- **BFS Alternative:** এটি BFS দিয়েও করা সম্ভব, সেখানেও আমাদের `parent` অ্যারে মেইনটেইন করতে হয়।
---

### 🔗 Related Topics:


---

_Created on: 2026-04-29_