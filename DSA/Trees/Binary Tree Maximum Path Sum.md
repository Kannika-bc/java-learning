# Binary Tree Maximum Path Sum – LeetCode 124

## 1. Problem Understanding

Find the maximum sum of a path in a binary tree.

A path:
- Can start and end at any node.
- Does not need to pass through the root.
- Can move through parent-child connections.
- Cannot visit the same node twice.

**Important:** A path can use both left and right children, but a recursive function can return only one branch to its parent.

## 2. Brute Force Approach

### Intuition

The basic idea is to consider every node as a possible starting point and calculate the maximum path sum starting from that node.

For each node:
1. Calculate all possible downward paths from that node.
2. Find the maximum path sum.
3. Repeat this for every node.
4. Keep track of the maximum answer.

### Why is it not efficient?

We are calculating paths repeatedly.

The same subtrees can be visited many times while calculating the answer for different starting nodes.

This creates unnecessary repeated work.

### Complexity

- **Time Complexity:** O(N²) in the worst case for a skewed tree.
- **Space Complexity:** O(N) for recursion in the worst case.

For a balanced tree, the brute-force approach can be better, but it still repeats work.

**Why is it not good?**

Because we are solving similar subproblems again and again instead of reusing their results.

## 3. Optimized Approach – Postorder DFS

We use DFS (Depth First Search).

At every node, we perform TWO calculations.

**Calculation 1: Maximum path passing through the current node**

We can use both left and right contributions.

```java
sum = leftsum + rightsum + root.val;
maxsum = Math.max(sum, maxsum);
```

**Calculation 2: Return a path to the parent**

We can return only one branch because a path cannot split into two branches and then continue upwards.

```java
return root.val + Math.max(leftsum, rightsum);
```

We also ignore negative contributions using `Math.max(0, ...)`.

## 4. My Code – Final Version

```java
class Solution {
    int maxsum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        findsum(root);
        return maxsum;
    }

    public int findsum(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int leftsum = findsum(root.left);
        int rightsum = findsum(root.right);

        leftsum = Math.max(0, leftsum);
        rightsum = Math.max(0, rightsum);

        int sum = leftsum + rightsum + root.val;
        maxsum = Math.max(sum, maxsum);

        if (leftsum > rightsum) {
            return leftsum + root.val;
        } else {
            return rightsum + root.val;
        }
    }
}
```

## 5. Explanation of My Code

| Code | Purpose |
|---|---|
| `maxsum = Integer.MIN_VALUE` | Handles trees containing only negative values |
| `if(root == null)` | Base case; returns 0 |
| `findsum(root.left)` | Finds the best contribution from left subtree |
| `findsum(root.right)` | Finds the best contribution from right subtree |
| `Math.max(0, leftsum)` | Ignores negative left contribution |
| `Math.max(0, rightsum)` | Ignores negative right contribution |
| `leftsum + rightsum + root.val` | Calculates complete path through current node |
| `Math.max(sum, maxsum)` | Updates the global answer |
| `if-else` | Returns only the better child branch |

## 6. Better / Cleaner Version

This version does exactly the same thing with fewer lines.

```java
class Solution {
    int maxsum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        findsum(root);
        return maxsum;
    }

    public int findsum(TreeNode root) {
        if (root == null) {
            return 0;
        }

        // Get child sums and ignore negative contributions
        int leftsum = Math.max(0, findsum(root.left));
        int rightsum = Math.max(0, findsum(root.right));

        // Maximum path passing through current node
        int sum = leftsum + rightsum + root.val;
        maxsum = Math.max(maxsum, sum);

        // Return only one branch to parent
        return root.val + Math.max(leftsum, rightsum);
    }
}
```

### What did I improve?

**Improvement 1: Removed unnecessary if-else**

My code:

```java
if (leftsum > rightsum) {
    return leftsum + root.val;
} else {
    return rightsum + root.val;
}
```

Cleaner version:

```java
return root.val + Math.max(leftsum, rightsum);
```

Both are logically equivalent.

**Improvement 2: Combined recursive calls and negative-value handling**

My code:

```java
int leftsum = findsum(root.left);
int rightsum = findsum(root.right);

leftsum = Math.max(0, leftsum);
rightsum = Math.max(0, rightsum);
```

Cleaner version:

```java
int leftsum = Math.max(0, findsum(root.left));
int rightsum = Math.max(0, findsum(root.right));
```

Both are equivalent.

## 7. Dry Run Example

```text
        1
       / \
      2   3
     / \
    4   5
```

| Node | Left | Right | Through-node sum | maxsum |
|---|---:|---:|---:|---:|
| 4 | 0 | 0 | 4 | 4 |
| 5 | 0 | 0 | 5 | 5 |
| 2 | 4 | 5 | 11 | 11 |
| 3 | 0 | 0 | 3 | 11 |
| 1 | 7 | 3 | 11 | 11 |

**Maximum path:** 4 → 2 → 5

**Answer: 11**

Note: Node 2 returns 7 to node 1, not 11, because it can return only one branch.

## 8. Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|---|---|---|
| Brute force | O(N²) worst case | O(N) |
| Optimized DFS | O(N) | O(H) |

Where:
- N = Number of nodes.
- H = Height of the tree.

**Why is optimized DFS better?**

Every node is visited only once. We calculate the required subtree contribution during that visit instead of repeatedly calculating it.

## 9. Simple Interview Explanation Script

"I will solve this problem using DFS.

First, I go to the left and right children and calculate their maximum path sums.

If a child gives a negative sum, I ignore it because adding a negative value will reduce my path sum.

Now, at every node, I calculate the path using the current node, left contribution and right contribution. I update my global maximum.

But when I return to the parent, I can return only one side, either left or right, because a path cannot split into two branches.

I repeat this for all nodes and finally return the global maximum.

The time complexity is O(N), because I visit every node only once. The space complexity is O(H), because of the recursion stack."

## 10. Important Interview Cross-Questions

**Q1. Why can we use both children while updating maxsum?**

Because a valid path can pass through the current node and connect its left and right branches.

**Q2. Why can we return only one child?**

Because the parent needs a single continuous path to extend.

**Q3. Why use Math.max(0, childSum)?**

To ignore negative contributions.

**Q4. Why initialize maxsum with Integer.MIN_VALUE?**

To correctly handle trees containing only negative values.

**Q5. Why use postorder traversal?**

Because we need the left and right subtree results before calculating the answer at the current node.

**Q6. Does the maximum path have to pass through the root?**

No. It can start and end anywhere in the tree.

## 11. Quick Revision – Remember This Pattern

1. Get left contribution.
2. Get right contribution.
3. Ignore negative contributions.
4. Calculate path using BOTH children.
5. Update global maximum.
6. Return only ONE child contribution to parent.

**Remember:** Global maximum uses both branches. Recursive return uses only one branch.
