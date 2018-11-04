## 1.二位数组中的查找
```
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```

* 主要是一个数组结构的问题，从右上角开始搜索，然后慢慢收敛就可以了。
 
```java
public class Solution {
        public boolean Find(int target, int [][] array) {
        int raw = array.length;
        int clu = array[0].length;
        int i = 0;
        int j = clu - 1;
        while(i<raw && j>=0){
                if(array[i][j]>target){
                    j--;
                    continue;
                }
                if(array[i][j]<target){
                    i++;
                    continue;
                }
                if(array[i][j]==target)
                    return true;
            }
        return false;
    }
}
```

## 2.替换空格
```
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```

* 先计算好替换后的字符串长度，然后申请空间，再从申请好的空间的后面开始补充即可。
* 可以用正则表达式 **replaceAll** 直接替换。

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        int count = 0;
        for(int i=0; i<str.length(); i++)
            if(str.charAt(i) == ' ')
                count ++;
        int oldLen = str.length();
        int newLen = oldLen + count*2;
        str.setLength(newLen);
        newLen--;
        oldLen--;
        while(oldLen>=0){
            if(str.charAt(oldLen) == ' ') {
                str.setCharAt(newLen--, '0');
                str.setCharAt(newLen--, '2');
                str.setCharAt(newLen--, '%');
            } else {
                str.setCharAt(newLen--, str.charAt(oldLen));
            }
            oldLen--;
        }
        return str.toString();
    }
}
```

## 3.从尾到头打印链表
```
输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
```

* 可以用入栈的方式来打印链表，但是递归本来就是一个栈调用的结构，因此可以用递归来实现。
* 也可以直接使用现成的数据结构来实现。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
public class Solution {
    ArrayList<Integer> ay = new ArrayList<>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode!=null){
            this.printListFromTailToHead(listNode.next);
            ay.add(listNode.val);
        }
        return ay;
    }
}
```

## 4.重建二叉树
```java
输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
```



