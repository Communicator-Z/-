# **第三章 哈希表**part01

### **哈希表理论基础** 

哈希表是通过哈希函数进行映射，映射成哈希表索引，然后在查询的时候直接根据索引去哈希表中获取索引对应的元素即可。

java中的哈希表相关：

`HashSet` 是 Java 中的一个集合类，它实现了 `Set` 接口，用于存储一组唯一的元素。HashSet 是基于哈希表实现的，它不保证元素的顺序，而且不允许存储重复的元素。

以下是 HashSet 的主要特点和用法：

1. 存储唯一元素：HashSet 中不允许存储重复的元素，如果试图添加重复的元素，HashSet 不会执行添加操作，也不会抛出异常，只是保留集合中的唯一元素。
2. 基于哈希表实现：HashSet 使用哈希表作为其内部存储结构，哈希表通过哈希函数将元素分散存储在内部的桶中。这使得在 HashSet 中执行添加、删除、查找等操作的平均时间复杂度为 O(1)。
3. 不保证元素顺序：HashSet 不保证元素的顺序，即元素在内部存储时不按照添加顺序进行排列。如果您需要按照特定顺序访问元素，可以考虑使用 `LinkedHashSet`，它保留了元素的插入顺序。
4. 线程不安全：HashSet 不是线程安全的，如果多个线程同时访问 HashSet，并且至少有一个线程修改了集合的结构（例如添加或删除元素），则必须在外部进行同步处理。
5. 允许 null 元素：HashSet 允许存储一个 null 元素。

**定义：**

```java
import java.util.HashSet;
public class Main {
    public static void main(String[] args) {
        HashSet<String> sites = new HashSet<String>(20);    
    }
}
```

**一些方法：**

```java
import java.util.HashSet;
import java.util.Iterator;
public class Main {
    public static void main(String[] args) {
        Set<Integer> set1 = new HashSet<>();
        sites.add("Google");
        sites.add("Runoob");
        sites.add("Runoob");  //重复的元素不会被添加
        System.out.println(sites.size());//size，返回尺寸
        for (String e : sites) {
            System.out.println(e);// 对hash进行遍历
        }
        System.out.println(sites.contains("Taobao"));//判断是否包含，返回ture
        sites.remove("Taobao");// 进行删除
        sites.clear();//清空所有元素
        System.out.println(sites.isEmpty());//判断是否为空
    }
}
```

一些常识——java中for (int i : arr) 的含义

```java
public class Apptest {
    public static void main(String[] args) {
        int[] arr = {4,5,6,7}; 
        /**
         * int : 指定数组元素的类型
         * i   : 给你需要遍历的数组指定的名字
         * arr : 你需要遍历的数组
         */
        for (int i : arr){
            System.out.println(" 数组元素 : " + i);//注意这里的i是arr[i]！！！！
        }
    }
}
//_______________________________________等价于

public class Apptest {
    public static void main(String[] args) {
        int[] arr = {4,5,6,7};
        
        for (int i = 0; i < arr.length; i++){
            System.out.println(" 数组元素 : " + arr[i]);//这里的i只是序号
        }
    }
}
```



### **242.有效的字母异位词** 

这道题注意用charAt-'a'的操作，因为所有的字母的ASKII码都是按顺序的。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] arr = new int[26];
        for(int i = 0; i < s.length(); i++){
            arr[s.charAt(i) - 'a']++;
        }
        for(int i = 0; i < t.length(); i++){
            arr[t.charAt(i) - 'a']--;
        }
        for(int i = 0; i < arr.length; i++){
            if(arr[i]!=0){
                return false;
            }
        }
        return true;
    }
}
```

https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html



### **349. 两个数组的交集** 

首先是哈希set解法：

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();
        for (int i : nums1) {
            set1.add(i);
        }
        for (int i : nums2) {
            if(set1.contains(i)){
                set2.add(i);
            }
        }
        int[]arr = new int[set2.size()];
        int j = 0;
        for(int i : set2){  //这种循环是把set2中i的值一一对应的赋值给arr
            arr[j++] = i; 
        }
        return arr;
    }
}
```

然后数组解法：

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
       int[] arr1 = new int[1005];
       int[] arr2 = new int[1005];
       for(int i:nums1){
        arr1[i]++;
       }
        for(int i:nums2){
        arr2[i]++;
       }
       ArrayList<Integer> reslist = new ArrayList<>();//数组链表，可以动态扩容
       for(int i = 0; i < 1005; i++){
        if(arr1[i] != 0 && arr2[i] != 0){
            reslist.add(i);
        }
       }
       int index = 0;
       int res[] = new int[reslist.size()];
       for(int i : reslist){
        res[index++] = i;
       }
       return res;
    }   
}
```

https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html



### **202. 快乐数**

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while(n!=1 && !record.contains(n)){
            record.add(n);
            n = getnextNumber(n);
        }
        return n==1;
    }
    public int getnextNumber(int n){
        int res = 0;
        while(n > 0){
            int m = n%10;
            res += m*m;
            n = n/10;
        }
        return res;
    }
}
```

https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html



### **1. 两数之和** 

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        if (nums == null || nums.length == 0) {
            return res;
        }
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            int temp = target - nums[i];
            if(map.containsKey(temp)){
                res[1] = i;
                res[0] = map.get(temp);
                break;
            }
            map.put(nums[i],i);
        }
        return res;
    }
}
```

https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html
