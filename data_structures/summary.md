# Summary

### Symbol Table Implementation Comparison

Balanced search trees

+ Stronger performance guarantee.
+ Support for ordered ST operations
+ Easier to implement compareTo() correctly than equals() and hashCode().

Hash tables

+ Simpler to code
+ No effective alternative for unordered keys
+ Faster for simple keys (a few arithmetic ops versus log N compares)
+ Better system support in Java for strings (e.g., cached hash code)

### Java System Included

- Red-black BSTs: `java.util.TreeMap`, `java.util.TreeSet`
- Has
