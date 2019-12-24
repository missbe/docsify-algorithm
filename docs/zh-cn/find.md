## 折半查找
折半查找是一种高效的查找算法，它可以明显地减少比较次数，提高查找效率。但是，折半查找的先决条件是查找表中的数据元素必须是有序的。
折半查找的算法如下：
首先，需要设3个变量low、mid、high，分别保存数组元素的开始、中间和末尾的序号。
假定有10个元素，开始令low=0，high=-9，mid=（low+high）/2=4接着进行以下判断:

（1）如果序号为mid的数组元素的值与x相等，表示查找到了数据，返回该数据的序三号mid。

（2）否则，如果假设x<a[mid]，表示要查找的数据x位于low与mid-1序号之间，就不需要再去查找mid与high序号之间的元素了。因此，将high变量的值改为mid-1，重新查找low与mid-1即high变量的新值）之间的数据。

（3）如果假设x>a[mid]，表示要查找的数据x位于mid+1与high序号之间，就不需要再去查找low与mid序号之间的元素了。因此，将low变量的值改为midtl，重新查找mid+1（即low变量的新值）与high之间的数据。

（4）逐步循环，如果到low>high时，还未找到目标数据x，则表示数组中无此数据;

```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/21 下午5:06
 **/
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = InputOutputTools.instanceArray(12);
        SelectionSort selectionSort = new SelectionSort();
        selectionSort.sort(arr);
        int findNum = InputOutputTools.inputInteger();
        System.out.println(new BinarySearch().binarySearch(arr, findNum));
    }
    public int binarySearch(int[] arr, int findNum) {
        int low = 0, high = arr.length - 1, mid, i = 0;
        while (low <= high) {
            mid = (low + high) / 2;
            if (arr[mid] == findNum) {
                System.out.println("count:" + i + ",index:" + mid + ",value:" + findNum);
                return mid;
            } else if (arr[mid] > findNum) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
            i++;
            System.out.println("count:" + i + ",value:" + arr[mid]);
        }///end while
        System.out.println("count:" + i);
        return -1;
    }
}
```
## 顺序查找算法
顺序查找的基本思想是从表的一端开始，顺序扫描线性表，依次将扫描到的结点、关键字和给定值k相比较，若当扫描到的结点与k相等，则查找成功；若扫描结束后仍未找到等于k的结点，则查找失败；
```java
import java.util.Scanner;
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/21 下午4:49
 **/
public class SequentialSearch {
    public static void main(String[] args) {
        int[] arr = InputOutputTools.instanceArray(12);
        System.out.println("please input find num:");
        Scanner sc = new Scanner(System.in);
        int num = sc.nextInt();
        for (int i = 0; i < arr.length; i++) {
            if (num == arr[i]) {
                System.out.println("index:" + i + ",value:" + arr[i]);
                break;
            }
        }///end for
        System.out.println("end.");
    }
}
```

## 哈希查找算法
哈希查找算法是通过计算数据元素的存储地址来进行查找的一种算法。
### 哈希查找的操作步骤：
1. 用给定的哈希函数构造哈希表；
2. 根据选择的冲突处理方法解决地址冲突；
3. 在哈希表的基础上执行哈希查找；

### 建立哈希表的步骤：
1. 取数据元素的关键字key，计算其哈希函数值（地址）。若该地址对应的存储空间还没有被占用，则将该元素存入；否则执行步骤2解决冲突。
2. 根据选择的冲突处理方法，计算关键字key的下一个存储地址。若下一个地址仍被占用，则继续执行步骤2，直到找到能用的存储地址为止。

### 哈希查找的步骤：
设哈希表为[0~m-1]，哈希函数取H（key），解决冲突的方法是R（x）。
1. 给定k值，计算哈希地址Di=H（k）；若HST为空，则查找失败；若HST=k，则查找成功；否则，执行步骤2（处理冲突）。
2. 重复计算处理冲突的下一个存储地址Dk=R（Dk-1），直到HST[Dk]为空，或HST[Dk]=k为止。若HSR[Dk]=k，则查找成功，否则查找失败。
在应用哈希查找时我们要先设定一个哈希函数和一种处理冲突的方法。

