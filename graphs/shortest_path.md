# Shortest Path

Given an edge-weighted digraph, find the shortest path from s to t.

## API

``` java
public interface SP
{
    double distTo(int v); //length of shortest path from s to v
    Iterable<DirectedEdge> pathTo(int v);
    boolean hasPathTo(int v);
}

```

### DirectedEdge

``` java
public interface DirectedEdge
{
    int from();
    int to ();
    double weight();
}

public class DirectedEdgeImpl implements DirectedEdge
{
    private final int v, w;
    private final double weight;

    public DirectedEdge(int v, int w, double weight)
    {
        this.v = v;
        this.w = w;
        this.weight = weight;
    }

    public int from()
    {
        return v;
    }

    public int to()
    {
        return w;
    }

    public int weight()
    {
        return weight;
    }
}
```

### Edge-weighted Digraph

``` java
public interface EdgeWeightedDigraph
{
    void addEdge(DirectedEdge e);
    Iterable<DirectedEdge> adj(int v);
    int V();
    int E();
    Iterable<DirectedEdge> edges();
}

public class EdgeWeightedDigraph
{
    private final int V;
    private final Bag<Edge>[] adj;

    public EdgeWeightedDigraph(int V)
    {
        this.V = V;
        adj = (Bag<DirectedGraph>[]) new Bag[V];
        for (int v = 0; v < V; v++)
        {
            adj[v] = new Bag<DirectedEdge>();
        }
    }

    public void addEdge(DirectedEdge e)
    {
        int v = e.from();
        adj[v].add(e);
    }

    public Iterable<DirectedEdge> adj(int v)
    {
        return adj[v];
    }
}
```

## Implementations

### Dijkstra's algorithm

*Constraints*:

+ Nonnegative weights

``` java
// + consider vertices in increasing order of distance from s
// + Add vertex to tree and relax all edges pointing from that vertex
public class DijkstraSP implements SP
{
    private DirectedEdge[] edgeTo;
    private double[] distTo;
    private IndexMinPQ<Double> pq;

    public DijkstraSP(EdgeWeightedDigraph G, int s)
    {
        edgeTo = new DirectedEdge[G.V()];
        distTo = new double[G.V()];
        pq = new IndexMinPQ<Double>(G.V());

        for (int v = 0; v < G.V(); v++)
        {
            distTo[v] = Double.POSITIVE_INFINITY;
        }
        distTo[s] = 0.0;

        pq.insert(s, 0.0);
        while (!pq.isEmpty())
        {
            int v = pq.delMin();
            for (DirectedEdge e: G.adj(v))
                relax(e);
        }
    }

    private void relax(DirectedEdge e)
    {
        int v = e.from(), w = e.to();
        if (distTo[w] > distTo[v] + e.weight())
        {
            distTo[w] = distTo[v] + e.weight();
            edgeTo[w] = e;
            if(pq.contains(w))
                pq.decreaseKey(w, distTo[w]);
            else
                pq.insert(w, distTo[w]);
        }
    }
}
```

## Topological sort algorithm

*Improvement*;

+ no directed cycles(DAG)
+ can be negative weighted DAG

``` java
// + Consider vertices in topplogical order
// + Relax all edges pointing from that vertex.
public class AcyclicSP implements SP
{
    private DirectedEdge[] edgeTo;
    private double[] distTo;

    public AcyclicSP(EdgeWeightedDigraph G, int s)
    {
        edgeTo = new DirectedEdge[G.V()];
        distTo = new double[G.V()];

        for (int v = 0; v < G(); v++)
        {
            distTo[v] = Double.POSITIVE_INFNITY;
        }
        distTo[s] = 0.0;

        Topological topological = new Topological(G);
        for (int v : topological.order())
        {
            for (DirectedEdge e : G.adj(v))
            {
                relax(e);
            }
        }
    }
}
```

## Bellman-Ford algorithm

*Improvement*: negative cycles

``` java
// + Initialize distTo[s] = 0 and distTo[v] = ∞ for all other vertices
// + Repeat V times:
//      Relax each edge
for (int i = 0; i < G.V(); i++)
    for (int v = 0; v < G.V(); v++)
        for(DirectedEdge e : G.adj(v))
            relax(e);
```



