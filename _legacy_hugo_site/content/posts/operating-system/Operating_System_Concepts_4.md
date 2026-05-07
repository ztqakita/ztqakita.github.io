---
title: "Process Synchronization"
date: 2020-10-14T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Process Synchronization
    identifier: Process-Synchronization
    parent: OS
    weight: 10
math: true
---

# Chapter 6 Process Synchronization

## 6.1 Background
### 6.1.1 Shared resources
**Def:**
  - the resources (e.g., data, CPU, I/O ports, memory) that can be accessed by several cooperating processes **concurrently**
  - the shared resources cannot be used by several processes simultaneously (or in parallel), only be used **mutual exclusively**(互斥) 

信号量的三种用法：
1. **资源互斥使用**：资源只有1个实例，各个进程通过二元信号量mutex互斥地进入临界区，使用资源。
Mutex：代表资源的控制权，初值为1。
    - mutex = 1：buffer空闲
    - mutex = 0：buffer阻塞
2. **资源竞争使用**：
资源有多个实例，允许多个进程竞争使用资源。
多元信号量表示：(1)资源可用数目，(2)资源使用权
e.g. empty, full
3. **进程间同步**：
进程间的执行步骤需要有先后顺序关系
同步二元信号sync，初值为0
    - sync = 0：未开始跑
    - sync = 1：前面进程跑完

### 6.1.2 Producer-Consumer Problem
又名Bounded-Buffer problem。
- **Atomic operation**: an operation that completes in its entirely without interruption. Otherwise, data inconsistency will occur.

Consider this situation:

```C++
//Producer process
while (1) {
    /* producing an item in nextProduced */
    while (counter == BUFFER_SIZE)
      ; /* do nothing */
    buffer[in] = nextProduced;
    in = (in + 1) % BUFFER_SIZE;
    counter++;
}

//Consumer process 
while (1) {
    while (counter == 0)
      ; /* do nothing */
    nextConsumed = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    counter--;
    /* consume the item in nextConsumed */
}
```
If the producer and the consumer access **counter** concurrently, data consistency can NOT be maintained.

**Race condition:**
- the situation where several processes access and manipulate shared data concurrently
- the final value of the shared data depends upon which process finishes last

## 6.2 The Critical-Section Problem
### 6.2.1 Concepts
1. Critical section(临界区)
each process has a code segment, called critical section, in which the **shared data is accessed**

2. Critical resources
resources accessed by Critical section.
要求：保证该进程进入临界区时，没有其他进程在访问。

### 6.2.2 General structure of process 
![](/images/posts/OS/41.jpg)

### 6.2.3 Solution to Critical-Section Problem
1. Mutual exclusion
if process Pi is executing in its critical section specific to the resource R, then no other processes can be executing in their critical sections specific to R.
2. Progress
    - 只考虑那些正处于entry(申请进入)、critical sections和exit sections 的进程。
    - 挑选过程不应被无限期阻止
3. Bounded Waiting
a bound or limit must exist on **the number of times** that other processes are allowed to enter their critical sections *after a process has made a request to enter its critical section and before that request is granted*.
    > 为了避免starvation

> note：进程在临界段内只应停留有限时间，不然会导致其他进程饿死。

## 6.3 Peterson Solution
Peterson's solution is restricted to **two processes** that alternate execution between their critical sections and remainder sections. 

```C++
int turn;     //资源访问权
bool flag[2]; //ready to enter its critical section

// for process Pi
do {
  flag[i] = true;  /*表明自身请求进入临界区*/
  turn = j;        /*假设先轮到对方进入临界区 */
  while (flag [j] and turn = j) 
   {  do-nothing  };  
    /*对方正在临界区中, Pi忙等待*/

  critical section

  flag [i] = false  /*表明自身不再需要进入临界区*/

  remainder section
} while (1);
```
![](/images/posts/OS/42.jpg)

