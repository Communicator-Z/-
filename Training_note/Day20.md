# **第六章 二叉树part0**7

### **235. 二叉搜索树的最近公共祖先** 

相对于 二叉树的最近公共祖先 本题就简单一些了，因为 可以利用二叉搜索树的特性。 

题目链接/文章讲解：[https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html](https://programmercarl.com/0235.二叉搜索树的最近公共祖先.html)  

视频讲解：https://www.bilibili.com/video/BV1Zt4y1F7ww  

```java

```



### **701.二叉搜索树中的插入操作**  

本题比想象中的简单，大家可以先自己想一想应该怎么做，然后看视频讲解，就发现 本题为什么比较简单了。

题目链接/文章讲解：[https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html](https://programmercarl.com/0701.二叉搜索树中的插入操作.html)  

视频讲解：https://www.bilibili.com/video/BV1Et4y1c78Y  

```java

```



### **450.删除二叉搜索树中的节点**  

相对于 插入操作，本题就有难度了，涉及到改树的结构 

题目链接/文章讲解：[https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0450.删除二叉搜索树中的节点.html)  

视频讲解：https://www.bilibili.com/video/BV1tP41177us  

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val == key) {
            if (root.left == null && root.right == null) {
                return null;
            } else if (root.left == null && root.right != null) {
                return root.right;
            } else if (root.left != null && root.right == null) {
                return root.left;
            } else {
                TreeNode cur = root.right;
                while (cur.left != null) {
                    cur = cur.left;
                }
                cur.left = root.left;
                return root.right;
            }
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }
        return root;
    }
}
```

这个题非常经典。
