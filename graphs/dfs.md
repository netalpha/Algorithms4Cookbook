# Undirected Graphs - DFS

+ Mimic maze exploration
+ Find all vertical connected to a given source vertex
+ Find a path between two vertices

## API

``` java
public interface Paths
{
    boolean hasPathTo(int v);
    Iterable<Integer> pathTo(int v);
}
```

## Implementation

``` javascript
// Put unvisited vertices on a stack
DFS(to visit a vertex v)
{
    Mark v as visted.
    Recursively visit all unmarked vertices w adjacent to v.
}
```

``` java
public class DepthFirstSearch implements Paths
{
    //marked[v] = true if v connected to s
    private boolean[] marked;
    //edgeTo[v]=previous vertex on path from s to v
    private int[] edgeTo;
    private int s;

    public DepthFirstSearch(Graph G, int s)
    {
        //...
        dfs(G, s);
    }

    //+ if *w* marked, then *w* connected to *s*.
    //+ if *w* connected to *s*, then *w* marked.
    private void dfs(Graph G, int v)
    {
        marked[v] = true;
        for(int w: G.adj(v))
        {
            if(!marked[w])
            {
                dfs(G, w);
                edgeTo[w] = v;
            }
        }
    }

    public boolean hasPathTo(int v)
    {
        return marked[v];
    }

    public Iterable<Integer> pathTo(int v)
    {
        if(!hasPathTo)
            return null;
        Stack<Integer> path = new Stack<Integer>();
        for(int x = v; x != s; x = edgeTo[x])
        {
            path.push(x);
        }
        path.push(s);
        return path;
    }

}
```

## Application: Connectivity Queries

+ Connected component: A maximal set of connected vertices.

### API

``` java
public interface CC
{
    boolean connect(int v, int w);
    int count(); //number of connected components
    int id(int v); //component identifier for v
}
```

### Implementation

``` javascript
Connected components
{
    Initilize all vertices v as unmarked.
    For each unmared vertex v, run DFS to identify all vertices discovered as part of the same component.
}
```

``` java
public class CCImpl implements CC
{
    private boolean marked[];
    private int[] id;  // id of component containing v
    private int count; // number of components

    public CCImpl(Graph G)
    {
        marked = new boolean[G.V()];
        id = new int[G.V()];

        for(int v = 0; v < G.V(); v++)
        {
            if(!marked[v])
            {
                dfs(G, v);
                count ++;
            }
        }
    }

    private dfs(Graph G, int v)
    {
        marked[v] = true;
        id[v] = count;
        for (int w: G.adj(v))
            if(!marked[w])
                dfs(G, w);
    }

    public int count()
    {
        return count;
    }

    public int id(int v)
    {
        return id[v];
    }
}
```

