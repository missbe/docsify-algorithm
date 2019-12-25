## 冒泡排序算法
冒泡排序算法的思路是将相邻的两个数进行比较，小的放在前头，大的放在后面。先比较第1个数和第2个数，小数放前大数放后；然后再比较第2个数和第3个数，
小数放前大数放后；以此类推直到最后两个数进行比较。重复上述步骤，直至排序完成。下面以45 36 18 53 72 30 48 93 15 35这一组数据进行说明：
```
[45 36] 18 53 72 30 48 93 15 35第1次比较，45比36大，互换。
36 [45 18] 53 72 30 48 93 15 35第2次比较，45比18大，互换。
36 18 [45 33] 72 30 48 93 15 35第3次比较，45比53小，不变。
36 18 45 [53 72] 30 48 93 15 35第4次比较，53比72小，不变。
36 18 45 53 [72 30] 48 93 15 35第5次比较，72比30大，互换。
36 18 45 53 30 [72 48] 93 15 35第6次比较，72比48大，互换。
36 18 45 53 30 48 [72 93] 15 35第7次比较，72比93小，不变。
36 18 45 53 30 48 72 [93 15] 35第8次比较，93比15大，互换。
36 18 45 53 30 48 72 15 [93 35]第9次比较，93比35大，互换。
36 18 45 53 30 48 72 15 35 93 第一趟执行完成后的结果。
```
代码如下：
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午3:10
 **/
public class BubbleSort {
    public static void main(String[] args) {
        BubbleSort bubbleSort = new BubbleSort();
        int[] arr = {45, 36, 18, 53, 72, 30, 48, 93, 15, 35};
        InputOutputTools.outputArray(arr);
        bubbleSort.bubbleSort(arr);
        InputOutputTools.outputArray(arr);
    }
    public void bubbleSort(int[] arr) {
        int length = arr.length;
        for (int i = 0; i < length - 1; i++) {
            System.out.println("第" + (i+1) + "趟排序过程:");
            for (int j = 0; j < length - 1 - i; j++) {
                if(arr[j] > arr[j+1]){
                    int tmp = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = tmp;
                }//end swap
                InputOutputTools.outputArray(arr);
            }//end for
            System.out.println("第" + (i+1) + "趟排序结果:");
            InputOutputTools.outputArray(arr);
        }//end for
    }
}
```
## 选择排序
选择排序是从未排序的数据中选出最小的一个元素顺序放在已经排好序的数列最后。
设有n个数，可经n-1趟选择排序得到有序结果。首先把数据看作从1~n的无序区，有序区为空，然后从无序区中选出最小的数i，将它与无序区的第一个数进行交换，
使它与后边的数分别变为新的有序区和无序区。再次从无序区选出最小的数x，将它放在i的后面，直到无序区空为止;
下面以9 5 3 0 1 4 8 2 6 7这一组数据为例进行说明:
```
0 5 3 9 1 4 8 2 6 7这是第一趟排序的结果，最小的数0与第一个数9进行了互换。
0 1 3 9 5 4 8 2 6 7这是第二趟排序的结果，除去已排好序的0，最小的数为I，把它放在0的后面，即1跟5进行了互换。
0 1 2 9 5 4 8 3 6 7这是第三趟排序的结果，除去已排好的，最小的数是2，把它放在已经排好的数1的后面，即2和3进行了互换。
0 1 2 3 5 4 8 9 6 7这是第四趟排序的结果，除去已排好的，最小的数是3，把它放在已经排好序的数字后一位。
0 1 2 3 4 5 8 9 6 7这是第五趟排序的结果，除去已排好的，最小的数是4，把它放在已经排好序的数字后一位。
0 1 2 3 4 5 8 9 6 7这是第六趟排序的结果，除去已排好的，最小的数是5，把它放在已经排好序的数字后一位。
0 1 2 3 4 5 6 9 8 7这是第七趟排序的结果，除去已排好的，最小的数是6，把它放在已经排好序的数字后一位。
0 1 2 3 4 5 6 7 8 9这是第八趟排序的结果，除去已排好的，最小的数是7，把它放在已经排好序的数字后一位。
0 1 2 3 4 5 6 7 8 9这是第九趟排序的结果，除去已排好的，最小的数是8，把它放在已经排好序的数字后一位。
因为只剩下最后一个数字9，所以直接把9放在数据的最后位即可;
```
代码如下：
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/21 下午5:10
 **/
public class SelectionSort {
    public void sort(int[] arr) {
        int len = arr.length;
        for (int i = 0; i < len; i++) {
            int m = i;
            for (int j = i + 1; j < len; j++) {
                if (arr[j] < arr[m]) {
                    m = j;
                }//end if
            }///end for
            if (m != i) {
                int tmp = arr[m];
                arr[m] = arr[i];
                arr[i] = tmp;
            }
        }///end for
        InputOutputTools.outputArray(arr);
    }
}
```
## 直接插入排序算法
直接插入排序算法就是将一个数据插入到已经排好序的有序数据中，从而得到一个新的个数加1的有序数据，此算法适用于少量数据的排序。程序主体是关系运算符以及运算符的优先级;
在程序运行时根据程序的提示输入数字，程序自动排序并输出排好序的数字;插入排序算法是把一个记录插入到已经排好序的有序序列中去，使整个序列在插入了该记录后仍然有序。
插入排序中较简单的一种方法便是直接插入排序，它插入位置的确定是通过将待插入的记录与有序区中的各记录自右向左依次比较其关键字值的大小来确定的。
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午3:25
 **/
