# Directed Graph - DFS


``` javascript
DFS(to visit a vertex v)
{
    Mark v as visited.
    Recursively visit all unmarked vertices w pointing from v.
}
```

``` java
public class DirectedDFS
{
    private boolean[] marked;

    public DirectedDFS(Digraph G, int s)
    {
        marked = new boolean[G.V()];
        dfs(G, s);
    }

    private void dfs(Digraph G, int v)
    {
        marked[v] = true;
        for(int w: G.adj(v))
        {
            if(!marked[w])
                dfs(G, w);
        }
    }
}
```

## Application

Basis:

+ 2-satisfiability
+ Directed Euler path
+ Strong-connected components

### Reachability

Find all vertices reachable from s along a directed path.

Application:

+ Program control-flow analysis
+ mark-sweep garbage collector

``` java
public boolean visited(int v)
{
    return marked[v];
}
```

### Path finding

NONE…

### Topological sort

+ DAG: Directed acyclic graph
+ Topological sort: Redraw DAG so all edges point upwards.

#### Implementation

``` java
public class DepthFirstOrder
{
    private boolean[] marked;
    private Stack<Integer> reversePost;

    public DepthFirstOrder(Digraph G)
    {
        reversePost = new Stack<Integer>();
        marked = new boolean[G.V()];
        for(int v = 0; v < G.V(); v++)
        {
            if (!marked[v])
                dfs(G, v);
        }
    }

    private void dfs(Digraph G, int v)
    {
        marked[v] = true;
        for(int w: G.adj(v))
        {
            if(!marked[w])
                dfs(G, w);
        }
        reversePost.push(v);
    }

    public Iterable<Integer> reversePost()
    {
        return reversePost;
    }
}
```

### Directed cycle detection

A digraph has a topological order iff no directed cycle.

application:

+ cyclic inheritance
+ spreadsheet recalculation

## Strongly-connected components

*Def*: Vertices v and w are strongly connected if there is both a directed path from v to w and a directed path from w to v. A strong component is a maximal subset of strongly-connected vertices.

Application:

+ ecological food webs
+ software modules

### Implementation with Kosaraju-Sharir algorithm

+ Phase 1: run DFS on G<sup>R</sup> to compute reverse post order.(Topological sort)
+ Phase 2: run DFS on G, considering vertices in order given by first DFS.


``` java
public class KosarajuSharirSCC
{
    private boolean marked[];
    private int id[];
    private int count;

    public KosarajuSharirSCC(Digraph G)
    {
        marked = new boolean[G.V()];
        id = new int[G.V()];

        DepthFirstOrder dfs = new DepthFirstOrder(G.reverse());
        for (int v: dfs.reversePost())
        {
            if(!marked[v])
            {
                dfs(G, v);
                count++;
            }
        }
    }

    private void dfs(Digraph G, int v)
    {
        marked[v] = true;
        id[v] = count;
        for(int w: G.adj(v))
        {
            if(!marked[w])
            {
                dfs(G, w);
            }
        }
    }

    public boolean stronglyConnected(int v, int w)
    {
        return id[v] == id[w];
    }
}
```
