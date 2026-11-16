## Invert Binary Tree – LeetCode

**Approach:** Recursion (DFS)

**Logic:**
1. Base case: If `root == null`, return `null`.
2. Swap the left and right children using a temporary variable.
3. Recursively invert the left subtree.
4. Recursively invert the right subtree.
5. Return the root.

**Code:**
```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }

    TreeNode temp = root.left;
    root.left = root.right;
    root.right = temp;

    invertTree(root.left);
    invertTree(root.right);

    return root;
}
```

**Complexity:**
- Time: O(n) – Each node is visited once.
- Space: O(h) – Recursive call stack, where h is the tree height. O(log n) for a balanced tree and O(n) for a skewed tree.

**Key Learning:** For tree inversion, swap the children of the current node and recursively apply the same operation to both subtrees.
