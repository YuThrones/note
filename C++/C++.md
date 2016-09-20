#C++
##Union
* “联合”与“结构”有一些相似之处。但两者有本质上的不同。在结构中各成员有各自的内存空间， 一个结构变量的总长度是各成员长度之和（空结构除外，同时不考虑边界调整）。而在“联合”中，各成员共享一段内存空间， 一个联合变量的长度等于各成员中最长的长度。应该说明的是， 这里所谓的共享不是指把多个成员同时装入一个联合变量内， 而是指该联合变量可被赋予任一成员值，但**每次只能赋一种值， 赋入新值则冲去旧值。**  

```
＃include <stdio.h>
union
{
　　int i;
　　char x[2];
}a;

void main()
{
　　a.x[0] =10; 
　　a.x[1] =1;
　　printf("%d",a.i);
}
答案：266 (低位低地址，高位高地址，内存占用情况是Ox010A）
```

##static_cast, dynamic_cast, reinterpret_cast, const_cast区别比较
* static_cast <new_type> (expression) 静态转换
 

静态转换是最接近于C风格转换，很多时候都需要程序员自身去判断转换是否安全。比如：

double d=3.14159265;

int i = static_cast<int>(d);
* dynamic_cast <new_type> (expression) 动态转换
 

动态转换确保类指针的转换是合适完整的，它有两个重要的约束条件，其一是要求new_type为指针或引用，其二是下行转换时要求基类是多态的（基类中包含至少一个虚函数）。
* reinterpret_cast <new_type> (expression) 重解释转换
 

这个转换是最“不安全”的，两个没有任何关系的类指针之间转换都可以用这个转换实现，举个例子：
```
class A {};
class B {};
A * a = new A;
B * b = reinterpret_cast<B*>(a);//correct!
```
* const_cast <new_type> (expression) 常量向非常量转换
这个转换好理解，可以将常量转成非常量。
* C风格转换是“万能的转换”，但需要程序员把握转换的安全性，编译器无能为力；static_cast最接近于C风格转换，但在无关类指针转换时，编译器会报错，提升了安全性；dynamic_cast要求转换类型必须是指针或引用，且在下行转换时要求基类是多态的，如果发现下行转换不安全，dynamic_cast返回一个null指针，dynamic_cast总是认为void*之间的转换是安全的；reinterpret_cast可以对无关类指针进行转换，甚至可以直接将整型值转成指针，这种转换是底层的，有较强的平台依赖性，可移植性差；const_cast可以将常量转成非常量，但不会破坏原常量的const属性，只是返回一个去掉const的变量。

