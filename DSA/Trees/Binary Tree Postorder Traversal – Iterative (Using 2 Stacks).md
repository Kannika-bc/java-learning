# Binary Tree Postorder Traversal – Iterative (Using 2 Stacks)

**Traversal Order:** Left → Right → Root

## 1. Logic

1. Create two stacks (`st1` and `st2`) and a list to store the result.
2. If root is `null`, return the empty list.
3. Push the root node into the first stack (`st1`).
4. Run a `while` loop until `st1` becomes empty:
   - Pop a node from `st1`.
   - Push that node into the second stack (`st2`).
   - If the node has a left child, push it into `st1`.
   - If the node has a right child, push it into `st1`.
5. Once `st1` becomes empty, run another loop until `st2` becomes empty.
6. Pop nodes from `st2` and add their values to the result list.
7. Return the result list.

## 2. Java Code

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {

        Stack<TreeNode> st1 = new Stack<>();
        Stack<TreeNode> st2 = new Stack<>();
        List<Integer> postOrder = new ArrayList<>();

        if (root == null) return postOrder;

        st1.push(root);

        while (!st1.isEmpty()) {
            root = st1.pop();
            st2.push(root);

            if (root.left != null)
                st1.push(root.left);

            if (root.right != null)
                st1.push(root.right);
        }

        while (!st2.isEmpty()) {
            postOrder.add(st2.pop().val);
        }

        return postOrder;
    }
}
```

## 3. Why Do We Use Two Stacks?

- First stack (`st1`) processes the tree nodes.
- Second stack (`st2`) stores the nodes in reverse traversal order.
- Popping from the second stack gives the required Postorder sequence.

**Remember:**

First stack processing order: Root → Right → Left

Second stack popping order: Left → Right → Root

## 4. Complexity Analysis

- **Time Complexity:** O(n) – Every node is pushed and popped once.
- **Space Complexity:** O(n) – Two stacks are used to store tree nodes.

## 5. Quick Revision

Postorder = Left → Right → Root

Using 2 stacks:
1. Push root into `st1`.
2. Pop from `st1` and push into `st2`.
3. Push left and right children into `st1`.
4. Pop from `st2` to get Postorder.

**Key idea:** Reverse the processing order using a second stack to get Left → Right → Root.
