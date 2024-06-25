# 第二章 链表part02

### **24. 两两交换链表中的节点** 

链表一般报错是容易出现空指针异常，经过我滴测试，只要确保有一个null就行，不能null之后还有指针，这样就是空指针了。

两两交换中，需要注意先后顺序。要先存好temp，否则找不到了。

另外，尽量使用一下虚拟节点，真滴可以避免很多特殊处理头结点的操作。

代码如下：

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1,head);
        ListNode cur = dummy;
        ListNode temp;ListNode temp1;
        while(cur.next!=null && cur.next.next!=null){
            temp = cur.next;
            temp1 = cur.next.next.next;
            cur.next = cur.next.next;
            cur.next.next = temp;
            temp.next = temp1;
            cur = cur.next.next;
        }
        return dummy.next;
    }
}
```

[https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0024.两两交换链表中的节点.html)



### 19.删除链表的倒数第N个节点  

如何寻找倒数第n个节点，采用快慢指针方法。

为了删除第N个节点，那么需要定位到前一个，所以快指针多移动一位，移动N+1步。然后快慢指针同时移动。这样快指针到达null时，慢指针正好在第N个节点的前一个。

代码如下：

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyhead = new ListNode(-1, head);
        ListNode fast = dummyhead;
        ListNode slow = dummyhead;
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        if (slow.next != null) {
            slow.next = slow.next.next;//防止空指针异常
        }
        return dummyhead.next;
    }
}
```

[https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html](https://programmercarl.com/0019.删除链表的倒数第N个节点.html)



### **面试题 02.07. 链表相交**  

这道题的思路值得思考，先进行尾对齐，然后一个一个的比较。先贴一份我自己写的代码，虽然通过了，但看上去是有一些冗余的。

但是这里就能看出来设置虚拟节点的用处，因为如果不设置虚拟节点，会发现进行头结点判断时，特别的复杂，而设置了之后，就可以直接套过来继续判断。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int sizeA = 0;
        int sizeB = 0;
        ListNode dummynodeA = new ListNode(-1,headA);
        ListNode dummynodeB = new ListNode(-1,headB);
        ListNode curA = dummynodeA;
        ListNode curA1 = dummynodeA;
        ListNode curB = dummynodeB;
        ListNode curB1 = dummynodeB;
        while (curA != null) {
            curA = curA.next;
            sizeA++;
        }
        while (curB != null) {
            curB = curB.next;
            sizeB++;
        }
        int k = 0;
        if (sizeA > sizeB) {
            k = sizeA - sizeB;
            for (int i = 0; i < k; i++) {
                curA1 = curA1.next;
            }
            while (curA1 != curB1) {
                curA1 = curA1.next;
                curB1 = curB1.next;
                if (curA1 == curB1) {
                    return curA1;
                }
            }
            return null;
        } else if (sizeA < sizeB) {
            k = sizeB - sizeA;
            for (int i = 0; i < k; i++) {
                curB1 = curB1.next;
            }
            while (curA1 != curB1) {
                curA1 = curA1.next;
                curB1 = curB1.next;
                if (curA1 == curB1) {
                    return curA1;
                }
            }
            return null;
        } else {
            while (curA1 != curB1) {    //头结点的作用
                //其实这么写是不太对的，判断的标准应该是指针不为空，而不是上来就要进行比较
                //看下一个官方题解
                curA1 = curA1.next;
                curB1 = curB1.next;
                if (curA1 == curB1) {
                    return curA1;
                }
            }
            return null;
        }
    }
}
```

修改之后的：

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int sizeA = 0;
        int sizeB = 0;
        ListNode curA = headA;
        ListNode curB = headB;
        while (curA != null) {
            sizeA++;// 这里这么写更合理，先++，再next
            curA = curA.next;
        }
        while (curB != null) {
            sizeB++;
            curB = curB.next;
        }
        curA = headA;// 这里从新更新cur，避免了定义全新的指针造成代码冗余
        curB = headB;
        // 这里进行的操作是保证curA为最长链表的头，lenA为其长度，省了接下来不停地判断
        if (sizeB > sizeA) {// 如果不能保证，就进行替换
            int temp = 0;
            ListNode tempL;
            temp = sizeB;// 尺寸互换
            sizeB = sizeA;
            sizeA = temp;
            tempL = curB;// 节点互换
            curB = curA;
            curA = tempL;
        } // 现在已经换好了，此时curA最长。
        int gap = sizeA - sizeB;
        for (int i = 0; i < gap; i++) {
            curA = curA.next;
        }
        while (curA != null) {//这个判断才对，用空不空来控制循环
            if (curA == curB) {
                return curA;
            }
            curA = curA.next;
            curB = curB.next;
        }
        return null;
    }
}
```

[https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html](https://programmercarl.com/面试题02.07.链表相交.html)



### **142.环形链表II** 

本题的关键就是，用快慢指针，快指针二倍速，去追慢指针。追上了，就是有环的。没追上没有环儿。

找入环的节点的方法是数学推理，推出来，会发现如果两个指针，同时从链表头和相遇点出发，不管转几圈，第一次相遇时，一定就是环的入口。（这个一定要多思考，不容易绕过来）

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next!= null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                ListNode index1 = fast;
                ListNode index2 = head;
                while(index1 != index2){
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;//注意最后return的时候，当循环体里没有return时，就执行这个，所以这么写再循环体外						//面没有问题
    }
}
```

[https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html](https://programmercarl.com/0142.环形链表II.html)



对于链表的题目，大家最大的困惑可能就是 什么使用用虚拟头结点，什么时候不用虚拟头结点？ 

一般涉及到 增删改操作，用虚拟头结点都会方便很多， 如果只能查的话，用不用虚拟头结点都差不多。 

当然大家也可以为了方便记忆，统一都用虚拟头结点。