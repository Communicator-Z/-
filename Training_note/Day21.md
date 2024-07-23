# **第六章 二叉树**part08

### **669. 修剪二叉搜索树** 

**这道题目比较难**，比 添加增加和删除节点难的多，建议先看视频理解。

题目链接/文章讲解： [https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html](https://programmercarl.com/0669.修剪二叉搜索树.html)  

视频讲解： https://www.bilibili.com/video/BV17P41177ud  

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }
        if (root.val < low) {
            return trimBST(root.right, low, high);//如果小了，往右子树修剪
        }
        if (root.val > high) {
            return trimBST(root.left, low, high);////如果大了，往左子树修剪
        }
        root.left = trimBST(root.left, low, high);//修剪左子树
        root.right = trimBST(root.right, low, high);//修剪右子树
        return root;
    }
}
```

这就是递归套递归，每一步只要想好往哪里修剪，然后调用递归，返回值返回对了即可。有点难理解，但是确实能行。



### **108.将有序数组转换为二叉搜索树**  

本题就简单一些，可以尝试先自己做做。

[https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html](https://programmercarl.com/0108.将有序数组转换为二叉搜索树.html)  

视频讲解：https://www.bilibili.com/video/BV1uR4y1X7qL  

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        TreeNode root = traversal(nums, 0, nums.length-1);
        return root;
    }
    public TreeNode traversal(int[] nums, int left, int right){
        //左闭右闭区间
        if(left > right){
            //左闭右闭，此时就非法了
            return null;
        }
        int mid = (left + right)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = traversal(nums, left, mid-1);
        root.right = traversal(nums, mid+1, right);
        return root;
    }
}
```



### **538.把二叉搜索树转换为累加树**  

本题也不难，在 求二叉搜索树的最小绝对差 和 众数 那两道题目 都讲过了 双指针法，思路是一样的。

[https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html](https://programmercarl.com/0538.把二叉搜索树转换为累加树.html)  

视频讲解：https://www.bilibili.com/video/BV1d44y1f7wP

```java
class Solution {
    int pre = 0;
    public TreeNode convertBST(TreeNode root) {
        traversal(root);
        return root;

    }
    public void traversal(TreeNode cur){
        if(cur == null){
            return;
        }
        traversal(cur.right);//右
        cur.val+=pre;//中
        pre = cur.val;
        traversal(cur.left);//左
    }
}
```

都快忘了怎么调用无返回值的方法了，多练习。



### **总结篇**  

好了，二叉树大家就这样刷完了，做一个总结吧

[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E6%80%BB%E7%BB%93%E7%AF%87.html](https://programmercarl.com/二叉树总结篇.html)   