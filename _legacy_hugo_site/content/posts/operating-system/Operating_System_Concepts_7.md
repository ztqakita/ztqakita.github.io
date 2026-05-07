---
title: "Mass-Storage Structure & I/O System"
date: 2020-12-10T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Mass-Storage Structure & I/O System
    identifier: Mass-Storage-Structure-I/O-System
    parent: OS
    weight: 10
math: true
---

# Chapter 12 Mass-Storage Structure
> hard disk drives(HDDs) and nonvolatile memory (NVM) 

## 12.1-12.2 Disk Structure
Mapping between disk and blocks
![](/images/posts/OS/46.jpg)
并行快速读取多个连续的blocks/sectors

1. **Transfer rate**: the rate at which data flow between the driver and the computer
2. **The position time**:
   1. **seek time**: the time for the disk to move the disk arm to the cylinder containing the desired sector.平均为移动1/2*(总磁道数)所需的时间
   2. **rotational latency**: the time for the desired sector to rotate to the disk head. 磁盘旋转半圈所需时间(depend on RPM)

3. 外部传输速率>内部传输速率
![](/images/posts/OS/47.jpg)
- 外部传输速率：硬盘缓存cache（磁盘控制器中的I/O寄存器）与内存间的传输速率；
- 内部传输速率：磁头在盘片上读写数据速率，取决于`Position time`

4. Drive attached to computer via I/O bus, including 
- IDE (Integrated Drive Electronics);  
- ATA
- SATA (Serial ATA ),  适用于PC 机、服务器
- mSATA
- SCSI (Small Computer System Interface)，中、高端服务器和高档工作站中
- Fibre Channe (光纤通道)，服务器、工作站
- USB接口

5. **Disk Controller**
Interfaces between the computer system and the disk drive hardware.
Function: 
    - checksum
    - bad sector remapping

## 12.4 Disk Scheduling
having a `fast access time` and `disk bandwidth`

1. FCFS scheduling

2. SSTF scheduling
Shortest-seek-time-first

3. SCAN scheduling
SCAN是每一次都要到达最外圈/最内圈才转向，转向后继续扫描并读block
   - C-SCAN schduling
    C-SCAN代表当走到最内圈以后迅速移到另一头的最外圈
4. LOOK 
LOOK是电梯调度算法，每一次方向都到达request中的最大值，然后立刻转向并继续扫描读block
    - C-LOOK代表每一次到达request中的最大值后迅速移到另一头的request最小值开始继续按原方向扫描

Note:
1. About preventing starvation in scheduling.
![](/images/posts/OS/48.jpg)
2. About NVM scheduling
    Random access.

## 12.5 Disk Management
### 12.5.1 Disk formatting (格式化)
1. **physical formatting**: 
Dividing a disk into sectors that the disk controller can read and write.
划分硬盘的磁柱面、建立扇区、选择扇区间隔比

2. **Logical formatting**
making a file system on the partitions.
- **格式化**：将分区所有磁道扫描一遍，清除所有扇区内容，移除文件；扫描同时，检查磁盘是否有坏扇区；加载文件系统(exFAT、NTFS、FAT32)
- **快速格式化**：清除FAT表内容，使系统认为盘上没有文件了，并不真正格式化全部硬盘。

To increase efficiency, most file systems group blocks into larger chunks, called clusters(簇)

### 12.5.2 Boot Block
> bootstrap -> Boot Block -> 内核程序

The boot block initializes system
- the bootstrap is stored in ROM.
- bootstrap loader program.

### 12.5.3 Bad Blocks


## 12.3 Disk Attachment
Disks may be attached via one of two ways
- host attached  via an I/O port talking to I/O busses,
    e.g. SCSI
- network attached via a network connection
    e.g. SAN, NAS, DAS

### 12.3.1 Network-Attached Storage(NAS)
- NFS and CIFS are common protocols
- Implemented via remote procedure calls (RPCs) between host and storage
- New iSCSI protocol uses IP network to carry the SCSI protocol

### 12.3.2 Storage Area network(SAN)

![](/images/posts/OS/49.jpg)

### 12.3.3 Direct Attached Storage(DAS)
![](/images/posts/OS/50.jpg)

## 12.5 RAID
1. Def: A data storage method in which data, along with information used for error correction, such as parity bits or Hamming codes, **is distributed among two or more hard disk drives in order to improve performance and reliability**
2. 特点：
   1. 可以存两个副本，用于防止数据丢失，增加可靠性；
   2. 将连续相关的数据放在相邻disk中，可以一并读取。
   3. RAID填补了CPU速度快与磁盘设备速度慢之间的间隙


## 12.6 Swap-Space Management
1. **Swap-space**: 
a part of disk space used as an extension of main memory to implement virtual memory, as a raw partition 

## 12.8 Stable-Storage Implementation
**Write-ahead log**：典型例子数据库的事务故障恢复log文件。
Step:
1. Replicate information on more than one nonvolatile storage media with independent failure modes;
2. Update information in a controlled manner to ensure that we can **recover the stable data after any failure during data transfer or recovery**.




# Chapter 13 I/O System
including *devices, device controllers, and I/O subsystem*
## 13.1 I/O Hardware
1. device controller registers：
   - status register
   - control register
   - data-in register
   - data-out register

> reading and writing device registers are conducted by **device drivers**!!!

2. The processor control the device in 2 ways:
    - direct I/O instructions
    - **memory-mapped I/O**: (用访问内存的方式访问I/O设备)
      - device-controller registers are mapped into address space of the CPU
      - CPU executes I/O requests using the standard memory instructions to access controller registers

3. The main control modes for data tranfer between memory/CPU and devices are:
    - polling
    - interrupt
    - DMA

## 13.2 Application I/O Interface
Linux：driver放在内核空间中，其他：driver放在用户态中（微内核）
封装I/O system calls 
- 应用接口通用性
- I/O 软件的设备独立性

I/O设备访问接口与文件读写类似：write(#device, block_address)

2. Driver
    - 由CPU执行，完成对具体I/O操作的管理和控制功能；
    - 将函数调用read、write与driver中的文件操作函数相匹配。
    - Device driver向device controller的控制寄存器CSR设置读写命令和参数，controller据此完成具体I/O操作

3. Block devices and character-stream devices
Block: read(), write(), seek();
stream: get(), put();

4. Network devices: socket

5. Blocking and Nonblocking I/O
   - 阻塞I/O
   - 非阻塞I/O
     - 多线程
     - 利用系统提供的非阻塞I/O系统调用
     - 异步I/O系统调用：the process runs while I/O executes. 


## 13.3 I/O subsystem
I/O subsystem consists of 
- kernel I/O subsystem
    - a component that conducts *scheduling, buffering, cacheing, and spooling, error handlinig*
    - a general device-driver interface
- drivers for specific hardware devices

DST：设备开关表：
对外提供的接口
### 13.3.1 Kernel I/O subsystem
1. Scheduling
2. Buffering:
   存储输入/输出数据
   - To cope with I/O device's speed mismatch with that of CPU memory access. (内部速率与外部速率不匹配)
   - To cope with I/O device transfer size mismatch 
> used for **Block device**

3. Caching
目的：做备份，提高访问速度
4. Spooling 
类似**超线程**，同时进行外围操作，使得从逻辑模拟出多台共享设备，实现由串行->并发，且无需信号量。
经典例子：**打印机队列**
在一个物理设备上模拟出多个逻辑设备。每一个进程共享虚拟设备的存储区
daemon：调度程序。

5. Error Handling
    Status register
6. I/O protection
    通过系统调用间接使用I/O instruction

## 13.4 Transforming I/O Request to Hardware Operations








