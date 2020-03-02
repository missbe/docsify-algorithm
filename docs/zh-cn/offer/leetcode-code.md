<!-- TOC -->

- [1.盛最多水的容器](#1盛最多水的容器)
- [2.最长回文子串](#2最长回文子串)
- [3.整数反转](#3整数反转)
- [4.最长公共前缀](#4最长公共前缀)
- [5.有效的括号](#5有效的括号)
- [6.合并两个有序链表](#6合并两个有序链表)
- [7.删除排序数组中的重复项](#7删除排序数组中的重复项)
- [8.移除元素](#8移除元素)
- [9.实现 strStr()](#9实现-strstr)
- [10.搜索插入位置](#10搜索插入位置)
- [11.外观数列](#11外观数列)
- [12.只出现一次的数字](#12只出现一次的数字)
- [13. 验证回文串](#13-验证回文串)
- [14.合并两个有序数组](#14合并两个有序数组)
- [15.最长回文串](#15最长回文串)
- [16.最长的回文子序列](#16最长的回文子序列)

<!-- /TOC -->
##### 1.盛最多水的容器
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/1/30 下午12:05
 * description:
 * 给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
 *
 * 说明：你不能倾斜容器，且 n 的值至少为 2
 *
 * 示例:
 *
 * 输入: [1,8,6,2,5,4,8,3,7]
 * 输出: 49
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/container-with-most-water
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution11 {
    public int maxArea(int[] height) {
        int distance, tmpArea, maxArea = Integer.MIN_VALUE;
        for (int i = 0; i < height.length; i++) {
            distance = maxDistance(height,i,height[i]);
            //distance为x,height[i]为y，固定y找最大的x，贪心思想
            tmpArea = distance * height[i];
            System.out.println("i:" + i + ",b:" + height[i] + ",distance:" + distance + ",tmpArea:" + tmpArea);
            if (tmpArea > maxArea) {
                maxArea = tmpArea;
            }//end if
        }
        return maxArea;
    }

    public int maxDistance(int[] height, int index, int value) {
        int distance = Integer.MIN_VALUE, tmpDistance;
        for (int i = 0; i < height.length; i++) {
            if (height[i] >= value) {
                tmpDistance = Math.abs(index - i);
                if (tmpDistance > distance) {
                    distance = tmpDistance;
                }
            }
        }
        return distance;
    }

    public int doubleFingerAcupuncture(int[] height) {
        int maxArea = Integer.MIN_VALUE, tmpArea;
        int start = 0, end = height.length - 1;
        while (start < end) {
            tmpArea = (end - start) * Math.min(height[start], height[end]);
            if (height[start] < height[end]) {
                start++;
            }else{
                end--;
            }
            if (tmpArea > maxArea) {
                maxArea = tmpArea;
            }
        }
        return maxArea;
    }
}
```
##### 2.最长回文子串
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019
 * @author lyg  2019/12/15 下午9:13
 * description:
 * 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
 *
 * 示例 1：
 *
 * 输入: "babad"
 * 输出: "bab"
 * 注意: "aba" 也是一个有效答案。
 * 示例 2：
 *
 * 输入: "cbbd"
 * 输出: "bb"
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/longest-palindromic-substring
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution5 {
    private int index, len;

    /**
     * 方法四：中心扩展算法
     * 事实上，只需使用恒定的空间，我们就可以在 O(n^2)O(n
     * 2
     *  ) 的时间内解决这个问题。
     *
     * 我们观察到回文中心的两侧互为镜像。因此，回文可以从它的中心展开，并且只有 2n - 12n−1 个这样的中心。
     *
     * 你可能会问，为什么会是 2n - 12n−1 个，而不是 nn 个中心？原因在于所含字母数为偶数的回文的中心可以处于两字母之间（例如 \textrm{“abba”}“abba” 的中心在两个 \textrm{‘b’}‘b’ 之间）。
     *
     * public String longestPalindrome(String s) {
     *     if (s == null || s.length() < 1) return "";
     *     int start = 0, end = 0;
     *     for (int i = 0; i < s.length(); i++) {
     *         int len1 = expandAroundCenter(s, i, i);
     *         int len2 = expandAroundCenter(s, i, i + 1);
     *         int len = Math.max(len1, len2);
     *         if (len > end - start) {
     *             start = i - (len - 1) / 2;
     *             end = i + len / 2;
     *         }
     *     }
     *     return s.substring(start, end + 1);
     * }
     *
     * private int expandAroundCenter(String s, int left, int right) {
     *     int L = left, R = right;
     *     while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
     *         L--;
     *         R++;
     *     }
     *     return R - L - 1;
     * }
     *
     * 时间复杂度：O(n^2)O(n
     * 2
     *  )，由于围绕中心来扩展回文会耗去 O(n)O(n) 的时间，所以总的复杂度为 O(n^2)O(n
     * 2
     *  )。
     *
     * 空间复杂度：O(1)O(1)。
     *
     * 作者：LeetCode
     * 链接：https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode/
     * 来源：力扣（LeetCode）
     * 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
     * @param s 字符串
     * @return 结果
     */
    public String longestPalindrome(String s) {
        if (s.length() < 2) {
            return s;
        }
        for (int i = 0; i < s.length() - 1; i++) {
            PalindromeHelper(s, i, i);
            PalindromeHelper(s, i, i + 1);
        }
        return s.substring(index, index + len);
    }

    public void PalindromeHelper(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            l--;
            r++;
        }
        if (len < r - l - 1) {
            index = l + 1;
            len = r - l - 1;
        }
    }
}
```
##### 3.整数反转
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019
 * @author lyg  2019/12/15 下午9:26
 * description:
 * 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
 *
 * 示例 1:
 *
 * 输入: 123
 * 输出: 321
 *  示例 2:
 *
 * 输入: -123
 * 输出: -321
 * 示例 3:
 *
 * 输入: 120
 * 输出: 21
 * 注意:
 *
 * 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/reverse-integer
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution7 {
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
##### 4.最长公共前缀
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/1 下午12:32
 * description:
 * 编写一个函数来查找字符串数组中的最长公共前缀。
 *
 * 如果不存在公共前缀，返回空字符串 ""。
 *
 * 示例 1:
 *
 * 输入: ["flower","flow","flight"]
 * 输出: "fl"
 * 示例 2:
 *
 * 输入: ["dog","racecar","car"]
 * 输出: ""
 * 解释: 输入不存在公共前缀。
 * 说明:
 *
 * 所有输入只包含小写字母 a-z
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/longest-common-prefix
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution14 {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) {
            return "";
        }
        String result = strs[0];
        for (int i = 1; i < strs.length; i++) {
            result = commonPrefix(result, strs[i]);
        }
        return result;
    }

    public String commonPrefix(String str1, String str2) {
        int length = Math.min(str1.length(), str2.length());
        int i = 0;
        while (i < length && str1.charAt(i) == str2.charAt(i)) {
            i++;
        }
        return i != 0 ? str1.substring(0, i) : "";
    }

    public String longestCommonPrefixV2(String[] strs) {
        if (strs.length == 0) {
            return "";
        }
        String prefix = strs[0];
        for (int i = 1; i < strs.length; i++) {
            ////一直循环直到找到相同公共字符串
            while (strs[i].indexOf(prefix) != 0) {
                prefix = prefix.substring(0, prefix.length() - 1);
                if (prefix.isEmpty()) {
                    return "";
                }
            }
        }
        return prefix;
    }

    /**
     * 想象数组的末尾有一个非常短的字符串，使用上述方法依旧会进行 S次比较。
     * 优化这类情况的一种方法就是水平扫描。我们从前往后枚举字符串的每一列，
     * 先比较每个字符串相同列上的字符（即不同字符串相同下标的字符）然后再进行对下一列的比较;
     */
    public String longestCommonPrefixV3(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        for (int i = 0; i < strs[0].length() ; i++){
            char c = strs[0].charAt(i);
            for (int j = 1; j < strs.length; j ++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
}
```
##### 5.有效的括号
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 *
 * @author lyg  2020/2/1 下午3:01
 * description:
 * 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
 * <p>
 * 有效字符串需满足：
 * <p>
 * 左括号必须用相同类型的右括号闭合。
 * 左括号必须以正确的顺序闭合。
 * 注意空字符串可被认为是有效字符串。
 * <p>
 * 示例 1:
 * <p>
 * 输入: "()"
 * 输出: true
 * 示例 2:
 * <p>
 * 输入: "()[]{}"
 * 输出: true
 * 示例 3:
 * <p>
 * 输入: "(]"
 * 输出: false
 * 示例 4:
 * <p>
 * 输入: "([)]"
 * 输出: false
 * 示例 5:
 * <p>
 * 输入: "{[]}"
 * 输出: true
 * <p>
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/valid-parentheses
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution20 {
    public boolean isValid(String s) {
        if (s.length() % 2 != 0) {
            return false;
        }
        char[] metaArray = s.toCharArray();
        char[] stack = new char[metaArray.length];
        int top = 0;
        for (char c : metaArray) {
            switch (c) {
                case '(':
                case '{':
                case '[':
                    stack[top++] = c;
                    break;
                case '}':
                    if (top > 0 && stack[top - 1] == '{') {
                        top--;
                    }else{
                        return false;
                    }
                    break;
                case ']':
                    if (top > 0 && stack[top - 1] == '[') {
                        top--;
                    }else{
                        return false;
                    }
                    break;
                case ')':
                    if (top > 0 && stack[top - 1] == '(') {
                        top--;
                    }else{
                        return false;
                    }
                    break;
                default:
                    break;
            }
        }
        return top == 0;
    }
}

```
##### 6.合并两个有序链表
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/1 下午2:06
 * description:
 * 将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
 *
 * 示例：
 *
 * 输入：1->2->4, 1->3->4
 * 输出：1->1->2->3->4->4
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/merge-two-sorted-lists
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution21 {

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode preNode, head = new ListNode(-1);
        preNode = head;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                preNode.next = l1;
                preNode = l1;
                l1 = l1.next;
            }else{
                preNode.next = l2;
                preNode = l2;
                l2 = l2.next;
            }
        }
        if (l1 != null) {
            preNode.next = l1;
        }
        if (l2 != null) {
            preNode.next = l2;
        }
        return head.next;
    }

    public ListNode mergeTwoListsV2(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        else if (l2 == null) {
            return l1;
        }
        else if (l1.val < l2.val) {
            l1.next = mergeTwoListsV2(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoListsV2(l1, l2.next);
            return l2;
        }
    }
}
```
##### 7.删除排序数组中的重复项
```java
import java.util.Arrays;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/3 上午11:05
 * description:
 * 给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
 *
 * 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
 *
 * 示例 1:
 *
 * 给定数组 nums = [1,1,2],
 *
 * 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。
 *
 * 你不需要考虑数组中超出新长度后面的元素。
 * 示例 2:
 *
 * 给定 nums = [0,0,1,1,1,2,2,3,3,4],
 *
 * 函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
 *
 * 你不需要考虑数组中超出新长度后面的元素。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution26 {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int preNumber = nums[0], startIndex = 1, count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (preNumber != nums[i]) {
                nums[startIndex] = nums[i];
                startIndex++;
                preNumber = nums[i];
                count++;
            }
        }
        return count;
    }
}

```
##### 8.移除元素
```java
import java.util.Arrays;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/3 下午3:27
 * description:
 * 给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。
 *
 * 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
 *
 * 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
 *
 * 示例 1:
 *
 * 给定 nums = [3,2,2,3], val = 3,
 *
 * 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
 *
 * 你不需要考虑数组中超出新长度后面的元素。
 * 示例 2:
 *
 * 给定 nums = [0,1,2,2,3,0,4,2], val = 2,
 *
 * 函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
 *
 * 注意这五个元素可为任意顺序。
 *
 * 你不需要考虑数组中超出新长度后面的元素。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/remove-element
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution27 {
    public int removeElement(int[] nums, int val) {
        if (nums.length == 0) {
            return 0;
        }
        int startIndex = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != val) {
                nums[startIndex++] = nums[i];
            }
        }
        return startIndex;
    }

    /**
     * 当我们遇到 nums[i] = valnums[i]=val 时，我们可以将当前元素与最后一个元素进行交换，并释放最后一个元素。
     * 这实际上使数组的大小减少了 1。
     *
     * 请注意，被交换的最后一个元素可能是您想要移除的值。但是不要担心，在下一次迭代中，我们仍然会检查这个元素。
     *
     */
    public int removeElementV2(int[] nums, int val) {
        int i = 0;
        int n = nums.length;
        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1];
                // reduce array size by one
                n--;
            } else {
                i++;
            }
        }
        return n;
    }
}
```
##### 9.实现 strStr()
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/3 下午3:45
 * description:
 * 实现 strStr() 函数。
 *
 * 给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
 *
 * 示例 1:
 *
 * 输入: haystack = "hello", needle = "ll"
 * 输出: 2
 * 示例 2:
 *
 * 输入: haystack = "aaaaa", needle = "bba"
 * 输出: -1
 * 说明:
 *
 * 当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。
 *
 * 对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/implement-strstr
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution28 {
    public int strStr(String haystack, String needle) {
        if (needle.isEmpty()) {
            return 0;
        }
        if (haystack.length() < needle.length() || (haystack.length() == needle.length() && haystack.charAt(0) != needle.charAt(0))) {
            return -1;
        }
        int startIndex, j;
        for (int i = 0, length = haystack.length(); i < length; i++) {
            if (haystack.charAt(i) == needle.charAt(0)) {
                startIndex = i + 1;
                j = 1;
                while (j < needle.length() && startIndex < haystack.length() && haystack.charAt(startIndex) == needle.charAt(j)) {
                    j++;
                    startIndex++;
                }
                if (j == needle.length()) {
                    return i;
                }
            }
        }
        return -1;
    }

    //
    public int strStrByKmp(String haystack, String needle) {
        if("".equals(needle)) {
            return 0;
        }
        if("".equals(haystack)) {
            return -1;
        }

        // 1.构造KMP中的dp矩阵
        int m = needle.length();
        // 各个状态(行)遇到下一个字符(列)跳转到哪个状态
        int[][] dp = new int[m][256];
        // 影子状态
        int X = 0;
        dp[0][needle.charAt(0)] = 1;
        for (int i = 1; i < m; i++) {
//            for (int j = 0; j < 256; j++) {
//                // method1:假设下个字符不匹配，此时要回去看影子状态，从而得知跳转到哪个状态
//                dp[i][j] = dp[X][j];
//            }
            //method2:假设下个字符不匹配，此时要回去看影子状态，从而得知跳转到哪个状态
            System.arraycopy(dp[X], 0, dp[i], 0, 256);
            // 只有pat上i的字符匹配，跳转到下个状态
            dp[i][needle.charAt(i)] = i + 1;
            // 更新影子状态
            X = dp[X][needle.charAt(i)];
        }
        // 2.构造dp完成后，开始search
        // 初始状态为0
        int s = 0;
        for (int i = 0; i < haystack.length(); i++) {
            s = dp[s][haystack.charAt(i)];
            if (s == m) {
                return i - m + 1;
            }
        }
        // 匹配失败，返回-1
        return -1;
    }
}

```
##### 10.搜索插入位置
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/4 下午6:24
 * description:
 * 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
 *
 * 你可以假设数组中无重复元素。
 *
 * 示例 1:
 *
 * 输入: [1,3,5,6], 5
 * 输出: 2
 * 示例 2:
 *
 * 输入: [1,3,5,6], 2
 * 输出: 1
 * 示例 3:
 *
 * 输入: [1,3,5,6], 7
 * 输出: 4
 * 示例 4:
 *
 * 输入: [1,3,5,6], 0
 * 输出: 0
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/search-insert-position
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution35 {
    public int searchInsert(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= target) {
                return i;
            }
        }
        return nums.length;
    }

    public int searchInsertV2(int[] nums, int target) {
        int left = 0, right = nums.length - 1, mid;
        while (left <= right) {
            ///left + (right - left)/2
            mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return left;
    }
}

```
##### 11.外观数列
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/4 下午6:46
 * description:
 * 「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：
 *
 * 1.     1
 * 2.     11
 * 3.     21
 * 4.     1211
 * 5.     111221
 * 1 被读作  "one 1"  ("一个一") , 即 11。
 * 11 被读作 "two 1s" ("两个一"）, 即 21。
 * 21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。
 *
 * 给定一个正整数 n（1 ≤ n ≤ 30），输出外观数列的第 n 项。
 *
 * 注意：整数序列中的每一项将表示为一个字符串。
 * 示例 1:
 *
 * 输入: 1
 * 输出: "1"
 * 解释：这是一个基本样例。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/count-and-say
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution38 {
    public String countAndSay(int n) {
        if (1 == n) {
            return "1";
        }
        String prev = countAndSay(n - 1);
        ///特别处理长度为1的问题
        if ("1".equals(prev)) {
            return "11";
        }
        char ch = prev.charAt(0);
        int length = 1;
        StringBuilder builder = new StringBuilder();
        for (int i = 1; i < prev.length(); i++) {
            if (ch != prev.charAt(i)) {
                builder.append(length);
                builder.append(ch);
                ch = prev.charAt(i);
                length = 1;
            }else{
                length++;
            }
        }
        ////连接末尾最后一段
        builder.append(length);
        builder.append(ch);
        return builder.toString();
    }
}

```
##### 12.只出现一次的数字
```java
import java.util.HashMap;
import java.util.Map;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/6 上午11:44
 * description:
 * 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
 *
 * 说明：
 *
 * 你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
 *
 * 示例 1:
 *
 * 输入: [2,2,1]
 * 输出: 1
 * 示例 2:
 *
 * 输入: [4,1,2,1,2]
 * 输出: 4
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/single-number
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution136 {
    /**
     * 方法 2：哈希表
     * 算法
     *
     * 我们用哈希表避免每次查找元素是否存在需要的 O(n)O(n) 时间。
     *
     * 遍历 \text{nums}nums 中的每一个元素
     * 查找 hash\_tablehash_table 中是否有当前元素的键
     * 如果没有，将当前元素作为键插入 hash\_tablehash_table
     * 最后， hash\_tablehash_table 中仅有一个元素，用 popitem 获得它
     * 时间复杂度： O(n \cdot 1) = O(n)O(n⋅1)=O(n) 。for 循环的时间复杂度是 O(n)O(n) 的。Python 中哈希表的 pop 操作时间复杂度为O(1)O(1) 。
     * 空间复杂度： O(n)O(n) 。hash\_tablehash_table 需要的空间与 \text{nums}nums 中元素个数相等。
     *
     * 作者：LeetCode
     * 链接：https://leetcode-cn.com/problems/single-number/solution/zhi-chu-xian-yi-ci-de-shu-zi-by-leetcode/
     * 来源：力扣（LeetCode）
     * 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
     */
    public int singleNumberBySelf(int[] nums) {
        Map<Integer, Object> map = new HashMap<>();
        for (int num : nums) {
            if (!map.containsKey(num)) {
                map.put(num, null);
            } else {
                map.remove(num);
            }
        }
        return map.keySet().iterator().next();
    }

    /**
     * 方法 4：位操作
     * 概念
     *
     * 如果我们对 0 和二进制位做 XOR 运算，得到的仍然是这个二进制位
     * a \oplus 0 = aa⊕0=a
     * 如果我们对相同的二进制位做 XOR 运算，返回的结果是 0
     * a \oplus a = 0a⊕a=0
     * XOR 满足交换律和结合律
     * a \oplus b \oplus a = (a \oplus a) \oplus b = 0 \oplus b = ba⊕b⊕a=(a⊕a)⊕b=0⊕b=b
     * 所以我们只需要将所有的数进行 XOR 操作，得到那个唯一的数字。
     * 时间复杂度： O(n)O(n) 。我们只需要将 \text{nums}nums 中的元素遍历一遍，所以时间复杂度就是 \text{nums}nums 中的元素个数。
     * 空间复杂度：O(1)O(1) 。
     *
     * 作者：LeetCode
     * 链接：https://leetcode-cn.com/problems/single-number/solution/zhi-chu-xian-yi-ci-de-shu-zi-by-leetcode/
     * 来源：力扣（LeetCode）
     * 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
     */
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int num : nums) {
            result = result ^ num;
        }
        return result;
    }
}

```
##### 13. 验证回文串
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/6 下午12:44
 * description:
 * 给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
 *
 * 说明：本题中，我们将空字符串定义为有效的回文串。
 *
 * 示例 1:
 *
 * 输入: "A man, a plan, a canal: Panama"
 * 输出: true
 * 示例 2:
 *
 * 输入: "race a car"
 * 输出: false
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/valid-palindrome
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution125 {
    public boolean isPalindrome(String s) {
        //to lower
        s = s.toLowerCase();
        ///处理指定字符以外的字符
        StringBuilder builder = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            int ch = s.charAt(i);
            if (ch >= '0' && ch <= '9' ) {
                builder.append((char)ch);
            }else if(ch >= 'a' && ch <= 'z'){
                builder.append((char)ch);
            }
        }
        s = builder.toString();
        if (s.isEmpty()) {
            return true;
        }
        int left = 0, right = s.length() - 1;
        while (left <= right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}

```
##### 14.合并两个有序数组
```java
import java.util.Arrays;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 *
 * @author lyg  2020/2/7 上午11:54
 * description:
 * 给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。
 * <p>
 * 说明:
 * <p>
 * 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
 * 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
 * 示例:
 * <p>
 * 输入:
 * nums1 = [1,2,3,0,0,0], m = 3
 * nums2 = [2,5,6],       n = 3
 * <p>
 * 输出: [1,2,2,3,5,6]
 * <p>
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/merge-sorted-array
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution88 {
    public void mergeBySelf(int[] nums1, int m, int[] nums2, int n) {
        if (nums2.length == 0 || n == 0) {
            return;
        }
        if (nums1.length == 0 || m == 0) {
            for (int i = 0; i < n; i++) {
                nums1[i] = nums2[i];
            }//end for
            return;
        }
        int length = m + n - 1;
        m--;
        n--;
        for (int i = length; i >= 0; i--) {
            if (nums1[m] > nums2[n]) {
                nums1[i] = nums1[m--];
            } else {
                nums1[i] = nums2[n--];
            }
            if (n < 0 || m < 0) {
                break;
            }
        }///end for
        ///处理nums2剩下的元素
        if (n >= 0) {
            for (int i = 0; i <= n; i++) {
                nums1[i] = nums2[i];
            }
        }///end if
    }

    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // two get pointers for nums1 and nums2
        int p1 = m - 1;
        int p2 = n - 1;
        // set pointer for nums1
        int p = m + n - 1;

        // while there are still elements to compare
        while ((p1 >= 0) && (p2 >= 0)) {
            // compare two elements from nums1 and nums2
            // and add the largest one in nums1
            nums1[p--] = (nums1[p1] < nums2[p2]) ? nums2[p2--] : nums1[p1--];
        }

        // add missing elements from nums2
        System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
    }

    public void mergeV2(int[] nums1, int m, int[] nums2, int n) {
        System.arraycopy(nums2, 0, nums1, m, n);
        Arrays.sort(nums1);
    }
}

```
##### 15.最长回文串
```java
import java.util.HashSet;

/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/27 下午2:46
 * description:
 * 给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。
 *
 * 在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。
 *
 * 注意:
 * 假设字符串的长度不会超过 1010。
 *
 * 示例 1:
 *
 * 输入:
 * "abccccdd"
 *
 * 输出:
 * 7
 *
 * 解释:
 * 我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/longest-palindrome
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution409 {
    /**
     * 考虑可以构成回文串的两种情况：
     *
     * 1.字符出现次数为双数的组合
     * 2.字符出现次数为双数的组合+一个只出现一次的字符
     * 统计字符出现的次数即可，双数才能构成回文。因为允许中间一个数单独出现，比如“abcba”，
     * 所以如果最后有字母落单，总长度可以加 1。首先将字符串转变为字符数组。然后遍历该数组，
     * 判断对应字符是否在hashset中，如果不在就加进去，如果在就让count++，然后移除该字符！
     * 这样就能找到出现次数为双数的字符个数。
     * @param s 目标字符串
     * @return 回文串长度
     */
    public  int longestPalindromeBySelf(String s) {
        if (s.length() == 0)
            return 0;
        // 用于存放字符
        HashSet<Character> hashset = new HashSet<Character>();
        char[] chars = s.toCharArray();
        int count = 0;
        for (char aChar : chars) {
            if (!hashset.contains(aChar)) {
                // 如果hashset没有该字符就保存进去
                hashset.add(aChar);
            } else {
                // 如果有,就让count++（说明找到了一个成对的字符），然后把该字符移除
                hashset.remove(aChar);
                count++;
            }
        }
        return hashset.isEmpty() ? count * 2 : count * 2 + 1;
    }


    /**
     * 贪心思想：
     *
     * 回文串是一个正读和反读都一样的字符串。对一个左边的字符 i 右边一定会有一个对称 i。比如 'abcba'， 'aa'，'bb' 这几个回文串。其中第一个有点特殊，中间的 c 是唯一的。
     *
     * 如果让你来造一个回文串你会怎么造？ 首先让它左右两边绝对对称，如果可能的话再加上一个唯一的中心。
     *
     * 算法
     *
     * 对于每个字母，假设它出现了 v 次。我们可以让 v // 2 * 2 个字母左右对称。例如，对于字符串 'aaaaa'，其中 'aaaa' 是左右对称，其长度为 5 // 2 * 2 = 4。
     *
     * 最后，如果有任何一个满足 v % 2 == 1 的 v，那么这个字符就可能是回文串中唯一的那个中心。针对这种情况，我们需要判断 v % 2 == 1 && ans % 2 == 0，后面的判断主要是为了防止重复计算。
     *
     * 作者：LeetCode
     * 链接：https://leetcode-cn.com/problems/longest-palindrome/solution/zui-chang-hui-wen-chuan-by-leetcode/
     * 来源：力扣（LeetCode）
     * 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
     * @param s 目标串
     * @return 结果长度
     */
    public int longestPalindrome(String s) {
        int[] count = new int[128];
        for (char c: s.toCharArray())
            count[c]++;

        int ans = 0;
        for (int v: count) {
            ans += v / 2 * 2;
            if (v % 2 == 1 && ans % 2 == 0)
                ans++;
        }
        return ans;
    }
}

```
##### 16.最长的回文子序列
```java
/**
 * Copyright (c) 2020.
 * Email: love1208tt@foxmail.com
 * @author lyg  2020/2/27 下午2:08
 * description:
 * 给定一个字符串s，找到其中最长的回文子序列。可以假设s的最大长度为1000。
 *
 * 示例 1:
 * 输入:
 *
 * "bbbab"
 * 输出:
 *
 * 4
 * 一个可能的最长回文子序列为 "bbbb"。
 *
 * 示例 2:
 * 输入:
 *
 * "cbbd"
 * 输出:
 *
 * 2
 * 一个可能的最长回文子序列为 "bb"。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode-cn.com/problems/longest-palindromic-subsequence
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 **/

public class Solution516 {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        int [][] dp = new int[len][len];
        for(int i = len - 1; i>=0; i--){
            dp[i][i] = 1;
            for(int j = i+1; j < len; j++){
                if(s.charAt(i) == s.charAt(j))
                    dp[i][j] = dp[i+1][j-1] + 2;
                else
                    dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
            }
        }
        return dp[0][len-1];
    }
}

```