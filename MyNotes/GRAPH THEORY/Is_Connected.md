### **🌐 Condition of a Connected Graph**

একটি গ্রাফকে কানেক্টেড বলা হবে যদি:

1. যেকোনো একটি সোর্স (যেমন নোড ১) থেকে BFS শুরু করলে সব নোডে পৌঁছানো যায়।
2. ভিজিট করার পর `vis` অ্যারেতে কোনো নোড `0` (Unvisited) না থাকে।
---

### **🚀 Template: Check if Graph is Connected**

আপনার দেওয়া `bfs` ফাংশনটি ব্যবহার করেই আমরা এটি চেক করতে পারি।

```cpp
    bool connected = true;
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            connected = false; // কোনো একটি নোড ভিজিট না হলে কানেক্টেড নয়
            break;
        }
    }

    if (connected) cout << "The graph is Connected" << endl;
    else cout << "The graph is Disconnected" << endl;
```

