# Quick Sort

## Implementation

### Psudocode

``` javascript
QUICKSORT(A, lo, hi)
{
    if (lo < hi)
        then x ← PARTITION(A, lo, hi)
            QUICKSORT(A, lo, x - 1)
            QUICKSORT(A, x + 1, hi)
}

PARTITION(A, lo, hi)
{
    x ← A[hi]
    i ← lo-1
    for j ← lo to hi-1
        do if A[j] <= x
            then i ← i+1
                exchange A[i] with A[j]
    exchange A[i+1] with A[hi]
    return i+1
}
```

### Java Implementation

``` java
public class Quck
{
    private static void sort(Comparable[] a)
    {
        StdRandom,shuffle(a); //shuffle needed for performance guarantee
        sort(a, 0, a.length - 1);
    }

    private static void sort(Comparable[] a, int lo, int hi)
    {
        if(hi <= lo)
            return;
        int j = partition(a, lo, hi);

        sort(a, lo, j-1);
        sort(a, j+1, hi);
    }

}
private static int partition(Comparable[] a, int lo, int hi)
{
    int i = lo;
    int j = hi + 1;
    while(true)
    {
        while(less(a[++i], a[lo])) //find item from left to swap
            if (i == hi)
                break;
        while(less(less(a[lo], a[--j]))) //find iterm from right to swap
            if (j == lo)
                break;

        if (i >= j) //check if pointers cross
            break;

        swap(a, i, j); //swap
    }

    exch(a, lo, j); //swap with partition iterm
    return j; //return index of item known to be in place
}
```

## Analysis

### Performance

+ Worst case: 1/2 N<sup>2</sup> compares
+ Average case: `~1.39NlogN`=`~2NlnN` compares and `~1/3 NlnN` exchanges

+ Probabilistic guarantee againist worst case
+ faster than merge sort in practice because of less data movement

### Order of Growth

T(n) = O(nlgn)

## Improvement

### Insertion sort small subarrays

``` java
private static void sort(Comparable[] a, int lo, int hi)
{

    if(hi <= lo + CUTOFF - 1)
    {
        Insertion.sort(a, lo, hi);
        return;
    }

    int j = partition(a, lo, hi);

    sort(a, lo, j-1);
    sort(a, j+1, hi);
}
```

### Median of Sample

``` java
private static void sort(Comparable[] a, int lo, int hi)
{

    if(hi <= lo)
        return;

    // Estimate true median by taking median of sample
    // Median-of-3 (random) items
    int m = medianOf3(a, lo, lo + (hi - lo)/2, hi);
    swap(a, lo, m);

    int j = partition(a, lo, hi);

    sort(a, lo, j-1);
    sort(a, j+1, hi);
}
```

### the Problem of Duplicate Keys

Algorithm goes quadratic unless partitioning stops on equal keys.

#### Dijkstra 3-way partitioning

+ let v be partitioning item a[lo]
+ Scan i from left to right
    + (a[i]  < v): exchange a[lt] with a[i]; increment both lt and i
    + (a[i]  > v): exchange a[gt] with a[i]; decrement gt
    + (a[i] == v): increment i

``` java
private static void sort(Comparable[] a, int lo, int hi)
{
    if(hi >= lo)
        return;

    int lt = lo, gt = hi;
    Comparable v = a[lo];
    int i = lo + 1;

    while (i <= gt)
    {
        int cmp = a[i].compareTo(v);
        if(cmd < 0)
            exch(a, lt++, i++);
        else if (cmp > 0)
            exch(a, i, gt--);
        else
            i++;
    }
    sort(a, lo, lt - 1);
    sort(a, gt+1, hi);
}
```

