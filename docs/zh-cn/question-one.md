## 汉诺塔问题
任何可以使用计算机求解的问题所需的计算时间都与其规模有关。问题的规模越小，解题所需要的计算时间就越少，而且也较容易解决。要想直接解决一个较大的问题，  有时操作起来是相当困难的。分治法的设计思想是，将一个难以直接解决的大问题，分割成一些规模较小的相同问题，以便各个击破，分而治之。
实际上，由分治法产生的子问题往往是原问题的缩小版，这样就为使用递归技术提供了方便。分治与递归就像一对挛生兄弟，在设计算法时经常是同时应用的;
> 汉诺塔问题是一个古典数学问题，是递归算法的典型例子。其内容是：古代有一个梵塔，塔内有3个座A，B，C，A座上64个盘子，盘子的大小不等，大的在下面，小的在上面。
有一个老和尚想把这64个盘子从A座移到C座，但每次只允许移动一个盘子，且在移动过程中3个座上的盘子都要保持大盘子在下面，小盘子在上面;

知道了问题，那么我们就来讨论一下过程： 要将n个盘子从一个座移到另一个座上每个移动者要做的工作，除了第一个移动者外，每个移动者都要命令其他的移动者。就是第一个移动者开始，任务层层下放，最后将一个盘子从一个座上移到另一个座上的，这是第一个移动者自己做的工作;
```
n=3的情况：
  A->C
  A->B
  C->B
  A->C
  B->A
  B->C
  A->C
```
```
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/22 下午7:57
 **/
public class Hanoi {
    public static void main(String[] args) {
        new Hanoi().hanoi(32,'A','B','C');
    }
    /**
     * 递归函数定义：
     *  实现n个盘子，从A柱通过B移动到C柱
     * @param n 盘子个数
     * @param A A柱
     * @param B B柱
     * @param C C柱
     */
    public void hanoi(int n, char A, char B, char C) {
        if (n == 1) {
            move(A, C);
        }else{
            ///将A柱上的n-1个盘子通过C移动到B柱上
            hanoi(n - 1, A, C, B);
            ///移动A柱上面的最后一个盘子到C柱上
            move(A, C);
            ///将B柱上剩下的n-1个盘子通过A移动到C柱上
            hanoi(n - 1, B, A, C);
        }
    }
    private void move(char x,char y) {
        System.out.println("move "+x + "->" + y);
    }
}
```
## 找零钱问题一
如果我们有面值为1元、3元和5元的硬币若干枚，如何用最少的硬币凑够11元？
 
动态规划的思想:
 
  1. 当我们遇到一个大问题时，总是习惯把问题的规模变小，这样便于分析讨论；
 
  2. 这个规模变小后的问题和原来的问题是同质的，除了规模变小，其它的都是一样的，本质上它还是同一个问题(规模变小后的问题其实是原问题的子问题)；
 
