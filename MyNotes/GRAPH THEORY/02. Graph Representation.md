## 1. Adjacency Matrix (2D Array)

এটি একটি $V \times V$ সাইজের ম্যাট্রিক্স বা 2D অ্যারে, যেখানে `adj[u][v]` নির্দেশ করে নোড $u$ এবং $v$-এর মধ্যে কোনো এডজ আছে কি না।

### **Unweighted Graph:**

যদি এডজ থাকে তবে `1`, না থাকলে `0` স্টোর করা হয়।

```cpp
int adj[N][N];
// Edge between u and v
adj[u][v] = 1;
adj[v][u] = 1; // For undirected
```

### **Weighted Graph:**

`1`-এর পরিবর্তে এডজ-এর ওজন বা **Weight** স্টোর করা হয়। যদি এডজ না থাকে, তবে একটি বড় মান (Infinity) রাখা হয়।


```cpp
int adj[N][N];
adj[u][v] = weight;
```

---

## 2. Adjacency List (Vector of Vectors)

এটি সবচেয়ে বেশি ব্যবহৃত পদ্ধতি। এখানে প্রতিটি নোডের জন্য একটি লিস্ট (বা ভেক্টর) থাকে, যেখানে তার সাথে যুক্ত সব নেইবারদের (Neighbors) রাখা হয়।

### **Unweighted Graph:**

```cpp
vector<int> adj[N];
adj[u].push_back(v);
adj[v].push_back(u); // For undirected
```

### **Weighted Graph:**

এখানে প্রতিটি নেইবারের সাথে তার ওয়েট রাখার জন্য `pair<int, int>` ব্যবহার করা হয়।


```cpp
vector<pair<int, int>> adj[N];
adj[u].push_back({v, weight});
```

---

## ⏱️ Time & Space Complexity Comparison

|**Operation**|**Adjacency Matrix**|**Adjacency List**|
|---|---|---|
|**Space Complexity**|$O(V^2)$|$O(V + E)$|
|**Add Edge**|$O(1)$|$O(1)$|
|**Check if $u \rightarrow v$ exists**|$O(1)$|$O(degree(u))$|
|**Find all neighbors of $u$**|$O(V)$|$O(degree(u))$|
|**Remove Edge**|$O(1)$|$O(E)$|

---

## 🤔 When to use what? (Decision Making)

### **Use Adjacency Matrix when:**

- গ্রাফটি অনেক বেশি **Dense** (অর্থাৎ এডজ সংখ্যা $V^2$ এর কাছাকাছি)।
- আপনাকে বারবার চেক করতে হয় যে দুটি নির্দিষ্ট নোডের মধ্যে এডজ আছে কি না ($O(1)$ lookup)।
- নোড সংখ্যা খুব কম (সাধারণত $V \le 5000$)। কারণ $O(V^2)$ মেমোরি লিমিট ক্রস করতে পারে।

### **Use Adjacency List when:**

- গ্রাফটি **Sparse** (অর্থাৎ এডজ সংখ্যা নোড সংখ্যার তুলনায় কম)। কম্পিটিটিভ প্রোগ্রামিংয়ে বেশিরভাগ গ্রাফই এমন থাকে।
- আপনাকে একটি নোডের সব নেইবারদের নিয়ে কাজ করতে হবে (যেমন: BFS, DFS)।
- নোড সংখ্যা অনেক বেশি ($V = 10^5$ বা তার বেশি)। এটি মেমোরি সাশ্রয়ী।
---

## 💡 Pro-Tips for Obsidian Note

১. **Memory Limit:** যদি $V = 10^5$ হয়, তবে `int adj[100000][100000]` ডিক্লেয়ার করলে **Memory Limit Exceeded (MLE)** খাবেন। সবসময় Adjacency List ব্যবহার করার চেষ্টা করুন।

২. **Self-loops & Multiple Edges:** ম্যাট্রিক্সে মাল্টিপল এডজ রাখা কঠিন (শুধু ছোট ওয়েটটি রাখা যায়), কিন্তু লিস্টে সহজেই সব এডজ রাখা যায়।

৩. **Input Style:**

```cpp
// Fast way to iterate Adjacency List
for(auto &edge : adj[u]) {
    int v = edge.first;
    int w = edge.second;
    // logic
}
```

---
