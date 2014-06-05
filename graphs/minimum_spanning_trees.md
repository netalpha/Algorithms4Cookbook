# Minimum Spanning Trees

`A spanning tree of G`: a subgraph T that is **connected** and **acyclic**.

`MST`: a min weight spanning tree

`cut`: a partition of its vertices into two (nonempty) sets

`cross edge`: connects a vertex in one set with a vertex in the other

`Cut property`: Given any cut, the crossing edge of min weight is in the MST

## API

``` java
public interface MST
{
    Iterable<Edge> edges();
    double weight(); //weight of MST
}
```

## Implementation with Greedy Algorithms

*Simplifying assumptions*:

+ Edge weights are distinct: Greedy MST algorithm still correct if equal weights are present
+ Graph is connected: Compute minimum spanning forest = MST of each component

### Kruskal's algorithm

+ Consider edges in ascending order of weight
+ Add next edge to tree T unless doing so would create a cycle

``` java
public class KruskalMST implements MST
{
    private Queue<Edge> mst = new Queue<Edge>();

    public KruskalMST(EdgeWeightedGraph G)
    {
        MinPQ<Edge> pq = new MinPQ<Edge>();
        for(Edge e: G.edges())
        {
            pq.insert(e);
        }

        UF uf = new UF(G.V());
        while(!pq.isEmpty() && mst.size() < G.V() -1)
        {
            Edge e = pq.delMin();
            int v = e.either(), w = e.other(v);
            if (!uf.connected(v, w))
            {
                uf.union(v, w);
                mst.enqueue(e);
            }
        }
    }

    public Iterable<Edge> edges()
    {
        return mst;
    }
}
```

#### Analysis

| Operation | frequency | time per op |
| ------------ | ------------- | ------------ |
| build pq | 1  | ElogE |
| delete-min | E  | logE(why?) |
| union | V | lgV|
| connected | E | lgV |

### Prim's algorithm

``` java
public class LazyPrimMST implements MST
{
    private boolean[] marked; //MST vertices
    private Queue<Edge> mst;
    private MinPQ<Edge> pq;

    public LazyPrimMST(WeightedGraph G)
    {
        pq = new MinPQ<Edge>();
        mst = new Queue<Edge>();
        marked = new boolean[G.V()];
        visit(G, 0);

        while (!pq.isEmpty() && mst.size() < G.V() -1)
        {
            Edge e = pq.delMin();
            int v = e.either(), w = e.other(v);
            if (marked[v] && marked[w])
                continue;
            mst.enqueue(e);
            if (!marked[v])
                visit(G, v);
            if (!marked[w])
                visit(G, w);
        }
    }

    private void visit(WeightedGraph G, int v)
    {
        marked[v] = true;
        for(Edge e: G.adj(v))
        {
            if(!marked[e.other(v)])
                pq.insert(e);
        }
    }

    public Iterable<Edge> mst()
    {
        return mst;
    }
}
```

### Borüvka's algorithm

None...

