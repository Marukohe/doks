---
title: "NJU Problem Solving OJ Template"
description: "NJU OJ Template"
lead: "问求OJ模板"
date: 2021-03-31T23:49:13+08:00
lastmod: 2021-03-31T23:49:13+08:00
draft: false
weight: 50
images: []
toc: true
contributors: [He Wei]
---

## 并查集

```c
int Make_set(int n) {
    for(int i = 0; i < n; i++) {
        par[i] = i;
        renk[i] = 0;
    }
}

int Find_set(int x) {
    if(par[x] == x)
        return x;
    else
        return par[x] = Find_set(par[x]);
}

void Union_set(int x,int y ) {
    x = Find_set(x);
    y = Find_set(y);
    if(x == y)
        return;
    if(renk[x] < renk[y]) {
        par[x] = y;
    }
    else{
        par[y] = x;
        if(renk[x] == renk[y])
            renk[x] += 1;
    }
}
```

## 生成树

```C++
/*
* prime
*/
int cost[MAX_V][MAX_V];
int mincost[MAX_V];
bool used[MAX_V];
int V;

int prime(){
    for(int i = 0; i < V; i++) {
        mincost[i] = INF;
        used[i] = false;
    }
    mincost[0] = 0;
    int res = 0;
    while(true){
        int v = -1;
        for(int u = 0; u < V; u++) {
            if(!used[u] && (v == -1|| mincost[u] < mincost[v])) v=u;
        }
        if(v == -1) break;
        used[v] = true;
        res += mincost[v];
        for(int u = 0; u < V; u++) {
            mincost[u] = min(mincost[u], cost[v][u]);
        }
    }
    return res;
}

/*
* kruskal
*/
struct edge{int u,v,cost;};

bool cmp(const edge &e1,const edge &e2){
    return e1.cost<e2.cost;
}
edge es[MAX_V];
int V,E;

int  kruskal(){
    sort(es, es + E, cmp);
    init_union_find(V);   //并查集初始化
    int res = 0;
    for(int i = 0; i < E; i++) {
        edge e = es[i];
        if(!same(e.u,e.v)) {
            unite(e.u,e.v);
            res += e.cost;
        }
    }
    return res;
}
```

## 搜索

```c++
/*
* dfs
*/
int N,M;
char field[MAX_V][MAX_V];
void dfs(int s,int t){
    field[s][t] = '.';
    
    for(int dx =- 1; dx <= 1; dx++) {
        for(int dy = -1; dy <= 1; dy++) {
            int nx = s + dx, ny = t + dy;
            if(nx >= 0 && nx < N && 0 <= ny && ny <= M && field[nx][ny] == 'W')
                dfs(nx,ny);
        }
    }
    return ;
}

/*
* bfs
*/

typedef pair<int,int> P;
char maze[MAX_V][MAX_V];
int sx,sy;        //起点
int gx,gy;        //终点

int d[MAX_V][MAX_V];

int dx[4] = {1,-1,0,0}, dy[4]={0,0,1,-1};

int bfs(){
    queue<P>que;
    for(int i = 0; i < N; i++)
        for(int j = 0; j < M; j++)
            d[i][j] = INF;
    que.push(P(sx,sy));
    d[sx][sy] = 0;
    while(!que.empty()) {
        P p = que.front(); que.pop();
        if(p.first == gx && p.second == gy) break;
        
        for(int i = 0; i < 4; i++){
            int nx = p.first + dx[i];
            int ny = p.second + dy[i];
            
            if(0 <= nx && nx <N && 0 <= ny && ny < M & maze[nx][ny] != '#' && d[nx][ny] == INF) {
                que.push(P(nx,ny));
                d[nx][ny] = d[p.first][p.second] + 1;
            }
        }
    }
    return d[gx][gy];
}

/*
* 拓扑排序
*/

/*
* SCC
*/
```

## 单源最短路

```c++
/*
* bellman-ford
*/
struct edge{int from,to,cost;};

edge es[MAX_V];

int d[MAX_V];
int V,E;

void bellman_ford(int s) {
    for(int i = 0; i < V; i++) d[i] = INF;
    while(true){
        bool update = false;
        for(int i = 0; i < E; i++) {
            edge e = es[i];
            if(d[e.from] != INF && d[e.to] > d[e.from] + e.cost) {
                d[e.to] = d[e.from] + e.cost;
                update = true;
            }
        }
        if(!update) break;
    }
}

//检查负圈,返回true有负圈
bool find_negative_loop(){
    memset(d, 0, sizeof(d));
    for(int i = 0; i < V; i++) {
        for(int j = 0; j < E; j++) {
            edge e = es[j];
            if(d[e.to > d[e.from] + e.cost]) {
                d[e.to] = d[e.from] + e.cost;
                
                //如果第n次仍然更新了，说明有负圈
                if(i == V - 1) return true;
            }
        }
    }
    return false;
}
```

