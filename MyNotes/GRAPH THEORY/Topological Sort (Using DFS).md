**Code 1 :**

```cpp
vector<int> adj[N],indegree(N),tps,vis(N);
void dfs(int u,int par) {
    tps.emplace_back(u);
    vis[u]=1;
    for (int v:adj[u]) {
        if(v!=par){
            if(!--indegree[v])dfs(v,u);
        }
    }
}
void code();
signed main(){
    go_abo();
    sieve();
    int t=1;
    // cin >> t;
    for(int i=0;i<t;i++){
        // cout << "Case " << i << ": ";
        code();
    }
}
void code(){
    int n,m;cin>>n>>m;
    for(int i=0;i<m;i++){
        int u,v;cin>>u>>v;
        indegree[v]++;
        adj[u].emplace_back(v);
    }for(int i=1;i<=n;i++)if(!indegree[i]&&!vis[i])dfs(i,0);
    if(tps.size()==n){vout(tps);}
    else cout<<"IMPOSSIBLE"<<endl;
}
```

**Code 2:**
[[Topological Sort (DFS Method)]]

