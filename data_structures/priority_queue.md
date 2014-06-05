# Priority Queue

## API ##

``` java
public interface MaxPQ<Key extends Comparable<Key>>
{
    public void insert(Key v);
    public Key delMax();
    public boolean isEmpty();
    public Key Max();
    public int size();
}
```

## Implementation ##

### Unordered Array Implementation ###

``` java
public class UnorderMaxPQ<Key extends Comparable<Key>> implements MaxPQ
{
    private Key[] pq;
    private int N;

    public UnorderMaxPQ(int capacity)
    {
        pq = (Key[]) new Comparable[capacity];
    }

    public boolean isEmpty()
    {
        return N == 0;
    }

    public void insert(Key x)
    {
        pq[N++] = x;
    }

    public Key delMax()
    {
        int max = 0;
        for(int i = 0; i < N; i++)
        {
            if(less(max, i)) max = i;
        }
        exch(max, N-1);
        return pq[--N];
    }
}
```

### Ordered Array Implementation ###

Not implemented yet...

### Binary Heap Implementation ###

``` java
public class MaxPQ<Key extends Comparable<Key>>
{
    private Key[] pq;
    private int N;

    public MaxPQ(int capacity)
    {
        pq = (Key[]) new Comparable[capacity + 1];
    }

    public boolean isEmpty()
    {
        return N == 0;
    }

    public void insert(Key x)
    {
        //see the code below
    }

    public Key delMax()
    {
        //see the code below
    }

    private void swim(int k)
    {
        //see the code below
    }

    private void sink(int k)
    {
        //see the code below
    }

    private boolean less(int i, int j)
    {
        return pq[i].compareTo(pq[j]) < 0;
    }

    private void exch(int i; int j)
    {
        Key t  pq[i];
        pq[i] = pq[j];
        pq[j] = t;
    }
}
```

#### heap-ordered binary tree ####

* keys in nodes
* parent's key no smaller than children's keys.

#### Promotion in a heap ####

Scenario: Child's key becomes larger than its parent's key.

``` java
private void swim(int k)
{
    while(k > 1 && less(k/2, k))
    {
        exch(k, k/2);
        k = k/2;
    }
}
```

#### Insertion in a Heap ####

``` java
// add node at end, then swim it up with 1+lgN compares
public void insert(Key x)
{
    pq[++N] = x;
    swim(N);
}
```

#### Demotion in a Heap ####

Scenario: Parent's key becomes smaller than one or both of its children's.

``` java
private void sink(int k)
{
    while(2*k <= N)
    {
        int j = 2*k;
        if (j < N && less(j, j+1))
            j++;
        if (!less(k, j))
            break;
        exch(k, j);
        k = j;
    }
}
```

#### Delete the Maximum in a Heap ####

``` java
public Key delMax()
{
    Key max = pq[1];
    exch(1, N--);
    sink(1);
    pq[N+1] = null;
    return max;
}
```

## Analysis ##

| implementation | insert | del max | max|
| ------------ | ------------- | ------------ | ------------|
| unordered array | 1  | N | N |
| ordered array | N  | 1 | 1 |
| binary heat | logN | logN | 1 |
| d-ary heap | log<sub>d</sub>N | d log<sub>d</sub>N | 1 |
| Fibonacci | 1 | logN⁺ | 1 |
