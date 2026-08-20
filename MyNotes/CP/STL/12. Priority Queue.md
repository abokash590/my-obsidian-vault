### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `priority_queue<int> pq;` | Max-Heap (Default: Largest element at top) | $O(1)$ |
| `priority_queue<int, vector<int>, greater<int>> pq;` | Min-Heap (Smallest element at top) | $O(1)$ |
| `priority_queue<int> pq(v.begin(), v.end());` | Build heap from a vector | $O(N)$ |
### 2. Core Operations
| Task | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Push** | `pq.push(x);` | Adds element $x$ and maintains heap order | $O(\log N)$ |
| **Pop** | `pq.pop();` | Removes the top element | $O(\log N)$ |
| **Top Access** | `pq.top();` | Accesses the element with highest priority | $O(1)$ |
### 3. Capacity & Utilities
| Task | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Size** | `pq.size();` | Returns the number of elements | $O(1)$ |
| **Empty Check** | `pq.empty();` | Returns `true` if empty | $O(1)$ |
| **Swap** | `pq1.swap(pq2);` | Swaps contents of two priority queues | $O(1)$ |
### **💡 Important Patterns & Tips (CP-এর জন্য)**

১. **Max-Heap vs Min-Heap:**

- **Max-Heap:** ডিফল্টভাবে `priority_queue<int> pq;` বড় এলিমেন্টকে উপরে রাখে।
    
- **Min-Heap:** যদি আপনি চান ছোট এলিমেন্ট উপরে থাকুক, তবে ডিক্লারেশনটি একটু বড় হয়: `priority_queue<int, vector<int>, greater<int>> pq;`
    

২. **Dijkstra's Algorithm:** গ্রাফের শর্টেস্ট পাথ বের করার জন্য প্রিওরিটি কিউ অপরিহার্য। এখানে সাধারণত `pair<int, int>` ব্যবহার করা হয় যাতে `{distance, node}` স্টোর করা যায়।

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
pq.push({0, start_node});
```

৩. **Custom Comparator:** যদি আপনি নিজের তৈরি কোনো স্ট্রাকচার বা বিশেষ লজিক অনুযায়ী সর্ট করতে চান:

```cpp
struct cmp {
    bool operator()(int a, int b) {
        return a > b; // এটি Min-Heap তৈরি করবে
    }
};
priority_queue<int, vector<int>, cmp> pq;
```

৪. **কখন Priority Queue ব্যবহার করবেন?** যখন আপনার বারবার একটি ডাইনামিক লিস্ট থেকে সবচেয়ে বড় বা সবচেয়ে ছোট এলিমেন্টটি খুঁজে বের করা এবং ডিলিট করা প্রয়োজন (যেমন: Huffman Coding, Prim's Algorithm, বা Greedy প্রবলেমস)।