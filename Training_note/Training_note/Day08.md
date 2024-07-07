# **第四章 字符串**part01

### **344.反转字符串**

这道题注意在for循环判断中，已经i++了，所以循环体中只需要j--即可。

```java
class Solution {
    public void reverseString(char[] s) {
        int len = s.length;
        for(int i = 0, j = len-1; i < len/2; i++){
            char temp = s[i];
            s[i] = s[j];
            s[j] = temp;
            j--;
        }
    }
}
```

https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html



### **541. 反转字符串II**

记得写for循环的时候不要固化思维，直接一段一段的跳也是可以的。

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0; i < ch.length; i += 2*k){//这里i可以直接加2k,不要固化思维
            if(i+k <= ch.length){
                ch = reverse(ch, i, i+k-1);
                continue;//这里翻转完了直接进入下一个循环,这里包含了两种翻转完整k的情况
            }else{
                ch = reverse(ch, i, ch.length-1);
            }
        }
        return new String(ch);
    }
    public char[] reverse(char[] ch, int i , int j){//翻转函数
        for(;i < j; i++,j--){
            char temp  = ch[i];
            ch[i] = ch[j];
            ch[j] = temp;
        }
        return ch;
    }
}
```

https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html



### **卡码网：54.替换数字**

对于线性数据结构，填充或者删除，后序处理确实会高效的多。先进行扩容，然后从后面一个一个往前补，不然如果从前往后补的话，每补一位后面的还得整体后移。

```java
import java.util.*;
public class Main {
public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        int len = s.length();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                len += 5;//数组扩容
            }
        }
        char[] ret = new char[len];
        for (int i = 0; i < s.length(); i++) {
            ret[i] = s.charAt(i);
        }
        for (int i = s.length() - 1, j = len - 1; i >= 0; i--) {
            if (ret[i] >= '0' && ret[i] <= '9') {
                ret[j--] = 'r';
                ret[j--] = 'e';
                ret[j--] = 'b';
                ret[j--] = 'm';
                ret[j--] = 'u';
                ret[j--] = 'n';
            } else {
                ret[j--] = ret[i];
            }
        }
        System.out.println(ret);
    }
}
```

https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html