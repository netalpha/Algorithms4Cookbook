# Undirected Graphs

## API

``` java
public interface Graph
{
    void addEdge(int v, int w);
    Iterable<Integer> adj(int v);
    int V(); //number of verticles
    int E(); //number of edges
    String toString();
}
```

## Implementation

Representation:

+ linked list or array
+ adjacency-matrix graph: V-by-V boolean array
+ adjacency-list graph

``` java
public class UndirectedGraph implements Graph
{
    private final int V;
    private Bag<Integer>[] adj;

    public Graph(int V)
    {
        this.V = V;
        adj = (Bag<Integer>[]) new Bag[V];
        for(int v = 0; v < V; v++)
            adj[v] = new Bag<Integer>();
    }

    public void addEdge(int v, int w)
    {
        adj[v].add(w);
        adj[w].add(v);
    }

    public Iterable<Integer> adj(int v)
    {
        return adj[v];
    }

    public static int degree(Graph G, int v)
    {
        int degree = 0;
        for (int w: G.adj(v))
            degree++;
        return degree;
    }

    public static int maxDegree(Graph G)
    {
        int max = 0;
        for (int v = 0; v < G.V(); v++)
        {
            if(degree(G, V) > max)
                max = degree(G, V);
            return max;
        }
    }

    public static double averageDegree(Graph G)
    {
        return 2.0 * G.E() / G.V();
    }

    public static int numberOfSelfLoops(Graph G)
    {
        int count = 0;
        for(int v = 0; v < G.V(); v++)
        {
            for(int w: G.adj(v))
                if(w == v)
                    count ++;
        }
        return count / 2;
    }
}
```

## Analysis

| **representation** | **space** | **add edge** | **edge between v and w?** | **iterate over vertices adjacent to v?** |
| ------------ | ------------- | ------------ | ------| ---|
| list of edges | E| 1  | E | E |
| adjacency matrix | V<sup>2</sup> | 1  | 1 | V |
| adjacency lists | E + V | 1 | degree(v) | degree(v) |

