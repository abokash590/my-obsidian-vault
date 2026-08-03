### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `queue<int> q;` | Declares an empty queue | $O(1)$ |
| `queue<int, list<int>> q;` | Declares a queue using `list` as underlying container | $O(1)$ |
| `queue<int, deque<int>> q;` | Declares a queue using `deque` (Default) | $O(1)$ |
### 2. Push & Pop (Core Operations)
| Task | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Push** | `q.push(x);` | Adds element $x$ at the back (rear) | $O(1)$ |
| **Pop** | `q.pop();` | Removes the element from the front | $O(1)$ |
| **Emplace** | `q.emplace(x);` | Directly constructs and adds $x$ at the back | $O(1)$ |
### 3. Access & Capacity
| Task             | Syntax       | Description                                 | Time Complexity |
| :--------------- | :----------- | :------------------------------------------ | :-------------- |
| **Front Access** | `q.front();` | Returns the first element                   | $O(1)$          |
| **Back Access**  | `q.back();`  | Returns the last element                    | $O(1)$          |
| **Size**         | `q.size();`  | Returns the number of elements in the queue | $O(1)$          |
| **Empty Check**  | `q.empty();` | Returns `true` if queue is empty            | $O(1)$          |
### ### **4. Traversal & Swap**
| Operation | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Swap** | `q1.swap(q2);` | Swaps contents of two queues | $O(1)$ |
| **Clear** | `while(!q.empty()) q.pop();` | Clears all elements (No direct `clear()`) | $O(N)$ |
### **💡 Important Patterns & Tips (CP-এর জন্য)**

১. **BFS (Breadth-First Search):** গ্রাফ থিওরি প্রবলেমে লেভেল বাই লেভেল ট্রাভার্সাল করার জন্য কিউ ব্যবহার করা হয়।

```cpp
queue<int> q;
q.push(start_node);
while(!q.empty()){
    int u = q.front(); q.pop();
    for(int v : adj[u]){
        if(!visited[v]){
            visited[v] = true;
            q.push(v);
        }
    }
}
```

২. **Front vs Back:** মনে রাখবেন, `pop()` সবসময় **front** থেকে এলিমেন্ট সরায়। `push()` সবসময় **back**-এ এলিমেন্ট যোগ করে। স্ট্যাকের মতো এখানেও `empty()` চেক না করে `front()` বা `pop()` করলে কোড ক্র্যাশ করবে।

৩. **No Iterators:** স্ট্যাকের মতো কিউতেও কোনো ইটারেটর নেই। আপনি মাঝখানের কোনো এলিমেন্ট সরাসরি অ্যাক্সেস করতে পারবেন না।

৪. **কখন Queue ব্যবহার করবেন?** যখন ডাটাগুলো যে অর্ডারে আসবে ঠিক সেই অর্ডারেই প্রসেস করতে হবে (যেমন: BFS, Task Scheduling, বা Buffering)।