# **第四章 字符串**part02

### **151.翻转字符串里的单词** 

这道题需要掌握一下java中的StringBuilder的用法。

```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = removeExtraSpaces(s);//先去了空格
        reverseString(sb, 0, sb.length() - 1);// 2.反转整个字符串
        reverseEachWord(sb);// 3.反转各个单词
        return sb.toString();
    }

    public StringBuilder removeExtraSpaces(String str) {
        char[] s = str.toCharArray();
        int start = 0;
        int end = s.length - 1;
        while (s[start] == ' ')
            start++;
        while (s[end] == ' ')
            end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            if(s[start] != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                //这里判断在什么时候加入空格的非常巧妙，当结束位置有了一个空格就不再append了
                sb.append(s[start]);   
            }
            start++;
        }
        return sb;
    }

    public void reverseString(StringBuilder sb, int start, int end){
        while(start < end){
            char temp = sb.charAt(start);
            //setCharAt(int index,Char ch),第一个参数是取代的位置 索引从0开始 第二个参数是你要替换为的字符串
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;end--;
        }
    }

    private void reverseEachWord(StringBuilder sb){
        //先进行全部的翻转，然后对每个单词进行翻转，再正过来
        int start = 0;
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while(end < n && sb.charAt(end) != ' '){
                end++;
            }
            reverseString(sb,start,end-1);//在
            start = end+1;//到了下一个单词
            end = start+1;//从start的下一个进行搜索
        }
    }

}
```

https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html



### **卡码网：55.右旋转字符串** 

nice，没看题解自己做出来哩，思路和题解是一样的。

```java
import java.util.*;
public class test4 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int k = sc.nextInt();
        String s = sc.next();
        //思路是先全部翻转，之后根据k分两段，每段分别翻转
        String s1 = reversefunction(s,0, s.length()-1);
        String s2 = reversefunction(s1,0,k-1);
        String res = reversefunction(s2,k,s2.length()-1);
        System.out.println(res);
    }
    public static String reversefunction(String s, int start, int end){
        StringBuilder sb = new StringBuilder(s);
        while(start < end){
            char temp =  sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        return sb.toString();
    }
}
```

但是java字符串的一些工具需要自己好好整理一下。

https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html



后面两道拓展，明天休息日听一听，二刷的时候再来啃。