public class DirectInsertionSort {
    public static void main(String[] args) {
        int[] valueArr={25,12,36,45,29,39,22,98,37};
        int[] arr = new int[10];
        int i =1;
        for(int value : valueArr){
            arr[i++] = value;
        }
        InputOutputTools.outputArray(arr);
        new DirectInsertionSort().insertSort(arr);
        InputOutputTools.outputArray(arr);
    }
    public void insertSort(int[] arr) {
        int pivot, j;
        for (int i = 2; i < arr.length; i++) {
            System.out.println("第" + (i-1) + "趟排序过程:");
            pivot = arr[i];
            ////比最大的还要大
            if (arr[i - 1] > pivot) {
                for ( j = i - 1; arr[j] > pivot; j--) {
                    arr[j+1] = arr[j];
                }///end for
               arr[j+1] = pivot;
               InputOutputTools.outputArray(arr);
            }//end if
        }///end for
    }
}
```

## 快速排序算法
快速排序算法是对冒泡排序算法的一种改进。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中实现的步骤如下：
1. 设置两个变量ij，排序开始时，i=0，j=n。
2. 以第一个数组元素作为关键数据，赋值给key。
3. 从j开始向前搜索即由后开始向前搜素（j=j-1），找到第一个小于key的值aj]并与a[i]交换。
4. 从i开始向后搜索即由前开始向后搜索（i=i+1），找到第一个大于key的值ai]与ai]交换。
5. 重复步骤（3）~（5），直到ij为止;

下面以45 36 18 53 72 30 48 93 15 35这一组数为例进行说明:
```
35    36    18    53    72    30    48    93    15    53
35    36    18    15    72    30    48    93    72    53
35    36    18    15    30    30    48    93    72    53
30    36    18    15    36    45    48    93    72    53
30    15    18    15    36    45    48    93    72    53
18    15    18    35    36    45    48    93    72    53
15    15    30    35    36    45    48    93    72    53
15    18    30    35    36    45    48    93    72    53
15    18    30    35    36    45    48    53    72    53
15    18    30    35    36    45    48    53    72    93
```
代码实现如下：
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午7:35
 **/
public class QuickSort {
    public static void main(String[] args) {
        int[] arr = {45, 36, 18, 53, 72, 30, 48, 93, 15, 35};
        System.out.println("Quick Sort start:");
        InputOutputTools.outputArray(arr);
        new QuickSort().quickSort(arr, 0, arr.length - 1);
        System.out.println("Quick Sort end:");
        InputOutputTools.outputArray(arr);
    }
    public void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivot = pivot(arr,low,high);
            ///left sort
            quickSort(arr, low, pivot - 1);
            ///right sort
            quickSort(arr, pivot + 1, high);
        }///end
    }
    private int pivot(int[] arr, int low, int high) {
        int pivot = arr[low];
        System.out.println("low = " + low + ",high=" + high);
        System.out.println("pivot = " + pivot + " \t--- start\t");
        while (low < high) {
           while(low < high && arr[high] >= pivot) {
               high--;
           }///end while
           arr[low] = arr[high];
            while (low < high && arr[low] <= pivot) {
                low++;
            }//end while
            arr[high] = arr[low];
            InputOutputTools.outputArray(arr);
        }//end while
        arr[low] = pivot;
        System.out.println("pivot = " + pivot + " \t--- end\t");
        InputOutputTools.outputArray(arr);
        return low;
    }
}
```
## 归并排序算法分析
归并是将两个或多个有序记录序列合并成一个有序序列。归并方法有多种，一次对两个有序记录序列进行归并，称为二路归并排序，也有三路归并排序及多路归并排序。本实例是二路归并排序，基本方法如下：
1. 将n个记录看成是n个长度为1的有序子表;
2. 将两两相邻的有序子表进行归并;
3. 重复执行步骤（2），直到归并成一个长度为n的有序表;

