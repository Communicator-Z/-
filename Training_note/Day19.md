# **第六章 二叉树**part06

## **详细布置** 

### **530.二叉搜索树的最小绝对差** 

需要领悟一下二叉树遍历上双指针操作，优先掌握递归 

题目链接/文章讲解：[https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html](https://programmercarl.com/0530.二叉搜索树的最小绝对差.html) 

视频讲解：https://www.bilibili.com/video/BV1DD4y11779 

```java
class Solution {
    TreeNode pre;
    int result = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        if (root == null)
            return -1;
        traversal(root);
        return result;
    }

    public void traversal(TreeNode cur) {
        if (cur == null)
            return;
        traversal(cur.left);
        if (pre != null) {
            result = Math.min(result, cur.val - pre.val);
        }
        pre = cur;//这个不能放在下面
        traversal(cur.right);
    }
}
```

递归的思想其实就是，左左左……一直走到底，然后在往上翻，左中右顺序不能乱。



###  **501.二叉搜索树中的众数** 

和 530差不多双指针思路，不过 这里涉及到一个很巧妙的代码技巧。

可以先自己做做看，然后看我的视频讲解。

[https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html](https://programmercarl.com/0501.二叉搜索树中的众数.html)  

视频讲解：https://www.bilibili.com/video/BV1fD4y117gp  

```java
class Solution {
    ArrayList<Integer> resList;
    int maxCount;
    int count;
    TreeNode pre;

    public int[] findMode(TreeNode root) {
        resList = new ArrayList<>();
        maxCount = 0;
        count = 0;
        pre = null;
        findMode1(root);
        int[] res = new int[resList.size()];
        for (int i = 0; i < resList.size(); i++) {
            res[i] = resList.get(i);
        }
        return res;
    }

    public void findMode1(TreeNode root) {
        if (root == null) {
            return;
        }
        findMode1(root.left);

        int rootValue = root.val;
        // 计数
        if (pre == null || rootValue != pre.val) {
            count = 1;
        } else {
            count++;
        }
        // 更新结果以及maxCount
        if (count > maxCount) {
            resList.clear();
            resList.add(rootValue);
            maxCount = count;
        } else if (count == maxCount) {
            resList.add(rootValue);
        }
        pre = root;

        findMode1(root.right);
    }
}
```



###  **236. 二叉树的最近公共祖先** 

本题其实是比较难的，可以先看我的视频讲解 

[https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html](https://programmercarl.com/0236.二叉树的最近公共祖先.html) 

视频讲解：https://www.bilibili.com/video/BV1jd4y1B7E2

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) { // 递归结束条件
            return root;
        }

        // 后序遍历
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (left == null && right == null) { // 若未找到节点 p 或 q
            return null;
        } else if (left == null && right != null) { // 若找到一个节点
            return right;
        } else if (left != null && right == null) { // 若找到一个节点
            return left;
        } else { // 若找到两个节点
            return root;
        }
    }
}
```

这个真的不好想，要多练递归。