动态规划的求解过程中有两个概念:状态和状态转移方程； 状态就是如何描述这个问题并且如何求解的过程；  在这个问题中：可以采用dp(i)=j来表示i元由j个硬币组成的;  表示出了这个子问题过后，进行如何求解的过程。dp(0)=0表示0元由0个硬币组成，dp(1)=dp(1-1)+1,表示拿起一个面值为1的硬币，接下来只需要凑够0元即可，而这个是已经知道答案的，即d(0)=0；  在大部分的动态规划中都有当前状态依赖于前一个状态，并且独立于后面的状态。要当前所求的值最小，只要满足前一个依赖的状态最小就可以达到要求。  再接下来分析,dp(2)=dp(2-1)+1,因为还是只有1元硬币可以使用。所以只能取一个一元的硬币，凑够dp(1)就即可。  在dp(3)时可以有两种取法。一种是dp(3)=dp(3-1)+1或者dp(3)=dp(3-3)+1.分别表示取一个一元硬币和先取3元硬币，就有两种值。从这些状态中我们可以推出  状态转移方程：d(i)=min{ d(i-vj)+1 }，其中i-vj >=0，vj表示第j个硬币的面值;有了状态转移方程，就可以很快解决这个问题。具体如下:
```
i = 1    dp = 1
i = 2    dp = 2
i = 3    dp = 1
i = 4    dp = 2
i = 5    dp = 1
i = 6    dp = 2
i = 7    dp = 3
i = 8    dp = 2
i = 9    dp = 3
i = 10    dp = 2
i = 11    dp = 3
```
```java
import java.util.Arrays;
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/28 下午2:54
 *
 **/
public class TheProblemOfFindingChange {
    public static void main(String[] args) {
        int[] coins = {1,3,5};
        int yuan = 11;
        System.out.println("Count:" + new TheProblemOfFindingChange().dynamicSolveMethod(coins,yuan));
    }
    /**
     * 动态规划方法：指定金额最少硬币划分
     * @param coins 硬币面值数组
     * @param yuan 指定的金额
     * @return 最终需要硬币个数
     */
    public int dynamicSolveMethod(int[] coins, int yuan) {
        int[] dp = new int[yuan+1];
        ////初始化为最大值
        Arrays.fill(dp, Integer.MAX_VALUE);
        ///起点为0
        dp[0] = 0;
        for (int i = 1; i <= yuan; i++) {
            ////找到硬币面值小于当前金额，并且dp值最小
            for (int j = 0; j < coins.length; j++) {
                if(i >= coins[j] && (dp[i-coins[j]] + 1 < dp[i])) {
                    dp[i] = dp[i-coins[j]] + 1;
                    System.out.print("\ti = " + i + "\tdp = " + dp[i]);
                }
            }//end for
            System.out.println();
        }///end for
        System.out.println("DP:");
        InputOutputTools.outputArray(dp);
        return dp[yuan];
    }///end function
}
```
## 找零钱问题二
如果我们有面值为1元、3元和5元的硬币若干枚，如何用最少的硬币凑够11元？
贪心算法的基本概念：首先来看一个贪心算法的经典案例——找零钱的问题。假设有3种硬币，面值分别是1元、5角和1角。这3种硬币各自数量不限，如果现在需要给一位顾客找2元7角，请问如何才能使找出的硬币个数最少。
我们也许会不假思索地找出2枚1元的、1枚5角的和2枚一角的。这样就下意识地使用了贪心算法，每一步判断都尽量使用最大的硬币，也就下意识地遵循了下面的步骤：
  1. 首先找到的是面值不超过2元7角的最大硬币，也就是1元的。
  2. 然后从2元7角中减去1元，还剩1元7角，再找一个面值不超过1元7角的最大硬币，还是1元的；
  3. 然后从1元7角中再减去1元，得到7角的余额，再找一个面值不超过7角的最大硬币，那就是5角的；
  4. 从7角中减去5角，还剩2角，再找一个面值不超过2角的最大硬币，那就是1角的；
  5. 从2角中减去1角，还剩1角，再找一个面值不超过1角的最大硬币，那就是1角的。整个找钱过程结束；

上面的整个找钱过程就应用了典型的贪心算法的思想。不难看出，贪心算法总是作出在当前看来是最好的选择，也就是说贪心算法并不是从整体最优上考虑的，也就是说它所作出的选择只是在某种意义上的局部最优选择。
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/28 下午3:48
 *
 **/