我们来分析一下上面介绍的方法：首先是将元素个数为n的序列看成是n个子序；然后将相邻的两个子序列两两合并并排序，得到n/2个子序列，此时每个序列是有序的。继续将相邻的子序列两两合并，得到n/4个有序子序列。依此类推，重复上述操作，直到合并为一个有序数列。例如，将一组元素为695 458 362 789 12 15 163 23的数列使用二路归并的方法排序的过程如图所示：
```
m=0,n=1,p=0 === 458    695    0    0    0    0    0    0    
m=2,n=3,p=2 === 458    695    362    789    0    0    0    0    
m=0,n=3,p=1 === 362    458    695    789    0    0    0    0    
m=4,n=5,p=4 === 362    458    695    789    12    15    0    0    
m=6,n=7,p=6 === 362    458    695    789    12    15    23    163    
m=4,n=7,p=5 === 362    458    695    789    12    15    23    163    
m=0,n=7,p=3 === 12    15    23    163    362    458    695    789
```
代码实现如下：
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午3:45
 **/
public class MergeSort {
    private int[] arrTmp = new int[8];
    public static void main(String[] args) {
        int p;
//        int[] arr = {8,9,12,44,13,24,33,56};
        int[] arr = {695, 458, 362, 789, 12, 15, 163, 23};
        new MergeSort().mergeSort(arr,arr,0,arr.length-1);
    }
    /**
     * 递归，进行归并排序
     * @param r 原数组
     * @param s 原数组
     * @param m 开始下标
     * @param n 结束下标
     */
    public void mergeSort(int[] r, int[] s,int m, int n) {
        int p;
        if (m == n) {
            s[m] = r[m];
        }else{
            p = (m + n) / 2;
            ///递归调用mergeSort将s[m]-s[p]归并成有序的t[m]-t[p]
            mergeSort(r,arrTmp,m,p);
            ///递归调用mergeSort将s[p+1]-s[n]归并成有序的t[p+1]-t[n]
            mergeSort(r,arrTmp,p+1,n);
            ///调用函数将前两部分归并到s[m]-s[n]中
            merge(arrTmp,r,m,p,n);
            /*
             * 这里需要注意arrTmp是排序后的数据，r是未被排序的；
             * 进行一次merge归并后arrTmp是有序的，因为后面进行归并需要用到r，
             * 所以应该保持有序，将排序后内容赋值给r，r也保持有序；
             */
            if (n + 1 - m >= 0) {
                System.arraycopy(arrTmp, m, r, m, n + 1 - m);
            }
            System.out.print("m="+ m + ",n="+n+",p="+p + " === ");
            InputOutputTools.outputArray(arrTmp);
        }
    }
    /**
     * 将原有R无序数组进行排序放置进S数组
     * @param s 排序后有序的S数组
     * @param r 原来的无序数组
     * @param rStartOne 无序数组第一部分开始位置,下标从0开始
     * @param rStartTwo 无序数组第二部分开始位置,下标从0开始
     * @param rEnd 无序数组是后结束的位置,下标从0开始
     */
    private void merge(int[] s, int[] r, int rStartOne, int rStartTwo, int rEnd) {
        int i, j, k;
        i = rStartOne;
        j = rStartTwo + 1;
        k = rStartOne;
        while (i <= rStartTwo && j <= rEnd) {
            if (r[i] <= r[j]) {
                s[k++] = r[i++];
            } else {
                s[k++] = r[j++];
            }
        }///end while
        while (i <= rStartTwo) {
            s[k++] = r[i++];
        }
        while (j <= rEnd) {
            s[k++] = r[j++];
        }
    }///end function
}
```