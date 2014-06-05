# Red-black Search Trees


+ represent 2-3 tree as a BST
+ Use 'internal' left-leaning links as 'glue' for 3-nodes.

![](http://farm8.staticflickr.com/7378/9191629658_7747d796a8.jpg "red-black BST def")

## Implementation

``` java
public class RedBlackTree
{
    private static final boolean RED   = true;
    private static final boolean BLACK = false;

    private class Node
    {
        Key key;
        Value val;
        Node left, right;
        Boolean color; // color of parent link

        Node(Key key, Value val, Boolean color)
        {
            this.key = key;
            this.val = val;
            this.color = color;
        }
    }

    //the color of the right node is RED
    private Node rotateLeft(Node h)
    {
        Node x  = h.right;
        h.right = x.left;
        x.left  = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }

    //the color of the left node is RED
    private Node rotateRight(Node h)
    {
        Node x  = h.left;
        h.left  = x.right;
        x.right = h;
        x.color = h.color;
        h.color = RED;
        return x;
    }

    private void flipColors(Node h)
    {
        h.color       = RED;
        h.left.color  = BLACK;
        h.right.color = BLACk:
    }

    private Node put(Node h, Key key, Value val)
    {
        if(h == null)
            return new Node(key, Value, RED);

        int cmp = key.compareTo(h.key);
        if(cmp < 0)
            h.left = put(h.left, key, val);
        else if(cmp > 0)
            h.right = put(h.right, key, val);
        else
            h.val = val;

        //case 1: insert into a 2-node at the bottom
        //case 2: insert into a 3-node at the bottom
        if(!isRed(h.left) && isRed(h.right))
            h = rotateLeft(h);
        if(isRed(h.left) && isRed(h.left.left))
            h = rotateRight(h);
        if(isRed(h.left) && !isRed(h.right))
            flipColor(h);

        return h;
    }

    //Search is the same as for elementary BST(ignore color)
    public Val get(Key key)
    {
        Node x = root;
        while(x != null)
        {
            int cmp = key.compareTo(x.key);
            if(cmp < 0)
                x = x.left;
            else if (cmp > 0)
                x = x.right;
            else
                return x.val;
        }
        return null;
    }

    //Same for floor(), iteration(), selection()
}
```

## Analysis

| ST implementation | worst-case cost          ||| average case           ||| ordered iteration | key interface |
|------------------ | ----------|--------|-------- | -----------|---|---------- |-------------------|---------------|
|                   | search | insert | delete   | search  | insert |delete |                   |               |
|**unordered list** | N      | N      | N        | N/2     | N      | N/2   | no                | equals()      |
|**ordered array**  | lgN    | N      | N        | lgN     | N/2    | N/2   | yes               | compareTo()   |
|**BST**            | N      | N      | N        | 1.39lgN |1.39lgN | √n    | yes               | compareTo()   |
| **2-3 tree**    | clgN     | clogN  | clgN     | clgN    | clgN   | clogN | yes               | compareto()   |
|**red-black BST**| 2lgN     | 2logN  | 2lgN     | 1lgN    | 1lgN   | 1logN | yes               | compareto()   |





