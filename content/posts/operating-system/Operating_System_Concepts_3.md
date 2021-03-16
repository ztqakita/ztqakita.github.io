---
title: "CPU Scheduling"
date: 2020-10-12T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: CPU Scheduling
    identifier: CPU-Scheduling
    parent: OS
    weight: 10
math: true
---

# Chapter 5 CPU Scheduling
## 5.1 Concepts on Scheduling
### 5.1.1 basic concepts
1. CPU Scheduling: (短期调度)
==selecting + allocating + enabling, in kernel mode.==
The procedure of ***selecting*** running entities in main memory ( i.e., processes or threads in the ***ready*** queue) according to some criteria, ***allocating*** CPU to the selected running entities, and then ***enabling*** them to run on CPU

2. CPU burst occurs: ***running*** state
3. I/O burst occurs: ***waiting*** state
4. Process execution cycle: CPU burst + I/O burst

### 5.1.2 Scheduler (Focus)
1. ***selects*** from the processes in memory that are ready to execute
— using *scheduling algorithms*
2. ***allocates*** the CPU to one of them 
— 组织和维护(就绪)进程/线程队列

### 5.1.3 Dispatcher
The  dispatcher gives control of the CPU to the process selected by the scheduler, i.e. starts the selected process:
- process context switching (取出PCB)
- switching to user mode
- jumping to the proper location in the user program to ***restart that program***

### 5.1.4 When Scheduling occurs？
When CPU is ***idle*** or some events occur in systems, the OS scheduler should make scheduling decision:
1. process terminates 
2. the running process ***switches from running to waiting state*** (for example, I/O requests)
3. the running process ***switches from running to ready state*** ( e.g., interrupts occurs, or the timeslot is out)
4. (maybe) ***switches from waiting to ready*** (e.g. , completion of I/O)
> Scheduling under condition 1 and 2 is nonpreemptive. 
Scheduling under condition 3 and 4 is preemptive 

### 5.1.5 Dispatch latency (调度等待时间)
![](/images/posts/OS/33.JPG)
- system calls: 保护现场
- scheduler：选择下一个进程
- dispatcher：启动下一个进程
- 整个t0-t3的时间被称为调度等待时间

## 5.2 Scheduling Criteria
![](/images/posts/OS/34.JPG)
1. **CPU utilization**
Def：CPU运行在用户模式下的时间占比
![](/images/posts/OS/35.JPG)
2. **Throughput**
Def: the number of processes that complete their execution per time unit
3. **Turnaround time**
Def: the length of the procee lifetime cycle.
4. **Waiting time**
Def: amount of time a process has been waiting in the ***ready queue***. NOT ***waiting state***!!!
5. **Response time**
the time it takes to ***start responding***,  not the time it takes to output the response 
![](/images/posts/OS/36.JPG)
> 想达到吞吐量最大，则在进行调度时，先执行CPU burst小的进程。

## 5.3 Scheduling Algorithm
### 5.3.1 FCFS
Implementation
1. the new created process is inserted into a FIFO ready queue, i.e. inserted into the tail of the ready queue.
2. scheduler selects the first process in the ready queue

Features
1. nonpreemptive
2. the average waiting time under FCFS is generally not minimal

### 5.3.2 Shortest-Job-First scheduling(SJF)
Implementation
1. CPU burst 最短的优先
2. CPU burst相同时，采用FCFS
> 如何预测下一个CPU burst：指数平滑估计

Feature   
1. with respect to ***minimum average waiting time*** for a given set of processes, SJF is optimal 

Types
1. nonpreemptive
![](/images/posts/OS/37.JPG)
2. preemptive(SRTF)
![](/images/posts/OS/38.JPG)

### 5.3.3 Priority scheduling
smaller interger = higher priority
Types
1. nonpreemptive or preemptive
2. static or dynamic

Starvation Problems
low priority processes may never execute.
> Using dynamic priority can avoid this problem.

### 5.3.4 Round Robin
时间片大约为10-100ms，选择队列中的第一个进程放入时间片，当这个时间片完成之后，将其终止并将PCB从队列头挂到队列尾。
Features   
1. typically, higher average turnaround than SJF, but better response time 
2. commonly used in time-sharing systems
![](/images/posts/OS/39.JPG)
> time slice q 的值需要权衡，太大变为FCFS，太小的话context switch过于频繁，开销过大。

If there are n processes in the ready queue and the time quantum is q, then 
- each process gets 1/n of the CPU time in chunks of at most q time units at once
- no process waits more than (n-1)q time units

### 5.3.5 Mutilevel Queue
The ready queue is partitioned into separate queues and different algorithms
- foreground (interactive) —— RR
- background (batch) —— FCFS
==优先服务前端队列==

**But, starvation problems again!!!**

- Mutilevel Feedback Queue
进程优先级变化：高优先级Queue $\rightarrow$ 低优先级Queue

### 5.3.6 Highest Response Rate First(HRRF)
> 在这里的响应时间与上面的并不相同

响应时间 = 进程进入系统后的等待时间 + 估计计算时间(CPU burst)
响应比 = 响应时间/估计计算时间 = 1 + 等待时间/估计计算时间
特点：
- 非抢占式
- SJF < HRRF 的平均周转时间 < FCFS

![](/images/posts/OS/40.JPG)

### 5.3.7 Scheduling in Real-time systems
1. Def: a computer and/or software system that **reacts to external events** in limited time intervals (deadline) before the events become obsolete (失效)
> deadline 是绝对时间

2. deadline到达时，如果进程没有结束，应继续执行，不会被强行终止

3. Real-time system is ==event-driven== and ==time-critical== system
external events $\rightarrow$ interrupts(硬中断) $\rightarrow$ scheduling among critical processes $\rightarrow$ the critical process responsible for reacting to and processing these events in limited time ( before the deadline). 

4. Dispatch latency consists of two phases:
    - conflict phase
    调度算法优先处理事件
    - dispatch phase

5. - Scheduling in hard real-time system
    硬实时，绝对不能超过DDL；通过可调度性分析判断能否得到及时处理
    - Scheduling in soft real-time system

## 5.4 Mutiple-Processor Scheduling
2级调度
1. 任务分配给CPU
2. 在CPU上调度process

## 5.5 Thread Scheduling
略

> 结合第五章作业与例题PPT进行复习！




