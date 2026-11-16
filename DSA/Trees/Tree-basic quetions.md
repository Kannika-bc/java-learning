# **1. Create a Binary Tree and Print Its Nodes**

**Question:** Create a binary tree with 5 nodes and print all node values.

**Code:**

```
class Node {
    int data;
    Node left, right;

    Node(int data) {
        this.data = data;
    }
}

public class Main {
    public static void main(String[] args) {

        Node root = new Node(1);

        root.left = new Node(2);
        root.right = new Node(3);

        root.left.left = new Node(4);
        root.left.right = new Node(5);

        System.out.println(root.data);
        System.out.println(root.left.data);
        System.out.println(root.right.data);
        System.out.println(root.left.left.data);
        System.out.println(root.left.right.data);
    }
}
```

**Tree structure:**

[Tree Traversal in Python (Inorder, Preorder & Postorder)](https://images.openai.com/static-rsc-4/UMFN-1MNeWtGbguIHjeaC4HjoxOF53UEnMprNiTS9omci62x1vNomLLxCJ5IGpsdc8Qprrp58slAFCg9huAWEXy_bie7a_-BxSvCQnd68MjfmFOS3FsGq-RCekOnV43LtVDYfdE7RCgaE3YUGVJEShf0GSH2UFHVAaTxnTCuBu0?purpose=inline)

[favtutor.com](https://favtutor.com/blogs/tree-traversal-python-with-recursion?utm_source=chatgpt.com)

**Output:**

```
1
2
3
4
5
```

**Explanation:**

- `root` is the first node.
- `root.left` connects node 2 to node 1.
- `root.right` connects node 3 to node 1.
- `root.left.left` connects node 4 to node 2.
- `root.left.right` connects node 5 to node 2.

# **2. Count Total Nodes in a Binary Tree**

**Question:** Write a recursive function to count the total number of nodes.

**Code:**

```
public class Main {

    static int countNodes(Node root) {

        if (root == null) {
            return 0;
        }

        return 1 + countNodes(root.left)
                 + countNodes(root.right);
    }

    public static void main(String[] args) {

        Node root = new Node(1);
        root.left = new Node(2);
        root.right = new Node(3);
        root.left.left = new Node(4);
        root.left.right = new Node(5);

        System.out.println(countNodes(root));
    }
}
```

**Output:**

```
5
```

**Explanation:**

We use recursion to count nodes.

```
if (root == null)
    return 0;
```

If there is no node, return 0.

```
return 1 + countNodes(root.left)
         + countNodes(root.right);
```

- `1` counts the current node.
- `countNodes(root.left)` counts left subtree nodes.
- `countNodes(root.right)` counts right subtree nodes.

# **3. Find the Sum of All Nodes**

**Question:** Calculate the sum of all node values.

**Code:**

```
static int sumNodes(Node root) {

    if (root == null) {
        return 0;
    }

    return root.data
            + sumNodes(root.left)
            + sumNodes(root.right);
}
```

Call it using:

```
System.out.println(sumNodes(root));
```

**Output:**

```
15
```

**Explanation:**

Our tree contains:

```
       1
      / \
     2   3
    / \
   4   5
```

Sum = 1 + 2 + 3 + 4 + 5 = 15.

# **4. Find the Maximum Element**

**Question:** Find the largest value in a binary tree.

**Code:**

```
static int findMax(Node root) {

    if (root == null) {
        return Integer.MIN_VALUE;
    }

    int leftMax = findMax(root.left);
    int rightMax = findMax(root.right);

    return Math.max(root.data,
           Math.max(leftMax, rightMax));
}
```

**Output:**

```
5
```

**Explanation:**

We find the maximum value in:

1. Current node.
2. Left subtree.
3. Right subtree.

`Math.max()` returns the bigger of two values.

# **5. Find the Minimum Element**

**Question:** Find the smallest value in a binary tree.

**Code:**

```
static int findMin(Node root) {

    if (root == null) {
        return Integer.MAX_VALUE;
    }

    int leftMin = findMin(root.left);
    int rightMin = findMin(root.right);

    return Math.min(root.data,
           Math.min(leftMin, rightMin));
}
```

**Output:**

```
1
```

**Explanation:**

We compare the current node with the minimum values from the left and right subtrees.

# **6. Count Leaf Nodes**

**Question:** Count the nodes that do not have any children.

**Code:**

```
static int countLeafNodes(Node root) {

    if (root == null) {
        return 0;
    }

    if (root.left == null && root.right == null) {
        return 1;
    }

    return countLeafNodes(root.left)
            + countLeafNodes(root.right);
}
```

**Output:**

```
3
```

**Explanation:**

A leaf node has no left or right child.

In our tree, leaf nodes are 4, 5 and 3.

# **7. Inorder Traversal**

**Question:** Print nodes in Left → Root → Right order.

[Binary Tree Traversal : Inorder Preorder Postorder - Tutorial](https://images.openai.com/static-rsc-4/JnkdW3D2CUcL-kF8kEDpkNlECHV-50LkPP5owb4iiLw0OPe2wX5MvG8SwlERDAbfrDHO0peGsp56CkEo-NumejGbcEVzuY77AmyIzZNkiig1YUDO8V-jAl1VsucR7xnXSe5z9ire65Oi65NtVG5Q3hWh4AiepoMZvJ4NbHGEfWg?purpose=inline)

[takeuforward.org](https://takeuforward.org/binary-tree/binary-tree-traversal-inorder-preorder-postorder?utm_source=chatgpt.com)

**Code:**

```
static void inorder(Node root) {

    if (root == null) {
        return;
    }

    inorder(root.left);

    System.out.print(root.data + " ");

    inorder(root.right);
}
```

**Output:**

```
4 2 5 1 3
```

**Explanation:**

The order is:

1. Visit left subtree.
2. Print current node.
3. Visit right subtree.

# **8. Preorder Traversal**

**Question:** Print nodes in Root → Left → Right order.

**Code:**

```
static void preorder(Node root) {

    if (root == null) {
        return;
    }

    System.out.print(root.data + " ");

    preorder(root.left);

    preorder(root.right);
}
```

**Output:**

```
1 2 4 5 3
```

**Explanation:**

1. Print current node.
2. Visit left subtree.
3. Visit right subtree.

# **9. Postorder Traversal**

**Question:** Print nodes in Left → Right → Root order.

**Code:**

```
static void postorder(Node root) {

    if (root == null) {
        return;
    }

    postorder(root.left);

    postorder(root.right);

    System.out.print(root.data + " ");
}
```

**Output:**

```
4 5 2 3 1
```

**Explanation:**

1. Visit left subtree.
2. Visit right subtree.
3. Print current node.

**Remember the difference:**

| Traversal | Order               |
| --------- | ------------------- |
| Inorder   | Left → Root → Right |
| Preorder  | Root → Left → Right |
| Postorder | Left → Right → Root |

# **10. Search an Element in a Binary Tree**

**Question:** Check whether a given value exists in a binary tree.

**Code:**

```
static boolean search(Node root, int target) {

    if (root == null) {
        return false;
    }

    if (root.data == target) {
        return true;
    }

    return search(root.left, target)
            || search(root.right, target);
}
```

Call:

```
System.out.println(search(root, 5));
```

**Output:**

```
true
```

**Explanation:**

- If the node is null, return false.
- If the current node matches the target, return true.
- Otherwise, search the left and right subtrees.
- `||` means OR. If either side returns true, the result is true.

# **Important concepts you should understand**

Before moving to the next set of tree problems, make sure you understand these three concepts:

**1. Base condition**

```
if (root == null) {
    return 0;
}
```

This stops recursion when there are no more nodes.

**2. Recursive call**

```
countNodes(root.left);
countNodes(root.right);
```

The function calls itself to process smaller parts of the tree.

**3. Returning results**

```
return 1 + left + right;
```

Combines the results from both subtrees and the current node.

## **Complexity of these problems**

Let `n` represent the total number of nodes.

| Problem             | Time complexity |
| ------------------- | --------------- |
| Count nodes         | O(n)            |
| Sum nodes           | O(n)            |
| Maximum element     | O(n)            |
| Minimum element     | O(n)            |
| Count leaf nodes    | O(n)            |
| Inorder traversal   | O(n)            |
| Preorder traversal  | O(n)            |
| Postorder traversal | O(n)            |
| Search              | O(n)            |

## **What to practise next**

After these 10 questions, move to:

1. Find height of a binary tree.
2. Find depth of a node.
3. Count nodes at a particular level.
4. Level order traversal using Queue.
5. Check whether two trees are identical.
6. Check whether a tree is symmetric.
7. Find diameter of a binary tree.
8. Invert a binary tree.
9. Check whether a binary tree is balanced.
10. Find left and right views.

**My suggestion:** Don't just copy these programs. Type each one yourself, dry-run the recursive calls, and try changing the tree structure to see how the output changes. This will help you understand trees much better for interviews.
