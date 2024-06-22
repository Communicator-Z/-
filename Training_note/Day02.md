# 第一章：数组Part02

###  **977.有序数组的平方** 

1. ##### 暴力解法

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

2. **双指针解法**

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

###  **209.长度最小的子数组**

​	滑动窗口的应用，比较巧妙，多回顾。	

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



###  **59.螺旋矩阵II**

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] nums = new int[n][n];
        int startX = 0, startY = 0;  // 每一圈的起始点
        int offset = 1;
        int count = 1;  // 矩阵中需要填写的数字
        int loop = 1; // 记录当前的圈数
        int i, j; // j 代表列, i 代表行;

        while (loop <= n / 2) {

            // 顶部
            // 左闭右开，所以判断循环结束时， j 不能等于 n - offset
            for (j = startY; j < n - offset; j++) {
                nums[startX][j] = count++;
            }

            // 右列
            // 左闭右开，所以判断循环结束时， i 不能等于 n - offset
            for (i = startX; i < n - offset; i++) {
                nums[i][j] = count++;
            }

            // 底部
            // 左闭右开，所以判断循环结束时， j != startY
            for (; j > startY; j--) {
                nums[i][j] = count++;
            }

            // 左列
            // 左闭右开，所以判断循环结束时， i != startX
            for (; i > startX; i--) {
                nums[i][j] = count++;
            }
            startX++;
            startY++;
            offset++;
            loop++;
        }
        if (n % 2 == 1) { // n 为奇数时，单独处理矩阵中心的值
            nums[startX][startY] = count;
        }
        return nums;
    }
}
```

