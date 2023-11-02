Converting an unrooted tree to a Prufer sequence:

    1. Find the leaf node with the smallest label.
    2. Remove that node and add the label of the node connected to it to the sequence.
    3. Repeat steps 1 and 2 until only two nodes remain in the tree.

Converting a Prufer sequence to an unrooted tree:

    1. Take the first element, $u$, from the Prufer sequence.
    2. Find the smallest unused label, $v$, in the set of vertices $V$ (initialized as $ { 1,2,...,n } $), which does not appear in the Prufer sequence.
    3. Connect nodes $u$ and $v$ with an edge, remove $u$ from the sequence, and remove $v$ from $V$.
    4. Repeat steps 1, 2, and 3 until the sequence is empty. At this point, there are only two nodes left in the set $V$, connect them with an edge.

The set of vertices $V$ can be maintained using a set data structure to achieve a complexity of $O(n\log n)$.

Proof:

Note that if a node $A$ is not a leaf node, it has at least two edges. However, after the process of converting the unrooted tree to the Prufer sequence, there is only one edge remaining in the entire tree. Therefore, at least one adjacent node of node $A$ has been removed, and the label of node $A$ will appear in the corresponding Prufer code of the tree. Conversely, numbers that appear in the Prufer code cannot be the leaves of the tree (initially). Thus, we observe that the numbers that have not appeared in the Prufer code exactly correspond to the leaf nodes of the tree (initially). By removing the first element of the sequence and the smallest unused point from $V$, we effectively remove a leaf node and its corresponding edge from the original tree. The remaining new tree, Prufer sequence, and $V$ still satisfy the above properties (with the additional constraint that the point must be present in $V$ since the points not present in $V$ are no longer in the new tree). To understand this correspondence, you can consider drawing two diagrams: the first diagram represents the original tree (which can be seen as $V$), and the second diagram is the tree constructed using the Prufer sequence, where each removal of a vertex and an edge in the original tree corresponds to the addition of an edge in the second diagram.

Properties:

    1. The number of occurrences of node $i$ in the Prufer sequence is $d_i-1$.
    2. A rooted tree with $n$ nodes uniquely corresponds to a sequence of length $n-2$, where each number in the sequence is in the range from $1$ to $n$ (considering the four steps of the conversion, each step is always feasible. In the third step, the size of the sequence is always larger than $V$ by $2$, so there is always a satisfying $v$).
    3. There are $n^{(n-2)}$ labeled unrooted trees with $n$ nodes, which can also be interpreted as the number of spanning trees in a complete undirected graph.
    4. The number of unrooted trees with $n$ nodes and degrees $d_1, d_2, ..., d_n$ is $\frac {(n-2)!} {\Pi(d_i-1)!}$, as node $i$ appears $d_i-1$ times in the Prufer sequence. This property is often used in counting labeled unrooted trees with degree constraints using dynamic programming (DP).
    5. There are $n^{(n-1)}$ rooted trees with $n$ nodes.
    6. Consider a rooted tree forest with $n$ nodes and $m$ trees. We introduce a virtual vertex $n+1$ and construct an unrooted tree with $n+1$ nodes, assuming that vertex $n+1$ is the root. Then, remove the virtual vertex, and its children will form several rooted tree forests. We only need to ensure that the degree of vertex $n+1$ is exactly $m$. Therefore, the number of valid configurations is $C_{n-1}^{m-1}\ n^{n-m}$.
    7. Next, let's consider the case where there is no restriction on the number of rooted tree forests. Using the method of introducing the virtual vertex mentioned above, we can directly obtain the number of configurations as $(n+1)^{n-1}$.

无根树转化为$prufer$序列:

    1. 每次找到编号最小的叶子节点
    2. 删除该节点并在序列中添加与该节点相连的节点的编号
    3. 重复$1,2$操作，直到整棵树只剩下两个节点

$prufer$序列转化为无根树:

    1. 每次取出$prufer$序列中最前面的元素$u$
    2. 在点集$V$（$V$初始化为$ \{ 1,2,...,n \} $）中找到编号最小的没有在$prufer$序列中出现的元素$v$
    3. 给$u,v$连边然后在序列中删除$u$，在$V$中删除$v$
    4. 重复$1,2,3$,直到序列为空。此时在点集$V$中剩下两个节点，给它们连边即可

其中点集$V$用$set$维护可以做到$P(nlogn)$的复杂度

证明：

注意到，如果一个节点$A$不是叶子节点，那么它至少有两条边；但在无根树转化为$prufer$序列的过程结束后，整个图只剩下一条边，因此节点$A$的至少一个相邻节点被去掉过，节点A的编号将会在这棵树对应的$prufer$编码中出现。反过来，在$prufer$编码中出现过的数字显然不可能是这棵树（初始时）的叶子。于是我们看到，没有在$prufer$编码中出现过的数字恰好就是这棵树（初始时）的叶子节点。把序列的第一个元素和$V$中最小编号的未出现点删除后，即在原树中删除了一个叶子节点和对应的边，剩下的新树和剩下的$prufer$序列以及$V$仍然满足以上性质（只是多了个必须在$V$中出现的限制，这是因为不在$V$中出现的点此时已经不在新树中了）（如果想不明白对应关系的话，可以考虑画两个图，第一个图即原树（可以看作$V$），第二个图是利用$prufer$序列构造的树，每在原树中删除一个顶点一条边，就在第二个图中加入一条边）

性质：

    1. $prufer$序列中节点$i$出现的次数为$d_i-1$
    2. 一棵$n$个节点的无根树唯一地对应了一个长度为$n-2$的数列，数列中的每个数都在$1$到$n$的范围内（考虑转换中的$4$步，每一步都一定可行，第$3$步中序列的大小总比$V$大$2$，所以总能找到满足的$v$）
    3. $n$个节点的有标号无根树有$n^{(n-2)}$个，也可以看作完全无向图的生成树个数
    4. $n$个节点的度数分别为$d_1,d_2,...,d_n$的无根树共有$\frac {(n-2)!} {\Pi(d_i-1)!}$个，因为此时$prufer$序列中节点$i$出现了$d_i-1$次（又因为该性质的存在，$prufer$序列常用于$dp$计算有度数限制的有标号无根树计数中）
    5. $n$个节点的有根树有$n^{(n-2)}*n=n^{(n-1)}$
    6. 考虑有$n$个节点，$m$棵树的有根树森林计数我们建一个虚点$n+1$，然后构造一棵$n+1$个点的无根树，并假定以$n+1$号点为根，然后把虚点拆掉，它的儿子就会形成若干棵有根树森林，我们只需要保证$n+1$号点的度数恰好为$m$即可。所以方案数为$C_{n-1}^{m-1}\  n^{n-m}$
    7. 接下来我们考虑不限制有多少棵树的有根树森林。可以由上面建虚点的方法直接得到方案数$(n+1)^{n-1}$