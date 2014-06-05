# Insertion Sort

**Input**: A sequence of n numbers **a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub>**

**Output**: A permutation **a'<sub>1</sub>, a'<sub>2</sub>, ..., a'<sub>n</sub>** of the input sequence such that **a'<sub>1</sub> <= a'<sub>2</sub><= ... <= a'<sub>n</sub>**

## Implementation

#### Pseudocode

``` java
INSERTION-SORT(A)
for i ← 2 to n
    do key ← A[i]
    ▷ Insert A[i] into the sorted sequence A[1 ... i-1].
    j ← i - 1
    while j > 0 and A[j] > key
        do A[j + 1] ← A[j]
            j ← j - 1
    A[j + 1] ← key
```

#### Java Implementation

``` java
public class Insertion
{
    public public static void sort(Comparable[] a)
    {
        int N = a.length;
        for (int i = 0; i < N; i++)
            for (int j = i; j > 0; j--)
                if (less(a[j], a[j-1]))
                    exch(a, j, j-1);
                else
                    break;
    }

    private static boolean less(Comparable v, Comparable w)
    {
        return v.compareTo(w) < 0;
    }

    private static void exch(Comparable[] a, int i, int j)
    {
        Comparable swap = a[i];
        a[i] = a[j];
        a[j] = swap;
    }
}
```

## Analysis

#### Performance

##### Best case

The array is already sorted. `T(n) = n` with `n-1` compares and `0` exchange.

##### Worst case

If the array is in descending order, it makes ~1/2n<sup>2</sup> compares and ~1/2n<sup>2</sup> exchanges.

#### Order of Growth

T(n)=O(n<sup>2</sup>)