### 哈希函数构造的方法：
#### 直接定址法
取关键字或关键字的某个线性函数值为哈希地址。即：
H（key）=key或H（key）=a*key+b其中，a和b为常数（这种哈希函数叫作自身函数）;
#### 数字分析法
假设关键字是以r为基数的，并且哈希表中可能出现的关键字都是事先知道的，则可取关键字的若干数位组成哈希地址。
#### 平方取中法
取关键字平方后的中间几位作为哈希地址。这是一种较为常用的构造哈希函数的方法。通常在选定哈希函数时不一定能知道关键字的全部情况，取关键字平方后的数的其中哪几位也不确定，而又因为一个数平方后的中间几位数和这个数的每一位都相关，由此使随机分布的关键字得到的哈希地址也是随机的。所取位数由表长决定。
#### 折叠法
将关键字分割成位数相同的几部分（最后一部分的位数可以不同），然后取这几部分的叠加和（舍去进位）作为哈希地址，这种方法称为折叠法。关键字位数很多，而且关键字中每一位的数字分布大致均匀时，可以采用折叠法得到哈希地址。
#### 除留余数法
取关键字被某个不大于哈希表表长m的数p除后所得余数为哈希地址。即H（key）=key MO-p，p<=m这是一种最简单、最常用的构造哈希函数的方法。它不仅可以对关键字直接取模（MOD），也可在折叠法、平方取中法等运算之后取模。
值得注意的是，在使用除留余数法时，对p的选择很重要。若p选得不好，容易产生同义词（具有相函数值的关键字对哈希函数来说称作同义词）。
一般情况下，可以选p为质数或不包含小于20的质因数的合数。
#### 随机数法
选择一个随机函数，取关键字的随机函数值为它的哈希地址，即H（key）=random（key），其中random为随机函数，通常，当关键字长度不等时采用此法构造哈希函数较为恰当。
实际工作中需视不同的情况采用不同的哈希函数。通常需要考虑的因素有：
①.计算哈希函数所需的时间（包括硬件指令的因素）。
②关键字的长度。③哈希表的大小。
④关键字的分布情况。⑤记录的查找频率;
处理冲突的方法：
#### 开放定址法
Hi=（H（key）+d）MOD m i=1，2，3…，k（k≤m-1）
其中：H（key）为哈希函数；m为哈希表表长；d为增量序列；可有下列3种取法：
①d=1，2，3…，m-1，线性探测再散列；
②-d=12，-12，22-22，32…，Ek2（k≤m/2）称二次探测再散：
③-d；=伪随机探测再散列。
#### 再哈希法
H，=RHH（key）i=l，2，3…k
RH；均是不同的哈希函数，即在同义词产生地址冲突时计算另一个哈希函数地址，直到冲突不再发生。这种方法不易产生“聚集”（在处理冲突过程中发生的两个第一个哈希也址不同的纪录，争夺同一个后继哈希地址的现象称为“二次聚集”，即“聚集），但增加了计算的时间。
#### 链地址法
将所有关键字为同义词的记录存储在同一线性链表中。假设某哈希函数产生的哈希地址在区间[0，m-1]上，则设立一个指针型向量。
Chain ChainHash[m]；其每个分量的初始状态都是空指针。凡哈希地址为i的记录都插入到头指针为ChainHash[i]的链表中。在链表中的插入位置可以在表头或表尾；也可以在中间，以保持同义词在同一线性表中按关键字有序。
#### 建立一个公共溢出区
这也是处理冲突的一种方法。假设哈希函数的值域为[0，，m-1]，则设向量Hash Table[0…
m-1]为基本表，每个分量存放一个记录，另设立向量Over Table[0v]为溢出表。所有关键字和基本表中关键字为同义词的记录，不管它们由哈希函数得到的哈希地址是什么，一旦发生冲突都填入溢出表。
```java
import cn.hutool.core.util.RandomUtil;
import java.util.ArrayList;
import java.util.List;
public class HashSearch {
    private static int PrimeFactor = 7;
    public int hash(int num) {
        int p;
        p = num % PrimeFactor;
        return p;
    }///end function
    public HashTable initHashTable(int num) {
        HashTable hashTable = new HashTable(num);
        for (int i = 0; i < num; i++) {
            ElementType type = new ElementType();
            type.name = RandomUtil.randomString(5);
            ///initialize -1
            type.num = -1;
            System.out.print( type);
            hashTable.elementTypes.add(type);
        }
        System.out.println();
        return  hashTable;
    }///end function
    public void outputHashTable(HashTable hashTable) {
        for (ElementType type : hashTable.elementTypes) {
            System.out.print(type);
        }
        System.out.println();
    }
    public int searchHash(HashTable table,int key) {
        int count = 0,flag = 1;
        int p = hash(key),index=p;
        while (table.elementTypes.get(index).num != key && table.elementTypes.get(index).num != -1) {
            if (flag == 1) {
                ///两次进行一次加一
                count++;
                index = (p + (count * count)) % PrimeFactor;
                System.out.println("+ alter index：" + index);
                ///变换 +1 -1 +4 -4
                flag = -1;
            }else{
                int tmp = (p - (count * count)) % PrimeFactor;
                if (tmp >= 0) {
                    index = tmp;
                    System.out.println(" - alter index：" + index);
                }
                flag = 1;
            }
        }///end while
        if (table.elementTypes.get(index).num == key || table.elementTypes.get(index).num == -1) {
            System.out.println("result index:" + index);
            return  index;
        }
        return -1;
    }///end function
    public void insertHash(HashTable hashTable, int num) {
        int p = searchHash(hashTable, num);
        if (p >= 0) {
            hashTable.elementTypes.get(p).num = num;
            hashTable.count++;
        }
        System.out.println("p->" + p);
    }///end function
    public static void main(String[] args) {
//        Scanner sc = new Scanner(System.in);
//        int p = sc.nextInt();
        HashSearch hashSearch = new HashSearch();
        HashTable hashtable = hashSearch.initHashTable(PrimeFactor);
        int[] arr = {4,11,18,5,25,32};
        for (int i = 0; i < arr.length; i++) {
             System.out.println("please input number:");
            int p = arr[i];
            int location = hashSearch.searchHash(hashtable, p);
            if (location >= 0) {
                hashSearch.insertHash(hashtable,p);
                hashSearch.outputHashTable(hashtable);
            }else{
                System.out.println("already exist.");
            }
        }///end for
    }
}
class ElementType{
    int num;
    String name;
    @Override
    public String toString() {
        return "[num="+num+",name="+name +"]";
    }
}///end class
class HashTable{
    List<ElementType> elementTypes;
    int count;
    int sizeIndex;
    public HashTable(int num){
        elementTypes = new ArrayList<>(num);
        count = 0;
        sizeIndex = num;
    }
}///end class
```
## 分块查找算法
分块查找算法的描述是将n个数据元素“按块有序”划分为m块（m≤n）。每一块中的结点不必有序，但块与块之间必须“按块有序”；即第一块中任一元素的关键字都必须小于第二块中任一元素的关键字；而第二块中任一元素的关键字又都必须小于第三块中的任一元素的关键字,操作步骤如下：
1. 先取各块中的最大关键字构成一个索引表。
2. 查找分为两部分，先对索引表进行二分查找或顺序查找，以确定待查记录在哪一块中。
3. 然后，在已经确定的块中用顺序法进行查找。
#### 分块查找的平均查找长度（ASLbs）：
分块查找是两次查找过程，整个查找过程的平均查找长度是两次查找的平均查找长度之和。其计算公式为：ASLbs=L6+Lw其中Lb表示查找索引表以确定记录所在块的平均查找长度；Lw表示在查找到的块中，查找记录的平均查找长度。
+ 以二分查找来确定块，分块查找成功时的平均查找长度ASLs-L6+Lw=1g（b+l）-1+（s+1）/2=lg(n/s+1)+s/2；
+ 以顺序查找来确定块，分块查找成功时的平均查找长度ASLb。-Lh+Lw=（b+1）/2+（s+1）/2=(s2+2s+n)/(2s);
> 分块查找中块的大小：可根据表的特征进行分块，不一定要将线性表分成大小相等的若干块。各块可放在不同的向量中，也可将每一块存放在一个单链表中

