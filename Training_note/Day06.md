# **第三章 哈希表**part02 

首先回顾一下相关哈希表语法：

```java
Set<Integer> set1 = new HashSet<>();
sites.size();
sites.contains("Taobao");
set.add(i);

Map<Integer, Integer> map = new HashMap<>();
map.containsKey(temp)
res[0] = map.get(temp);
value = map.get(key);
map.put(nums[i],i);
Map.getOrDefault(Object key, V defaultValue);//是Java中Map接口的一个方法，用于获取指定键对应的值，如果键不存在，则返回一个默认值。
```



### **454.四数相加II** 

这道题注意Map.getOrDefault(Object key, V defaultValue);方法的使用。主要思路是把sum当做key，出现的次数当做value。用了这个方法，就可以每次查出来之前出现了几次，接着加一。要是没有，就加0，然后再加上这个自己的一次。

注意这道题其实是从互相独立的四个数组中取值，不需要去重复。所以可以很方便的用哈希。

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer,Integer> map = new HashMap<>();
        int sum = 0;
        int res = 0;
        for(int i = 0; i < nums1.length; i++){
            for(int j = 0; j < nums2.length; j++){
                sum = nums1[i]+nums2[j]; 
                map.put(sum,map.getOrDefault(sum,0)+1); 
            }
        }
        for(int i : nums3){
            for(int j : nums4){
                res += map.getOrDefault(0-i-j,0);
            }
        }
        return res;
    }
}
```

https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html



### **383. 赎金信**

哈希数组解法，一般不区分大小写字母的，都用数组就能应付

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        int index = 0;
        for (char i : magazine.toCharArray()) {
            arr[i - 'a'] += 1;
        }
        for (char i : ransomNote.toCharArray()) {
            arr[i - 'a'] -= 1;
        }
        for (int i : arr) {
            if (i < 0) {
                return false;
            }
        }
        return true;
    }
}
```

哈希map解法：（注意看如果不用getOrDefault方法，如何用常规方法实现。）

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        char[] str1 = ransomNote.toCharArray();
        char[] str2 = magazine.toCharArray();
        Map<Character,Integer> map = new HashMap<Character,Integer>();
        for(char i : str1){
            map.put(i,map.getOrDefault(i,0)+1);
            // if(map.containsKey(z)){
            //		map.put(z,map.get(z) + 1);
            //}else{
            // 		map.put(z,1);}
        }
        for(char z : str2){
            if(map.containsKey(z)){
                map.put(z,map.get(z)-1);//通过遍历magazine字符来抵消ransomNote字符           
            }
        }
        for(char z : str1){
            int index = map.get(z);
            if(index > 0){
                return false;
            }
        }
        return true;
    }
}
```

https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html



### **15. 三数之和**

双指针+去重。有时间记得用哈希去重练习一下，比较复杂。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // 用链表不用数组是因为链表是动态的
        List<List<Integer>> result = new ArrayList<>();//二维链表
        Arrays.sort(nums);// 先排序
        for (int i = 0; i < nums.length; i++) {
            // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) {
                return result;
            }
            if (i > 0 && nums[i] == nums[i - 1]) { // 去重a
                continue;// 检测到之前已经出现，直接出循环
            }
            int left = i + 1;
            int right = nums.length - 1;
            while (right > left) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (right > left && nums[right] == nums[right - 1])
                        right--;// 对right进行去重
                    while (right > left && nums[left] == nums[left + 1])
                        left++;

                    right--;// 继续往下走
                    left++;
                }
            }
        }
        return result;
    }
}
```

https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html



### **18. 四数之和** 

三数之和的升级版

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0 && nums[i] > target) {
                return result; // 这一步是剪枝操作，如果出现，后面的就不用再看了，返回当前结果
            }
            if (i > 0 && nums[i - 1] == nums[i]) { // 对nums[i]去重，第一次去重
                continue;
            }
            for (int j = i + 1; j < nums.length; j++) {
                if (j > i + 1 && nums[j - 1] == nums[j]) {
                    continue; // 对nums[j]去重，第二次去重
                }
                int left = j + 1;
                int right = nums.length - 1;
                while (right > left) {
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];// 防止溢出
                    if (sum > target) {
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while (right > left && nums[right] == nums[right - 1])
                            right--;
                        while (right > left && nums[left] == nums[left + 1])
                            left++;

                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html
