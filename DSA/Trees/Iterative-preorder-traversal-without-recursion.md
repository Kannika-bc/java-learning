## **Preorder Traversal (Iterative) – Logic**

**Order: Root → Left → Right**

1. Create an `ArrayList` to store the preorder result.
2. Create a `Stack` to store tree nodes.
3. If root is `null`, return the empty list.
4. Push the root node into the stack.
5. Run a `while` loop until the stack becomes empty.
6. Pop the top node from the stack.
7. Add its value to the result list.
8. Push the right child first (if it exists).
9. Push the left child next (if it exists).
10. Repeat until the stack is empty and return the result.

### **Important points**

- `push()` → Adds a node to the stack.
- `pop()` → Removes the top node.
- Stack follows **LIFO (Last In, First Out)**.
- Push Right first, then Left, so Left gets processed first.

**Remember:** Root → Left → Right = Pop Root → Push Right → Push Left.

       class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> preOrder = new ArrayList<>();
        Stack<TreeNode> st = new Stack<>();

        if (root == null) return preOrder;

        st.push(root);

        while (!st.isEmpty()) {

            TreeNode rootVal = st.pop();
            preOrder.add(rootVal.val);

            if (rootVal.right != null) {
                st.push(rootVal.right);
            }

            if (rootVal.left != null) {
                st.push(rootVal.left);
            }
        }

        return preOrder;
    }
    }


<img width="1800" height="2250" alt="image" src="https://github.com/user-attachments/assets/8c33fb3e-782c-44bc-bd12-e792ebd53c84" />
