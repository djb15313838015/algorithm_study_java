## 算法复杂度分析
### 什么是复杂度分析？
1. 数据结构和算法解决的是“如何让计算机更快时间，更小空间解决问题”；
2. 因此需要从执行时间和占用空间两个维度来评估数据结构和算法性能；
3. 分别使用空间复杂度和时间复杂度两个概念来描述性能问题，二者统称为复杂度分析；
4. 复杂度描述的是算法执行时间（占用空间）和数据规模的增长关系；
### 为什么需要复杂度分析?
1. 和性能测试相比，复杂度分析有不依赖执行环境，成本低，效率高，易操作、指导性强的特点
2. 掌握复杂度分析，对编写出性能更优的代码，以及重构、调优代码有指导性的意义；
### 如何进行复杂度分析？
1. 大O表示法
    1. 来源：
    算法的执行时间与每行代码的执行次数成正比，用T(n) = O(f(n))来表示，其中T(n)表示算法执行总时间，f(n)表示每行代码执行总次数，n表示数据的规模；
    2. 特点
    以时间复杂度为例，由于时间复杂度描述的是算法执行时间与数据规模的增长变化趋势，所以常量阶、低阶以及系数实际对这种增长趋势不产生决定性影响，所以在做时间复杂度分析时忽略这些项；

    3. 时间复杂度分析方法
***
* 只关注循环执行次数最多的一段代码
```java
public class ForeachDemo{
    public int cal(int n){
        int sum = 0;            //1
        int i = 0;              //2
        for(; i <= n; i++){      //3
            sum = sum + i;      //4
        }                       //5
        retuen sum;
    }
}
```
分析：1、2行代码都是常量的执行时间和n无关，执行次数最多的是3、4行代码，总共执行了n次，所以总的时间复杂度就是O(n);
***
* 加法法则：总复杂度等于量级最大的那段代码的复杂度
```java
public class ForeachDemo{
    public int cal(int n){
        int sum1 = 0;             //1
        int i = 0;                //2
        for(; i <= n; i++){        //3
            sum1 = sum1 + i;      //4
        }                         
        int sum2 = 0;             
        int j = 0;        
        int x = 0;
        for(; j <= n; j++){            //5
            for(; x <= n; x++){        //6
                sum2 = sum2 + j + x;   //7
            }
        }
        int sum3 = sum1 + sum2;
        retuen sum3;
    }
}
```
分析： 3、4行的时间复杂度是O(n), 6、7行的时间复杂度是O(n²),总复杂度取量级最大的时间复杂度，所以为O(n²);
***
* 乘法法则：嵌套代码的复杂度等于嵌套内外代码复杂度的乘积
```java
public class ForeachDemo{
    public int cal(int n){
        int ret = 0;             //1
        int i = 0;                //2
        for(; i <= n; i++){        //3
            ret = ret + f(i);      //4
        }                         
        retuen ret;
    }
    private int f(int n){
        int sum = 0;
        int i = 0;
        for(; i <= n; i++){       //5
            sum = sum + i;        //6
        }
        return sum;
    } 
}
```
分析： 假设f()只是一个普通操作，那么cal()函数3、4行的时间复杂度为T1(n) = O(n),但是f()函数本身5、6行的时间复杂度为T2(n) = O(n),所以cal()函数的时间复杂度为T(n) = T1(n) * T2(n) = O(n²);
***

### 常用的复杂度级别？
1. 多项式阶：随着数据规模的增长，算法的执行时间和空间占用，按照多项式的比例增长。
    包括：O(1)-常数阶、O(㏒n)-对数阶、O(n)-线性阶、O(n㏒n)-线性对数阶、O(n²)-平方阶、O(n³)-立方阶；
2. 非多项式阶：随着数据规模的增长，算法的执行时间和空间占用暴涨，这类算法性能极差。
    包括：O(2ⁿ)-指数阶、O(n!)-阶乘阶
    
### 如何掌握好复杂度分析方法？
复杂度分析关键在于多练、多分析；

### 复杂度分析的四个概念
1. 最好情况时间复杂度：代码在最理想情况下执行的时间复杂度；
2. 最坏情况时间复杂度：代码在最坏情况下执行的事件复杂度；
3. 平均时间复杂度：用代码在所有情况下执行的次数的加权平均值表示；
4. 均摊时间复杂度：在代码执行的所有复杂度情况中绝大部分是低级别的复杂度，个别情况是高级别复杂度且发生具有时序关系时，可以将个别高级别复杂度均摊到低级别复杂度上，基本上均摊结果就等于低级别复杂度；

### 为什么引入这四个概念？
1.同一段代码在不同的数据情况下，时间复杂度会出现量级差异，为了更全面，更准确的选择算法，所以引入这四个概念；
如：
```java
public class Demo1{
    public int find1(int[] array, int x){
        int i = 0;
        int pos = -1;
        for(; i < array.length; i++){
            if(array[i] = x){
                pos = i;
                break;
            }
        }
        return pos;
    } 
    public int find2(int[] array, int x){
        int i = 0;
        int pos = -1;
        for(; i < array.length; i++){
            if(array[i] = x){
                pos = i;
            }
        }
        return pos;
    }    
}
```
分析：
find1()
1. 最好时间复杂度，正好数组中的第一个元素就是要找的值，则时间复杂度为O(1);
2. 最坏时间复杂度，数组的最后一个元素是要找的值，或者数组中不存在要找的值，则时间复杂度为O(n);
3. 平均时间复杂度，要找的x要么在数组中，要么不在，我们假设在数组中和不在数组中的概率都是1/2；
另外要查找的数据出现在0~n-1的位置的概率也是一样的，为1/n。那么平均复杂度的计算公式为
***
    1*1/2n + 2*1/2n + 3*1/2n..... + n/2n + n*1/2 = 3n+1/4
***
用大O表示法来表示，去掉系数和常量，那么这段代码的平均事件复杂度就是O(n);

find2()
1. 最好时间复杂度，即使找到也需要遍历完成，所以为O(n);
2. 最坏时间复杂度，不存在或者寻找的值是最后一个，需要遍历完成，所以也为O(n);
3. 平均时间复杂度，存在不存在都要遍历完全数组，所以为O(n);

总结：大部分情况不需要去区分最好，最坏和平均，但是通过上面的演示可以发现，对代码编写有一定的指导意义；

### 均摊时间复杂度和平均时间复杂度区别？
```java
public class Demo{
    int[] array = new int[n];
    int count = 0;
    public void insert(int val){
        if(count == array.length){
            int sum = 0;
            for(int i = 0; i < array.length; i++){
                sum = sum + array[i];
            }
            array[0] = sum;
            count = 1;
        }
        array[count] = val;
        ++count;
    }
}
```
分析： 
最好时间复杂度： 数组里有空闲空间，只需要将数据插入数组下标为count的位置就可以了，为O(1);
最坏时间复杂度： 数组满了，需要遍历求和，然后再讲数据插入，为O(n);
平均时间复杂度： 根据数组插入位置的不同，发现n-1次都是O(1)的复杂度，只有数组满了才是O(n)，所以平均复杂度为O(1)；
均摊时间复杂度： O(1)时间复杂度的插入和O(n)时间复杂度的插入，出现很有规律，而且有一定的前后时序关系，
一般都是一个O(n)复杂度的插入，紧跟着n-1个O(1)复杂度的插入，循环往复；这种情况，就可以理解为将O(n)复杂度的插入
均摊到每次O(1)复杂度的插入中，一般均摊时间复杂度就等于最好时间复杂度；








