**Purpose:** সর্টেড (Sorted) অ্যারে বা রেঞ্জ থেকে $O(\log n)$ টাইম কমপ্লেক্সিটিতে কোনো এলিমেন্ট বা পজিশন খুঁজে বের করা।

_ডিকশনারিতে শব্দ খোঁজার মতো চিন্তা করতে পারো।_

---

## 🛠️ Generalized Form

যেকোনো প্রবলেমে বাইনারি সার্চ ইমপ্লিমেন্ট করার সাধারণ ফরম্যাট:


```cpp
int left = 0, right = n;
while(left < right){
    int mid = left + (right - left) / 2; // Avoids overflow
    if(check(mid)) left = mid + 1;
    else right = mid;
}
// Final Answer = left (or left-1 based on condition)
```

---

## 📏 Lower Bound vs Upper Bound

|**Type**|**Condition**|**Meaning**|
|---|---|---|
|**Lower Bound**|`vec[mid] < x`|First index where value $\ge x$|
|**Upper Bound**|`vec[mid] <= x`|First index where value $> x$|

### 🔹 Lower Bound Snippet

"সোজা বাংলায় কাঙ্ক্ষিত element এর index খুঁজবো, না পাইলে এর চেয়ে বড় (next) element এর index খুঁজবো।"


```cpp
int left = 0, right = n;
while(left < right){
    int mid = left + (right - left) / 2;
    if(vec[mid] < x) left = mid + 1;
    else right = mid;
}
// Index = left;
```

### 🔹 Upper Bound Snippet

"কাঙ্ক্ষিত element এর ঠিক পরবর্তী (next) বড় index খুঁজবো।"

C++

```
int left = 0, right = n;
while(left < right){
    int mid = left + (right - left) / 2;
    if(vec[mid] <= x) left = mid + 1;
    else right = mid;
}
// Index = left;
```

---

## 🎯 Finding First & Last Occurrence of 'x'

### 1. First Element:

**Lower Bound** এর কোড ব্যবহার করলেই প্রথম ইনডেক্স পাওয়া যাবে।

_শর্ত: `if(vec[left] == x)` চেক করে নিতে হবে।_

### 2. Last Element:

**Upper Bound** দিয়ে পরবর্তী ইনডেক্স খুঁজে তার আগেরটা (`left - 1`) নিতে হবে।

C++

```cpp
// Last Element index logic
int left = 0, right = n;
while(left < right){
    int mid = left + (right - left) / 2;
    if(vec[mid] <= x) left = mid + 1;
    else right = mid;
}
int last_idx = left - 1; 
```

---

## 💡 Binary Search on Answer (Optimization)

প্রবলেমের ডিমান্ড অনুযায়ী `isOk()` ফাংশন ম্যানিপুলেট করে বড় রেঞ্জে সার্চ করা যায়।

C++

```cpp
bool isOk(int mid, int x){
    // Logic based on problem requirement
    if(condition) return true;
    return false;
}

void solve(){
    int left = 0, right = 1e9; // Search space
    while(left < right){
        int mid = left + (right - left) / 2;
        if(isOk(mid, x)) left = mid + 1;
        else right = mid;
    }
}
```

---

## 📌 Built-in C++ STL Functions

সবসময় নিজে কোড না লিখে সরাসরি STL ব্যবহার করা যায়:

- **Index:** `lower_bound(v.begin(), v.end(), x) - v.begin();`
    
- **Element:** `*lower_bound(v.begin(), v.end(), x);`
    
- **Presence:** `binary_search(v.begin(), v.end(), x);` (Returns true/false)
    

---

**NB:** যদি কাঙ্ক্ষিত এলিমেন্ট না পাওয়া যায়, তবে সবসময় _Next Element_ এর ইনডেক্স পাওয়া যাবে।

_Created on: 2026-05-11_

_Tags: #BinarySearch #Algorithms #CP #ProblemSolving #Pupil_Level_