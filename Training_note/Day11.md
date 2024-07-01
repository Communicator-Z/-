



# 第五章 栈与队列part01

### **理论基础** 

了解一下栈与队列的内部实现机制，以及java相关语法。

**栈**

```java
public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(12);//入栈
        stack.push(34);
        stack.push(15);
        stack.push(56);
        
        Integer m = stack.pop();//出栈
        System.out.println(m);
 
        System.out.println(stack.size());//获取个数
        
        Integer x = stack.peek();//获取栈顶元素但是不删除
        System.out.println(x);
        
        boolean flg = stack.empty();//判断是否为空
        boolean flg2 = stack.isEmpty();//isEmpty是vector的方法，这俩判断是否为空的方法没有明显区别！！！
    }
```

​	栈是一种仅支持在表尾进行插入和删除操作的线性表，这一端被称为栈顶，另一端被称为栈底。元素入栈指的是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；元素出栈指的是从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。栈中的元素遵守后进先出（LIFO)的原则.
​	用链表实现栈非常容易。还可以用顺序表实现。

```java
 public static void main(String[] args) {
        LinkedList<Integer> stack = new LinkedList<>();
        stack.push(10);
        
        Deque<Integer> stack2 = new LinkedList<>();
        stack2.push(1);
    }
```

**队列**

只允许在一端进行插入数据操作，在另一端进行删除数据操作的特殊线性表，队列特点先进先出

```java
public static void main(String[] args) {
        Queue<Integer> q = new LinkedList<>();
        q.offer(1);//尾插法
        q.offer(2);
        q.offer(3);
        q.offer(4);
        System.out.println(q.size());
        System.out.println(q.peek()); //获取队头元素
        q.poll();//出队，头删
        System.out.println(q.poll()); //从队头出队列，并将删除的元素返回
        if(q.isEmpty()){
            System.out.println("队列空");
         }else{
            System.out.println(q.size());
        }
    }
```

###  **232.用栈实现队列** 

这个最好写一个dumpstackIn()函数，否则调来掉去的会乱掉。

```java
class MyQueue {

    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

    /** Initialize your data structure here. */
    public MyQueue() {
        stackIn = new Stack<>(); // 负责进栈
        stackOut = new Stack<>(); // 负责出栈
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stackIn.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {    
        dumpstackIn();
        return stackOut.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        dumpstackIn();
        return stackOut.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stackIn.isEmpty() && stackOut.isEmpty();
    }

    // 如果stackOut为空，那么将stackIn中的元素全部放到stackOut中
    private void dumpstackIn(){
        if (!stackOut.isEmpty()) return; 
        while (!stackIn.isEmpty()){
                stackOut.push(stackIn.pop());
        }
    }
}
```

https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html



### **225. 用队列实现栈**

这里有一个问题卡了很久，就是top的时候为什么不能直接peek？当然是不能的，因为peek了之后，顺序是乱的。就是还处于翻转的状态。所以需要poll了之后，再塞回去，保持顺序是不变的。

```java
class MyStack {
    Queue<Integer> queue;

    public MyStack() {
        queue = new LinkedList<>();
    }

    public void push(int x) {
        queue.add(x);
    }

    public int pop() {
        rePosition();
        return queue.poll();
    }

    public int top() {
        rePosition();
        int res = queue.poll();
        queue.add(res);
        return res;
    }

    public boolean empty() {
        return queue.isEmpty();
    }

    public void rePosition() {
        int size = queue.size();
        while (size - 1 > 0) {
            queue.add(queue.poll());
            size--;
        }
    }
}
```

https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html



### **20. 有效的括号**

这道题分了三种情况，尤其是最后的判断，非常巧妙，好好体会。

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();
        if (s.length()%2 != 0) return false;
        char ch;
        for(int i = 0; i < s.length(); i++){
            ch = s.charAt(i);
            if(ch == '('){
                st.push(')');
            }else if(ch == '{'){
                st.push('}');
            }else if(ch == '['){
                st.push(']');
            }else if(st.isEmpty() || ch != st.peek()){
                return false;//情况2和3都在这里判断
            }else{
                st.pop();
            }
        }
        return st.isEmpty();//情况1在这里判断的，很巧妙。
    }
}
```

https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html



### **1047. 删除字符串中的所有相邻重复项**

对字符串的应用还不太熟练。

```java
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> ch = new Stack<>();
        char[] s1 = s.toCharArray();
        for(char c:s1){
            if(ch.isEmpty() || ch.peek() != c){
                ch.push(c);
            }else{
                ch.pop();
            }
        }
        String str = "";
        hile(!ch.isEmpty()){
            str += ch.pop();
        }
        StringBuilder res = new StringBuilder(str);
        return res.reverse().toString();
    }
}
```

这里对最后的处理不同，字符串倒着加可以避免一步翻转。

```java
while(!ch.isEmpty()){
            str = ch.pop() + str;
        }
        return str;
```

https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html