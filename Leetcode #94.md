####  题目：Leetcode #94

给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。

```java
输入：root = [1,null,2,3]
输出：[1,3,2]

// 节点数据结构
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
    
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
    }
 }
```



#### 解题

首先中序遍历，按照定义中序遍历左子树->访问根节点->遍历右子树，那么按照定义，递归的思路为：

递归调用(root.left)

res.add(root)

递归调用(root.right)

观察给定的函数，发现入参没有用于记录递归过程中的值，怎么办？再加个函数呗，按照自己的需求，定义一个辅助函数，完成递归计算，在给定的函数中调用它。

```java
    /**
     * 递归
     *
     * @param root 根节点
     * @return 中序遍历
     */
    List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<>();
        inOrder(root, res);
        return res;
    }

    void inOrder(TreeNode root, List<Integer> res) {
        if (null == root)
            return;

        // 左子树
        inOrder(root.left, res);
        // 根节点
        res.add(root.val);
        // 右子树
        inOrder(root.right, res);
    }
```

换个迭代的方式，用栈结构存储访问顺序，中序遍历，观察访问要求，它是一直不断访问左子树，直到左子树为null，然后转向根节点，根节点访问完，转向右子树，到了右子树之后，会重复前面的动作，也就是循环访问此刻右子树的左子树，直到左子树为null，访问根节点，根节点访问完，转向右子树...

也就是说，在外部访问节点过程中，会先循环遍历左子树，遍历完再是根节点，接着转向右子树。

```java
List<Integer> inorderTraversal2(TreeNode root) {
        LinkedList<Integer> res = new LinkedList<>();
        if (null == root)
            return res;
        LinkedList<TreeNode> stack = new LinkedList<>();

        while (null != root || !stack.isEmpty()) {

            while (null != root) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            res.add(root.val);
            root = root.right;
        }

        return res;
    }
```

