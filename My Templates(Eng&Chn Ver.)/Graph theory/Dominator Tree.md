A dominator tree is applicable to a single-source directed graph.

$semi[x]$ represents a semi-dominating point. There exists a path from $semi[x]$ to $x$ (excluding $x$ and $semi[x]$), and all the vertices along this path have a $dfn$ (depth-first number) greater than $dfn[x]$. Moreover, $semi[x]$ is the smallest $dfn$ among all the points satisfying this condition.

$idom[x]$ represents the immediate dominator. After removing $idom[x]$, it is not possible to reach $x$ from the source vertex $S$, and $idom[x]$ is the closest point to $x$ among all the points satisfying this condition.

$G$ refers to the original graph, $revG$ refers to the reverse graph, $G1$ represents the dominator tree, and $G2$ represents the semi-dominator tree (which may not be useful).

支配树适用于单源有向图。

$semi[x]$是半支配点。从$semi[x]$到$x$存在一条路径，掐头去尾（去除$x,semi[x]$），经过的所有点的$dfn$都大于$dfn[x]$，且$semi[x]$为所有满足条件的点中$dfn$最小的。

$idom[x]$是最近支配点。删除$idom[x]$后，从起点$S$无法到达$x$，且$idom[x]$为所有满足条件的点中离$x$最近的。

$G$是原图，$revG$是反图，$G1$是支配树，$G2$是半支配树（半支配树应该没用）。

```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 3e5+5;

int n,m,cnt,totw,S;
int fa[N],dfn[N],pre[N],p[N];
int semi[N],idom[N];
int mn[N];

struct nd{
	int ne,to;
}e[N*4];

struct graph{
	int head[N];
	
	void in(int x,int y){
		e[++cnt].to=y;e[cnt].ne=head[x];head[x]=cnt;
	}
}G,revG,G1,G2;

int find(int x){
	if(x==fa[x])return x;
	int t=fa[x];
	fa[x]=find(fa[x]);
	if(dfn[semi[mn[t]]]<dfn[semi[mn[x]]])mn[x]=mn[t];
	return fa[x];
}

void dfs(int x){
	dfn[x]=++totw;
	p[totw]=x;
	for(int i=G.head[x];i;i=e[i].ne)
	if(!dfn[e[i].to]){
		int y=e[i].to;
		pre[y]=x;
		dfs(y);
	}
}

void tarjan(){
	dfs(S);
	for(int i=totw;i>=2;--i){
		int res=n;
		int x=p[i];
		for(int j=revG.head[x];j;j=e[j].ne)
		if(dfn[e[j].to]){
			int y=e[j].to;
			find(y);
			res=min(res,dfn[semi[mn[y]]]);
		}
		fa[x]=pre[x];
		semi[x]=p[res];
		G2.in(semi[x],x);
		int y=p[i-1];
		for(int j=G2.head[y];j;j=e[j].ne){
			int z=e[j].to;
			find(z);
			if(semi[mn[z]]==y)idom[z]=y;
			else idom[z]=mn[z];
		}
	}
	for(int i=2;i<=totw;++i){
		int x=p[i];
		if(idom[x]!=semi[x]){
			idom[x]=idom[idom[x]];
		}
	}
}

int sz[N]; 

void dfs1(int x){
	sz[x]=1;
	for(int i=G1.head[x];i;i=e[i].ne){
		dfs1(e[i].to);
		sz[x]+=sz[e[i].to];
	}
}

int main(){
	scanf("%d%d",&n,&m);
	S=1;
	for(int i=1,x,y;i<=m;++i){
		scanf("%d%d",&x,&y);
		G.in(x,y);
		revG.in(y,x);
	}
	for(int i=1;i<=n;++i)fa[i]=semi[i]=mn[i]=i;
	tarjan();
	for(int i=1;i<=n;++i)
	if(i!=S){
		G1.in(idom[i],i);
	}
	dfs1(S);
	for(int i=1;i<=n;++i)printf("%d ",sz[i]);
}

```