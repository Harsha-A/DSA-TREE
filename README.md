# DSA-TREE
DSA Tree

That’s an excellent question — and it’s **one of the most important design decisions** when solving binary tree problems 🌳

Let’s go step-by-step so you know **when to choose DFS vs BFS** (and which variant of DFS).

---

## 🧭 The Big Picture

There’s **no one-size-fits-all** — both **DFS** and **BFS** are tools suited for different purposes.

| Traversal Type                 | Strategy                                        | Typical Implementation |
| ------------------------------ | ----------------------------------------------- | ---------------------- |
| **DFS (Depth-First Search)**   | Explore as deep as possible before backtracking | Recursion or Stack     |
| **BFS (Breadth-First Search)** | Explore level by level                          | Queue                  |

---

## 🌲 DFS — “Go Deep First”

### ✅ When to Use DFS:

Use **DFS** when your problem depends on:

* **Depth information** (e.g., max depth, path sums)
* **Tree structure** (e.g., checking subtree equality)
* **Bottom-up aggregation** (e.g., heights, sums, paths)
* **Recursive relationships** (parent-child dependencies)

### 🧩 Common Problems Solved with DFS:

| Problem                      | Why DFS works best                     |
| ---------------------------- | -------------------------------------- |
| Max Depth of Binary Tree     | Need to go to deepest node             |
| Path Sum / Max Path Sum      | Path computation depends on children   |
| Validate BST                 | Compare node with its subtrees         |
| Lowest Common Ancestor       | Must explore full subtrees to decide   |
| Is Subtree / Same Tree       | Structural comparison — recursive      |
| Serialize / Deserialize Tree | Preorder traversal fits recursion well |

### 🔍 Example:

```js
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

DFS is **natural** here because you calculate depth recursively from children upward.

---

## 🌳 BFS — “Go Level by Level”

### ✅ When to Use BFS:

Use **BFS** when your problem depends on:

* **Levels or distances** (shortest path, width)
* **Processing level by level**
* **Finding nearest elements** (first X found in tree)
* **Layered updates or traversal order**

### 🧩 Common Problems Solved with BFS:

| Problem                        | Why BFS works best                      |
| ------------------------------ | --------------------------------------- |
| Level Order Traversal          | Process each level in order             |
| Minimum Depth                  | Stops as soon as a leaf is found        |
| Zigzag / Bottom-up Level Order | Works directly on levels                |
| Connect Nodes at Same Level    | BFS makes it natural to connect peers   |
| Serialization (by level)       | Natural order for output reconstruction |

### 🔍 Example:

```js
function minDepth(root) {
    if (!root) return 0;
    let queue = [[root, 1]];

    while (queue.length) {
        const [node, depth] = queue.shift();
        if (!node.left && !node.right) return depth;
        if (node.left) queue.push([node.left, depth + 1]);
        if (node.right) queue.push([node.right, depth + 1]);
    }
}
```

BFS is **faster here** since it finds the first leaf (minimum depth) early.

---

## ⚔️ **DFS vs BFS: Quick Comparison**

| Category                      | **DFS**                                    | **BFS**                                |
| ----------------------------- | ------------------------------------------ | -------------------------------------- |
| **Use Case**                  | Structural or path-based                   | Level-based or shortest distance       |
| **Data Structure**            | Stack / Recursion                          | Queue                                  |
| **Memory**                    | O(h) (height of tree)                      | O(w) (max width of tree)               |
| **Best For**                  | Deep calculations, recursive relationships | Shallow search, level-wise traversal   |
| **Early Stop?**               | Usually must explore full tree             | Can often stop early (e.g., min depth) |
| **Implementation Simplicity** | Recursive (cleaner code)                   | Iterative (more boilerplate)           |

---

## 💡 Rule of Thumb

| Problem Type                                     | Preferred Approach |
| ------------------------------------------------ | ------------------ |
| “Find maximum/minimum **depth/path/sum**”        | DFS                |
| “Check if two trees are **identical / subtree**” | DFS                |
| “Traverse tree **level by level**”               | BFS                |
| “Find **shortest distance / minimum steps**”     | BFS                |
| “Calculate **aggregate from leaves up**”         | DFS                |
| “Return **nodes grouped by level**”              | BFS                |
| “Find **ancestor / structural property**”        | DFS                |

---

## 🧠 Hybrid Insight

Sometimes, you combine both!

For example:

* Use **DFS** to build a list of nodes per depth.
* Then use **BFS** to analyze level-by-level properties.

---

## 🏁 In short:

> 💬 **If your problem depends on structure → DFS.**
> 💬 **If it depends on levels or shortest path → BFS.**

---


=================================================================================================================
Sure — let’s go through your **JavaScript solution for finding the maximum depth of a binary tree**, step by step.

---

### 🧩 **Goal**

You want to find the **maximum depth (or height)** of a binary tree.
That is, the number of **levels (layers)** in the tree.

Example:

```
        1
       / \
      2   3
     / \
    4   5
