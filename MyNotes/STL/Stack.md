### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `stack<int> st;` | Declares an empty stack | $O(1)$ |
| `stack<int, vector<int>> st;` | Declares a stack using `vector` as underlying container | $O(1)$ |
| `stack<int, deque<int>> st;` | Declares a stack using `deque` (Default behavior) | $O(1)$ |
### 2. Push & Pop (Core Operations)
| Task        | Syntax           | Description                                 | Time Complexity |
| :---------- | :--------------- | :------------------------------------------ | :-------------- |
| **Push**    | `st.push(x);`    | Adds element $x$ at the top                 | $O(1)$          |
| **Pop**     | `st.pop();`      | Removes the top element                     | $O(1)$          |
| **Emplace** | `st.emplace(x);` | Constructs and adds $x$ at the top (Faster) | $O(1)$          |
### 3. Access & Capacity
| Task            | Syntax        | Description                                 | Time Complexity |
| :-------------- | :------------ | :------------------------------------------ | :-------------- |
| **Top Access**  | `st.top();`   | Returns the top-most element                | $O(1)$          |
| **Size**        | `st.size();`  | Returns the number of elements in the stack | $O(1)$          |
| **Empty Check** | `st.empty();` | Returns `true` if stack is empty            | $O(1)$          |
### 4. Traversal & Swap
| Operation | Syntax | Description | Time Complexity |
| :--- | :--- | :--- | :--- |
| **Swap** | `st1.swap(st2);` | Swaps contents of two stacks | $O(1)$ |
| **Clear** | `while(!st.empty()) st.pop();` | Clears all elements (No direct `clear()`) | $O(N)$ |
| **Print All** | Requires popping all elements one by one | Iterates through stack elements | $O(N)$ |
### **💡 Important Patterns & Tips (CP-এর জন্য)**

১. **Empty Check (সতর্কতা):**

সবচেয়ে বেশি ভুল হয় যখন `st.empty()` চেক না করে `st.top()` বা `st.pop()` করা হয়। এতে কোড ক্র্যাশ করবে (Runtime Error)। সবসময় চেক করে নিন:

```cpp
if(!st.empty()) {
    cout << st.top();
    st.pop();
}
```

২. **Monotonic Stack Pattern:**

কম্পিটিটিভ প্রোগ্রামিংয়ে "Next Greater Element" বা "Previous Smaller Element" বের করার জন্য স্ট্যাকের এই প্যাটার্নটি বহুল ব্যবহৃত। এটি লিনিয়ার টাইমে ($O(N)$) কাজ সমাধান করতে সাহায্য করে।

৩. **No Iterators:**

মনে রাখবেন, স্ট্যাকে `begin()` বা `end()` ইটারেটর নেই। অর্থাৎ আপনি `for(auto x : st)` বা `sort(st.begin(), st.end())` ব্যবহার করতে পারবেন না। আপনাকে শুধু ওপরের এলিমেন্ট (`top`) নিয়েই কাজ করতে হবে।

৪. **কখন Stack ব্যবহার করবেন?**

যখন আপনার কোনো কিছু রিভার্স অর্ডারে দরকার হয়, অথবা যখন রিকার্সন (Recursion) হ্যান্ডেল করতে হয় (যেমন: DFS, Balanced Parentheses, বা Expression Evaluation)।