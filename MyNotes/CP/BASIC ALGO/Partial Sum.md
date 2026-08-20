

### Purpose:

- To answer range sum queries quickly (e.g., sum from index l to r).
- To reduce time complexity from **O(n)** per query to **O(1)** after **O(n)** preprocessing.
```cpp
int n; cin >> n;
vector<ll> a(n), pre(n+1);
for(int i=0; i<n; i++) cin >> a[i], pre[i+1] = pre[i] + a[i];
**// sum of a[l..r] = pre[r+1] - pre[l]**
```

# Maximum Sum Subarray Using Partial Sum

**Idea:**

- Use prefix sum array: pre[i] = a[0] + a[1] + ... + a[i-1]
- The maximum subarray sum ending at i is pre[i] - min(pre[j]) where j < i

```cpp
int n; cin >> n;
vector<ll> a(n), pre(n+1);
for(int i=0; i<n; i++) cin >> a[i], pre[i+1] = pre[i] + a[i];
ll ans = LLONG_MIN, mn = 0;
for(int i=1; i<=n; i++){
    ans = max(ans, pre[i] - mn);
    mn = min(mn, pre[i]);
}cout << ans << endl;
```

**Note:**  
==LLONG_MIN== is the smallest possible value for a long long integer. It ensures we select a **non-empty subarray**, even if all elements are negative.

---

# Kadane’s Algorithm

**Purpose:**

- Find the **maximum subarray sum** in **O(n)** without using prefix sum.

```cpp
int n; cin >> n;
vector<ll> a(n); for(auto &x:a) cin >> x;
ll ans = a[0], cur = a[0];
for(int i=1; i<n; i++){
    cur = max(a[i], cur + a[i]);
    ans = max(ans, cur);
}cout << ans << endl;
```

---

📌 Use **Partial Sum** when you need range queries too.📌 Use **Kadane’s** when you only need the max subarray sum efficiently.

# Range Update using Partial Sum

**Idea:**

- To add val from index l to r, do:

- diff[l] += val
- diff[r+1] -= val

Then compute prefix sum to get final array.

---

```cpp
int n, q; cin >> n >> q;
vector<ll> diff(n+2, 0);
while(q--){
int l, r, val;
cin >> l >> r >> val; **// 0-based indexing**
diff[l] += val;
diff[r+1] -= val;
}
vector<ll> a(n);
a[0] = diff[0];
for(int i=1; i<n; i++) a[i] = a[i-1] + diff[i];
// final array a
```

✅ Efficient for applying multiple range updates.⚠️ Don’t forget r+1 for subtracting value.

# Count Subarrays with Given Sum Using Partial Sum

**Used For:**

·         Counting subarrays with a specific sum in **O(n)** time.

**Key Idea:**

·         Track prefix sums.

·         Use a hashmap to store frequency of each prefix sum.

·         For each index, check how many times `prefix_sum - x` has occurred.

```cpp
int n, x; cin >> n >> x;
vector<ll> a(n); for(auto &i : a) cin >> i;
map<ll, int> cnt;
cnt[0] = 1;
ll pre = 0, ans = 0;
for(int i=0; i<n; i++){
    pre += a[i],ans += cnt[pre - x]cnt[pre]++;
}cout << ans << endl;
```

---

✅ Efficient for **subarray sum count** problems.  
📌 Requires elements to be **integers**, not just non-negative.