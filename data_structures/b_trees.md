# B Trees

Time required for a probe is much larger then time to access within a page.

B-tree: Generalize 2-3 trees by allowing up to M-1 key-link pairs per node.

+ At least 2 key-link pairs at root.
+ At least M/2 key-link pairs in other nodes
+ External nodes contain client keys.
+ Internal nodes contain copies of keys to guide search.

![](http://farm8.staticflickr.com/7286/9192873902_95fd9b5185.jpg "B-tree")

## Elementary Operation

### Search

+ Start at root
+ Find interval for search key and take corresponding link
+ Search terminates in external node

![](http://farm6.staticflickr.com/5498/9190100859_4c43386e10.jpg "Seach in B-tree")

### Insertion

+ Search for new key
+ Insert at bottom
+ Split nodes with M key-link pairs on the way up the tree

![](http://farm4.staticflickr.com/3822/9192942858_12aeacf630.jpg "Insert in B-tree")

## Analysis

A search or an insertion in a B-tree of order M with N keys requires: log<sub>m-1</sub>N ≤ probes ≤ log<sub>m/2</sub>N.

If M = 1024 and N = 62 billion, log<sub>m/2</sub>N ≤ 4.

## Application

Variants: B+ tree, B* tree, B# tree, ...

+ Java: `java.util.TreeMap` `java.util.TreeSet`
+ C++ STL: `map` `multimap` `multiset`
+ linux kernal: `complete fair scheduler` `linux/rbtree.h`
+ Windows: `HPFS`
+ Mac: `HFS`, `HFS+`
+ Linux: `ReiserFS`, `XFS`, `Ext3FS`, `JFS`
+ Database: `Oracle`, `DB2`, `INGRES`, `SQL`, `PostgreSQL`...




