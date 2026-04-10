BFS হলো গ্রাফ ট্রাভার্সাল করার একটি পদ্ধতি যেখানে একটি নির্দিষ্ট নোড থেকে শুরু করে তার চারপাশের সব নেইবারদের (Neighbors) আগে ভিজিট করা হয়। এটি অনেকটা পানিতে পাথর ছুড়লে তৈরি হওয়া ঢেউয়ের মতো কাজ করে।

---

### **🔍 মূল টার্মিনোলজি (Key Terms)**

১. **Source (উৎস):** যে নোড থেকে গ্রাফ ট্রাভার্সাল শুরু হয়। একে 'Root' বা 'Starting Point'-ও বলা হয়। আপনার কোডে `src = 3` হলো সোর্স।

২. **Visited (ভিজিটেড):** একটি অ্যারে যা ট্র্যাক রাখে কোন কোন নোড আমরা অলরেডি দেখে ফেলেছি। এটি ব্যবহার করা হয় যাতে একই নোড বারবার ভিজিট করে আমরা **Infinite Loop**-এ না পড়ি।

- `vis[v] = 1` মানে নোডটি অলরেডি কিউতে ঢুকে গেছে বা ভিজিট করা শেষ।

৩. **Level (লেভেল):** সোর্স থেকে কোনো নোড কত দূরে আছে তার হিসাব।
- সোর্সের নিজের লেভেল সবসময় `0` হয়।
- সোর্সের সরাসরি নেইবারদের লেভেল হয় `1`, তাদের নেইবারদের লেভেল হয় `2` — এভাবেই চলতে থাকে।

---

### **💡 গুরুত্বপূর্ণ পর্যবেক্ষণ (Observation)**

> [!important] **The Shortest Path Property**
> 
> **Unweighted Graph** (যেখানে সব এডজ-এর ওয়েট সমান বা ১)-এর ক্ষেত্রে, **Source থেকে অন্য যেকোনো নোডের সর্বনিম্ন দূরত্ব (Shortest Distance) হলো ওই নোডটির Level Number।** > BFS সবসময় লেভেল বাই লেভেল আগায়, তাই এটি প্রথমবার যখনই কোনো নোডে পৌঁছায়, সেটিই হয় সেই নোডের সবচেয়ে কম দূরত্ব।

---

### **🛠️ BFS Code Template (Your Practice Based)**

নিচে আপনার করা কোডটিকে একটি স্ট্যান্ডার্ড টেমপ্লেট আকারে দেওয়া হলো:

C++

```cpp
vector<int> bfs(int src, vector<int> adj[], int n){
    vector<int>lvl(n+1,-1),vis(n+1,0);
    queue<int>q;
    lvl[src]=0;
    vis[src]=1;
    q.push(src);
    while(!q.empty()){
        int u=q.front();q.pop();
        for(auto v:adj[u]){
            if(!vis[v]){
                vis[v]=1;
                lvl[v]=lvl[u]+1;
                q.push(v);
            }
        }
    }
    return lvl;
}
```

---

### **⏱️ Complexity Analysis**

- **Time Complexity:** $O(V + E)$
    (এখানে $V$ হলো নোড সংখ্যা এবং $E$ হলো এডজ সংখ্যা। আমরা প্রতিটি নোড এবং এডজ একবার করে দেখি।)
- **Space Complexity:** $O(V)$
    (Adjacency list এবং `vis`, `lvl` অ্যারে রাখার জন্য।)
---

# Related Topic
- [[Is_Tree]]
- [[Is_Connected]]
- [[Number of Connected Graph]]
- [[Grid Traversal (dx, dy)]]
- [[Multi-Source BFS]]
- [[Shortest Path (BFS)]]
- 