# DSA-TREE
DSA Tree


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

Would you like me to rewrite your BFS version in **pure JavaScript (without a custom Queue class)** — using an array instead — so you can run it directly in a browser or LeetCode?
