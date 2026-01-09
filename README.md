# smallest-subtree-with-all-the-deepest-nodes-in-java

This is **LeetCode 865 – Smallest Subtree with all the Deepest Nodes**.
I’ll explain the **idea clearly**, then give a **clean Java solution** (DFS, single pass).

---

## 🔍 Key Idea (Intuition)

We want the **lowest node** whose subtree contains **all deepest nodes**.

Think bottom-up (postorder DFS):

For **every node**, compute:

* **depth** of its subtree (max depth below it)
* **answer node** for that subtree

### At a node:

* Get `(leftDepth, leftNode)` from left child
* Get `(rightDepth, rightNode)` from right child

Now:

1. **If leftDepth == rightDepth**
   → deepest nodes are on **both sides**
   → current node is the **smallest subtree root**

2. **If leftDepth > rightDepth**
   → deepest nodes only on left
   → propagate `leftNode`

3. **If rightDepth > leftDepth**
   → propagate `rightNode`

Depth returned = `max(leftDepth, rightDepth) + 1`

---

## 🧠 Why this works

* Deepest nodes are determined by **height**
* The **Lowest Common Ancestor (LCA)** of all deepest nodes is the answer
* This DFS computes that **implicitly**

---

## ✅ Java Solution (Optimal)

```java
class Solution {

    class Pair {
        TreeNode node;
        int depth;

        Pair(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }

    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        return dfs(root).node;
    }

    private Pair dfs(TreeNode root) {
        if (root == null) {
            return new Pair(null, 0);
        }

        Pair left = dfs(root.left);
        Pair right = dfs(root.right);

        if (left.depth == right.depth) {
            // deepest nodes on both sides
            return new Pair(root, left.depth + 1);
        } else if (left.depth > right.depth) {
            return new Pair(left.node, left.depth + 1);
        } else {
            return new Pair(right.node, right.depth + 1);
        }
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(n)` (each node visited once)
* **Space:** `O(h)` (recursion stack, `h` = tree height)

---

## 📌 Example (from image)

Deepest nodes: **7 and 4**
Their **lowest common ancestor** is **2**
➡️ Smallest subtree root = **2**

---


