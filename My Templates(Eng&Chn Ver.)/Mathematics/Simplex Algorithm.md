Maximize $\sum_j^n c_jx_j$

Subject to constraints $\sum_j^n a_{ij}x_j\leq b_i$, where $i=1,2,...,m$

and satisfying the condition $x_j\geq 0$, where $j=1,2,...,n$

最大化$\sum_j^n c_jx_j$

满足约束$\sum_j^n a_{ij}x_j\leq b_i$，其中$i=1,2,...,m$

且满足条件$x_j\geq 0$，其中$j=1,2,...,n$
```c++
int n,m,type;
db a[N][N],ans[N];
int id[N<<1];
int q[N];

void pivot(int l,int e){
    swap(id[n+l],id[e]);
    db t=a[l][e];a[l][e]=1;
    for(int j=0;j<=n;++j)a[l][j]/=t;
    for(int i=0;i<=m;++i)
	if(i!=l&&abs(a[i][e])>eps){
        t=a[i][e];a[i][e]=0;
        for(int j=0;j<=n;++j)a[i][j]-=a[l][j]*t;
	}
}

bool init(){
    while(1){
        int e=0,l=0;
        for(int i=1;i<=m;++i)if(a[i][0]<-eps&&(!l||(rand()&1)))l=i;
        if(!l)break;
        for(int j=1;j<=n;++j)if(a[l][j]<-eps&&(!e||(rand()&1)))e=j;
        if(!e){puts("Infeasible");return 0;}
        pivot(l,e);
    }
    return 1;
}

bool simplex(){
    while(1){
        int l=0,e=0; db mn=inf,mx=0;
        for(int j=1;j<=n;++j)if(a[0][j]-eps>mx)mx=a[0][j],e=j;
        if(!e)break;
        for(int i=1;i<=m;++i)
		if(a[i][e]>eps&&a[i][0]/a[i][e]<mn)
        mn=a[i][0]/a[i][e],l=i;
        if(!l){puts("Unbounded");return 0;}
        pivot(l,e);
    }
    return 1;
}

int main(){
    srand(time(NULL));
    n=read();m=read();type=read();
    for(int i=1;i<=n;++i)a[0][i]=read();
    for(int i=1;i<=m;++i){
        for(int j=1;j<=n;++j)a[i][j]=read();
        a[i][0]=read();
    }
    for(int i=1;i<=n;++i)id[i]=i;
    if(init()&&simplex()){
        printf("%.8lf\n",(double)-a[0][0]);
        if(type){
            for(int i=1;i<=m;++i)ans[id[n+i]]=a[i][0];
            for(int i=1;i<=n;++i)printf("%.8lf ",(double)ans[i]);
        }
    }
}

```