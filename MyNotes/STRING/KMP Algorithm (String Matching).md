# 🧵 KMP Algorithm (String Matching)

**Agenda:** একটি বড় টেক্সট ($T$) এর মধ্যে একটি নির্দিষ্ট প্যাটার্ন ($P$) কতবার বা কোথায় আছে তা $O(n + m)$ টাইম কমপ্লেক্সিটিতে খুঁজে বের করা। সাধারণ ব্রুট-ফোর্স মেথডে যা $O(n \times m)$ সময় নেয়, KMP সেখানে অপ্রয়োজনীয় তুলনা (redundant comparisons) এড়িয়ে চলে।

---

### 🧠 Core Insights

- **The "Waste No Effort" Policy:** ব্রুট-ফোর্সে কোনো ক্যারেক্টার না মিললে আমরা আবার শুরু থেকে শুরু করি। KMP বলে—যেহেতু আমরা জানি আগের ক্যারেক্টারগুলো কী ছিল, তাই সেগুলো আবার চেক করার দরকার নেই।
    
- **LPS Array (Longest Proper Prefix which is also Suffix):** এটিই KMP-এর প্রাণ। এটি আমাদের জানায় যে প্যাটার্নের কোনো অংশে ম্যাচিং ফেইল করলে আমরা প্যাটার্নের ঠিক কোন ইনডেক্স থেকে আবার চেক করা শুরু করবো।
    
    - _Proper Prefix:_ স্ট্রিংয়ের এমন একটি সাবস্ট্রিং যা পুরো স্ট্রিংটি নয় (যেমন "abc" এর প্রিফিক্স "a", "ab")।
    - _Suffix:_ স্ট্রিংয়ের শেষের অংশ।

---

### 🛠️ The LPS Construction (Preprocessing)

LPS অ্যারে তৈরি করার লজিক হলো নিজের সাথেই নিজের তুলনা করা।

```cpp
vector<int> createLPS(string p) {
    int m = p.size();
    vector<int> lps(m, 0);
    int j = 0; // length of previous longest proper prefix suffix

    for (int i = 1; i < m; i++) {
        while (j > 0 && p[i] != p[j]) {
            j = lps[j - 1];
        }
        if (p[i] == p[j]) {
            j++;
        }
        lps[i] = j;
    }
    return lps;
}
```

---

### 💻 KMP Algorithm Code


```cpp
void KMP(string text, string pattern) {
    int n = text.size();
    int m = pattern.size();
    vector<int> lps = createLPS(pattern);
    int j = 0; // index for pattern

    for (int i = 0; i < n; i++) { // i: index for text
        while (j > 0 && text[i] != pattern[j]) {
            j = lps[j - 1]; // ফেইল করলে LPS দেখে লাফ দেওয়া
        }
        if (text[i] == pattern[j]) {
            j++;
        }
        if (j == m) {
            cout << "Pattern found at index " << i - m + 1 << endl;
            j = lps[j - 1];
        }
    }
}
```

---

### 📊 Simulation (Dry Run)

**Text:** `ababcabcabababd`

**Pattern:** `ababd`

1. **Preprocessing (LPS for `ababd`):**
    
    - `a` -> 0
        
    - `ab` -> 0
        
    - `aba` -> 1 (`a` is prefix & suffix)
        
    - `abab` -> 2 (`ab` is prefix & suffix)
        
    - `ababd` -> 0
        
    - **LPS:** `[0, 0, 1, 2, 0]`
        
2. **Matching Process:**
    
    - টেক্সটের প্রথম ৪টি ক্যারেক্টার `abab` প্যাটার্নের সাথে মিলে যাবে।
        
    - ৫ম ক্যারেক্টার টেক্সটে `c` কিন্তু প্যাটার্নে `d`। mismatch!
        
    - এখন আমরা শুরু থেকে শুরু করবো না। `j = lps[4-1] = lps[3] = 2` এ চলে যাবো। অর্থাৎ প্যাটার্নের ৩য় ক্যারেক্টার থেকে চেক শুরু হবে।
        

---

### 📝 Speciality & Complexity

- **Time Complexity:** $O(n + m)$ — টেক্সট একবার স্ক্যান হয়, প্যাটার্ন প্রি-প্রসেস হয়।
    
- **Space Complexity:** $O(m)$ — LPS অ্যারের জন্য।
    
- **Application:** মেমোরি কম থাকলে বা খুব বড় টেক্সটে সার্চ করতে হলে এটি সবচেয়ে কার্যকর।
    
- **Insight for Pupil Rank:** কোডফোর্সেসে স্ট্রিং পিরিওডিসিটি (String Periodicity) বা বর্ডার (Border) সংক্রান্ত প্রবলেমে LPS লজিক সরাসরি কাজে লাগে।
    

---

_Created on: 2026-05-11_

_Tags: #String #KMP #Algorithm #LPS #CP #ObsidianNotes_