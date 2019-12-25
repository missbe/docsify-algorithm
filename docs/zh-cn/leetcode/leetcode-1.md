## 1. 两数之和
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
实现:
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {    
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
        
}
```
## 2. 两数相加
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
实现:
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode head = res;
        boolean flag = true;
        int valB = 0, result = 0;

        int count = -1;
        while(l1 != null){
            result = (flag == true) ? l1.val + l2.val : l1.val;
            result += count == -1 ? 0 : count;////判断是否进位的情况


            if(result >= 10){ ///进位
                count = result / 10; ///保存进位
                result = result % 10;///个位数
            }else{///不进位
                count = -1;
            }

            res.next = new ListNode(result);
            res = res.next;

            ///后移
            if(l1.next != null){
                l1 = l1.next;
            }else{
                break;////end l1
            }
            if(l2.next != null){
                l2 = l2.next;
            }else{
                flag = false; ///处理l1未完，l2完了的情况
            }
        }///end while
        ////计算未加上的
        while(l2.next != null){
            l2 = l2.next;
            result = count == -1 ? l2.val : l2.val + count;////判断是否进位的情况

            if(result >= 10){ ///进位
                count = result / 10; ///保存进位
                result = result % 10;///个位数
            }else{///不进位
                count = -1;
            }
            res.next = new ListNode(result);
            res = res.next;
        }
        ////处理最后的进位
        if(count != -1){
            res.next = new ListNode(count);
        }
//       ListNode node = head.next;
//        while (node != null){
//            System.out.print(node.val + "->>");
//            node = node.next;
//        }
        return head.next;
    }
}
```
## 5.最长回文子路
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```
示例 2：
```
输入: "cbbd"
输出: "bb"
```
实现:
```java
class Solution {
    public String longestPalindrome(String s) {
       if(s.equals("")){
            return "";
        }

        char[] sChars = s.toCharArray();
        int len = sChars.length;
        boolean[][] p = new boolean[len][len];

        ////initial
        for(int i = 0; i < len; i++){
            p[i][i] = true; ///1
            if(i != len - 1){
                p[i][i+1] = sChars[i] == sChars[i+1]; ///2
            }//end
        }

        ///process
        for (int i = 0; i < len; i++){
            for(int j = 0; j < len; j++){
                if(j != i + 1 && i != j) { ///3
                    if( i > j){
                        p[i][j] = p[i - 1][j + 1] && sChars[i] == sChars[j];
                    }else{
                        p[i][j] = p[i + 1][j - 1] && sChars[i] == sChars[j];
                    }
                }///end if
            }
        }///end for

        int start = -1,end = -1,max = -1;
        for(int i = 0; i < len; i++){
            for(int j = 0; j < len; j++){
                if(p[i][j] && (j - i > max || i - j > max)){
                    max = i > j ? i-j : j-i;
                    start = i > j ? j : i;
                    end = i > j ? i : j;
                }
//                System.out.print(" p[" + i + "][" + j +"]=" + p[i][j]);
            }//end inner for
//            System.out.println();
        }///end outer for
//        System.out.println(max + ":" + start + ":" + end);
        return s.substring(start, end + 1);
    }
}
```
## 7.整数反转
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
```
示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21
```
注意:假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
实现：
```java
class Solution {
    public int reverse(int x) {
       int res = 0,tmp;

        while(x != 0){
            int pop = x % 10;
            x /= 10;

            int flag = res >=0 ?res : -res;
            if(flag > Integer.MAX_VALUE / 10 || (flag == Integer.MAX_VALUE /10 && pop > 7)){
                return 0;
            }

            tmp = res *10 + pop;
            res = tmp;
        }
        return res;


    }
}
```
## 9.回文数
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```
示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```
实现:
```java
///method 1
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0){
            return false;
        }
        char[] res = String.valueOf(x).toCharArray();
        int left = 0,right = res.length - 1;
        while (left < right){
            if(res[left] == res[right]){
                left++;
                right--;
            }else{
                return false;
            }
        }
        return true;
    }
}
///method 2
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0){
            return false;
        }
        boolean res = true;
        int[] remainder = new int[32];
        int len = 0;
        while(x != 0){
            remainder[len++] = x % 10;
            x /= 10;
        }//end
        for(int i = 0, j = len - 1; i < j; i++, j--){
            if( remainder[i] != remainder[j]){
                return false;
            }///continue
        }

        return res;
    }
}
```