# Merge Sort

## Divide and Conquer

**Divide** the problem into a number of subproblems.

**Conquer** the subproblems by solving them recursively.

**Combine** the subprolems solutions to give a solution to the original problem.

## Implementation

### Pseudocode

``` js
MERGE-SORT(A, p, r)
if p < r
    then q ← ⎣(p+r)/2⎦
        MERGE-SORT(A, p, q)
        MERGE-SORT(A, q+1, r)
        MERGE(A, p, q, r)
```

### Java Implementation

``` java
public class Merge
{
    private static void merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi)
    {
        for (int k = lo, k <= hi, k++)
            aux[k] = a[k];

        int i = lo;
        int j = mid + 1;

        for(int k = lo; k <= hi; k++)
        {
            if(i > mid)
                a[k] = aux[j++];
            else if (j > hi)
                a[k] = aux[i++];
            else if (less(aux[j], aux[i]))
                a[k] = aux[j++];
            else
                a[k] = aux[i++];
        }
    }

    private static sort(Comparable[] a, Comparable[] aux, int lo, int hi)
    {
        if(hi <= lo)
            return;

        int mid = lo + (hi - lo) / 2;
        sort(a, aux, lo, mid);
        sort(a, aux, mid+1, hi);
        merge(a, aux, lo, mid, hi);
    }

    public static sort(Comparable[] a)
    {
        //The reason new a Comparable array here is to save the memory allocation and garbage collection time.
        Comparable[] aux = new Comparable[a.length];
        sort(a, aux, 0 , a.length - 1);
    }

}
```

## Analysis

### Performance

#### Analyzing the divide and conquer algorithms

T(n) = aT(n/b) + D(n) + C(n) when n >= c

+ D(n) = the time to divide a size-n problem
+ C(n) = the time to combine solutions
+ a = the number of subproblems
+ b = each 1/b the size of the original

#### Analyzing the merge sort

T(n) = 2T(n/2) + ɵ(n) if n > 1

Uses at most NlogN compares and 6NlogN array accesses.

#### Draw a recursion tree

T(n) = 2T(n/2) + cn if n > 1. For the original problem, we have a cost of cn, plus the two subproblems, each costing T(n/2).

![recursionTree](http://d.pr/i/M5AK)

### Order of Growth

T(n) = O(NlogN)

## Improvement

### Use Insertion sort for small subarrays.
Mergesort has too much overhead for tiny subarrays. Cutoff to insertion sort for ~7 items.

``` java
private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi)
{
    if (hi <= lo + CUTOFF - 1)
        Insertion.sort(a, lo, hi);
    int mid = lo + (hi - lo) / 2;
    sort(a, aux, lo, mid);
    sort(a, aux, mid + 1, hi);
    merge(a, aux, lo, mid, hi);
}
```

### Stop if already sorted

``` java
private static void sort(Comparable[] a, Comparable[] aux, int lo, int hi)
{
    if (hi <= lo)
        return;
    int mid = lo + (hi - lo) / 2;
    sort(a, aux, lo, mid);
    sort(a, aux, mid + 1, hi);

    if(!less(a[mid+1], a[mid]))
        return;

    merge(a, aux, lo, mid, hi);
}
```

### Eliminate the copy to the auxiliary array

``` java
private static void merge(Comparable[] a, Comparable[] aux, int lo, int mid, int hi)
{
    int i = lo;
    int j = mid + 1;

    for(int k = lo; k <= hi; k++)
    {
        if(i > mid)
            aux[k] = a[j++];
        else if (j > hi)
            aux[k] = a[i++];
        else if (less(a[j], a[i]))
            aux[k] = a[j++];
        else
            aux[k] = a[i++];
    }
}

private static sort(Comparable[] a, Comparable[] aux, int lo, int hi)
{
    if(hi <= lo)
        return;

    int mid = lo + (hi - lo) / 2;
    sort(aux, a, lo, mid);
    sort(aux, a, mid+1, hi);
    merge(aux, a, lo, mid, hi);
}
```

