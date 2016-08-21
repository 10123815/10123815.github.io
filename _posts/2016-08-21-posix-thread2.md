---
layout:     post
title:      "POSIX 线程(2)"
date:       2016-08-21
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - linux
---

#### 条件变量

+ 允许线程以无竞争的方式等待条件的发生。
当一个线程获得互斥锁后，发现自己需要等待某个条件变为真，
如果是这样，该线程就可以等待在某个条件上
+ 互斥量是用于上锁，条件变量用于等待

```cpp
int pthread_cond_wait(
       pthread_cond_t *restrict cond,
       pthread_mutex_t *restrict mutex);
int pthread_cond_timedwait(
       pthread_cond_t *restrict cond,
       pthread_mutex_t *restrict mutex,
       const struct timespec *restrict timeout);
       
int pthread_cond_signal(pthread_cond_t *cond); 
int pthread_cond_broadcast(pthread_cond_t *cond);
```

+ 互斥量对该条件进行保护，传入的互斥量必须是已经锁住的，原子操作
    + 将调用线程放到等待条件的线程列表上，即进入睡眠；
    + 对互斥量进行解锁；
+ 被阻塞的线程可以被`pthread_cond_signal`函数，`pthread_cond_broadcast`函数唤醒，
也可能在被信号中断后被唤醒。
+ `pthread_cond_wait`函数的返回并不意味着条件的值一定发生了变化，必须重新检查条件的值。
+ `pthread_cond_wait`函数返回时，相应的互斥锁将被当前线程锁定，即使是函数出错返回。
+ 一般一个条件表达式都是在一个互斥锁的保护下被检查。当条件表达式未被满足时，线程将仍然阻塞在这个条件变量上。
当另一个线程改变了条件的值并向条件变量发出信号时，等待在这个条件变量上的一个线程或所有线程被唤醒，__接着都试图再次占有相应的互斥锁__
+ 阻塞在条件变量上的线程被唤醒以后，直到`pthread_cond_wait()`函数返回之前条件的值都有可能发生变化。
所以函数返回以后，在锁定相应的互斥锁之前，__必须重新测试条件值__。
最好的测试方法是__循环调用__`pthread_cond_wait`函数，并把满足条件的表达式置为循环的终止条件
+ 一定要在改变条件状态后，再给线程发送信号

例子

```cpp
struct{  
    pthread_mutex_t mutex;  
    pthread_cond_t cond;  
    queue<int> product;  
}sharedData = {PTHREAD_MUTEX_INITIALIZER, PTHREAD_COND_INITIALIZER};  
  
void * produce(void *ptr)  
{  
    for (int i = 0; i < 10; ++i)  
    {  
        pthread_mutex_lock(&sharedData.mutex);  
  
        sharedData.product.push(i);  
  
        pthread_mutex_unlock(&sharedData.mutex);  
  
        if (sharedData.product.size() == 1)  
            pthread_cond_signal(&sharedData.cond);  
  
        //sleep(1);  
    }  
}  
  
void * consume(void *ptr)  
{  
    for (int i = 0; i < 10;)  
    {  
        pthread_mutex_lock(&sharedData.mutex);  
  
        while(sharedData.product.empty())  
            pthread_cond_wait(&sharedData.cond, &sharedData.mutex);  
  
        ++i;  
        cout<<"consume:"<<sharedData.product.front()<<endl;  
        sharedData.product.pop();  
  
        pthread_mutex_unlock(&sharedData.mutex);  
  
        //sleep(1);  
    }  
}  
```