```

Here,

* Level 1 → Node 1
* Level 2 → Nodes 2, 3
* Level 3 → Nodes 4, 5
  ✅ Max Depth = 3

---

### 🧱 **TreeNode Definition**

```js
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

Each node stores:

* `val`: node’s value
* `left`: pointer to left child
* `right`: pointer to right child

---

### 🧮 **Main Logic — BFS (Level Order Traversal)**

```js
maxDepth(root) {
    const q = new Queue();
    if (root !== null) {
        q.push(root);
    }
```

* You’re using a **Queue** to perform **Breadth-First Search (BFS)** traversal.
* If `root` exists, it’s added to the queue.

---

### 🔁 **Processing Each Level**

```js
let level = 0;
while (q.size() > 0) {
    const size = q.size();
```

* The outer `while` runs **once per level**.
* `size` stores the number of nodes at the current level.
* Then, for each of these nodes, you’ll add their children to the queue for the next level.

---

### 👣 **Traversing Nodes at the Current Level**

```js
for (let i = 0; i < size; i++) {
    const node = q.pop();
    if (node.left !== null) {
        q.push(node.left);
    }
    if (node.right !== null) {
        q.push(node.right);
    }
}
```

* You dequeue (pop) one node at a time.
* If it has a left child, enqueue it.
* If it has a right child, enqueue it.
* After processing all `size` nodes, one full level is completed.

---

### 📈 **Increment Level Counter**

```js
level++;
```

* Each iteration of the outer `while` represents **one complete level** of the tree.
* So after processing all nodes of that level, increment `level`.

---

### 🏁 **Return Result**

```js
return level;
```

When the queue becomes empty (no more nodes to visit),
`level` will contain the **maximum depth** of the tree.

---

### ✅ **Summary of Execution**

Example:

```
Input tree: [3, 9, 20, null, null, 15, 7]
```

Execution:

| Step   | Queue Content | Level Count | Description      |
| ------ | ------------- | ----------- | ---------------- |
| Start  | [3]           | 0           | Root node        |
| Pass 1 | [9, 20]       | 1           | Children of 3    |
| Pass 2 | [15, 7]       | 2           | Children of 20   |
| Pass 3 | []            | 3           | No more children |

**Output:** `3`

---

### 💡 **Alternate Recursive Version**

For comparison, here’s a simpler recursive version:

```js
maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(this.maxDepth(root.left), this.maxDepth(root.right));
}
```

Both return the same result — BFS is **iterative**, recursion is **DFS-based**.

---
=============================================================================

Perfect 🔥 — here’s your **Ultimate Tree Problem Cheat Sheet (DFS vs BFS)** — tailored for **SDE-3-level interviews**.
Each entry includes: ✅ **Approach**, 🧠 **Reasoning**, and 💻 **Pattern / Tip** for implementation.

---

# 🌳 Binary Tree Problem-Solving Cheat Sheet

### 🧭 When to use DFS vs BFS

