# Symbol Table

## API ##

- Value are not `null`.
- Method `get()` returns `null` if key not present.
- Method `put()` overwrites old value with new value.

``` java
public interface ST<Key, Value>
{
    public void put(Key key, Value val);
    public Value get(Key key);
    public void delete(Key key);
    public boolean contain(Key key);
    public boolean isEmpty();
    public int size();
    public Iterable<Key> keys();

    //applied to ordered symbol table
    public Key min();
    public Key max();
    public Key floor(Key key); // the greatest key <= key
    public Key ceiling(Key key); // the smallest key >= key
    public int rank(Key key); // the number of keys < key
    public Key select(int k); //key of rank k
    public void deleteMin();
    public void deleteMax();
    public int size(Key lo, Key hi);
    public Iterable<Key> key(Key lo, Key hi);
}
```

## Implementation ##

### Equality test of keys for user-defined types ###

``` java
//typically unsafe to use equals() with inheritance
public final class Date implements Comparable<Date>
{
    private final int month;
    private final int day;
    private final int year;

    public boolean equals(Object y)//must be Object
    {
        if (y == this)
            return true;
        if (y == null)
            return false;
        if (y.getClass() != this.getClass())
            return false;

        Date that = (Date)y;
        if(this.day != that.day) return false;
        if(this.month != that.month) return false;
        if(this.year != that.year) return false;

        return true;
    }
}
```

### Unordered Linked List of Key-Value Pairs ###

**Search**: Scan through all keys until find a match

**Insert**: Scan through all keys until find a match; if no match add to front.

### Ordered Array of Key-Value Pairs ###

``` java
public Value get(Key key)
{
    if (isEmpty())
        return null;
    int i = rank(key);
    if(i < N && keys[i].compareTo(key) == 0)
        return vals[i];
    else
        return null;
}
private int rank(Key key)
{
    int lo = 0;
    int hi = N-1;
    while(lo <= hi)
    {
        int mid = lo + (hi - lo) / 2;
        int cmp = key.compareTo(keys[mid]);
        if (cmp < 0 )
            hi = mid -1;
        else if (cmp > 0 )
            lo = mid + 1;
        else
            return mid;
    }
    return lo;//return the number of keys < key
}
```

## Analysis ##

### ST Analysis

| ST implementation | worst-case cost || average case || ordered iteration | key interface |
| --                | --     | --     | --    | --     | --                | --            |
|                   | search| insert   |search| insert |                   |               |
|**unordered list** | N | N | N/2 | N | no | equals() |
|**ordered array** | lgN  | N | lgN | N/2 | yes | compareTo() |


#### Ordered Symbol Table Operation ###

|                | unordered list | binary search |
| -- | --| --|
|**search**  | N | lgN |
|**insert/delete**| N| N |
|**min/ max**| N| 1|
|**floor/ceiling**| N| lgN|
|**rank**| N|lgN|
|**select**| N| 1|
|**ordered iteration**  | NlgN| N|


