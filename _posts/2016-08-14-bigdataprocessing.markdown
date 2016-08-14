---
layout:     post
title:      "各种海量数据处理面试题"
date:       2016-08-14
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - big data processing
---

>海量日志数据，提取出某日访问百度次数最多的那个IP。

IP地址是32位的二进制数,所以共有N=2^32=4G个不同的IP地址, 创建一个`unsigned count[N]`的数组,
即可统计出每个IP的访问次数,而`sizeof(count) == 4G*4byte=16G`, 远远超过了32位计算机所支持的内存大小,
因此不能直接创建这个数组.
                        
下面采用划分法解决这个问题.                           
假设允许使用的内存是512M,`512M/4byte=128M`,子问题大小为128M，即512M内存可以统计128M个不同的IP地址的访问次数.                                
而`N/128M = 4G/128M = 32` ,所以只要把IP地址划分成32个不同的区间,分别统计出每个区间中访问次数最大的IP, 
然后就可以计算出所有IP地址中访问次数最大的IP了.                         
因为2^5=32, 所以可以把IP地址的最高5位作为区间编号, 剩下的27为作为区间内的值,
建立32个临时文件,代表32个区间,把相同区间的IP地址保存到同一的临时文件中.       
如果某个区间大于内存，就继续分割。

------------------------------------------------------------------

>Top K. 搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为1-255字节。

依然是先将大文件散裂成小文件(小文件大于内存就继续分割)，每个小文件用hash统计词频。
然后每个小文件可以用各种方法求TopK词频的记录。        
如果用最小堆，可以先假设小文件的记录中的前K个记录为TopK，将这K个记录建立最小堆，堆顶就是目前第K多的记录；
然后每次来一个新的记录，和堆顶比较，如果大于堆顶，则替换堆顶，并维护最小堆。                  
然后在对所有小文件的TopK记录归并。                 
下面是大堆的代码，小堆类似

```python
# 大堆：A[parent(i)]>=A[i]，小堆相反，堆排序中使用大堆
# 维护一个大堆。假设A[i]的两个子树都已经是大堆，但A[i]可能较小。
# 此方法让A[i]在堆中逐层下降。A[i]下降后，以其为根的堆可能不是大堆，需递归调用此方法，复杂度lgn.
# assume that 2 subtrees of A[i] are already maxheap
def max_heapify(A, i, size):
    l = left(i)
    r = right(i)
    largest = i
   if l < size and A[l] > A[largest]:
        largest = l
   if r < size and A[r] > A[largest]:
        largest = r
   if largest != i:
        tmp = A[largest]
        A[largest] = A[i]
        A[i] = tmp
        max_heapify(A, largest, size)

# 把任意数组变成一个大堆。
# 数组A的n/2+1到n为树的叶子节点，可以看成大堆，对非叶子节点自底向上的调用max_heapify()。线性时间
def build_max_heap(A):
   # from bottom to top
   for i in range(0, len(A) / 2 + 1)[::-1]:
        max_heapify(A, i, len(A))
```


