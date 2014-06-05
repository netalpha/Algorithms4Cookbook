# Directed Graphs

## API

``` java
public interface Digraph
{
    void addEdge(int v, int w); //add a directed edge v->w
    Iterable<Integer> adj(int v);//vertices pointing from v
    int V();
    int E();
    Digraph reverse();
    String toString();
}
```

## Implementation

``` java
public class DegraphImpl implements Digraph
{
    private final int V;
    private final Bag<Integer>[] adj;

    public  DegraphImpl(int V)
    {
        this.V = V;
        adj = (Bag<Integer>[]) new Bag[V];
        for(int v = 0; v < V; v++)
        {
            adj[v] = new Bag<Integer>();
        }
    }

    public void addEdge(int v, int w)
    {
        adj[v].add(w);
    }

    public Iterable<Integer> adj(int v)
    {
        return adj[v];
    }
}
```

## Analysis

| **representation** | **space** | **add edge** | **edge between v and w?** | **iterate over vertices adjacent to v?** |
| ------------ | ------------- | ------------ | ------| ---|
| list of edges | E| 1  | E | E |
| adjacency matrix | V<sup>2</sup> | 1  | 1 | V |
| adjacency lists | E + V | 1 | outdegree(v) | outdegree(v) |

