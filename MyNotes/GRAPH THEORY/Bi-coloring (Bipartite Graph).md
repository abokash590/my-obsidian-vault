**Definition:** একটি গ্রাফকে **Bipartite** বলা হয় যদি তার নোডগুলোকে দুটি রঙে (ধরি ১ এবং ২) এমনভাবে রঙ করা যায় যেন কোনো দুটি সংযুক্ত নোডের (Adjacent nodes) রঙ এক না হয়।

### 📌 Key Observations

১. **Odd Cycle:** একটি গ্রাফ বাই-পার্টাইট হবে যদি এবং কেবল যদি তাতে কোনো **বিজোড় দৈর্ঘ্যের সাইকেল (Odd Cycle)** না থাকে।
২. **Tree Property:** সকল **Tree** সবসময় বাই-পার্টাইট।
৩. **Disconnected Components:** গ্রাফে যদি একাধিক বিচ্ছিন্ন অংশ (Disconnected components) থাকে, তবে প্রতিটি অংশ আলাদাভাবে বাই-পার্টাইট হতে হবে।
৪. **2-Colorable:** বাই-পার্টাইট গ্রাফকে ২-কালারেবল গ্রাফও বলা হয়।

---

### 💻 Implementation (Using Your Style)

```cpp
vector<int> adj[N];
vector<int>vis(N),color(N);
bool ok=1;
void dfs(int u,int c){
    if(vis[u]){
        if(color[u]==c)return;
        ok=0;return;
    }vis[u]=1,color[u]=c;
    for (int v:adj[u]){
        dfs(v,(c==1?2:1));
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

- **Cycle Check:** তোমার কোডে `if(vis[u])` এর ভেতর `color[u] != c` চেক করাটা খুবই জরুরি। এটি মূলত গ্রাফে কোনো ব্যাক-এজ (Back-edge) আছে কি না এবং সেই ব্যাক-এজটি কোনো অড-সাইকেল তৈরি করছে কি না তা চেক করে।
- **BFS Approach:** রিকারশন ডেপথ খুব বেশি হলে (যেমন $N=10^6$) BFS দিয়ে বাই-কালারিং করা বেশি নিরাপদ।
- **Applications:** * চেক করা যে গ্রাফটি ২-কালারেবল কি না।
    - গ্রাফকে দুটি ইনডিপেন্ডেন্ট সেটে ভাগ করা।
    - Max Flow বা Matching প্রবলেমে গ্রাফ প্রিপারেশন।

---

### 🔗 Related Topics:


---

_Created on: 2026-04-29_