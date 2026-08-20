**Related Topics:** `[[BFS]]`, `[[DFS]]`, `[[Matrix Problems]]`, `[[Shortest Path]]`

গ্রিড বা ম্যাট্রিক্স প্রবলেমে একটি সেল $(r, c)$ থেকে তার নেইবার সেলে যাওয়ার জন্য **Direction Arrays** ব্যবহার করা হয়। এটি কোডকে ক্লিন রাখে এবং ভুল কমায়।

---

### 🗺️ Direction Array logic

একটি সেল $(r, c)$ থেকে তার চারপাশে যাওয়ার জন্য আমরা স্থানাঙ্ক পরিবর্তন করি।
#### **1. 4-Directional (Up, Down, Left, Right)**

এটি সবচেয়ে বেশি ব্যবহৃত হয়।

```cpp
int dx[] = {1, -1, 0, 0};
int dy[] = {0, 0, 1, -1};
```

#### **2. 8-Directional (Diagonal সহ)**

যদি কোণাকুণি মুভমেন্টের অনুমতি থাকে (যেমন দাবার রাজা)।

```cpp
int dx[] = {1, -1, 0, 0, 1, 1, -1, -1};
int dy[] = {0, 0, 1, -1, 1, -1, 1, -1};
```

#### **3. Knight's Move (দাবার ঘোড়া)**

ঘোড়ার L-shape চালের জন্য:

```cpp
int dx[] = {1, 1, 2, 2, -1, -1, -2, -2};
int dy[] = {2, -2, 1, -1, 2, -2, 1, -1};
```

---

### 🚀 Grid BFS Template

আপনার `bfs` টেমপ্লেটের সাথে গ্রিড লজিক যুক্ত করলে তা দেখতে এমন হবে:

```cpp
int n, m; 
int vis[1005][1005], dis[1005][1005];
int dx[] = {1, -1, 0, 0};
int dy[] = {0, 0, 1, -1};

bool isValid(int r, int c) {
    return (r >= 0 && r < n && c >= 0 && c < m && !vis[r][c]);
}

void bfs(int si, int sj) {
    queue<pair<int, int>> q;
    q.push({si, sj});
    vis[si][sj] = 1;
    dis[si][sj] = 0;

    while (!q.empty()) {
        pair<int, int> curr = q.front();
        q.pop();

        for (int i = 0; i < 4; i++) {
            int nr = curr.first + dx[i];
            int nc = curr.second + dy[i];

            if (isValid(nr, nc)) {
                vis[nr][nc] = 1;
                dis[nr][nc] = dis[curr.first][curr.second] + 1;
                q.push({nr, nc});
            }
        }
    }
}
```

---

### 🧠 Why Use This?

1. **Compact Code:** ৪ বার `if-else` লেখার বদলে একটি `for` লুপেই সব নেইবার চেক করা যায়।
2. **Boundary Check:** `isValid` ফাংশন ব্যবহার করলে ইনডেক্স আউট অফ বাউন্ড (Out of Bounds) হওয়ার ভয় থাকে না।
3. **Easy Maintenance:** যদি ৮ দিকে মুভ করতে হয়, শুধু `dx`, `dy` আর লুপের রেঞ্জ পরিবর্তন করলেই কাজ হয়ে যায়।