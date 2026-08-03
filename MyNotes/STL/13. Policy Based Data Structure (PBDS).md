### **Header & Initialization:**


```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;

template<typename T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;
```

- `less<T>`: ডুপ্লিকেট গ্রহণ করে না (Set এর মতো).
    
- `less_equal<T>`: ডুপ্লিকেট গ্রহণ করে (Multiset এর মতো).
    

### **Special Abilities:**

1. `st.order_of_key(x)`: $x$ এর চেয়ে ছোট কতটি সংখ্যা সেটে আছে তা জানায়.
    
2. `*st.find_by_order(x)`: $x$ নম্বর ইনডেক্সে কোন ভ্যালু আছে তা জানায়.