```c++
/*
* dijkstra
*/
struct edge {int to,cost;};
typedef pair<int,int>P;   //first是最短距离，second是顶点编号

int V;
vector<edge>G[MAX_V];
int d[MAX_V];

void dijkstra(int s){
    priority_queue<P, vector<P>, greater<P>>que;
    fill(d, d + V, INF);
    d[s] = 0;
    que.push(P(0, s));
    
    while(!que.empty()){
        P p = que.top(); que.pop();
        int v = p.second;
        if(d[v] < p.first) continue;      //防止重点
        for(int i = 0; i < G[v].size(); i++){
            edge e = G[v][i];
            if(d[e.to] > d[v] + e.cost){
                d[e.to] = d[v] + e.cost;
                que.push(P(d[e.to], e.to));
            }
        }
    }
}
```

## 多源最短路

```c++
int d[MAX_V][MAX_V]; //d[i][i]=0
int V;
void floyd(){
    for(int k = 0; k < V; k++)
        for(int i = 0; i < V; i++)
            for(int j = 0; j < V; j++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

## 连通度

```c++
struct Edge {
    int u, v;
} edge[MAX_V];

vector<int>G[MAX_V];

void tarjan(int u, int fa){
    low[u] = dfn[u] = ++dfs_num;
    for(int i = 0; i < G[u].size(); i++){
        int v = G[u][i];
        if(v == fa)
            continue;
        if(!dfn[v]){
            tarjan(v, u);
            low[u] = min(low[u], low[v]);
            if(low[v] >= dfn[u]) {
                if(u == 1) child++;
                else subnet[u]++;
                int from, to;
                if(low[v] != dfn[u]) {
                    from = u;
                    to = v;
                    if (from > to)
                        swap(from, to);
                    edge[cntnode].u = from;
                    edge[cntnode++].v = to;
                }
            }
        }
        else if(dfn[v] < dfn[u]) low[u] = min(low[u], dfn[v]);
    }
}

```

## 匹配

```c++
vector<int>G[MAX_V];
bool used[MAX_V];
int llink[MAX_V];
int rlink[MAX_V];

bool dfs(int now){
    for(int i = 0; i < G[now].size(); i++){
        int v = G[now][i];
        if(used[v]) continue;
        used[v] = true;
        if(rlink[v] == -1 || dfs(rlink[v])){
            rlink[v] = now;
            llink[now] = v;
            return true;
        }
    }
    return false;
}

int main(){
	...
    int ans = 0;
    memset(rlink, -1, sizeof(rlink));
    memset(llink, -1, sizeof(llink));
    for (int i = 1; i <= n; i++) {
        memset(used, false, sizeof(used));
        if (dfs(i)) ans++;
    }
}
```

## 网络流

```c++
int F[MAX_E],D[MAX_E];
struct edge {int to, cap,rev;};

vector<edge>G[MAX_E];
bool used[MAX_E];

void add_edge(int from, int to ,int cap){
    G[from].push_back((edge){to,cap,(int)G[to].size()});
    G[to].push_back((edge){from,0,(int)G[from].size()-1});
};

int dfs(int v,int t,int f){
    if(v == t) return f;
    used[v] = true;
    for(int i = 0; i < G[v].size(); i++){
        edge &e = G[v][i];
        if(!used[e.to] && e.cap > 0){
            int d = dfs(e.to, t, min(f, e.cap));
            if(d > 0){
                e.cap -= d;
                G[e.to][e.rev].cap += d;
                return d;
            }
        }
    }
    return 0;
}

int max_flow(int s,int t){
    int flow = 0;
    for(;;){
        memset(used, 0, sizeof(used));
        int f = dfs(s, t, INF);
        if(f == 0) return flow;
        flow += f;
    }
}

```

```c++
/*
* dinic
*/
struct edge{int to, cap, rev;};
//int A[305][2005];
vector<int>A[1005];
vector<edge>G[MAX_V];
int level[MAX_V];
int iter[MAX_V];
 
void add_edge(int from, int to, int cap){
    G[from].push_back((edge){to, cap, (int)G[to].size()});
    G[to].push_back((edge){from, 0, (int)G[from].size()-1});
}
 
void bfs(int s){
    memset(level,-1,sizeof(level));
    queue<int>que;
    level[s] = 0;
    que.push(s);
    while(!que.empty()){
        int v = que.front();que.pop();
        for(int i = 0; i < G[v].size(); i++){
            edge &e = G[v][i];
            if(e.cap > 0 && level[e.to] < 0){
                level[e.to] = level[v]+1;
                que.push(e.to);
            }
        }
    }
}
 
int dfs(int v, int t, int f){
    if(v == t) return f;
    for(int &i = iter[v]; i < G[v].size(); i++){
        edge &e = G[v][i];
        if(e.cap > 0 && level[v] < level[e.to]){
            int d = dfs(e.to, t, min(f,e.cap));
            if(d > 0) {
                e.cap -= d;
                G[e.to][e.rev].cap+=d;
                return d;
            }
        }
    }
    return 0;
}
 
int max_flow(int s, int t){
    int flow = 0;
    for(;;){
        bfs(s);
        if(level[t] < 0 ) return flow;
        memset(iter, 0, sizeof(iter));
        int f;
        while((f = dfs(s, t, INF)) > 0)
            flow += f;
    }
}
```
