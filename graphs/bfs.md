# Undirected Graphs - BFS

+ BFS examines vertices in increasing distance from s.
+ put unvisited vertices on a queue
+ queues always consists of zero or more vertices of distance k from s.

## API

None.

## Implementation

``` javascript
BFS(from source vertex s)
{
    Put s onto a FIFO queue, and mark s as visited.
    Repeat until the queue is empty:
        remove the least recently added vertex v
        add each of v's unvisited neighbors to the queue, and mark them as visited.
}
```

``` java
public class BreadthFirstSearch
{
    private boolean[] marked;
    private boolena[] edgeTo[];
    private final int s;

    //...

    private void bfs(Graph G, int s)
    {
        Queue<Integer> q = new Queue<Integer>();
        q.enqueue(s);
        marked[s] = true;
        while(!q.isEmpty())
        {
            int v = q.dequeue();
            for(int w: G.adj(v))
            {
                if(!marked[w])
                {
                    q.enqueue(w);
                    marked[w] = true;
                    edgeTo[w] = v;
                }
            }
        }
    }
}
```
