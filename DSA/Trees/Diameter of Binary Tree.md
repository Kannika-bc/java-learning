
# **Diameter of Binary Tree — DSA Revision Notes**



DFS + Recursion

Easy to revise



## **1. Problem understanding**

The **diameter of a binary tree** is the longest path between any two nodes in a binary tree.

- The path can pass through the root, but it doesn't have to.
- We calculate the diameter in terms of edges, not nodes.
- The longest path can pass through any node.

Example:

[Calculate the Diameter of a Binary Tree - Tutorial](https://images.openai.com/static-rsc-4/A2zn0qhj8Nnh7GoGcIbe8OnqKSBT0bUHjwfxrpVETOCCZOdyK2OC95VvOUndkTFLTURkd3dHqRLu6tOVAwmS4JQyX_i2ZdnT1gOYKXc-QJqe1fxBTAyv-ylsjgkwhCEmT7ZRCcJ_guKVqtWoXgxa3y_YqzmX5rWdUD_UYOI_VoQ?purpose=inline)

[takeuforward.org](https://takeuforward.org/data-structure/calculate-the-diameter-of-a-binary-tree?utm_source=chatgpt.com)

```
        1
       / \
      2   3
     / \
    4   5
```

Longest path:

```
4 → 2 → 1 → 3
```

**Answer = 3 edges**

## **2. Intuition (Most Important)**

Think about any node in the binary tree.

The longest path passing through that node can be calculated using:

\\[ D = leftHeight + rightHeight \\]

For every node:

1. Find the height of its left subtree.
2. Find the height of its right subtree.
3. Add both heights to calculate the diameter passing through that node.
4. Update the maximum diameter.

**Important observation:** The diameter doesn't always pass through the root. So we calculate it at every node.

## **3. Quick algorithm**

## **Algorithm — Step by step**

1. Initialize a global variable `diameter = 0`.
2. Call the recursive `height(root)` function.
3. If the node is null, return `0`.
4. Calculate left height using `height(root.left)`.
5. Calculate right height using `height(root.right)`.
6. Calculate diameter at the current node using `leftHeight + rightHeight`.
7. Update the global maximum diameter.
8. Return `1 + Math.max(leftHeight, rightHeight)`.
9. After recursion finishes, return the maximum diameter.

## **4. Java code**

```

class Solution {

    int diameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        height(root);
        return diameter;
    }

    public int height(TreeNode root) {

        if (root == null) {
            return 0;
        }

        int leftHeight = height(root.left);
        int rightHeight = height(root.right);

        // Calculate diameter at current node
        diameter = Math.max(diameter,
                            leftHeight + rightHeight);

        // Return height to parent
        return 1 + Math.max(leftHeight, rightHeight);
    }
}

```

## **5. Dry run**

Input:

```
        1
       / \
      2   3
     / \
    4   5
```

| Node | Left height | Right height | Diameter |
| ---- | ----------- | ------------ | -------- |
| 4    | 0           | 0            | 0        |
| 5    | 0           | 0            | 0        |
| 2    | 1           | 1            | 2        |
| 3    | 0           | 0            | 0        |
| 1    | 2           | 1            | 3        |

Final answer = 3

Remember that we're using height measured in nodes for the return value, but diameter measured in edges.

## **6. Simple interview explanation script**

The diameter of a binary tree is the longest path between any two nodes, measured in edges.

I'll solve this using DFS and recursion.

My idea is that at every node, I calculate the height of its left and right subtrees. Adding these two heights gives the diameter passing through that node.

Since the diameter doesn't necessarily pass through the root, I'll calculate it at every node and maintain the maximum diameter using a global variable.

For calculating the height, I'll return 0 when the node is null.

Then I'll recursively calculate the left and right heights.

I'll update the maximum diameter using leftHeight + rightHeight.

Finally, I'll return 1 + the maximum of the left and right heights.

After all recursive calls finish, I'll return the maximum diameter.

The time complexity is O(N), because we visit every node once.

The auxiliary space complexity is O(H), where H is the height of the tree, because of the recursion stack.

## **7. Important interview questions and cross-questions**

**Q1. Why are you using DFS?**

Because we need to calculate the height of each subtree. DFS allows us to explore the children first and then calculate the height of the parent.

**Q2. Why can't we calculate the diameter only at the root?**

Because the longest path might pass through another node instead of the root.

**Q3. Why are you using a global variable?**

Because the recursive function needs to return height to its parent, while we also need to maintain the maximum diameter found so far.

**Q4. Why do you add left height and right height?**

Because the longest path passing through the current node consists of a path from the left subtree to the current node and then to the right subtree.

**Q5. Why do you use Math.max() when returning height?**

Because height represents the longest single downward path. We can only travel through one child when calculating height.

**Q6. What is the base condition?**

When the node is null, return 0 because an empty subtree has height zero.

**Q7. What is the time complexity?**

O(N), because each node is visited exactly once.

**Q8. What is the space complexity?**

O(H), where H is the height of the tree.

- Balanced tree: O(log N)
- Skewed tree: O(N)

**Q9. Can we solve this without a global variable?**

Yes. We can use an instance variable, an array of size one, or return both height and diameter together using a custom class or pair.

**Q10. What happens if the tree is empty or has only one node?**

- Empty tree: diameter = 0.
- Single-node tree: diameter = 0.

## **8. Common mistakes**

| Mistake                                    | Correct approach                         |
| ------------------------------------------ | ---------------------------------------- |
| Returning left + right height              | Return `1 + max(left, right)`            |
| Checking diameter only at root             | Check every node                         |
| Counting nodes instead of edges            | Return `leftHeight + rightHeight`        |
| Declaring diameter inside recursive method | Maintain a shared variable               |
| Recalculating height repeatedly            | Calculate height and diameter in one DFS |

## **9. Test your understanding**

Try answering these without looking at the notes.

**Quick interview quiz**

0/5 answered

**1. What does the height function return to its parent?**

Diameter

Height of current subtree

Left and right heights

**2. What is the diameter formula at a node?**

1 + max(left, right)

left + right

max(left, right)

**3. Why do we maintain a global diameter?**

To store the maximum across all nodes

To calculate height

To avoid recursion

**4. What is the time complexity?**

O(log N)

O(N²)

O(N)

**5. What is the diameter of a single-node tree?**

0

1

2

Check answers

## **10. Related problems to practice**

After understanding this problem, try these in order:

1. Maximum Depth of Binary Tree — LeetCode 104.
2. Balanced Binary Tree — LeetCode 110.
3. Diameter of Binary Tree — LeetCode 543.
4. Binary Tree Maximum Path Sum — LeetCode 124.

**My recommendation:** Before moving to the next problem, try implementing Diameter of Binary Tree once without looking at the code. If you can independently derive the two formulas, you've understood the problem rather than memorized it.
