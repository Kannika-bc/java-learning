## Same Tree – LeetCode

**Approach:** Recursion (DFS)

**Logic:**
1. If both nodes are `null`, return `true` (both trees have ended).
2. If only one node is `null`, return `false` (structures are different).
3. If `p.val != q.val`, return `false` (node values are different).
4. Recursively compare left subtrees: `isSameTree(p.left, q.left)`.
5. Recursively compare right subtrees: `isSameTree(p.right, q.right)`.
6. Use `&&` because both left and right subtrees must be identical.

**Code:**
```java
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
```

**Complexity:**
- Time: O(n) – Each corresponding node is visited once.
- Space: O(h) – Recursive call stack, where h is the tree height.

**Key Learning:**
- Check null conditions first.
- Use `!=` to detect mismatched node values.
- Use `&&` to ensure both subtrees are identical.
- Recursion compares corresponding nodes of both trees.