#### 分块查找的优点：
1. 在表中插入或删除一个记录时，只要找到该记录所属的块，就在该块内进行插入和删除运算。
2. 因块内记录的存放是任意的，所以插入或删除比较容易，无须移动大量记录。
分块查找算法的主要代价是增加一个辅助数组的存储空间;
```java
/**
 * Email: love1208tt@foxmail.com
 * Copyright (c)  2019. missbe
 * @author lyg   2019/9/21 下午5:44
 **/
public class BlockLookup {
    public static void main(String[] args) {
        int[] arr = InputOutputTools.instanceArray(12);
        SelectionSort selectionSort = new SelectionSort();
        selectionSort.sort(arr);
        int findNum = InputOutputTools.inputInteger();
        int blockSize = 4;
        System.out.println(new BlockLookup().blockLookup(arr,findNum,blockSize));
    }
    public int blockLookup(int[] arr, int findNum, int blockSize) {
        int length = arr.length;
        int index = 0;
        while (index < length-1 && findNum > arr[index]) {
            System.out.println("index:" + index + ",value:"+ arr[index]);
            index = index + blockSize;
        }
        System.out.println("index:" + index);
        int endIndex = index,startIndex = index - blockSize;
        while (startIndex < endIndex ) {
            System.out.println("index:" + startIndex + ",value:"+ arr[startIndex]);
            if (arr[startIndex] == findNum) {
                return index;
            }
            startIndex++;
        }
        return -1;
    }
}
```
