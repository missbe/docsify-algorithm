## 最大子段和
动态规划算法与分治法很类似，其基本思想也是将规模较大的问题分解成较小的子问题，先求解子问题，然后通过这些子问题的解得到原问题的解，
但是与分治法不同的是，适合于用动态规划法求解的问题，经过分解后得到的子问题之间并不是相互独立的；

动态规划法的基本思想:动态规划法与分治法的不同之处在于分解后的子问题是相互不独立的。如果使用分治法去解这类问题，分解得到的子问题数目就会很多，而且有些子问题被重复计算多次。如果可以保存已经解决过的子问题的解，需要时再找出已经求解的答案，这样就免去了大量的重复计算。为了达到这一目的，可以用一个表来记录所有已经解决的子问题的答案。这样不管计算过的子问题的答案在后面的求解过程中是否被用到都会被记录，这就是动态规划的思想；

动态规划的应用——最大子段和
给定n个整数（可能为负整数）组成的序列a1，az，an，求该序列的子段和的最大值。当所有整数均为负数时定义其最大子段和为0；
根据上面的问题，我们可以知道在最大子段和的子段序列中不应包含有零或负数。根据此分析并应用动态规划算法的思想，
可以将序列的各子段的和记录到一个子段和的数组中，然后比较子段和数组中的元素从而得到最大子段和；
```
事例一：
7, 8, -5, 4, 6, -2, 3, 1
Max:29
StartIndex:0
EndIndex:7

事例二：
4, -3, 5, -2, -1, 2, 6, -2
```
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午8:27
 **/
public class MaximumSubsectionSum {
    private int startIndex;
    private int endIndex;
    public static void main(String[] args) {
        MaximumSubsectionSum subsectionSum = new MaximumSubsectionSum();
//        int[] arr = {7, 8, -5, 4, 6, -2, 3, 1};
//        int[] arr = {4, 3, -5, -2, -1, 2, 4, -2};
//        int[] arr = {-4, 3, -5, -2, -1, 2, 4, -2};
        int[] arr = {4, 5, -2, 1, -5, 3, 2};
//        int[] arr = InputOutputTools.instanceArray(12);
        long startTime = System.currentTimeMillis();
        System.out.println("Max Sum:"+subsectionSum.maxSum(arr));
        System.out.println("StartIndex:"+subsectionSum.startIndex);
        System.out.println("EndIndex:"+subsectionSum.endIndex);
        long endTime = System.currentTimeMillis();
        System.out.println("Time:" + (endTime-startTime));
        System.out.println("-------------------------------------");
        startTime = System.currentTimeMillis();
        System.out.println("Max Sum:"+subsectionSum.violence(arr));
        System.out.println("StartIndex:"+subsectionSum.startIndex);
        System.out.println("EndIndex:"+subsectionSum.endIndex);
        endTime = System.currentTimeMillis();
        System.out.println("Time:" + (endTime-startTime));
    }
    public int violence(int[] arr) {
        int i, j, max = 0, sum;
        for (i = 0; i < arr.length; i++) {
            sum = 0;
            for (j = i; j < arr.length; j++) {
                sum += arr[j];
                if (sum > max) {
                    max = sum;
                    startIndex = i;
                    endIndex = j;
                }//end if
            }//end for
        }//end for
        return max;
    }
    public int maxSum(int[] arr) {
        int i = 0,j,max = 0;
        int[] currentSum = new int[arr.length];
        int[] maxSum = new int[arr.length];
        for (j = 0; j < arr.length; j++) {
            ////如果前一个子段和大于0则进行相加，否则最大子段和为当前值
            if (j != 0 && currentSum[j - 1] >= 0) {
                currentSum[j] = currentSum[j-1] + arr[j];
            }else{
                currentSum[j] = arr[j];
                ///当前一个子段和已经负时更新开始位置；
                i = j;
            }///end if
            ///sum数组统计从startIndex开始位置到目前位置最大的子段和
            ///如果当前位置的子段和小于前一个位置的子段和，修改当前位置最大子段和为前一个位置的子段和
            ///如果当前位置的子段和比前一个位置的子段和值大，修改startIndex开始位置和当前位置的子段和
            if (j != 0 && currentSum[j] <= maxSum[j - 1]) {
                maxSum[j] = maxSum[j - 1];
            } else {
                maxSum[j] = currentSum[j];
                ///记录最大值
                max = maxSum[j];
                ///记录起始位置
                startIndex = i;
                ////如果当前位置的子段和大于sum维持的最大子段和更新结束位置
                endIndex = j;
            }//end if
        }///end for
        System.out.println("MAX SUM:" );
        InputOutputTools.outputArray(currentSum);
        System.out.println("SUM:" );
        InputOutputTools.outputArray(maxSum);
        ////返回最大值
        return max;
    }
}
```