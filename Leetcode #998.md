#### 题目：Leetcode #998

> （题目描述不清，不必探究）

最大树定义：一个树，其中每个节点的值都大于其子树中的任何其他值。给出最大树的根节点 `root`。给定的树是从列表 `A`（`root = Construct(A)`）递归地使用下述 `Construct(A)` 例程构造的：

- 如果 A 为空，返回 null
- 否则，令 A[i] 作为 A 的最大元素。创建一个值为 A[i] 的根节点 root
- root 的左子树将被构建为 Construct([A[0], A[1], ..., A[i-1]])
- root 的右子树将被构建为 Construct([A[i+1], A[i+2], ..., A[A.length - 1]])
- 返回 root

请注意，我们没有直接给定 A，只有一个根节点 `root = Construct(A)`.

假设 `B` 是 `A` 的副本，并在末尾附加值 `val`。题目数据保证 `B` 中的值是不同的。返回 `Construct(B)`。

```java
   class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        TreeNode() {
        }

        TreeNode(int val) {
            this.val = val;
        }

        TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }
    
public TreeNode insertIntoMaxTree(TreeNode root, int val) {
}
```



#### 解题

题目的意思是本来构造最大数的数组是A，在A中找出最大的值当做根，最大值左边的值为左子树，右边的数组为右子树。

而现在向A的后面插入了val，如果val是最大的，那么根节点的值就要是val，val前面的数（也就是前面的整棵树）做为val的左子树。如果val不是最大的，那么就把val往右子树上面插（val的位置是最后，肯定在最大值右边）。

所以也就是向最大树root中添加一值为val的节点，如果val大于root的值，那么就把root当做值为val节点左孩子，否则，就把val插入到右孩子的相应位置。

```java
    public TreeNode insertIntoMaxTree(TreeNode root, int val) {
        if (null == root || root.val < val) {
            TreeNode node = new TreeNode(val);
            node.left = root;
            return node;
        }
        TreeNode right = insertIntoMaxTree(root.right, val);
        root.right = right;
        return root;
    }
```

