#### 题目：Leetcode #589

给定一个 N 叉树，返回其节点值的 前序遍历 。

N 叉树 在输入中按层序遍历进行序列化表示，每组子节点由空值 null 分隔（请参见示例）。

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[1,3,5,6,2,4]

// 节点数据结构
   class Node {
        public int val;
        public List<Node> children;

        public Node() {
        }

        public Node(int val) {
            this.val = val;
        }

        public Node(int val, List<Node> children) {
            this.val = val;
            this.children = children;
        }
    }
    
    class Solution {
         public List<Integer> preorder(Node root) {
    
         }
    }
```



#### 解题

首先前序遍历，定义为遍历根节点->遍历左子树->遍历右子树。那么根据定义，递归的思路为：

res.add(root)

递归调用(root.left)

递归调用(root.right)

由于是N叉树，那么套用上面的方式，改为:

res.add(root)

递归调用(root.[0])

递归调用(root.[1])

...

递归调用(root.[n])

观察递归遍历，可不就是循环遍历根节点的子节点吗？转化为代码

```
    /**
     * 递归
     * @param root 根节点
     * @return 前序遍历
     */
     List<Integer> preorder(Node root) {
        List<Integer> res = new ArrayList<>();
        if (null == root)
            return res;
        // 根节点
        res.add(root.val);
        // 左子树
        // 右子树
        for (Node node : root.children) {
            res.addAll(preorder(node));
        }

        return res;
    }
```

换个迭代的方式，树的遍历方式通常可以用栈结构进行操作，为什么选择栈呢，因为树的遍历方式，都有攒一波再访问的特点，而且攒的顺序与访问时有倒逆过来。栈结构先进后出，比较符合。

那么看一下，前序遍历，除了根节点访问，左右子树放入栈中，按照前序遍历，左子树先访问，右子树后访问，那么入栈时，则需要右子树先入栈，左子树后入栈，这样才能做到出栈时满足访问顺序。由于节点的子树存入数组时按照从左往右，而入栈时需要倒过来，所以在入栈前，需要给节点的子树进行一次逆操作。

这里有个取巧的方式，用链表模拟栈，在链表的尾部进行入栈和出栈，代价非常小。

```
    /**
     * 迭代，利用栈结构，结合遍历的要求
     *
     * @param root 根节点
     * @return 前序遍历
     */
    List<Integer> preorder2(Node root) {
        List<Integer> res = new LinkedList<>();
        if (null == root)
            return res;
        LinkedList<Node> stacks = new LinkedList<>();
        stacks.add(root);
        // 将元素出栈，然后将children入栈
//        Iterator<Node> iterator = stacks.iterator();//不能删除操作
        while (!stacks.isEmpty()) {
            // 从尾部弹出和加入，代价最小
            Node next = stacks.pollLast();
            res.add(next.val);
            Collections.reverse(next.children);
//            Lists.reverse(next.children);//[1, 5, 10, 9, 13, 4, 8, 12, 3, 7, 11, 14, 6, 2]
            for (Node node :
                    next.children) {
                stacks.add(node);
            }
        }
        return res;
    }
```