### 6.3.1 Busy-waiting
Pi alternatively stays in running and ready states. (一直处于running态，无法进入waiting态)
Example：
![](/images/posts/OS/43.jpg)
问题分析：在ts[3]时间片中，此时flag[j]=true, turn=i, 导致while循环无法进入，直接进入临界区执行；但并没有执行完，时间片用完强行切换到下一个进程。在ts[4]时间片中，此时flag[i]=true, turn=i, 满足while循环条件，则一直处于**忙等待状态**。
解决办法：将running改为waiting，第4片给Pi，这样避免忙等待。

### 6.3.2 Bakery Algorithm
A software solution for synchronize among ***n processes.***
Principles:
  - before entering its critical section, process receives a **number** (denoted for ticket #,)
  - the holder of the ***smallest number*** enters the critical section.
  - if processes Pi and Pj receive the same number, if i < j, then Pi is served first; else Pj is served first.
  - the ***numbering scheme*** always generates numbers in increasing order of enumeration; i.e., 1,2,3,3,3,3,4,5...

> 利用二元组比大小：(a, b) < (c, d)

```C++
bool choosing[n];         
/* Pi begins/wants to apply for ticket*/ 
int number[n];            
/* for Pi, number[i]>0 表示Pi需要进入临界区: */

do {
    choosing[i] = true;    
    /*begin to apply for ticket*/
    number[i] 
        = max(number[0], number[1], …, number[n – 1]) + 1;
    //分配新的number，要求number单调不减

    choosing[i] = false;    /* end application */

    for (j = 0; j < n; j++)           /*考虑所有进程*/
    {               
        while (choosing[j]) ;    /*如有其它进程正在申请ticket,等待Pj
                                                        申请完*/
        while ((number[j] != 0) &&  {(number[j], j) < (number[i,], i)}) ;   
        /*如有票号小于自身的其它进程Pj 正在或等待运行，
        等待Pj访问完*/
        //此处是二元组判断
    }

    critical section;

    number[i] = 0;    //丢弃票，退出临界区

    remainder section;

}while(1);

```

## 6.4 Synchronization Hardware
### 6.4.1 Principles of TestAndSet Instruction and Swap Instruction
- 每个临界资源设置一个布尔变量lock，初值为false；lock用机器字来实现
- CPU提供专门的硬件指令TestAndSet 或Swap，允许对一个字的内容进行检测和修正，或交换两个字的内容
- 硬件指令可以解决共享变量的完整性和正确性，防止出现race condition

### 6.4.2 Principles of Interrupt masking based hardware synchronization 
- “开关中断”指令保证用户态下的进程在临界区执行时不被其它进程中断,从而实现多个进程互斥访问临界区
- 进入临界区前执行：“关中断”指令
- 离开临界区后执行：执行“开中断”指令

## 6.5 Semaphores
### 6.5.1 Background
1. **Def:** 由操作系统在**内核空间**提供的一种用于多个合作进程间==同步与互斥==的机制。
> 信号量也被称为锁(lock)，但锁专指*二元信号量*。

2. **Wait() / P()**
请求分配一个单位的资源S给执行wait操作的进程
```C
wait(S)   //这种进程仍会有忙等待的问题
{
    while(S <= 0)
        do no-op;
    S--;
}
```

3. **Signal() / V()**
进程释放一个单位的资源S
```C
signal(S) 
{
    S++;
}
```

These two operations are performed **atomatically**.

### 6.5.2 Usage
Two types of semaphores:
- counting semaphore (计数、一般信号量) 
- binary semaphore (二元信号量，又称互斥锁)

1. **Mutual exclusion**
semaphore mutex: 1 资源可用，0 资源不可用
```C
//Process Pi: 
do {
    wait(mutex);
    //critical section
    signal(mutex);
    //remainder section
} while (1);
```

2. **Synchronizing**
semaphore synch
```
P1: M1;
    S1;
    signal(synch);  // 0->1
P2: M2;
    wait(synch);    // 1->0, P1执行完后执行S2
    S2;
```
> PPT中的例子记得看！400米接力跑

### 6.5.3 Semaphores without busy waiting
Two simple operations:
- **block()**: The process are changed from running to waiting. (挂起)
- **wakeup()**: Resumes the execution of a blocked process P. (唤醒) 
```C
//多元信号量版本
wait(S)  
{		
    S.value--;            /*申请一个单位的资源*/
    if (S.value < 0) {    /*无可用资源*/
      add this process to S.list;				
      block();
    }
}

Signal(S) 
{	        
    S.value++;         /*释放一个单位的资源*/
    if (S.value <= 0)  /*仍有被阻塞进程*/
    {
      remove a process P from S.list;
      wakeup(P);       /*唤醒被阻塞进程*/
    }
}
```

### 6.5.4 Deadlock and Starvation
1. Example of deadlock
![](/images/posts/OS/44.jpg)

## 6.6 Classical Problems of Synchronization
此处只列举典型问题及其应用的简单描述，具体分析过程及答案请详见叶文版PPT 6-例题与作业

1. Bounded-Buffer Problem
    - producer-consumer problem
    - 和尚取水问题
    此问题要归结为两个生产者消费者问题
    - Extended problem:
      1. One producer and one consumer are permitted to **simultaneously** enter their critical sections to access the buffer.
      ***mutex1 for producer, mutex2 for consumer***
      2. "父母-子女-橘子/苹果"问题
      3. 多条串行生产线/流程中的中间工序
      *process 同时具有生产者、消费者的角色*
      4. 生产者每次向buffer放入m (>2)个数据项，消费者每次只消费1个数据项 [或：消费者每次消费n个数据项]；
      ***设置计数变量count和mutex2，替代empty的计数功能，控制生产者***

2. Readers-Writers Problem
    - Soldiers in two queues, passing a bridge
    - 过桥问题
    - 最多允许M个读者同时读
    - 每当M个读者依次读完后，应允许写者进入临界段
    - 最多允许M个读者同时读，或者最多允许N个写者同时写 
    - 控制队列中读者数目
    - 写者/读者公平竞争
    通过添加优先信号量wp
    - 写者/读者优先

3. Dining-Philosophers Problem 
    - 多类型资源申请
    ```C++
    int Mnum = 3, IOnum = 3;
    semaphore self[6];
    semaphore mutex = 1;
    enum {idle, apply, running} state[6];
    
    void process(int i)
    {
        while(1)
        {
            idle();
            apply(i);
            running();
            release(i);
        }
    }

    void apply(int i)
    {
        wait(mutex);
        state[i] = apply;
        test(i);
        signal(mutex);    //这里的释放顺序很重要，互换会导致死锁
        wait(self[i]);  
    }

    void test(int i)
    {
        if(Mnum > 0 && IOnum > 0 && state[i] == apply)
        {
            state[i] = running;
            Mnum--;
            IOnum--;
            signal(self[i]);
        }
    }

    void release(int i)
    {
        wait(mutex);
        state[i] = idle;
        Mnum++;
        IOnum++;
        for(int i=0;i<6;i++)
            test(i);
        signal(mutex);
    }
    ```
4. The sleeping-barber problem
问题特点：
（1）资源竞争
    顾客竞争chairs
    顾客竞争barbers       
（2）同步
    顾客-barber

The basic version:
```C++
#define CHAIR 5
semaphore barber = 1, customer = 1; //这对信号量很关键
semaphore mutex = 1;
int waiting = 0;

void barber()
{
    wait(customer);
    wait(mutex);
    waiting--;
    signal(mutex);
    signal(barber);
    cuthair();
}

void customer()
{
    wait(mutex);
    if(waiting < CHAIR)
    {
        waiting++;
        signal(customer);   //通知理发师有顾客
        signal(mutex);

        wait(barber);
        getcut();
    }
    else
    {
        signal(mutex);      //离开店铺
    }
}
```


## 6.7 Monitors (管程)





第一题：选择题
第二题：简答题——概念，用硬件命令实现互斥
第三题：大题：调度、信号量、死锁(银行家算法)