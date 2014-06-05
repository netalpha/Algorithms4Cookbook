# Linked List

### Node abstraction

``` java
private class Node
{
    Item item;
    Node next;
}
```

### Building a linked list

To build a linked list that contains the items to, be, and or, we create a Node for each item, set the item field in each of the nodes to the desired value, and set the next fields to build the linked list.

![](http://algs4.cs.princeton.edu/13stacks/images/linked-list.png "Building a linked list")

### Insert at the beginning

The easiest place to insert a new node in a linked list is at the beginning.

![](http://algs4.cs.princeton.edu/13stacks/images/linked-list-insert-front.png "insert at the beginning")

### Remove from the beginning

![](http://algs4.cs.princeton.edu/13stacks/images/linked-list-remove-first.png "remove from the beginning")

### Insert at the end

![](http://algs4.cs.princeton.edu/13stacks/images/linked-list-insert-end.png "insert at the end")

### Traversal

``` java
for (Node x = first; x != null; x = x.next)
{
   // process x.item
}
```
