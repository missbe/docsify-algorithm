# 公共工具代码

> 此页放置的是实现算法过程中需要用到的一些公共使用的工具代码或者类；
## InputOutputTools
```java
import cn.hutool.core.util.RandomUtil;
import java.util.Scanner;
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/21 下午5:01
 * description:公共输入输出类.
 **/
public class InputOutputTools {
    public static int[] instanceArray(int length){
        int[] arr = new int[length];
        for (int i = 0; i < length; i++) {
            arr[i] = RandomUtil.randomInt(1000);
        }
        System.out.println("instance arr:");
        InputOutputTools.outputArray(arr);
        return arr;
    }
    public static void outputArray(int[] arr) {
        System.out.println("start output arr.");
        int i =0;
        for (int value : arr) {
            System.out.print(i + ":" +value + "\t");
            System.out.print(value + "\t");
            i++;
        }
        System.out.println();
        System.out.println("\nend output arr.");
    }
    public static int inputInteger() {
        System.out.println("please input find num:");
        Scanner sc = new Scanner(System.in);
        return sc.nextInt();
    }
}
```


