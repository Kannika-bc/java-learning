# **Binary Tree Boundary Traversal – Revision Notes**



DSA – Binary Trees

DFS + Recursion



## **1. Problem statement**

Given a binary tree, return its boundary nodes in **anti-clockwise order**, starting from the root.

The boundary consists of four parts:

1. Root node.
2. Left boundary (excluding leaf nodes).
3. All leaf nodes (left to right).
4. Right boundary (excluding leaf nodes, bottom to top).

[Boundary Traversal of a Binary Tree - Tutorial](https://images.openai.com/static-rsc-4/Yq3EzbB0Y4RV1sTgkBgotkmLjBP_IP1vHhShhGL05nMyJYxWO4lplrq7sYS1_dSqH2IxXEvIGlwk1om_4pxgNCNYtDNqmExAT2lBgDWJck79R14JmtbuKfQrkRGELNgjhsZvhI1g7_e56MN4YXwocR8Zy08XnhQIOFUiK0C3epA?purpose=inline)

[takeuforward.org](https://takeuforward.org/data-structure/boundary-traversal-of-a-binary-tree?utm_source=chatgpt.com)

## **2. Core intuition**

Instead of solving the entire problem using one traversal, divide it into three independent helper methods.

| Component      | Direction    | Logic                        |
| -------------- | ------------ | ---------------------------- |
| Root           | First        | Add root                     |
| Left boundary  | Top → Bottom | Prefer left, otherwise right |
| Leaf nodes     | Left → Right | DFS                          |
| Right boundary | Bottom → Top | Prefer right, otherwise left |

**Important:** Leaf nodes are excluded from both boundary methods because they are collected separately.

## **3. Java solution**

```

import java.util.*;

class Solution {

    ArrayList<Integer> boundaryTraversal(Node root) {

        ArrayList<Integer> result = new ArrayList<>();

        if (root == null) return result;

        // 1. Root
        if (isLeaf(root)) {
            result.add(root.data);
            return result;
        }

        result.add(root.data);

        // 2. Left boundary
        addLeftBoundary(root.left, result);

        // 3. Leaf nodes
        addLeaves(root, result);

        // 4. Right boundary
        addRightBoundary(root.right, result);

        return result;
    }

    // Check leaf node
    boolean isLeaf(Node root) {
        return root.left == null && root.right == null;
    }

    // Left boundary: Top to Bottom
    void addLeftBoundary(Node root, ArrayList<Integer> result) {

        if (root == null || isLeaf(root)) return;

        result.add(root.data);

        if (root.left != null) {
            addLeftBoundary(root.left, result);
        } else {
            addLeftBoundary(root.right, result);
        }
    }

    // Leaves: Left to Right
    void addLeaves(Node root, ArrayList<Integer> result) {

        if (root == null) return;

        if (isLeaf(root)) {
            result.add(root.data);
            return;
        }

        addLeaves(root.left, result);
        addLeaves(root.right, result);
    }

    // Right boundary: Bottom to Top
    void addRightBoundary(Node root, ArrayList<Integer> result) {

        if (root == null || isLeaf(root)) return;

        if (root.right != null) {
            addRightBoundary(root.right, result);
        } else {
            addRightBoundary(root.left, result);
        }

        result.add(root.data);
    }
}

```

## **4. Understand the three helper methods**

### **A. Left boundary**

**Intuition:** Travel down the left side. If left child doesn't exist, use the right child.

```
if (root == null || isLeaf(root)) return;

result.add(root.data);

if (root.left != null) {
    addLeftBoundary(root.left, result);
} else {
    addLeftBoundary(root.right, result);
}
```

Remember:

- Add current node first.
- Then recurse.
- This gives top-to-bottom order.

### **B. Leaf nodes**

**Intuition:** A leaf node has no children.

```
if (root == null) return;

if (isLeaf(root)) {
    result.add(root.data);
    return;
}

addLeaves(root.left, result);
addLeaves(root.right, result);
```

Remember:

- Visit left subtree first.
- Visit right subtree next.
- This gives left-to-right leaf order.

### **C. Right boundary**

**Intuition:** Travel down the right side. If right child doesn't exist, use the left child.

```
if (root == null || isLeaf(root)) return;

if (root.right != null) {
    addRightBoundary(root.right, result);
} else {
    addRightBoundary(root.left, result);
}

result.add(root.data);
```

Remember:

- Recurse first.
- Add current node afterwards.
- This gives bottom-to-top order.

## **5. Most important recursion concept**

This is the key learning from this problem.

**Preorder-style processing**

Add → Recurse

```
result.add(root.data);
recursiveCall(child);
```

Used in: Left boundary.

**Postorder-style processing**

Recurse → Add

```
recursiveCall(child);
result.add(root.data);
```

Used in: Right boundary.

Why does this matter?

Because recursion doesn't always mean processing a node immediately. Sometimes you need to wait until the recursive calls return.

## **6. Edge cases to remember**

| Input tree          | Expected behaviour                   |
| ------------------- | ------------------------------------ |
| Empty tree          | Empty list                           |
| Single node         | Add root only                        |
| Left-skewed tree    | Root → remaining nodes               |
| Right-skewed tree   | Root → remaining nodes               |
| Missing left child  | Follow right child for left boundary |
| Missing right child | Follow left child for right boundary |

## **7. Complexity analysis**

Let N = number of nodes and H = height of tree.

| Complexity      | Value | Reason                                          |
| --------------- | ----- | ----------------------------------------------- |
| Time            | O(N)  | Each node is visited a constant number of times |
| Auxiliary space | O(H)  | Recursive call stack                            |
| Output space    | O(N)  | Boundary result list                            |

## **8. Interview revision – Quick questions**

**Test your recall**

0/6 reviewed

**1. Why do we exclude leaf nodes from left and right boundary methods?**

Reveal answer

**2. Why do we add nodes before recursion in left boundary?**

Reveal answer

**3. Why do we add nodes after recursion in right boundary?**

Reveal answer

**4. What if the left child doesn't exist during left boundary traversal?**

Reveal answer

**5. What if the right child doesn't exist during right boundary traversal?**

Reveal answer

**6. What is the leaf-node condition?**

Reveal answer

## **9. What to remember before an interview**

**Boundary Traversal – One-minute revision**

- Break the problem into root, left boundary, leaves and right boundary.
- Left boundary: Prefer left, otherwise right.
- Leaves: DFS, left before right.
- Right boundary: Prefer right, otherwise left.
- Exclude leaves from boundary methods.
- Left boundary uses add-before-recursion.
- Right boundary uses add-after-recursion.
- Overall time complexity: O(N).

**My practice recommendation:** Try solving this problem again tomorrow without looking at these notes. Your goal is to reproduce the logic from intuition, not memorize the implementation.
