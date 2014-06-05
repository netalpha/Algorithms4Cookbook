# 2-3 Search Trees

- `2`: one key, two children
- `3`: two keys, three children
- Perfect balanced
- Symmetric order: In–order traversal yields keys in ascending order

## Search

1 Compare search key against keys in node
2 Find interval containing search key
3 Follow associated link recursively

## Insertion

Insertion into a 3–node:

1 Add new key to 3–node to create a temporary 4–node.
2 Move middle key in 4–node into parent
3 Repeat up the tree if necessary
4 If you reach the roof and it's a 4–node, split it into three 2–nodes.

## Implementation

Complicated…

## Analysis

All operation with `clgN`.


