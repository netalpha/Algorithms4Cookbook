# Directed Graph - BFS

``` javascript
BFS(from source vertex s)
{
    Put s onto a FIFO queue, and mark s as visited.
    Repeat until the queue is empty:
        remove the least recently added vertex v.
        for each unmarked vertex pointing from v:
            add to queue and mark as visited.
}
```

## Application

### Multisource Shortest Paths

Given a digraph and a set of source vertices, find shortest path from any vertex in the set to each other vertex.

Solution: Use BFS, but initialize by enqueuing all source vertices.

### Web crawler

Solution:

+ Choose root web page as source s
+ Maintain a Queue of websites to explore
+ Maintain a SET of discovered websites
+ Dequeue the next website and enqueue websites to which it links

#### Implementation

``` java
public class WebCrawler
{
    private Queue<String> queue = new Queue<String>();
    private Set<String> marked = new Set<String>();

    String root = "http://www.yoursite.com";
    queue.enqueue(root);
    marked.add(root);

    while(!queue.isEmpty)
    {
        String v = queue.dequeue();

        StdOut.println(v);
        In in = new In(v);
        String input = in.readAll();

        String regexp = "http://(\\w+\\.)*(\\w+)";
        Pattern pattern = Pattern.compile(regexp);
        Matcher matcher = pattern.matcher(input);
        while(matcher.find())
        {
            String w = matcher.group();
            if(!marked.contain(w))
            {
                marked.add(w);
                queue.enqueue(w);
            }
        }
    }
}
```

