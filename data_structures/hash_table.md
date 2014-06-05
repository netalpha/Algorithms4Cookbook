# Hash Table

## Introduction

Hash table: a key-indexed table.

## Issue

+ Equality test
+ Computing the hash function
+ Collision resolution

### Equality Test

x.equals(y) » x.hashCode() == y.hashcode

+ hashCode(): return a 32-bit int.
+ Default Implementation: Memory address

Customized implementation: `Integer`, `Double`, `String`, `File`, `URL`, `Date`, …

``` java
public class Integer
{
    private find int value;

    public int hashCode()
    {
        return value;
    }
}

public final class Boolean
{
    private find boolean value;

    public int hashCode()
    {
        if(value) return 1231;
        else return 1237;
    }
}

public final class Double
{
    private final double value;

    public int hashCode()
    {
        long bits = doubleToLongBits(value);
        //convert to IEEE 64-bit representation;
        return (int) (bits ^ (bits >>> 32));
    }
}

public final class String
{
    private final char[] s;
    private int hash = 0;

    //Horner's method
    public int hashCode()
    {
        int h = hash;
        if(h != 0)
            return h;

        for(int i = 0; i < length(); i++)
        {
            h = s[i] + (31 * h);
        }
        hash = h;
        return h;
    }
}

public final class User_Defined
{
    private final String who;
    private final Date when;
    private final double amount;

    //- 31x+y
    //- use wrapper type hashCode()
    //- return 0 when field is null
    //- return hashCode when field is reference type
    //- apply to each entry when field is an array
    public int hashCode()
    {
        int hash = 17;
        hash = 31 * hash + who.hashCode();
        hash = 31 * hash + when.hashCode();
        hash = 31 * hash + ((Double) amount).hashCode();
        return hash;
    }
}

```

### Computing the hash function

Method for computing array index from key.

–2<sup>31</sup> ≤ Hash code ≤ 2<sup>31</sup> - 1

0 ≤ hashing ≤ M - 1 (for use as array index)

``` java
private int hash(Key key)
{
    //X: return key.hashCode() % M
    //X: return Math.abs(key.hashCode()) % M;
    return (key.hashCode() & 0x7fffffff) % M;
}
```

#### Applicaitons

- Bins and balls: Throw balls uniformly at random into M bins.
- Birthday problem: Expect two balls in the same bin after ~ sqrt(π M/2) tosses.
- Coupon collector: Expect every bin has ≥ 1 ball after ~ MlnM tosses
- Load balancing: After M tosses, expect most loaded bin has Θ(logM/loglogM) balls.

### Collisions

Two distinct keys hashing to the same index.

#### Separate chaining symbol table

``` java
public class SeperateChainingHashST<Key, Value>
{
    private int M = 97; //number of chains
    private Node[] st = new Node[M];

    private static class Node
    {
        private Object key;
        private Object val;
        private Node next;
    }

    private int hash(Key key)
    {
        return (key.hashCode() && 0x7fffffff) % M;
    }

    public Value get(Key key)
    {
        int i = hash(key);
        for(Node x = st[i]; x != null; x = x.next)
        {
            if(key.equals(x.key))
                return (Value) x.val;
        }
        return null;
    }

    public void put(Key key, Value val)
    {
        int i = hash(key);
        for(Node x = st[i]; x != null; x = x.next)
        {
            if(key.equals(x.key))
            {
                x.val = val;
                return;
            }
        }
        st[i] = new Node(key, val, st[i]);
    }
}
```

##### Analysis

+ probes ≈ N/M
+ typical choice: M ≈ N/5

#### Open Addressing

When a new key collides, find next empty slot, and put it there. Array size M must be greater than number of key-value pairs N.

``` java
public class LinearProbingHashST<Key, Value>
{
    private int M = 30001;
    private Value[] vals = (Value[]) new Object[M];
    private Key[] keys = (Key[]) new Object[M];

    private int hash(Key key)
    {
        //as before
    }

    public void put(Key key, Value val)
    {
        int i;
        for(i = hash(key); keys[i] != null; i = (i+1)%M)
        {
            if(keys[i].equals(key))
                break;
        }
        keys[i] = key;
        vals[i] = val;
    }

    public Value get(Key key)
    {
        for(int i = hash(key); keys[i] != null; i = (i+1)%M)
        {
            if(key.equals(keys[i]))
            {
                return vals[i];
            }
        }
        return null;
    }
}
```

##### Analysis

typical choice: N/M ≈ 1/2

+ probes for search hit  ≈ 3/2
+ probes for search miss ≈ 5/2


| ST implementation | worst-case cost          ||| average case           ||| ordered iteration | key interface |
| --                | --     | --     | --       | --      |     -- |    -- |   --              | --            |
|                   | search | insert | delete   | search  | insert |delete |                   |               |
|**unordered list** | N      | N      | N        | N/2     | N      | N/2   | no                | equals()      |
|**ordered array**  | lgN    | N      | N        | lgN     | N/2    | N/2   | yes               | compareTo()   |
|**BST**            | N      | N      | N        | 1.39lgN |1.39lgN | √n    | yes               | compareTo()   |
| **2-3 tree**    | clgN     | clogN  | clgN     | clgN    | clgN   | clogN | yes               | compareto()   |
|**red-black BST**  | 2lgN     | 2logN  | 2lgN     | 1lgN    | 1lgN   | 1logN | yes             | compareto()   |
|**separate chaining**| lgN  | lgN| lgN| 3-5 | 3-5 | 3-5 | no | equals() |
|**linear probing**| lgN  | lgN| lgN| 3-5 | 3-5 | 3-5 | no | equals() |

## Variations

Two-probe hashing(separate-chaining variant)

+ Hash to two positions, insert key in shorter of the two chains.
+ Reduces expected length of the longest chain to log log N.

Double hashing(linear-probing variant)

+ Use linear probing, but skip a variable amount, not just 1 each time
+ Effectively eliminates clustering
+ Can allow table to become nearly full.
+ More difficult to implement delete.

Cuckoo hashing(linear-probing variant)

+ Hash key to two positions; insert key into either position; if occupied, reinsert displaced key into its alternative position (and recur).
+ Constant worst case time for search




