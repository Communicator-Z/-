# **第五章 栈与队列**part02

### **150. 逆波兰表达式求值**

还是字符串的一些操作，字符串数组，String[ ]，Char[ ]这些之间的区别，需要总结。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> st = new Stack<>();
        int res = 0;
        for(String s:tokens){
            //这里的判断语句由于力扣的问题，只能用equals()来做。
            if("+".equals(s) ||"-".equals(s) ||"*".equals(s) ||"/".equals(s)){
                int num1 = st.pop();
                int num2 = st.pop();
                if("+".equals(s)){
                    res = num1+num2;
                    st.push(res);
                    }else if("-".equals(s)){
                        res = num2-num1;
                        st.push(res);
                    }else if("*".equals(s)){
                        res = num2*num1;
                        st.push(res);
                    }else{
                        res = num2/num1;
                        st.push(res);
                    }
            }else{
                int n = Integer.valueOf(s);//这里一步字符转数字
                st.push(n);
            }
        }
        int result = st.pop();
        return result;
    }
}
```

https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html



### **239. 滑动窗口最大值 （有点难度，可能代码写不出来，但一刷**至少需要理解思路）

确实难嗷，等二刷。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 1) {
            return nums;
        }
        int len = nums.length - k + 1;
        // 存放结果元素的数组
        int[] res = new int[len];
        int num = 0;
        MyQueue myQueue = new MyQueue();
        for (int i = 0; i < k; i++) {
            myQueue.add(nums[i]);// 先将前k的元素放入队列

        }
        res[num++] = myQueue.peek();//和下面等价操作，就是先出一个第一个区间的结果。
        // res[num] = myQueue.peek();
        // num = num+1;
        for (int i = k; i < nums.length; i++) {
            // 滑动窗口移除最前面的元素，移除是判断该元素是否放入队列
            myQueue.poll(nums[i - k]);
            // 滑动窗口加入最后面的元素
            myQueue.add(nums[i]);
            // 记录对应的最大值
            res[num++] = myQueue.peek();
        }
        return res;
    }
}

class MyQueue {
    Deque<Integer> deque = new LinkedList<>();// Deque（java.util.Deque）接口代表着双向队列，意思就是可以从队列的两端增加或者删除元素，Deque就是双向Queue的意思。
    // 弹出元素时，比较当前要弹出的数值是否等于队列出口的数值，如果相等则弹出
    // 同时判断队列当前是否为空

    void poll(int val) {
        if (!deque.isEmpty() && val == deque.peek()) {
            deque.poll();
        }
    }

    // 添加元素时，如果要添加的元素大于入口处的元素，就将入口元素弹出
    // 保证队列元素单调递减
    // 比如此时队列元素3,1，2将要入队，比1大，所以1弹出，此时队列：3,2
    void add(int val) {
        while (!deque.isEmpty() && val > deque.getLast()) {
            deque.removeLast();// 这里是对最后一个进行判断
        }
        deque.add(val);
    }

    int peek() {
        return deque.peek();
    }
}
```

https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html



### **347.前 K 个高频元素  （有点难度，可能代码写不出来，一刷至少需要理解思路）**

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>(); // key为数组元素值,val为对应出现次数
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);// 这里在统计次数
        }
        // 出现次数按从队头到队尾的顺序是从小到大排,出现次数最低的在队头(相当于小顶堆)
        PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2) -> pair1[1] - pair2[1]);// 这里是在定义小顶堆
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {// https://blog.csdn.net/weixin_44626980/article/details/134921438
            if (pq.size() < k) { // 小顶堆元素个数小于k个时直接加
                pq.add(new int[] { entry.getKey(), entry.getValue() });
            } else {
                if (entry.getValue() > pq.peek()[1]) { // 当前元素出现次数大于小顶堆的根结点(这k个元素中出现次数最少的那个)
                    pq.poll(); // 弹出队头(小顶堆的根结点),即把堆里出现次数最少的那个删除,留下的就是出现次数多的了
                    pq.add(new int[] { entry.getKey(), entry.getValue() });
                }
            }

        }
        int[] ans = new int[k];
        for (int i = k - 1; i >= 0; i--) { // 依次弹出小顶堆,先弹出的是堆的根,出现次数少,后面弹出的出现次数多
            ans[i] = pq.poll()[0];//pq.poll()[0] 是从小顶堆中取出并返回最小元素，然后返回该元素。
        }
        return ans;
    }
```

[代码随想录 (programmercarl.com)](https://programmercarl.com/0347.前K个高频元素.html#思路)