##排序算法
![排序算法](http://img.my.csdn.net/uploads/201207/17/1342514529_5795.jpg)  

* 当n较大，则应采用时间复杂度为O(nlog2n)的排序方法：快速排序、堆排序或归并排序序。

   快速排序：是目前基于比较的内部排序中被认为是最好的方法，当待排序的关键字是随机分布时，快速排序的平均时间最短；

* **插入排序—直接插入排序(Straight Insertion Sort)**  
  将一个记录插入到已排序好的有序表中，从而得到一个新，记录数增1的有序表。即：先将序列的第1个记录看成是一个有序的子序列，然后从第2个记录逐个进行插入，直至整个序列有序为止，插入排序是稳定的，时间复杂度：O（n^2）.  

* **插入排序\—希尔排序（Shell`s Sort）**  
  先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录“基本有序”时，再对全体记录进行依次直接插入排序，希尔排序方法是一个不稳定的排序方法。  

* **选择排序—简单选择排序（Simple Selection Sort）**  
  在要排序的一组数中，选出最小（或者最大）的一个数与第1个位置的数交换；然后在剩下的数当中再找最小（或者最大）的与第2个位置的数交换，依次类推，直到第n-1个元素（倒数第二个数）和第n个元素（最后一个数）比较为止。  

* **选择排序—堆排序（Heap Sort）**  
  堆的定义如下：具有n个元素的序列（k1,k2,...,kn),当且仅当满足  
![堆排序](http://img.my.csdn.net/uploads/201207/18/1342589718_3742.jpg)  
  时称之为堆。由堆的定义可以看出，堆顶元素（即第一个元素）必为最小项（小顶堆）。
若以一维数组存储一个堆，则堆对应一棵完全二叉树，且所有非叶结点的值均不大于(或不小于)其子女的值，根结点（堆顶元素）的值是最小(或最大)的。建堆时的比较次数不超过4n 次，因此堆排序最坏情况下，时间复杂度也为：O(nlogn )。

* **交换排序—冒泡排序（Bubble Sort）**  
  在要排序的一组数中，对当前还未排好序的范围内的全部数，自上而下对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即：每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。  

* **交换排序—快速排序（Quick Sort）**  
  基本思想：

  a）选择一个基准元素,通常选择第一个元素或者最后一个元素,

  b）通过一趟排序讲待排序的记录分割成独立的两部分，其中一部分记录的元素值均比基准元素值小。另一部分记录的 元素值比基准值大。

  c）此时基准元素在其排好序后的正确位置

  d）然后分别对这两部分记录用同样的方法继续进行排序，直到整个序列有序。

  快速排序的示例：

```
[cpp] view plain copy print?
void print(int a[], int n){  
    for(int j= 0; j<n; j++){  
        cout<<a[j] <<"  ";  
    }  
    cout<<endl;  
}  
  
void swap(int *a, int *b)  
{  
    int tmp = *a;  
    *a = *b;  
    *b = tmp;  
}  
  
int partition(int a[], int low, int high)  
{  
    int privotKey = a[low];                             //基准元素  
    while(low < high){                                   //从表的两端交替地向中间扫描  
        while(low < high  && a[high] >= privotKey) --high;  //从high 所指位置向前搜索，至多到low+1 位置。将比基准元素小的交换到低端  
        swap(&a[low], &a[high]);  
        while(low < high  && a[low] <= privotKey ) ++low;  
        swap(&a[low], &a[high]);  
    }  
    print(a,10);  
    return low;  
}  
  
  
void quickSort(int a[], int low, int high){  
    if(low < high){  
        int privotLoc = partition(a,  low,  high);  //将表一分为二  
        quickSort(a,  low,  privotLoc -1);          //递归对低子表递归排序  
        quickSort(a,   privotLoc + 1, high);        //递归对高子表递归排序  
    }  
}  
  
int main(){  
    int a[10] = {3,1,5,7,2,4,9,6,10,8};  
    cout<<"初始值：";  
    print(a,10);  
    quickSort(a,0,9);  
    cout<<"结果：";  
    print(a,10);  
  
}  
```

* **归并排序（Merge Sort）**  
  归并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。然后再把有序子序列合并为整体有序序列。
  **过程如下**
  设r[i…n]由两个有序子表r[i…m]和r[m+1…n]组成，两个子表长度分别为n-i +1、n-m。
  a.j=m+1；k=i；i=i; //置两个子表的起始下标及辅助数组的起始下标
  b.若i>m 或j>n，转⑷ //其中一个子表已合并完，比较选取结束
  c.//选取r[i]和r[j]较小的存入辅助数组rf
  如果r[i]<r[j]，rf[k]=r[i]； i++； k++； 转⑵
  否则，rf[k]=r[j]； j++； k++； 转⑵
  d.//将尚未处理完的子表中元素存入rf
  如果i<=m，将r[i…m]存入rf[k…n] //前一子表非空
  如果j<=n ,  将r[j…n] 存入rf[k…n] //后一子表非空
  合并结束。
* **桶排序/基数排序(Radix Sort)**  
  是将阵列分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递回方式继续使用桶排序进行排序）。桶排序是鸽巢排序的一种归纳结果。当要被排序的阵列内的数值是均匀分配的时候，桶排序使用线性时间（Θ（n））。但桶排序并不是 比较排序，他不受到 O(n log n) 下限的影响。

##KMP算法
[http://blog.csdn.net/yutianzuijin/article/details/11954939/](http://blog.csdn.net/yutianzuijin/article/details/11954939/ "KMP算法详解")

##判断单链表是否存在环
[http://blog.sina.com.cn/s/blog_725dd1010100tqwp.html](http://blog.sina.com.cn/s/blog_725dd1010100tqwp.html "单链表是否存在环")

* 在计算机编程语言中用来为复杂的声明定义简单的别名，与宏定义有些差异。它本身是一种存储类的关键字，与auto、extern、mutable、static、register等关键字不能出现在同一个表达式中。

##泛型
* 泛型函数：
```
template  <typename T1, typename T2>
T1 fun(T1, T2, int )
{  //…..}
```  
* 泛型类：
```
##声明部分
template  <class T1, class T2 >  
class A {
   private:
   int a;
  T1 b;  //成员变量也可以用模板参数
  public: 
  int fun1(T1 x, int y );
 T2 fun2(T1 x, T2 y);
}
##实现部分
template  <class T1, class T2 >
int A<T1>:: fun1(T1 x, int y ){//实现…… }
 template  <class T1, class T2 >
T2 A<T1,T2>:: fun2(T1 x, T2 y) {//实现…… }
```