| #    | Problem                                              | Approach             | 🧠 Reasoning                                                        | 💻 Pattern / Implementation Tip                       |
| ---- | ---------------------------------------------------- | -------------------- | ------------------------------------------------------------------- | ----------------------------------------------------- |
| 1️⃣  | **Max Depth / Height of Tree**                       | ✅ **DFS**            | You must go to the deepest leaf and count height bottom-up.         | Recursive postorder traversal: `1 + max(left, right)` |
| 2️⃣  | **Min Depth of Tree**                                | ✅ **BFS**            | The first leaf you encounter (level-wise) gives minimum depth.      | Queue with `[node, depth]`, return on first leaf      |
| 3️⃣  | **Level Order Traversal**                            | ✅ **BFS**            | You explicitly need nodes level by level.                           | Use queue, process each level with size loop          |
| 4️⃣  | **Zigzag / Reverse Level Order**                     | ✅ **BFS**            | Traversal order depends on level.                                   | Track level count, reverse or alternate directions    |
| 5️⃣  | **Sum of Nodes / Count of Nodes**                    | ✅ **DFS**            | Aggregate values recursively (subtree-based).                       | Return `val + dfs(left) + dfs(right)`                 |
| 6️⃣  | **Check if Tree is Balanced**                        | ✅ **DFS**            | Requires height check of left/right subtrees recursively.           | Postorder traversal returning height & balance flag   |
| 7️⃣  | **Validate BST**                                     | ✅ **DFS**            | Must ensure each node value is within (min, max) range recursively. | Pass bounds: `(leftBound, rightBound)`                |
| 8️⃣  | **Lowest Common Ancestor (LCA)**                     | ✅ **DFS**            | Requires exploring full paths of both nodes recursively.            | Return node if match found in subtrees                |
| 9️⃣  | **Path Sum (root-to-leaf)**                          | ✅ **DFS**            | Sum depends on cumulative path values (top-down).                   | Track running sum along path                          |
| 🔟   | **Maximum Path Sum**                                 | ✅ **DFS**            | Must combine left and right subtree contributions at each node.     | Global max tracker with `val + left + right`          |
| 11️⃣ | **Is Subtree / Same Tree**                           | ✅ **DFS**            | Structure & values comparison — recursion fits naturally.           | Compare root + recursive left/right                   |
| 12️⃣ | **Symmetric Tree**                                   | ✅ **DFS** *(or BFS)* | Check mirror property of left & right subtrees.                     | Recursive pair check `(left.left, right.right)`       |
| 13️⃣ | **Invert / Mirror Tree**                             | ✅ **DFS**            | Swap subtrees recursively.                                          | Simple recursive swap at each node                    |
| 14️⃣ | **Diameter of Tree**                                 | ✅ **DFS**            | Needs max path through subtrees — bottom-up aggregation.            | Compute heights, update global diameter               |
| 15️⃣ | **Right Side View**                                  | ✅ **BFS**            | Need last node at each level.                                       | Level traversal → take last node each level           |
| 16️⃣ | **Left Side View**                                   | ✅ **BFS**            | First node at each level is visible.                                | Level traversal → take first node                     |
| 17️⃣ | **Average of Levels**                                | ✅ **BFS**            | Need level-by-level averages.                                       | Queue with level sum/count                            |
| 18️⃣ | **Connect Nodes at Same Level**                      | ✅ **BFS**            | Direct access to next node in same level.                           | Track previous node in each level                     |
| 19️⃣ | **Serialize / Deserialize Tree (Preorder)**          | ✅ **DFS**            | Recursive traversal naturally builds structure.                     | Preorder encode/decode with null markers              |
| 20️⃣ | **Serialize / Deserialize Tree (Level Order)**       | ✅ **BFS**            | Level order fits queue-based reconstruction.                        | Use queue for nodes, include nulls                    |
| 21️⃣ | **Bottom View / Top View** (of Binary Tree)          | ✅ **BFS**            | You need horizontal distance mapping per level.                     | Queue with `[node, hd]`, map last seen nodes          |
| 22️⃣ | **Boundary Traversal**                               | ✅ **DFS**            | Complex: left boundary, leaves, right boundary recursively.         | Handle left, leaves, right separately                 |
| 23️⃣ | **Vertical Order Traversal**                         | ✅ **BFS**            | Need column-based grouping → track horizontal distance per level.   | Queue with `[node, hd, level]`, map values            |
| 24️⃣ | **Flatten Binary Tree to Linked List**               | ✅ **DFS**            | Must modify tree recursively in preorder sequence.                  | Use right-pointer re-linking                          |
| 25️⃣ | **Distance K from Target Node**                      | ✅ **BFS**            | Expanding out from target node level by level.                      | Build graph, then BFS outward                         |
| 26️⃣ | **Cousins in Binary Tree**                           | ✅ **BFS**            | Need level information to group cousins.                            | Level-based traversal with parent tracking            |
| 27️⃣ | **Tree Traversals (Inorder / Preorder / Postorder)** | ✅ **DFS**            | Defined by recursive visiting order.                                | Stack or recursion                                    |
| 28️⃣ | **Width of Binary Tree**                             | ✅ **BFS**            | Need first & last node in each level to measure width.              | Queue with positional index                           |
| 29️⃣ | **Find Duplicate Subtrees**                          | ✅ **DFS**            | Need serialized subtree structure bottom-up.                        | Use map of subtree strings                            |
| 30️⃣ | **Sum Root to Leaf Numbers**                         | ✅ **DFS**            | Depends on concatenating path values top-down.                      | Carry accumulated number in recursion                 |

---

## 🧠 Quick Summary Table

| Category                                 | Approach   | Examples                            |
| ---------------------------------------- | ---------- | ----------------------------------- |
| **Depth / Height / Diameter / Path Sum** | DFS        | MaxDepth, MaxPathSum, Diameter      |
| **Level / Layered Problems**             | BFS        | LevelOrder, MinDepth, RightSideView |
| **Structure Validation**                 | DFS        | ValidateBST, IsBalanced, IsSubtree  |
| **Shortest Distance / Closest Node**     | BFS        | MinDepth, Distance K, Cousins       |
| **Serialization / Views**                | BFS or DFS | SerializeTree, Top/Bottom View      |
| **Aggregations / Recursion**             | DFS        | CountNodes, Sum, MaxPath            |

---

## 💡 Pro Tip (for interviews)

| Scenario                                                      | Preferred Traversal |
| ------------------------------------------------------------- | ------------------- |
| Asked for **“path-based”** computation → DFS                  |                     |
| Asked for **“level-wise”** output → BFS                       |                     |
| Asked for **“maximum/minimum”** value based on subtrees → DFS |                     |
| Asked for **“shortest path”** → BFS                           |                     |
| Asked for **“global structure validation”** → DFS             |                     |

---

## 🚀 Interview Strategy

1. **Visualize the tree** — what does “state” depend on?
   → Path (DFS) or Level (BFS)?
2. **Check if you can early-exit** — if yes, likely BFS.
3. **If recursion feels natural** → DFS.
4. **If you need queue grouping or distance** → BFS.


