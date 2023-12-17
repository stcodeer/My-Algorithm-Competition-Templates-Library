```c++
struct UFS {
    stack<pair<int*, int>> stk;
    int fa[N], rnk[N];
    inline void init(int n) {
        for (int i = 0; i <= n; ++i) fa[i] = i, rnk[i] = 0;
    }
    inline int Find(int x) {
        while(x^fa[x]) x = fa[x];
        return x;
    }
    int Merge(int x, int y) {
    	int c=0;
        x = Find(x), y = Find(y);
        if(x == y){
        	++block;
			return c;
        }
        if(rnk[x] <= rnk[y]) {
            stk.push({fa+x, fa[x]});
            fa[x] = y;
			c++;
            if(rnk[x] == rnk[y]) {
                stk.push({rnk+y, rnk[y]});
                rnk[y]++;
                c++;
            }
        }
        else {
            stk.push({fa+y, fa[y]});
            fa[y] = x;
            c++;
        }
        return c;
    }
    #define fi first
    #define se second
    inline void Undo() {
        *stk.top().fi = stk.top().se;
        stk.pop();
    }
}ufs;
```