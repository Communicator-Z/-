# **第二章 链表**part01

### 链表理论基础

链表每个节点由两部分组成，一个是数据域，另一个是指针域。

链表的入口节点是head，最后一个节点的指针指向null，即空指针。

- 链表类型

​	单链表就是最单纯的链表；

​	双链表是一个节点有两个指针，一个指向下一个节点，一个指向上一个节点；

​	循环链表是指链表首尾相连。

手写一个链表，使用有参构造（this的使用以及方法名与类名保持相同），这样在初始化时候，可以直接赋值。代码如下：

```java
public class ListNode {
    int val;
    ListNode next;
    public ListNode(int val,ListNode next){
        this.val = val;
        this.next = next;
    }
}
```

就是不需要再去“.”了：

```java
//直接调用,像这样，而不需要再去listnode.val = val1; listnode next = ~;
ListNode listnode = new ListNode(val,next)
```

### 203.移除链表元素

设置虚拟头结点，需要注意的是要用到curr和pre，每次循环更新pre和curr。

代码如下：

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if(head==null){
            return head;
        }
        ListNode dummy = new ListNode(-1,head);
        ListNode curr = head;
        ListNode pre = dummy;
        while(curr!=null){
            if(curr.val==val){
                pre.next = curr.next;
            }else{
                pre = curr;//pre后移
            }
            curr = curr.next;//curr后移
        }
        return dummy.next;//不返回head是因为head有可能已经被删除了
    }
}
```

### **707.设计链表** 

```java
 class ListNode {
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val) {
        this.val=val;
    }
}
class MyLinkedList {
    //size存储链表元素的个数
    int size;
    //虚拟头结点
    ListNode head;

    //初始化链表
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    //获取第index个节点的数值，注意index是从0开始的，第0个节点就是头结点
    public int get(int index) {
        //如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode currentNode = head;
        //包含一个虚拟头节点，所以查找第 index+1 个节点
        for (int i = 0; i <= index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }

    //在链表最前面插入一个节点，等价于在第0个元素前添加
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    //在链表的最后插入一个节点，等价于在(末尾+1)个元素前添加
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    // 在第 index 个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果 index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果 index 大于链表的长度，则返回空
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index < 0) {
            index = 0;
        }
        size++;
        //找到要插入节点的前驱
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    //删除第index个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        if (index == 0) {
            head = head.next;
	    return;
        }
        ListNode pred = head;
        for (int i = 0; i < index ; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}

```

### **206.反转链表** 

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur!=null){
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
        
    }
}
```

