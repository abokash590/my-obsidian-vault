### 1. Declaration & Initialization
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `multiset<int> ms;` | Declares an empty multiset (Sorted ascending) | $O(1)$ |
| `multiset<int> ms = {1, 2, 2, 3};` | Initializes with duplicate values | $O(N \log N)$ |
| `multiset<int, greater<int>> ms;` | Declares a multiset sorted in descending order | $O(1)$ |
### 2. Input & Output (Insertion & Traversal)
| Task            | Syntax                               | Time Complexity            |
| :-------------- | :----------------------------------- | :------------------------- |
| **Insert**      | `ms.insert(x);`                      | $O(\log N)$                |
| **Traversal**   | `for(auto x : ms) cout << x << " ";` | $O(N)$                     |
| **Check Count** | `ms.count(x);`                       | $O(\log N + \text{count})$ |
| **Find**        | `ms.find(x);`                        | $O(\log N)$                |
### 3. Access & Search
| Syntax              | Description                                             | Time Complexity |
| :------------------ | :------------------------------------------------------ | :-------------- |
| `ms.size()`         | Returns total number of elements (including duplicates) | $O(1)$          |
| `*ms.begin()`       | Smallest element in the multiset                        | $O(1)$          |
| `*ms.rbegin()`      | Largest element in the multiset                         | $O(1)$          |
| `ms.lower_bound(x)` | Iterator to the first element $\ge x$                   | $O(\log N)$     |
| `ms.upper_bound(x)` | Iterator to the first element $> x$                     | $O(\log N)$     |
### 4. Deletion (Most Important Part)
| Syntax | Description | Time Complexity |
| :--- | :--- | :--- |
| `ms.erase(x);` | Deletes **ALL** instances of value $x$ | $O(\log N + \text{count})$ |
| `ms.erase(ms.find(x));` | Deletes only **ONE** instance of value $x$ | $O(\log N)$ |
| `ms.clear();` | Removes all elements | $O(N)$ |

### **💡 Important Patterns & Tips (CP-এর জন্য)**

১. **Single Element Deletion (সতর্কতা):** সবচেয়ে বেশি ভুল হয় `ms.erase(x)` ব্যবহারের সময়। যদি আপনার কাছে তিনটি `5` থাকে এবং আপনি `ms.erase(5)` লিখেন, তবে তিনটিই ডিলিট হয়ে যাবে। শুধুমাত্র একটি `5` ডিলিট করতে চাইলে ইটারেটর ব্যবহার করুন:

```cpp
auto it = ms.find(x);
if(it != ms.end()) ms.erase(it); // শুধুমাত্র একটি এলিমেন্ট ডিলিট হবে
```

২. **Count vs Find:** `ms.count(x)` লিনিয়ার টাইমে কাজ করতে পারে যদি অনেক বেশি ডুপ্লিকেট এলিমেন্ট থাকে। তাই কোনো এলিমেন্ট আছে কি না চেক করতে `ms.find(x) != ms.end()` ব্যবহার করা বেশি সেফ।

৩. **Finding Range:** একটি নির্দিষ্ট এলিমেন্ট কতবার আছে এবং তাদের রেঞ্জ কত তা বের করতে `equal_range` ব্যবহার করা যায়:

```cpp
auto range = ms.equal_range(x);
// range.first হলো lower_bound এবং range.second হলো upper_bound
```

৪. **কখন Multiset ব্যবহার করবেন?** যখন আপনার ডেটা সর্টেড থাকতে হবে এবং ডুপ্লিকেট ভ্যালুগুলোও আপনার জন্য গুরুত্বপূর্ণ (যেমন: একটি স্লাইডিং উইন্ডোতে মিডিয়ান বের করা বা কোনো সেটের সবচেয়ে ছোট ও বড় এলিমেন্ট বারবার ট্র্যাক করা)।