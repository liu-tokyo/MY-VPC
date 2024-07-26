# 有关虚拟机的资讯及技术

## 1. 当前比较流行的虚拟机软件：

- VirtualBox

  由Oracle免费提供，个人公司办公用该软件。

- VMware WorkstationPro 

  家里电脑一直用这个电脑，从2005年开始，就一直在使用。

- VirtualPC

  由微软提供，仅支持Windows下面安装，因为之前一直用VMware，所以这个VPC用的机会很少。
  
  现在微软支持的应该是 Hyper-V，不过一旦启用的话，就不能用其它的虚拟机软件。
  
- KVM

  这个软件用了之后，感觉客户机比主机的速度都快，这有点儿矛盾。可惜仅支持在Linux上安装，不支持Windows，感觉安装Windows客户机的支持不是很好。虽然有很多介绍的文章，但是依然没有找到很好的解决方案。
  
  因为自动支持远程管理，对于团体的一些应用，可以形成比较好的解决方案。有一部分 **容器** 的概念；不过最近 **Docker** 技术太流行了，盖过了 KVM 的风头。
  
- Parallels Desktop

  **MAC** 上的虚拟机，因为没有 MAC 的系统，个人没有实践过，和其它的虚拟机一样，该软件也可以在 MAC 上启动 Windows 等OS。

#### 1.1 **关于 JVM：**

- Java虚拟机，但是和之上的虚拟机属于不一样的虚拟概念。

---

## 2. 简介

虚拟机（VM）是一种创建于物理硬件系统（位于外部或内部）、充当虚拟计算机系统的虚拟环境，它模拟出了自己的整套硬件，包括 CPU、内存、网络接口和存储器。通过名为虚拟机监控程序的软件，用户可以将机器的资源与硬件分开并进行适当置备，以供虚拟机使用。 

配备了虚拟机监控程序（例如基于内核的虚拟机（KVM））的物理机被称为主机器、主机计算机、主机操作系统，或简称为主机。使用其资源的诸多虚拟机被称为虚拟客户机、虚拟客户计算机、虚拟客户机操作系统，或简称为虚拟客户机。虚拟机监控程序把计算资源（如 CPU、内存和存储器）视为一组可以在现有的虚拟客户机之间或向新的虚拟机进行重新分配的资源。

## 3. 虚拟机有什么用？

虚拟机隔离、独立于系统的其余部分，而且单个硬件（如服务器）上可以有多个虚拟机。您可以根据自己的需要在主机服务器之间移动这些虚拟机，更有效地利用资源。  

虚拟机允许在一台计算机上同时运行多个不同的操作系统，比如一台 MacOS 笔记本电脑上也装了 Linux® 发行版。每个操作系统的运行方式与通常操作系统或应用在主机硬件上使用的运行方式相同，因此在虚拟机中获得的最终用户体验与物理机上的实时操作系统体验也几乎毫无区别。  

进一步了解什么是虚拟化
虚拟机的工作原理是什么？
虚拟化技术允许多个虚拟环境共享一个系统。虚拟机监控程序负责管理硬件并将物理资源与虚拟环境分隔开。来自物理环境的资源根据需要进行分区后，会分配给虚拟机使用。

虚拟机运行时，当用户或程序发出需要从物理环境获取更多资源的指令，虚拟机监控程序会调度物理系统的资源请求，以便虚拟机的操作系统和应用可以访问共享的物理资源池。

## 4. 虚拟机监控程序有哪些类型？

虚拟化有两种不同类型的虚拟机监控程序可用。

**类型 1 裸机形式**
第 1 类虚拟机监控程序为裸机形式。虚拟机监控程序会直接向硬件调度虚拟机资源。KVM 就是典型的第 1 类虚拟机监控程序。从 2007 年开始，KVM 已被合并到 Linux® 内核中。因此，如果您使用的是较新版本的 Linux，就已经可以访问 KVM。 

**类型 2 托管形式**
第 2 类虚拟机监控程序为托管形式。虚拟机资源针对主机操作系统进行调度，然后针对硬件来执行。VMware Workstation 和 Oracle VirtualBox 就是典型的第 2 类虚拟机监控程序。 

## 5. 为什么要用虚拟机？

服务器整合是使用虚拟机的首要原因。部署到裸机时，大多数操作系统和应用部署都只会使用少量的物理资源。通过虚拟化服务器，您可以在每个物理服务器上设置大量虚拟服务器，从而提高硬件利用率。 

这样您就无需购买额外的物理资源（例如硬盘驱动器或硬盘），也不用压缩数据中心对电能、空间和冷却能力的需求。通过支持故障转移和冗余，虚拟机提供了额外的灾难恢复选项，而这以前只能通过增加硬件才能实现。

虚拟机可以提供一个与系统其余部分隔离开的环境。这样，无论虚拟机内部运行什么，都不会干扰主机硬件上运行的其他内容。

由于虚拟机处于隔离状态，因此堪称是测试新应用或设置生产环境的理想之选。此外，针对特定的进程，您还可以运行单用途虚拟机。

## 其它

- 一直用 Ubuntu 20.04 的版本弄虚拟机，2022.04.08 尝试了 Ubuntu 21.10，感受非常接近于 Debian 的流畅。

  原来一直折腾 Debian，就是因为 Debian 的流畅、快捷，但是容易宕机。好像是图形处理和 vmware-tools 驱动不太兼容。