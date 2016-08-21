---
layout:     post
title:      "POSIX 线程(1)"
date:       2016-08-21
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - linux
---

#### 标示

+ pthread_t 具体内容根据实现的不同而不同，有可能是一个Structure，因此不能将其看作为整数

```cpp
#include <pthread.h>
int pthread_equal(pthread_t tid1, pthread_t tid2); // 相等返回非0
pthread_t pthread_self(void);
```

#### 创建
+ `restrict`只可以用于限定指针，并表明指针是访问一个数据对象的唯一且初始的方式
+ pthread函数在出错的时候不会设置errno，而是直接返回错误值

```cpp
#include <pthread.h>
int pthread_create(
       pthread_t *restrict tidp,
       const pthread_attr_t *restrict attr,
       void *(*start_rtn)(void *), void *restrict arg);
``` 

#### 终止
+ 不终止整个进程的情况下，退出单个线程
    + 在线程函数中return
    + 被同一进程中的另外的线程Cancel掉
    + 线程调用`pthread_exit`函数
+ `pthread_join`
    + 线程A调用`pthread_join(B, &rval_ptr)`，被Block，进入Detached状态（如果已经进入Detached状态，则pthread_join函数返回EINVAL）。
    如果对B的结束代码不感兴趣，rval_ptr可以传NULL。
    + 线程B调用`pthread_exit(rval_ptr)`，退出线程B，结束代码为rval_ptr。
    注意rval_ptr指向的内存的生命周期，不应该指向B的Stack中的数据。
    + 线程A恢复运行，`pthread_join`函数调用结束，线程B的结束代码被保存到rval_ptr参数中去。
    如果线程B被Cancel，那么rval_ptr的值就是PTHREAD_CANCELLED。

```cpp
#include <pthread.h>
void pthread_exit(void *rval_ptr);
int pthread_join(pthread_t thread, void **rval_ptr);
```           
---------------------------

+ 线程可以安排在它退出的时候，某些函数自动被调用，类似atexit()函数

```cpp
#include <pthread.h>
void pthread_cleanup_push(void (*rtn)(void*), void* arg);
void pthread_cleanup_pop(int execute);
```

+ 在下面情况下`pthread_cleanup_push`所指定的thread cleanup handlers会被调用：
    + 调用`pthread_exit`
    + 相应cancel请求
    + 以非0参数调用`pthread_cleanup_pop`
        （如果以0调用`pthread_cleanup_pop`，那么handler不会被调用

#### Mutex

```cpp
#include <pthread.h>
int pthread_mutex_init(
    pthread_mutex_t *restrict mutex,
    const pthread_mutexattr_t *restrict attr) 
int pthread_mutex_destroy(pthread_mutex_t *mutex);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

+ `pthread_mutex_lock`用于Lock Mutex，如果Mutex已经被Lock，该函数调用会Block直到Mutex被Unlock，然后该函数会Lock Mutex并返回。
+ `pthread_mutex_trylock`类似，只是当Mutex被Lock的时候不会Block，而是返回一个错误值EBUSY。

-------------------------------------------------
+ 死锁
    + 线程对一个mutex加锁两次
    + 两个线程都在请求另一个线程拥有的资源

#### 读写锁

+ 多个线程可以同时获得读锁(Reader-Writer lock in read mode)，
但是只有一个线程能够获得写锁(Reader-writer lock in write mode)
+ 读写锁有三种状态
    + 一个或者多个线程获得读锁，其他线程无法获得写锁，直到全读完，且随后的读锁请求都被阻塞
    + 一个线程获得写锁，其他线程无法获得读＆写锁
    + 没有线程获得此读写锁

```cpp
#include <pthread.h>
int pthread_rwlock_init(
       pthread_rwlock_t *restrict rwlock,
       const pthread_rwlockattr_t *restrict attr) 
int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);
int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock); 
int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock); 
int pthread_rwlock_unlock(pthread_rwlock_t *rwlock); 
int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock); 
int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);
```