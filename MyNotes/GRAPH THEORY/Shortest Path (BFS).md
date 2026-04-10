**Related Topics:** `[[BFS]]`, `[[Graph Theory]]`, `[[Parent Array]]`

সাধারণ BFS আমাদের সোর্স থেকে সব নোডের **দূরত্ব** (Level) দেয়। কিন্তু যদি আমাদের সেই **নির্দিষ্ট পথটি (Path)** দরকার হয়, তবে আমাদের প্রতিটি নোডের "পিতা" বা **Parent** কে তা মনে রাখতে হয়।

---

### 🔍 মূল ধারণা (The Logic)

১. **Parent Array (`pre`):** BFS চলাকালীন যখনই আমরা কোনো নতুন নোড `v` ভিজিট করি, আমরা সেটির প্যারেন্ট হিসেবে `u` কে সেভ করে রাখি (`pre[v] = u`)। ২. **Backtracking:** ডেস্টিনেশন নোড `end` থেকে শুরু করে সোর্স `src` এ পৌঁছানো পর্যন্ত আমরা প্যারেন্টদের অনুসরণ করি। ৩. **Reversing:** যেহেতু আমরা শেষ থেকে শুরুতে আসি, তাই সোর্স থেকে ডেস্টিনেশন পাওয়ার জন্য ভেক্টরটিকে `reverse` করতে হয়।

---

### 🚀 Your Optimized BFS Path Template

C++

```cpp
vector<int> shortest_path(int src,int end,vector<int> adj[], int n){
    vector<int>lvl(n+1,-1),vis(n+1,0),pre(n+1),path;
    queue<int>q;
    lvl[src]=0;
    vis[src]=1;
    q.push(src);
    while(!q.empty()){
        int u=q.front();q.pop();
        for(auto v:adj[u]){
            if(!vis[v]){
                vis[v]=1;
                pre[v]=u;
                lvl[v]=lvl[u]+1;
                q.push(v);
            }
        }
    }
    path.emplace_back(end);
    while(end!=src){
        end=pre[end];
        path.emplace_back(end);
    }reverse(all(path));
    return path;
}
```

---

### ⚠️ গুরুত্বপূর্ণ সতর্কতা (Edge Cases)

- **Unreachable Node:** যদি `target` নোডে পৌঁছানো অসম্ভব হয়, তবে আপনার `while(end != src)` লুপটি ইনফিনিট লুপে পড়ে যেতে পারে। তাই আগে `vis[target]` চেক করে নেওয়া নিরাপদ।
    
- **Memory Optimization:** `lvl` অ্যারেটি যদি শুধু পাথ দরকার হয় তবে ঐচ্ছিক, কিন্তু দূরত্বের জন্য সেটি রাখা ভালো।
    

---

### 💡 প্রো-টিপ (Obsidian Linking)

আপনার এই নোটে `[[Multi-Source BFS]]` এবং `[[Grid Traversal (dx, dy)]]` লিঙ্ক করে রাখুন। কারণ গ্রিড প্রবলেমে (যেমন: গোলকধাঁধা বা Maze সলভ করা) এই পাথ রিকনস্ট্রাকশন টেকনিকটি সবচেয়ে বেশি কাজে লাগে।

---

**আপনার পরবর্তী চ্যালেঞ্জ:** গ্রিডে যদি কোনো বাধা (Obstacle) থাকে, তবে কি আপনি এই কোডটি দিয়ে শর্টেস্ট পাথ বের করতে পারবেন? আমরা কি **DFS** এর মাধ্যমে পাথ খোঁজা শিখব? (মনে রাখবেন, DFS কিন্তু সবসময় **শর্টেস্ট** পাথ দেয় না)।