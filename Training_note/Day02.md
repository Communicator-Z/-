# 第一章：数组Part02

###  **977.有序数组的平方** 

##### 暴力解法：

先都平方完，然后写个冒泡排序。循环整个数组，是nums.length。但是冒泡排序的时候，因为会排到j和j+1比较，所以循环时，用nums.length-1。

代码如下：

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i=0;i<nums.length;i++){
            nums[i] = nums[i]*nums[i];
        }
        int temp = 0;
        for(int i=0;i<nums.length-1;i++){
            for(int j=0;j<nums.length-i-1;j++){
                if(nums[j]>nums[j+1]){
                    temp = nums[j+1];
                    nums[j+1] = nums[j];
                    nums[j] = temp;
                }
            }
        }
        return nums;
    }
}
```

**双指针解法：**

要注意return，在while中不需要，但是方法体需要return。

代码如下：

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int k = nums.length - 1;
        int i = 0;
        int j = nums.length - 1;
        int[] arr = new int[nums.length];
        while (i <= j) {
            if (nums[i] * nums[i] > nums[j] * nums[j]) {
                arr[k] = nums[i] * nums[i];
                k--;
                i++;
            } else {
                arr[k] = nums[j] * nums[j];
                k--;
                j--;
            }
        }
        return arr;
    }
}
```

题目链接：https://leetcode.cn/problems/squares-of-a-sorted-array/

文章讲解：[https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html](https://programmercarl.com/0977.有序数组的平方.html)

视频讲解： https://www.bilibili.com/video/BV1QB4y1D7ep



###  **209.长度最小的子数组**

滑动窗口的应用，比较巧妙，多回顾。

回顾：

注意进入循环体后，每次更新最小区间，不要光赋值，要比较大小之后不断更新。

而且要先算出来区间之后，再进行减减的操作。	

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;

        for(int right=0;right<nums.length;right++){
            sum+=nums[right];
            while(sum>=target){
                result = Math.min(result,right-left+1);
                sum -= nums[left];
                left++;
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```

题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/

文章讲解：[https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html](https://programmercarl.com/0209.长度最小的子数组.html)

视频讲解：https://www.bilibili.com/video/BV1tZ4y1q7XE



###  **59.螺旋矩阵II**

这里需要注意多写几遍，很容易不熟。而且最后的处理奇数中心的时候，有两种处理方式，都可以。

```java
    public static int[][] generateMatrix(int n) {
        int[][] nums = new int[n][n];
        int startX = 0;//起始点
        int startY = 0;
        int offset = 1;//控制每圈的长度
        int count = 1;//需填入的数字
        int loop = 1;//当前圈数
        int i, j;//i表示行，j表示列
        while (loop <= n / 2) {
            //  ——
            for (j = startY; j < n - offset; j++) {
                nums[startX][j] = count++;
            }
            //   |
            for (i = startX; i < n - offset; i++) {
                nums[i][j] = count++;
            }
            //  __
            for (; j > startY; j--) {
                nums[i][j] = count++;
            }
            // |
            for (; i > startX; i--) {
                nums[i][j] = count++;
            }
            startX++;
            startY++;
            offset++;
            loop++;
        }
//        if (n % 2 == 1) {
//            nums[(n-1) / 2][(n-1) / 2] = n * n;
//        }
        if (n % 2 == 1) { // n 为奇数时，单独处理矩阵中心的值
            nums[startX][startY] = count;
        }
        return nums;
    }
```

题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/

文章讲解：[https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html](https://programmercarl.com/0209.长度最小的子数组.html)

视频讲解：https://www.bilibili.com/video/BV1tZ4y1q7XE
