# 第一章：数组01

### 数组理论基础

- 数组下标都是从0开始的。
- 数组内存空间的地址是连续的

- 因为数组在内存空间的地址是连续的，所以我们在删除或者增添元素的时候，就难免要移动其他元素的地址。

文章链接：[https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html](https://programmercarl.com/数组理论基础.html)

### **704. 二分查找** 

- 熟悉根据**左闭右开**，**左闭右闭**两种区间规则写出来的二分法

1. **左闭右闭：**

   右边界存在，所以可以一开始定义 right 的时候，要从 nums.length-1 开始，while的循环判断是 <=……

   直接看代码：

   ```java
   class Solution {
       public int search(int[] nums, int target) {
           int left = 0;
           int right = nums.length-1;
           int mid = 0;
           
           while(left<=right){
               mid = (left+right)/2;
               if(target<nums[mid]){
                   right = mid-1;
               }else if(target>nums[mid]){
                   left = mid+1;
               }else{
                   return mid;
               }
           }
           return -1;
       }
   }
   ```

2. **左闭右开：**

   右边界不存在，所以 right 要从 nums.length 开始。这个不用想太多，就当做为了要包括整个空间，但是右边是开区间，所以要多括进去一个，就比如说要包括 1, 2, 3 ，区间为 [ 1 , 4 )。

   代码：

   ```java
   class Solution {
       public int search(int[] nums, int target) {
           int left = 0;
           int right = nums.length;
           int mid = 0;
           
           while(left<right){
               mid = (left+right)/2;
               if(target<nums[mid]){
                   right = mid;
               }else if(target>nums[mid]){
                   left = mid+1;
               }else{
                   return mid;
               }
           }
           return -1;
       }
   }
   ```



​	题目链接：https://leetcode.cn/problems/binary-search/

​	文章讲解：https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html

​	视频讲解：https://www.bilibili.com/video/BV1fA4y1o715



### **27. 移除元素**

- 暴力的解法，可以锻炼一下我们的代码实现能力，建议先把暴力写法写一遍。 **双指针法是本题的精髓，今日需要掌握**，至于拓展题目可以先不看。 

1. **暴力解法：**

   用两层for循环，复杂度O(n^2)，这里需要注意的是，当第一层循环发现有相同的数据之后，第二层循环负责把所有的元素往前移动（赋值给前一个）。做完之后，由于在第一层进循环后，已经i+1，而移动之后的数组新数据从i开始，所以需要做一步i--，当然size也需要-1。

   代码如下：

   ```java
   class Solution {
       public int removeElement(int[] nums, int val) {
           int size = nums.length;
           for(int i=0; i<size; i++){
               if(nums[i]==val){
                   for(int j=i+1; j<size; j++){
                       nums[j-1] = nums[j];
                   }
                   i--;
                   size--;
               }
           }
           return size;
       }
   }
   ```

   

2. **双指针：**

   快指针进行搜索，慢指针负责赋值，很巧妙的算法。注意是在不同的时候，快慢指针同时移动，当遇到相同需要删除的元素时，快指针继续向下搜索，而慢指针原地等待赋值。

   代码如下：

   ```java
   class Solution {
       public int removeElement(int[] nums, int val) {
           int size = nums.length;
           int slow = 0;
           for(int fast=0; fast<size; fast++){
              if(nums[fast]!=val){
               nums[slow] = nums[fast];
               slow++;
              }
           }
           return slow;
       }
   }
   ```

​	

​	题目链接：https://leetcode.cn/problems/remove-element/ 

​	文章讲解：[https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html](https://programmercarl.com/0027.移除元素.html)

​	视频讲解：https://www.bilibili.com/video/BV12A4y1Z7LP 

