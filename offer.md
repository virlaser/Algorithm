## 1.二位数组中的查找

>在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。


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
>请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

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
>输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

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
>输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

* 主要是一个递归的过程，通过前序遍历和中序遍历得到的序列找到左子树和右子树在数组中的坐标然后递归重建二叉树就好了，主要是左右子树在数组中的坐标判断。

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {

public TreeNode reConstructBinaryTree(int [] pre, int [] in) {
        return reConstructBinaryTree(pre, 0, pre.length-1, in, 0, in.length-1);
    }

    private TreeNode reConstructBinaryTree(int[] pre, int preStart, int preEnd, int[] in, int inStart, int inEnd) {
        if(preStart>preEnd || inStart>inEnd)
            return null;

        TreeNode head = new TreeNode(pre[preStart]);

        for(int i=inStart; i<=inEnd; i++){
            if(in[i] == pre[preStart]) {
                head.left = reConstructBinaryTree(pre, preStart+1, preStart+i-inStart, in, inStart, i-1);
                head.right = reConstructBinaryTree(pre, i-inStart+preStart+1, preEnd, in, i+1, inEnd);
                break;
            }
        }
        return head;
    }
}
```

# 5.用两个栈实现队列
>用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

* 模仿队列的入队和出队，每次进入一个元素就压入一个栈里面，然后栈里面的元素全部出栈压入另一个栈即可。

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        if(stack2.isEmpty()) {
            stack1.push(node);
        } else {
            while(!stack2.isEmpty()) {
                stack1.push(stack2.pop());
            }
            stack1.push(node);
        }

    }

    public int pop() {
        if(stack1.isEmpty()) {
            return stack2.pop();
        } else {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
    }
}
```

# 6.旋转数组的最小数字
>把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

* 要是一个个比较也可以。
* 使用二分法会快一些，主要是判断边界条件。

```java
import java.util.ArrayList;
public class Solution {
public int minNumberInRotateArray(int [] array) {
        if(array.length == 0)
            return 0;

        int temp1 = array.length-1;
        int temp2 = 0;
        int pos = temp2 + (temp1-temp2)/2;

        while(temp2<temp1 && !(array[pos]>array[0] && array[pos+1]<array[0])) {
            if(array[pos] > array[0]) {
                temp2 = pos;
                pos = temp2 + (temp1-temp2)/2;
            } else {
                temp1 = pos;
                pos = temp2 + (temp1-temp2)/2;
            }
            if(temp2+1 == temp1)
                return array[0];
        }
        return array[pos+1];
    }
}
```

# 7.斐波那契数列
>大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39

* 可以使用递归，但是 n 太大了会爆栈。
* 尾递归优化不咋地。
* 使用普通的循环。

```java
public class Solution {
    public int Fibonacci(int n) {
        int temp = 1;
        int temp2 = 1;
        int temp3 = 2;
        if(n == 0)
            return 0;
        if(n == 1 || n == 2)
            return 1;
        for(int i=2; i<n; i++) {
            temp3 = temp+temp2;
            temp = temp2;
            temp2 =temp3;
        }
        return temp3;
    }
}
```

# 7.跳台阶
>一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

* 还是把问题分解为小的问题，然后使用动态规划解决。
* 可以使用递归，但是为了避免重复的递归计算就使用了普通循环，还可以将递归计算出的状态保存，这样也可以提高计算速度。

```java
public class Solution {
    public int JumpFloor(int target) {
        if(target == 0) {
            return 0;
        }
        if(target == 1) {
            return 1;
        }
        if(target == 2) {
            return 2;
        }
        int o1 = 1;
        int o2 = 2;
        int temp = 3;
        for(int i=2; i<target; i++) {
            temp = o1 + o2;
            o1 = o2;
            o2 = temp;
        }
        return temp;
    }
}
```

# 8.变态跳台阶
>一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

* 还是使用动态规划，使用一个求和函数将之前的情况求和即可。

```java
public class Solution {
    private static int[] index;
    public int JumpFloorII(int target) {
        index = new int[target];
        if(target == 0){
            return 0;
        }
        if(target == 1) {
            return 1;
        }
        index[0] = 1;
        index[1] = 2;
        for(int i=2; i<target; i++) {
            index[i] = this.sum(i) + 1;
        }
        return index[target-1];
    }

    private int sum(int dex) {
        int result = 0;
        for(int i=0; i<dex; i++) {
            result += index[i];
        }
        return result;
    }
}
```

# 9.矩形覆盖
>我们可以用2\*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2\*1的小矩形无重叠地覆盖一个2\*n的大矩形，总共有多少种方法？

* 动态规划

```java
public class Solution {
    public int RectCover(int target) {
        if(target == 0)
            return 0;
        if(target == 1)
            return 1;
        if(target == 2)
            return 2;
        int temp = 0;
        int o1 = 1;
        int o2 = 2;
        for(int i = 2; i<target; i++) {
            temp = o1 + o2;
            o1 = o2;
            o2 = temp;
        }
        return temp;
    }
}
```

# 10.二进制中1的个数

>输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

* 位运算技巧

```java
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;
        while(n!=0) {
            count ++;
            n = n&(n-1);
        }
        return count;
    }
}
```




