> PS:  [题目来源-牛客-剑指offer](https://www.nowcoder.com/ta/coding-interviews)
<!-- TOC -->

- [1.二维数组中的查找](#1二维数组中的查找)
- [2.替换空格](#2替换空格)
- [3.用两个栈实现队列](#3用两个栈实现队列)
- [4.旋转数组的最小数字](#4旋转数组的最小数字)
- [5.跳台阶](#5跳台阶)
- [6.变态跳台阶](#6变态跳台阶)
- [7.二进制中1的个数](#7二进制中1的个数)
- [8.调整数组顺序使奇数位于偶数前面](#8调整数组顺序使奇数位于偶数前面)
- [9.链表中倒数第k个结点](#9链表中倒数第k个结点)
- [10.二叉树的镜像-交换左右子树](#10二叉树的镜像-交换左右子树)
- [11.栈的压入、弹出序列](#11栈的压入弹出序列)
- [12.二叉搜索树的后序遍历序列](#12二叉搜索树的后序遍历序列)
- [13.求二叉树深度](#13求二叉树深度)
- [14.判断是否是平衡二叉树](#14判断是否是平衡二叉树)
- [15.数组中只出现一次的数字](#15数组中只出现一次的数字)
- [16.只出现一次的字符](#16只出现一次的字符)
- [17.数组中出现超过一半的数字](#17数组中出现超过一半的数字)
- [18.最小的K个数](#18最小的k个数)

<!-- /TOC -->

##### 1. 	二维数组中的查找
在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/28 下午9:21
 * description:
 * 题目描述
 * 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，
 * 每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，
 * 判断数组中是否含有该整数。
 **/

public class Solution1 {
    public boolean FindV1(int target, int [][] array) {
        for(int i=0;i<array.length;i++){
            for(int j=0;j<array[0].length;j++){
                if(array[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
    /**
     * 链接：https://www.nowcoder.com/questionTerminal/abc3fe2ce8e146608e868a70efebf62e?answerType=1&f=discussion
     * 来源：牛客网
     *
     * 从左下找
     * 1. 分析
     * 利用该二维数组的性质：
     *
     * 每一行都按照从左到右递增的顺序排序，
     * 每一列都按照从上到下递增的顺序排序
     * 改变个说法，即对于左下角的值 m，m 是该行最小的数，是该列最大的数
     * 每次将 m 和目标值 target 比较：
     *
     * 当 m < target，由于 m 已经是该行最大的元素，想要更大只有从列考虑，取值右移一位
     * 当 m > target，由于 m 已经是该列最小的元素，想要更小只有从行考虑，取值上移一位
     * 当 m = target，找到该值，返回 true
     * 用某行最小或某列最大与 target 比较，每次可剔除一整行或一整列
     */
    public boolean Find(int target, int [][] array) {
        if (array == null || array.length == 0 || array[0].length == 0) {
            return false;
        }
        int rows = array.length - 1, cols = 0, len = array[0].length - 1;
        while (rows >= 0 && cols <= len) {
            if (array[rows][cols] == target) {
                return true;
            } else if (array[rows][cols] > target) {
                rows--;
            }else{
                cols++;
            }
        }
        return false;
    }
    public boolean FindV2(int target, int [][] array) {
        if (array == null || array.length == 0 || array[0].length == 0) {
            return false;
        }
        int up = 0, down = array.length - 1, mid;
        while (up <= down) {
            mid = (up + down) / 2;
            if (array[mid][0] == target) {
                return true;
            }else if(array[mid][0] > target){
                down = mid - 1;
            }else{
                up = mid + 1;
            }
        }
        up = Math.min(up, down);
        int left = 0, right = array[0].length - 1;

        while (left <= right) {
            mid = (left + right) / 2;
            if (array[up][mid] == target) {
                return true;
            } else if (array[up][mid] > target) {
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return false;
    }
}
```
##### 2.替换空格
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 *
 * @author lyg  2020/2/28 下午10:15
 * description:
 * 请实现一个函数，将一个字符串中的每个空格替换成“%20”。
 * 例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
 **/

public class Solution2 {
    public String replaceSpace(StringBuffer str) {
        StringBuilder buffer = new StringBuilder();
        for (int i = 0, len = str.length(); i < len; i++) {
            if (Character.isSpaceChar(str.charAt(i))) {
                buffer.append("%20");
            } else {
                buffer.append(str.charAt(i));
            }
        }
        return buffer.toString();
    }

    public String replaceSpaceV2(StringBuffer str) {
        String res = str.toString();
        return res.replaceAll("\\s", "%20");
    }
}
```
##### 3.用两个栈实现队列
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
```java
import java.util.Stack;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 上午10:13
 * description:
 * 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
 **/

public class Solution4 {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        stack1.push(node);
    }

    public int pop() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```
##### 4.旋转数组的最小数字
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 上午10:15
 * description:
 * 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
 * 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
 * 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
 * NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
 **/

public class Solution5 {
    public int minNumberInRotateArray(int [] array) {
        if (array.length == 0) {
            return 0;
        }
        int pre = 0, back;
        for (int i = 1; i < array.length; i++) {
            if (array[pre] > array[i]) {
                return array[i];
            }
            pre++;
        }
        return 0;
    }
}
```
##### 5.跳台阶
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 上午10:28
 * description:
 * 一只青蛙一次可以跳上1级台阶，也可以跳上2级。
 * 求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
 **/

public class Solution7 {
    public int JumpFloor(int target) {
        if (target <= 2) {
            return target;
        }
        int pre1 = 1, pre2 = 2, sum = 0;
        for (int i = 3; i <= target; i++) {
            sum = pre1 + pre2;
            pre1 = pre2;
            pre2 = sum;
        }
        return sum;
    }
}
```
##### 6.变态跳台阶
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 上午10:34
 * description:
 * 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。
 * 求该青蛙跳上一个n级的台阶总共有多少种跳法。
 **/

public class Solution8 {
    public int JumpFloorII(int target) {
        int[] dp = new int[target+1];
        dp[1] = 1;
        for (int i = 2; i < dp.length; i++) {
            dp[i] = 2 * dp[i-1];
        }
        return dp[target];
    }

    int JumpFloorIIV2(int number) {
        return 1 << --number;//2^(number-1)用位移操作进行，更快
    }
}
```
##### 7.二进制中1的个数
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 上午11:31
 * description:
 * 输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
 **/

public class Solution9 {

    //---------------正解--------------------------------
    //思想：用1（1自身左移运算，其实后来就不是1了）和n的每位进行位与，来判断1的个数
    private static int NumberOf1_low(int n) {
        int count = 0;
        int flag = 1;
        while (flag != 0) {
            if ((n & flag) != 0) {
                count++;
            }
            flag = flag << 1;
        }
        return count;
    }
    //--------------------最优解----------------------------

    /**
     * 链接：https://www.nowcoder.com/questionTerminal/8ee967e43c2c4ec193b040ea7fbb10b8?answerType=1&f=discussion
     * 来源：牛客网
     * 独特思路：整数n，进行n&(n-1)运算，会把二进制表示中最右边的1变为0
     * 如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，
     * 那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1
     * (如果最右边的1后面还有0的话)。其余所有位将不会受到影响。
     * 举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。
     * 减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.
     * 我们发现减1的结果是把最右边的一个1开始的所有位都取反了。
     * 这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1
     * 那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，
     * 会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。
     */
    public static int NumberOf1(int n) {
        int count = 0;
        while (n != 0) {
            ++count;
            n = (n - 1) & n;
        }
        return count;
    }
}
```
##### 8.调整数组顺序使奇数位于偶数前面
```java
import java.util.Arrays;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 下午12:06
 * description:
 **/

public class Solution11 {
    public void reOrderArray(int [] array) {
        int pre = 0, back, tmp;
        for (back = 0; back < array.length; back++) {
            if (array[back] % 2 == 1) {
                tmp = array[back];
                ///right move
                if (back - pre >= 0)
                    System.arraycopy(array, pre, array, pre + 1, back - pre);
                array[pre] = tmp;
                pre++;
            }
        }///end fo
    }
}
```
##### 9.链表中倒数第k个结点
```java
import javax.swing.*;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/29 下午12:40
 * description:
 * 输入一个链表，输出该链表中倒数第k个结点。
 **/

public class Solution12 {
    public class ListNode {
        int val;
        ListNode next = null;

        ListNode(int val) {
            this.val = val;
        }
    }
    /**
     * 思路一：快慢指针，中间间隔k个长度
     */
    public ListNode FindKthToTail(ListNode head,int k) {
        ListNode fast = head, slow = null;
        int count = 0;
        boolean isArriveK = false;
        while (fast != null) {
            if (!isArriveK) {
                count++;
            }
            if (count == k) {
                isArriveK = true;
                slow = head;
                count = -1;
            }else if (count == -1 && slow != null) {
                slow = slow.next;
            }
            fast = fast.next;
        }
        return slow;
    }

    /**
     * 思路二：计算整个链表的长度n，再找第n-k个节点
     */
    public ListNode FindKthToTailV2(ListNode head,int k) {
        if (head == null) {
            return head;
        }

        ListNode node = head;
        int count = 0;
        while (node != null) {
            count++;
            node = node.next;
        }
        if (count < k) {
            return null;
        }

        ListNode p = head;
        for (int i = 0; i < count - k; i++) {
            p = p.next;
        }
        return p;
    }

    /**
     * 头插法实现顺序表逆转
     */
    public ListNode ReverseList(ListNode head) {
        ListNode res = null, p;
        while (head != null) {
            p = head.next;
            head.next = res;
            res = head;
            head = p;
        }
        return res;
    }

    /**
     * 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
     */
    public ListNode Merge(ListNode list1,ListNode list2) {
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
        ListNode head = null, p = null, p1 = list1, p2 = list2;
        while (p1 != null && p2 != null) {
            if (p1.val < p2.val) {
                if (p == null) {
                    head = p = p1;
                }else{
                    p.next = p1;
                    p = p.next;
                }
                p1 = p1.next;
            }else{
                if (p == null) {
                    head = p = p2;
                }else{
                    p.next = p2;
                    p = p.next;
                }
                p2 = p2.next;
            }
        }
        if (p1 != null) {
            p.next = p1;
        }
        if (p2 != null) {
            p.next = p2;
        }
        return  head;
    }
}
```
##### 10.二叉树的镜像-交换左右子树
```java

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午9:24
 * description:
 * 操作给定的二叉树，将其变换为源二叉树的镜像。
 **/

public class Solution13 {
    public void Mirror(TreeNode root) {
        if (root != null) {
            TreeNode tmp = root.left;
            root.left = root.right;
            root.right = tmp;
            Mirror(root.left);
            Mirror(root.right);
        }
    }
}
```
##### 11.栈的压入、弹出序列
```java
import java.util.Stack;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午9:35
 * description:
 * 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。
 * 假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，
 * 序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。
 * （注意：这两个序列的长度是相等的）
 **/

public class Solution15 {
    private Stack<Integer> stack = new Stack<>();
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        int popIndex = 0, correctIndex = 0;
        int[] correct = new int[pushA.length];
        for (int value : pushA) {
            if (value != popA[popIndex]) {
                stack.push(value);
            } else {
                correct[correctIndex++] = value;
                popIndex++;
            }
        }
        while (!stack.isEmpty()) {
            correct[correctIndex++] = stack.pop();
        }
        for (int i = 0; i < correct.length; i++) {
            if (correct[i] != popA[i]) {
                return false;
            }
        }
        return true;
    }
}
```
##### 12.二叉搜索树的后序遍历序列
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午10:32
 * description:
 * 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。
 * 如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。
 **/

public class Solution17 {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if (sequence == null || sequence.length == 0) {
            return false;
        }
        return isBst(sequence, 0, sequence.length - 1);
    }

    /**
     * 链接：https://www.nowcoder.com/questionTerminal/a861533d45854474ac791d90e447bafd?answerType=1&f=discussion
     * 来源：牛客网
     *
     * 题目：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果，如果是则输出Yes,否则输出No；假设输入的数组的任意两个数字都互不相同。
     *
     * 这道题的解题突破点就在于二叉树的后序遍历数组有着什么的特点？
     *
     * 特点：遍历的时候，如果遇到比最后一个元素大的节点，就说明它的前面都比最后一个元素小，该元素后面的所有值都必须大于最后一个值，
     * 这两个条件必须都要满足。否则就说明该序列不是二叉树后序遍历。
     * 例子： 2 4 3 6 8 7 5 这是一个正确的后序遍历
     * 这个例子的特点就是:最后一个元素是 5 ，首先遍历数组，当遍历到6的时候，6前面的值都小于5，如果在6后面的值有一个小于5就不是后序遍历，
     * 所以一旦在遍历的时候遇到比最后一个元素的值索引，那么之后的所有元素都必须大于5，否则就不是后序遍历序列。
     */
    private boolean isBst(int[] sequence, int start, int end) {
        if (start >= end) {
            return true;
        }
        int root = sequence[end], pivot = start;
        while (pivot < end && sequence[pivot] < root) {
            pivot++;
        }
        for (int i = pivot; i < end; i++) {
            if (sequence[i] < root) {
                return false;
            }
        }
        return isBst(sequence, start, pivot - 1) && isBst(sequence, pivot, end-1);
    }
}
```
##### 13.求二叉树深度
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午10:57
 * description:
 * 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）
 * 形成树的一条路径，最长路径的长度为树的深度。
 **/

public class Solution18 {
    public int TreeDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(TreeDepth(root.left), TreeDepth(root.right)) + 1;
    }
}
```
##### 14.判断是否是平衡二叉树
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午11:00
 * description:
 * 输入一棵二叉树，判断该二叉树是否是平衡二叉树。
 **/

public class Solution19 {
    public boolean IsBalanced_Solution(TreeNode root) {
        if (root == null) {
            return true;
        }
        return Math.abs(TreeDepth(root.left) - TreeDepth(root.right)) <= 1
                && IsBalanced_Solution(root.left)
                && IsBalanced_Solution(root.right);

    }

    public int TreeDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(TreeDepth(root.left), TreeDepth(root.right)) + 1;
    }
}
```
##### 15.数组中只出现一次的数字
```java
import java.util.Arrays;
import java.util.HashMap;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午11:05
 * description:
 * 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。
 **/

public class Solution20 {
    //可以先用最简单的HashMap的方法来做，这样主要是为了联系Map的用法。
    //链接：https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811?answerType=1&f=discussion
    //来源：牛客网
    public void FindNumsAppearOnceV2(int [] array,int num1[] , int num2[]) {
        //哈希算法
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int item : array) {
            if (map.containsKey(item))
                map.put(item, 2);
            else
                map.put(item, 1);
        }
        int count = 0;
        for (int value : array) {
            if (map.get(value) == 1) {
                if (count == 0) {
                    num1[0] = value;
                    count++;
                } else
                    num2[0] = value;
            }
        }
    }
    //num1,num2分别为长度为1的数组。传出参数
    //将num1[0],num2[0]设置为返回结果

    /**
     * 链接：https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811?answerType=1&f=discussion
     * 来源：牛客网
     *
     * 比较好用的剑指offer的做法：
     * 首先：位运算中异或的性质：两个相同数字异或=0，一个数和0异或还是它本身。
     * 当只有一个数出现一次时，我们把数组中所有的数，依次异或运算，
     * 最后剩下的就是落单的数，因为成对儿出现的都抵消了。
     *
     * 依照这个思路，我们来看两个数（我们假设是AB）出现一次的数组。我们首先还是先异或，
     * 剩下的数字肯定是A、B异或的结果，这个结果的二进制中的1，表现的是A和B的不同的位。
     * 我们就取第一个1所在的位数，假设是第3位，接着把原数组分成两组，分组标准是第3位是否为1。
     * 如此，相同的数肯定在一个组，因为相同数字所有位都相同，而不同的数，肯定不在一组。
     * 然后把这两个组按照最开始的思路，依次异或，剩余的两个结果就是这两个只出现一次的数字。
     */
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int xor1 = 0;
        for (int value : array)
            xor1 = xor1 ^ value;
        //在xor1中找到第一个不同的位对数据进行分类，分类为两个队列对数据进行异或求和找到我们想要的结果
        int index = 1;
        while((index & xor1)==0)
            index = index <<1;//因为可能有多个位为1所以需要求一下位置
        int result1 = 0;
        int result2 = 0;
        for (int value : array) {
            if ((index & value) == 0)
                result1 = result1 ^ value;
            else
                result2 = result2 ^ value;
        }
        num1[0] = result1;
        num2[0] = result2;
    }
}
```
##### 16.只出现一次的字符
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 上午11:48
 * description:
 * 在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置,
 * 如果没有则返回 -1（需要区分大小写）.
 **/

public class Solution21 {
    public int FirstNotRepeatingChar(String str) {
        int[] res = new int[128];
        char[] strChars = str.toCharArray();
        for (char ch : strChars) {
            res[ch] += 1;
        }
        ///hash思想
        for (int i = 0; i < strChars.length; i++) {
            if (res[strChars[i]] == 1) {
                return  i;
            }
        }
        return -1;
    }
}
```
##### 17.数组中出现超过一半的数字
```java
import java.util.Arrays;
import java.util.stream.IntStream;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 *
 * @author lyg  2020/3/1 上午11:57
 * description:
 * 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
 * 例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，
 * 超过数组长度的一半，因此输出2。如果不存在则输出0。
 **/

public class Solution22 {
    /**
     * 链接：https://www.nowcoder.com/questionTerminal/e8a1b01a2df14cb2b228b30ee6a92163?answerType=1&f=discussion
     * 来源：牛客网
     */
    public int MoreThanHalfNum_SolutionV2(int[] array) {
        Arrays.sort(array);
        int i = array[array.length / 2];
        return IntStream.of(array).filter(k -> k == i).count() > array.length / 2 ? i : 0;
    }

    /**
     * 链接：https://www.nowcoder.com/questionTerminal/e8a1b01a2df14cb2b228b30ee6a92163?answerType=1&f=discussion
     * 来源：牛客网
     * <p>
     * 解法二：高效遍历
     * 这个和解法一有相似之处，都是先把最有可能满足题意要求的数字保存起来，然后在最后判断它是否满足要求，
     * 如果满足就返回它的值，如果不满足就返回0，但是它比解法一高效的地方在于没有排序，直接进行的遍历。
     * <p>
     * 方法：我们在遍历数组的时候，保存两个值，一个是数组中的数字，另一个是次数，当遍历到下一个数字的时候，
     * 如果和上一次的数字一样则次数加1，如果不一样次数减一(相当于抵消了)，如果次数为0了，那就保存下一个数字，
     * 并把次数设置为1，因为我们要找的数字如果存在最后一定是把次数设置为1的那个数。
     */
    int MoreThanHalfNum_SolutionV3(int[] numbers) {
        if (numbers.length == 0) {
            return 0;
        }
        int temp = numbers[0];
        int time = 1;
        //[1,2,3,2,4,2,5,2,3]
        //{1,2,3,2,2,2,5,4,2}
        for (int i = 1; i < numbers.length; ++i) {
            if (time == 0) {
                temp = numbers[i];
                time = 1;
            } else if (temp == numbers[i]) {
                ++time;
            } else {
                --time;
            }
        }
        // 判断 temp 是否符合要求
        int count = 0;
        for (int number : numbers) {
            if (temp == number) {
                ++count;
            }
        }
        if (count > numbers.length / 2) {
            return temp;
        } else {
            return 0;
        }
    }
}
```
##### 18.最小的K个数
```java
import java.util.ArrayList;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/3/1 下午12:14
 * description:
 * 输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4
 **/

public class Solution23 {
    /**
     * 链接：https://www.nowcoder.com/questionTerminal/6a296eb82cf844ca8539b57c23e6e9bf?answerType=1&f=discussion
     * 来源：牛客网
     * 维持一个K长度的最小值集合，然后利用插入排序的思想进行对前K个元素的不断更新。
     */
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {

        ArrayList<Integer> result = new ArrayList<Integer>();
        if(k<= 0 || k > input.length)return result;
        //初次排序，完成k个元素的排序
        for(int i = 1; i< k; i++){
            int j = i-1;
            int unFindElement = input[i];
            while(j >= 0 && input[j] > unFindElement){
                input[j+1] = input[j];
                j--;
            }

            input[j+1] = unFindElement;
        }
        //遍历后面的元素 进行k个元素的更新和替换
        for(int i = k; i < input.length; i++){
            if(input[i] < input[k-1]){
                int newK = input[i];
                int j = k-1;
                while(j >= 0 && input[j] > newK){
                    input[j+1] = input[j];
                    j--;
                }
                input[j+1] = newK;
            }
        }
        //把前k个元素返回
        for(int i=0; i < k; i++)
            result.add(input[i]);
        return result;
    }
    /**
     * 链接：https://www.nowcoder.com/questionTerminal/6a296eb82cf844ca8539b57c23e6e9bf?answerType=1&f=discussion
     * 来源：牛客网
     *
     * 快排的partition函数会将原序列分为左右两个子序列，左边序列都小于pivot，右边序列都大于或等于pivot，
     * 当pivot为数组的第k个元素时，数组中pivot及其之前的元素都小于右边序列，即为n个整数中最小的k个数。
     */
    public ArrayList<Integer> GetLeastNumbers_SolutionV2(int [] input, int k) {
        if (input == null || input.length == 0 || input.length < k || k == 0) {
            return new ArrayList<>();
        }
        // 在数组中寻找位置为k - 1的pivot
        int start = 0, end = input.length - 1;
        int index = partition(input, start, end);
        while (index != k - 1) {
            if (index < k - 1) {
                start = index + 1;
            } else {
                end = index - 1;
            }
            index = partition(input, start, end);
        }

        // 收集这k个数
        ArrayList<Integer> res = new ArrayList<>();
        for (int i = 0; i <= index; i++) {
            res.add(input[i]);
        }
        return res;
    }

    // divide target range array to smaller hi value part and other part.
    private int partition(int[] arr, int start, int end) {
        int pivot = arr[end];
        int prePtr = start - 1;
        while (start < end) {
            if (arr[start] < pivot) {
                swap(arr, start++, ++prePtr);
            } else {
                start++;
            }
        }
        swap(arr, end, ++prePtr);
        return prePtr;
    }

    private void swap(int[] arr, int a, int b) {
        int temp = arr[b];
        arr[b] = arr[a];
        arr[a] = temp;
    }

}
```
