# 高并发系统限流方案
计数器 漏桶 令牌桶  （GUAVA实现）

# 锁
> 锁的种类  
> OS 同步方法
* 互斥量(锁)(pthread_mutex_t)--发生竞争时，获取不到锁就休眠 mutexLock
* 自旋锁(pthread_spin_t)--os空转 spinLock
* 信号量

平时说的自旋锁都是语言级别(如java自旋)的  与 操作系统自旋锁有区别
ReentrantLock在操作系统级别是自旋，但在OS级别不是自旋

> synchronized是不是自旋锁？  
* synchronized在OS底层不是自旋锁 如果是重量级锁（sync关键字为10）使用的是mutex   pthread_mutex_lock(&mutex)  pthread_mutex_unlock(&mutex)
* JVM内部获取锁的时候，也没有自旋
* mutex为什么是重量级锁？  
    * mutex获取不到锁就sleep(内核函数),发生了一次系统调用进入内核态;
> 内核态 用户态  
* hotspot线程模型:一对一模型
* 内核态、用户态:与线程无关，只与CPU有关，发生系统调用会进入内核态，如sleep
* 上下文切换，java指进程之间线程与线程之间的切换

# synchronized
> 