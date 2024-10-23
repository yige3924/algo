# Day8 第四章 字符串 part01

> 资料来源地址：[代码随想录 (programmercarl.com)](https://programmercarl.com/数组理论基础.html)

## 典型例题

### LeetCode344.反转字符串

> ==双指针==

**如果题目关键的部分直接用库函数就可以解决，建议不要使用库函数。**

毕竟面试官一定不是考察你对库函数的熟悉程度， 如果使用python和java 的同学更需要注意这一点，因为python、java提供的库函数十分丰富。

**如果库函数仅仅是 解题过程中的一小部分，并且你已经很清楚这个库函数的内部实现原理的话，可以考虑使用库函数。**

对于字符串，我们定义两个指针（也可以说是索引下标），一个从字符串前面，一个从字符串后面，两个指针同时向中间移动，并交换元素。

```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
            s[l] ^= s[r];  //构造 a ^ b 的结果，并放在 a 中
            s[r] ^= s[l];  //将 a ^ b 这一结果再 ^ b ，存入b中，此时 b = a, a = a ^ b
            s[l] ^= s[r];  //a ^ b 的结果再 ^ a ，存入 a 中，此时 b = a, a = b 完成交换
            l++;
            r--;
        }
    }
}

// 第二种方法用temp来交换数值更多人容易理解些
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while(l < r){
            char temp = s[l];
            s[l] = s[r];
            s[r] = temp;
            l++;
            r--;
        }
    }
}
```



### LeetCode541.反转字符串II

题目的意思其实概括为 每隔2k个反转前k个，尾数不够k个时候全部反转

```java
//解法一
class Solution {
    public String reverseStr(String s, int k) {
        StringBuffer res = new StringBuffer();
        int length = s.length();
        int start = 0;
        while (start < length) {
            // 找到k处和2k处
            StringBuffer temp = new StringBuffer();
            // 与length进行判断，如果大于length了，那就将其置为length
            int firstK = (start + k > length) ? length : start + k;
            int secondK = (start + (2 * k) > length) ? length : start + (2 * k);

            //无论start所处位置，至少会反转一次
            temp.append(s.substring(start, firstK));
            res.append(temp.reverse());

            // 如果firstK到secondK之间有元素，这些元素直接放入res里即可。
            if (firstK < secondK) { //此时剩余长度一定大于k。
                res.append(s.substring(firstK, secondK));
            }
            start += (2 * k);
        }
        return res.toString();
    }
}

//解法二（似乎更容易理解点）
//题目的意思其实概括为 每隔2k个反转前k个，尾数不够k个时候全部反转
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0; i < ch.length; i += 2 * k){
            int start = i;
            //这里是判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1, start + k - 1);
            //用异或运算反转 
            while(start < end){
                ch[start] ^= ch[end];
                ch[end] ^= ch[start];
                ch[start] ^= ch[end];
                start++;
                end--;
            }
        }
        return new String(ch);
    }
}


// 解法二还可以用temp来交换数值，会的人更多些
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0;i < ch.length;i += 2 * k){
            int start = i;
            // 判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1,start + k - 1);
            while(start < end){
                
                char temp = ch[start];
                ch[start] = ch[end];
                ch[end] = temp;

                start++;
                end--;
            }
        }
        return new String(ch);
    }
}

// 解法3
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        // 1. 每隔 2k 个字符的前 k 个字符进行反转
        for (int i = 0; i< ch.length; i += 2 * k) {
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= ch.length) {
                reverse(ch, i, i + k -1);
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转
            reverse(ch, i, ch.length - 1);
        }
        return  new String(ch);

    }
    // 定义翻转函数
    public void reverse(char[] ch, int i, int j) {
    	for (; i < j; i++, j--) {
            char temp  = ch[i];
            ch[i] = ch[j];
            ch[j] = temp;
    	}

    }
}
```



### 替换数字

使用Java刷题的录友，一定要使用辅助空间，因为Java里的string不能修改

```java
import java.util.Scanner;

public class Main {
    
    public static String replaceNumber(String s) {
        int count = 0; // 统计数字的个数
        int sOldSize = s.length();
        for (int i = 0; i < s.length(); i++) {
            if(Character.isDigit(s.charAt(i))){
                count++;
            }
        }
        // 扩充字符串s的大小，也就是每个空格替换成"number"之后的大小
        char[] newS = new char[s.length() + count * 5];
        int sNewSize = newS.length;
        // 将旧字符串的内容填入新数组
        System.arraycopy(s.toCharArray(), 0, newS, 0, sOldSize);
        // 从后先前将空格替换为"number"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i; j--, i--) {
            if (!Character.isDigit(newS[j])) {
                newS[i] = newS[j];
            } else {
                newS[i] = 'r';
                newS[i - 1] = 'e';
                newS[i - 2] = 'b';
                newS[i - 3] = 'm';
                newS[i - 4] = 'u';
                newS[i - 5] = 'n';
                i -= 5;
            }
        }
        return new String(newS);
    };
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String s = scanner.next();
        System.out.println(replaceNumber(s));
        scanner.close();
    }
}

// 为了还原题目本意，先把原数组复制到扩展长度后的新数组，然后不再使用原数组、原地对新数组进行操作。
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();
        int len = s.length();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= 0 && s.charAt(i) <= '9') {
                len += 5;
            }
        }
        
        char[] ret = new char[len];
        for (int i = 0; i < s.length(); i++) {
            ret[i] = s.charAt(i);
        }
        for (int i = s.length() - 1, j = len - 1; i >= 0; i--) {
            if ('0' <= ret[i] && ret[i] <= '9') {
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

