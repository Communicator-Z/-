# **第七章 回溯算法**part01 

### **理论基础** 

其实在讲解二叉树的时候，就给大家介绍过回溯，这次正式开启回溯算法，大家可以先看视频，对回溯算法有一个整体的了解。

题目链接/文章讲解：[https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/回溯算法理论基础.html)  

视频讲解：https://www.bilibili.com/video/BV1cy4y167mM  



### **77. 组合**  

对着 在 回溯算法理论基础 给出的 代码模板，来做本题组合问题，大家就会发现 写回溯算法套路。

在回溯算法解决实际问题的过程中，大家会有各种疑问，先看视频介绍，基本可以解决大家的疑惑。

本题关于剪枝操作是大家要理解的重点，因为后面很多回溯算法解决的题目，都是这个剪枝套路。 

题目链接/文章讲解：[https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html](https://programmercarl.com/0077.组合.html)   

视频讲解：https://www.bilibili.com/video/BV1ti4y1L7cv 

剪枝操作：https://www.bilibili.com/video/BV1wi4y157er   

```java
class Solution {
    List<Integer> path = new ArrayList<>();
    List<List<Integer>> res = new ArrayList<>();
    
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return res;
    }

    public void backtracking(int n, int k, int startindex) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;//这里要return，否则会死循环，这是终止条件。
        }
        for (int i = startindex; i <= n - (k - path.size()) + 1; i++) {
            path.add(i);
            backtracking(n, k, i + 1);
            path.removeLast();
        }
    }
}
```



### **216.组合总和III** 

如果把 组合问题理解了，本题就容易一些了。 

题目链接/文章讲解：[https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html](https://programmercarl.com/0216.组合总和III.html)   

视频讲解：https://www.bilibili.com/video/BV1wg411873x

```java
class Solution {
    List<Integer> path = new ArrayList<>();
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(n, k, 0, 1);
        return res;
    }
    public void backtracking(int targetsum, int k, int sum, int startindex){
        if(sum > targetsum){
            return;//第一次剪枝
        }
        if(path.size() == k){
            if(sum == targetsum){
                res.add(new ArrayList<>(path));
                return;
            }
        }

        for(int i = startindex; i <= 9-(k-path.size())+1; i++){
            path.add(i);
            sum += i;
            backtracking(targetsum, k, sum, i+1);
            sum -= i;
            path.removeLast();
        }
    }
}
```

注意，剪枝操作是小于等于。



### **17.电话号码的字母组合** 

本题大家刚开始做会有点难度，先自己思考20min，没思路就直接看题解。 

题目链接/文章讲解：[https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html](https://programmercarl.com/0017.电话号码的字母组合.html)   

视频讲解：https://www.bilibili.com/video/BV1yV4y1V7Ug

```java
class Solution {
    List<String> list = new ArrayList<>();

    public List<String> letterCombinations(String digits) {

        if (digits == null || digits.length() == 0) {
            return list;
        }
        String[] numString = { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
        backTracking(digits, numString, 0);
        return list;
    }

    StringBuilder temp = new StringBuilder();

    public void backTracking(String digits, String[] numString, int num) {
        if (num == digits.length()) {
            list.add(temp.toString());
            return;
        }
        String str = numString[digits.charAt(num) - '0'];
        for (int i = 0; i < str.length(); i++) {
            temp.append(str.charAt(i));
            // 递归，处理下一层
            backTracking(digits, numString, num + 1);
            // 剔除末尾的继续尝试
            temp.deleteCharAt(temp.length() - 1);
        }
    }

}
```

