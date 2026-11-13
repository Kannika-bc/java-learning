## **Level Order Traversal – Logic**

1. Create a Queue to store tree nodes and a `wrapList` to store the final answer.
2. If the root is `null`, return the empty `wrapList`.
3. Add the root node to the Queue.
4. Run a `while` loop until the Queue becomes empty.
5. Store the current queue size in `levelNum`. This tells us how many nodes are present at the current level.
6. Create a `subList` to store the values of the current level.
7. Run a `for` loop `levelNum` times:
   - Check if the front node has a left child. If yes, add it to the Queue.
   - Check if the front node has a right child. If yes, add it to the Queue.
   - Remove the front node using `poll()` and add its value to `subList`.
8. After processing all nodes at the current level, add `subList` to `wrapList`.
9. Repeat until the Queue becomes empty.
10. Return `wrapList`.

### **Important logic to understand**

- `levelNum = queue.size()` → Captures how many nodes belong to the current level.
- `for` loop → Processes only those nodes, not the next level.
- `peek()` → Looks at the front node without removing it.
- `poll()` → Removes the front node.
- `offer()` → Adds child nodes to the back of the Queue.
- `subList` → Stores values of one level.
- `wrapList` → Stores values of all levels.

**Remember this flow:**

`Queue → Get level size → Process nodes → Add children → Store level → Repeat`

This is the exact logic used in your code.

    public List<List<Integer>> levelOrder(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    List<List<Integer>> wrapList = new LinkedList<>();

    if (root == null) return wrapList;

    queue.offer(root);

    while (!queue.isEmpty()) {

        int levelNum = queue.size();
        List<Integer> subList = new LinkedList<>();

        for (int i = 0; i < levelNum; i++) {

            if (queue.peek().left != null)
                queue.offer(queue.peek().left);

            if (queue.peek().right != null)
                queue.offer(queue.peek().right);

            subList.add(queue.poll().val);
        }

        wrapList.add(subList);
    }

    return wrapList;
    }


<img width="1800" height="2040" alt="image" src="https://github.com/user-attachments/assets/77bab2a4-33fb-4d82-b7e2-c12b36fed3a1" />
