# Subtree of Another Tree – LeetCode 572

**Topic:** Binary Tree | Recursion  
**Difficulty:** Easy

## 1. Problem Statement

Given two binary trees `root` and `subRoot`, check whether `subRoot` is a subtree of `root`.

A subtree must have:
- Same node values.
- Same left and right child structure.
- All nodes must match exactly.

## 2. Approach – Recursion

We use two recursive functions:

| Function | Purpose |
|---|---|
| `isSubtree()` | Searches every node in the main tree. |
| `isSameTree()` | Checks whether two trees are exactly identical. |

### Function 1: isSubtree()

**Logic:**

1. If `root == null`, return `false` because the subtree cannot be found.
2. Check whether the current tree and `subRoot` are identical using `isSameTree()`.
3. If identical, return `true`.
4. Otherwise, search the left subtree.
5. Search the right subtree.
6. If either side returns true, return true.

### Function 2: isSameTree()

**Logic:**

1. If both nodes are null, return `true`.
2. If only one node is null, return `false`.
3. If node values are different, return `false`.
4. Recursively compare left children.
5. Recursively compare right children.
6. Both comparisons must return true.

## 3. Java Code

```java
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (root == null) {
            return false;
        }

        if (isSameTree(root, subRoot)) {
            return true;
        }

        return isSubtree(root.left, subRoot) ||
               isSubtree(root.right, subRoot);
    }

    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }

        if (p == null || q == null) {
            return false;
        }

        if (p.val != q.val) {
            return false;
        }

        return isSameTree(p.left, q.left) &&
               isSameTree(p.right, q.right);
    }
}
```

## 4. Important Recursion Patterns

### Pattern 1: Searching a Binary Tree

```java
return function(root.left) || function(root.right);
```

Use when searching for something anywhere in a binary tree.

`||` means either the left or right subtree can give the answer.

### Pattern 2: Comparing Two Binary Trees

```java
return function(p.left, q.left) &&
       function(p.right, q.right);
```

Use when both left and right structures must match.

`&&` means both conditions must be true.

### Pattern 3: Base Cases

**For searching:**

```java
if (root == null) {
    return false;
}
```

**For comparing two trees:**

```java
if (p == null && q == null) {
    return true;
}

if (p == null || q == null) {
    return false;
}
```

## 5. Time and Space Complexity

Let:
- `N` = Number of nodes in the main tree.
- `M` = Number of nodes in the subtree.

| Complexity | Value | Explanation |
|---|---|---|
| Time | O(N * M) | Each node in the main tree may require comparing up to M nodes. |
| Space | O(H + K) | Recursion stack for both tree traversals. |

Where H and K are the heights of the two trees.

Worst-case space complexity: **O(N + M)**.

## 6. Interview Key Points

- `isSubtree()` searches for a matching starting node.
- `isSameTree()` checks whether two trees are identical.
- Use `||` when either left or right side can contain the answer.
- Use `&&` when both left and right sides must match.
- Always handle null nodes before accessing their values.

## 7. Key Takeaway

**Remember this pattern:**

- Searching in a binary tree → `||`
- Comparing two binary trees → `&&`
- Both nodes null → `true`
- Only one node null → `false`

This combination of recursive searching and tree comparison is useful in many binary tree problems.
