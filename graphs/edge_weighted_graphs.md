# Edge Weighted Graphs


## API

### Edge

``` java
public interface Edge implements Comparable<Edge>
{
    int either();       //either endpoint
    itn other(int v);   //the endpoint that's not v
    int compareTo(Edge that);
    double weight();
}
```

### Edge-weighted graph

``` java
public interface EdgeWeightGraph
{
    void addEdge(Edge e);
    Iterable<Edge> adj(int v);
    Iterable<Edge> edges();
    int V();
    int E();
}
```

## Implementation

### Edge

``` java
public class EdgeImpl implements Edge
{
    private final int v, w;
    private double weight;

    public Edge(int v, int w, double weight)
    {
        this.v = v;
        this.w = w;
        this.weight = weight;
    }

    public int either()
    {
        return v;
    }

    public int other(int vertex)
    {
        if(vertex == v)
            return w;
        else
            return v;
    }

    public int compareTo(Edge that)
    {
        if(this.weight < that.weight)
            return -1;
        else if (this.weight > that.weight)
            return 1;
        else
            return 0;
    }
}
```

### Edge-weighted graph

*Representation*: Vertex-indexed array of Edge lists.

``` java
public class EdgeWeightedGraphImpl implements EdgeWeightGraph
{
    private final V;
    //same as Graph, but adjacency lists of Edges instead of integers
    private final Bag<Edge>[] adj;

    public EdgeWeightedGraphImpl(int v)
    {
        this.V = V;
        adj = (Bag<Edge>[]) new Bag[V];
        for(int v = 0; v < V; v++)
        {
            adj[v] = new Bag<Edge>();
        }
    }

    public void addEdge(Edge e)
    {
        int v = e.either(), w = e.other(v);
        adj[v].add(e);
        adj[w].add(e);
    }

    public Iterable<Edge> adj(int v)
    {
        return adj[v];
    }
}
```


