```cpp
void code(){
    int n,m,src;cin>>n>>m>>src;
    vector<pair<int,int>>adj[n+1];
    for(int i=0;i<m;i++){
        int u,v,w;cin>>u>>v>>w;
        adj[u].emplace_back(v,w);
        adj[v].emplace_back(u,w);
    }
    vector<int>distance(n+1,LLONG_MAX);
    priority_queue<pii,vector<pii>,greater<pii>>pq;
    pq.push({0,src});
    distance[src]=0ll;
    while(!pq.empty()){
        auto x=pq.top();
        pq.pop();
        int d=x.first,nd=x.second;
        if(d>distance[nd])continue;
        for(auto [a,b]:adj[nd]){
            if(d+b<distance[a])distance[a]=d+b,pq.push({d+b,a});
        }
    }
}
```