public class TheProblemOfFindingChangeTwo {
    public static void main(String[] args) {
        ///解决方案一：动态规划,1元=10角 2元7角=27角
        int[] coins = {5, 3, 1};
        int yuan = 11;
        System.out.println("DynamicSolveMethod Count:" + new TheProblemOfFindingChange().dynamicSolveMethod(coins,yuan));
        System.out.println("GreedySolveMethod Count:" + new TheProblemOfFindingChangeTwo().greedySolveMethod(coins,yuan));
    }
    /**
     * 贪心方法：指定金额最少硬币划分，每次选最大
     * @param coins 硬币面值数组，从大到小排序
     * @param yuan 指定的金额
     * @return 最终需要硬币个数
     */
    private int greedySolveMethod(int[] coins,int yuan) {
        int number = 0;
        while (yuan > 0) {
            int j = 0;
            ////找到硬币面值最大的一个
            while (yuan - coins[j] < 0) {
                j++;
            }///end while
            yuan -= coins[j];
            ///+1
            number++;
            System.out.print(coins[j] + "\t");
        }///end while
        System.out.println();
        return number;
    }///end function
}
```
## 最大公约数
常用算法:辗转相除法是求两个自然数的最大公约数的一种方法，也叫欧几里德算法。辗转相除法的原理如下:

例如，求（319，377）：

∵ 319÷377=0（余319）

∴（319，377）=（377，319）；

∵ 377÷319=1（余58）

∴（377，319）=（319，58）；

∵ 319÷58=5（余29），

∴ （319，58）=（58，29）；

∵ 58÷29=2（余0），

∴ （58，29）= 29；

∴ （319，377）=29.

通过辗转相除从而得到最大公约数，暂时想到这个方法。以后再进行补充，以备忘。求最小公倍数就是这两个数的乘积除以最大公约数即是，所以关键的问题还是怎么求最大公约数：
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/28 下午2:38
 *
 **/
public class GreatestCommonDivisor {
    public static void main(String[] args) {
        int max = 377, min = 319;
        int remainder = new GreatestCommonDivisor().euclideanAlgorithm(max,min);
        System.out.println("最大公约数:" + remainder);
        System.out.println("最小公倍数:" + (max * min)/remainder);
    }
    /**
     * 欧几里德算法求M和N的最大公约数
     * @param max 数一
     * @param min 数二
     * @return M和N的最大公约数
     */
    private int euclideanAlgorithm(int max,int min){
        if (max < min) {
            int t = max;
            max = min;
            min = t;
        }///end swap
        int remainder = max % min;
        while (remainder > 0) {
            max = min;
            min = remainder;
            remainder = max % min;
        }///end while
        return min;
    }
}
```
## 解一元二次方程
一元二次方程 ax2+bx+c=0 的a、b、c三个参数由用户自行定义，通过该程序输出该一元二次方程的根;
```java
import java.util.Scanner;
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/28 下午5:31
 *
 **/
public class Equation {
    public static void main(String[] args)
    {
        //从键盘接收数据
        Scanner scan = new Scanner(System.in);
        //输出函数
        System.out.println("请输入a,b,c,并以空格隔开：");
        int a=scan.nextInt();
        int b=scan.nextInt();
        int c=scan.nextInt();
        double delta=b*b-4*a*c;
        //根据判别式delta=b*b-4ac来判断方程的根
        if (delta>0)
        {
            double x1=(-b+Math.sqrt(delta))/(2*a);
            double x2=(-b-Math.sqrt(delta))/(2*a);
            System.out.println("有两个实数根，分别为：x1="+x1+"x2="+x2);
        }
        else if (delta==0)
        {
            double x1 = -b/(2*a);
            System.out.println("方程只有一个实根，x1=x2="+x1);
        }
        else if (delta<0)
        {
            System.out.println("方程无实根");
        }
    }
}
```
## 分解质因数
分解质因数即把一个合数写成几个质数相乘的形式表示,如12=2×2×3;
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 *
 * @author lyg   2019/9/28 下午5:35
 * 分解质因数即把一个合数写成几个质数相乘的形式表示，例如12=2×2×3，12为合数，
 * 2和3为质因数，任何一个合数都可以写成几个质数相乘的形式。
 **/
public class DecompositionPrimeFactor {
    public static void main(String[] args) {
        new DecompositionPrimeFactor().decompositionPrimeFactor(136);
    }
    public void decompositionPrimeFactor(int primeFactor) {
        int srcPrimeFactor = primeFactor;
        if (primeFactor % 2 == 0) {
            int i = 2;
            StringBuilder builder = new StringBuilder();
            ////1所有数都能除尽，从2开始
            while (primeFactor >= 2 ) {
                if (primeFactor % i == 0) {
                    primeFactor /= i;
                    builder.append(i).append("*");
                    i = 2;
                }else{
                    i++;
                }///end if
            }///end while
            System.out.println(srcPrimeFactor + "=" + builder.substring(0, builder.length() - 1));
        } else {
            System.out.println(primeFactor + "is not.");
        }
        System.out.println();
    }///end
}
```