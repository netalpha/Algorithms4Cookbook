# Shell Sort

Move entries more than one position at a time by *h-sorting* the array.

### Increment Sequence

**3x + 1**: 1, 4, 13, 40, 121, 364, ...

### Java Implementation

``` java
public class Shell
{
    public static void sort(Comparable[] a)
    {
        int N = a.length;

        int h = 1;
        while (h < N / 3) h = 3 * h + 1;

        while (h >= 1)
        {
            //h-sort the array
            for (int i = h; i < N; i ++)
            {
                for (int j = i; j >= h && less(a[j], a[j - h]); j -= h)
                    exch(a, j, j-h);
            }

            h /= 3;
        }
    }
}
```

### Analysis

T(n) = O(n<sup>3/2</sup>)

