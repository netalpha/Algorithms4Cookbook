# Binary Search Trees

A BST is a `binary tree` in `symmetric order`.

`binary tree` is either:

* empty
* two disjoint binary tree(left and right)

`symmetric order`: Each node has a key, and every node's key is:

* Larger then all keys in its left subtree.
* Smaller than all keys in its right subtree.

## API

``` java
public interface BST<Key extends Comparable<Key>, Value>
{
    public void put(Key key, Value val);
    public Value get(Key key);
    public void delete(Key key);
    public Iterable<Key> iterator();

    //ordered operation
    public Key max();
    public Key min();
    public Key floor(Key key); //largest key <= a given key
    public Key ceiling(Key key);//smallest key >= a given key
    public int size(); //the count at the root
    public int rank(Key key);
    public void deleteMin();
    public void deleteMax();
}
```


## Implementation

A BST is reference to a root node.

``` java
public class BSTImpl implements BST
{
    private Node root;

    private class Node
    {
        private Key key;
        private Value val;
        private Node left, right;

        //ordered operation
        private int count;

        public Node(Key key, Value val)
        {
            this.key = key;
            this.val = val;
        }
    }

    //Key in tree: reset value
    //Key not in tree: add new node
    public void put(Key key, Value val)
    {
        put(root, key, val);
    }

    private Node put(Node x, Key key, Value val)
    {
        if(x == null)
            return new Node(key, val);
        int cmp = key.compareTo(x.key);
        if(cmp < 0)
            x.left = put(x.left, key, val);
        else if (cmp > 0)
            x.right = put(x.right, key, val);
        else
            x.val = val;

        x.count = 1 + size(x.left) + size(x.right);
        return x;
    }

    //Return value corresponding to given key,
    //or null if no such key.
    public Value get(Key key)
    {
        Node x = root;
        while(x != null)
        {
            int cpm = key.compareTo(x.key);
            if(cmp < 0)
                x = x.left;
            else if(cmp > 0)
                x = x.right;
            else if(cpm == 0)
                return x.val;
        }
        return null;
    }

    public void delete(Key key)
    {
    }

    public Iterable<Key> iterator()
    {
    }

    /**
    * ORDERED OPERATION
    */

    public Key floor(Key key)
    {
        Node x = floor(root, key);
        if (x == null)
            return null;
        return x.key;
    }
    private Node floor(Node x, Key key)
    {
        if(x == null)
            return null;
        int cmp = key.compareTo(x.key);

        if(cmp == 0)
            return x;
        else if (cmp < 0)
            return floor(x.left, key);

        Node t = floor(x.right, key);
        if(t != null)
            return t;
        else
            return x;
    }

    public int size()
    {
        return size(root);
    }
    private int size(Node x)
    {
        if(x == null)
            return 0;
        return x.count;
    }

    public int rank(Key key)
    {
        return rank(root, key);
    }
    private int rank(Node x, Key key)
    {
        if(x == null)
            return 0;

        int cpm = key.compareTo(x.key);
        if(cpm < 0)
            return rank(x.left, key);
        else if(cpm > 0)
            return 1 + size(x.left) + rank(x.right, key);
        else
            return size(x.left);
    }

    public Iterable<Key> keys()
    {
        Queue<Key> q = new Queue<Key>();
        inorder(root, q);
        return q;
    }
    private void inorder(Node x, Queue<Key> q)
    {
        if(x == null)
            return;
        inorder(x.left, q);
        q.enqueue(x.key);
        inorder(x.right, q);
    }

    public void deleteMin()
    {
        root = deleteMin(root);
    }
    private Node deleteMin(Node x)
    {
        if(x.left == null)
            return x.right;
        x.left = deleteMin(x.left);
        x.count = 1 + size(x.left) + size(x.right);
        return x;
    }

    //case 0 with no children: delete t by setting parent link to null
    //case 1 with 1 child: delete t by replacing parent link
    //case 2 with 2 child: delete the min in t's right subtree and put it in t's spot.
    public void delete(Key key)
    {
        root = delete(root, key);
    }
    private Node delete(Node x, Key key)
    {
        if(x == null)
            return null;
        int cpm = key.compareTo(x.key);
        if(cpm < 0)
            x.left = delete(x.left, key);
        else if (cpm > 0)
            x.right = delete(x.right, key);
        else
        {
            if(x.right == null)
                return x.left;
            if(x.left == null)
                return x.right;

            Node t = x;
            x = min(t.right);
            x.right = deleteMin(t.right);
            x.left = t.left;
        }

        x.count = size(x.left) + size(x.right) + 1;
        return x;
    }
}


```

## Analysis

### ST Analysis

| ST implementation | worst-case cost ||| average case   ||| ordered iteration | key interface |
|------------------------ | --------------------- | ------------------ |----------------------|-------------------|
|                                | search | insert | delete |  search | insert |delete |                              |                       |
|**unordered list** | N | N | N | N/2 | N | N/2 | no | equals() |
|**ordered array** | lgN  | N | N | lgN | N/2 | N/2 | yes | compareTo() |
|**BST**                   | N | N | N | 1.39lgN |1.39lgN| sqrt(N) | yes |compareTo() |

### Ordered ST Analysis

|                | unordered list | binary search | BST |
|-----------------------|---------------------------|-------------------------|---------|
|**search**| N| lgN| h |
|**insert/delete**| N| N| h |
|**min/ max**| N| 1| h |
|**floor/ceiling**| N| lgN| h |
|**rank**| N|lgN| h |
|**select**| N| 1| h |
|**ordered iteration**| NlgN| N| N |

h: height of BST(proportional to lgN if keys inserted in a random order)

