```cpp
#define N 100005
vector<int>adj[N];
vector<int>lvl(N);
void dfs_lvl(int u,int par,int d) {
    lvl[u]=d;
    for (int v:adj[u]) {
        if(v!=par)dfs_lvl(v,u,d+1);
    }
}
```
