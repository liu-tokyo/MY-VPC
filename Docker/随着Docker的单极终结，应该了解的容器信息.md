# 随着Docker的单极终结，应该了解的容器信息

## 1. 背景介绍

如果你使用容器，你应该使用 Docker 的常识正在崩溃。 容器这种轻量级的虚拟环境，已经是从开发到发布不可或缺的工具，工程师绕不开它。 Docker 几乎是容器执行工具（Container Runtime）的唯一选择，当时认为它已经足够了，随着 cri-o 等其他 Container Runtime 的出现，情况发生了翻天覆地的变化。 本文为想使用Container的和想重新整理资料的朋友总结一下Container Runtime的相关情况。



## 2. 什么是Docker/Container类虚拟化？

### 2.1 什么是 Docker？

![Docker](https://res.cloudinary.com/zenn/image/fetch/s--2_gD7lAC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://1000logos.net/wp-content/uploads/2021/11/Docker-Logo-768x432.png)

一种用于自动将应用程序部署到容器的工具。 有可以免费使用的开源版Docker CE（社区版）和付费的Docker EE（企业版），Docker Desktop是一个叫做Docker Engine的软件，它是处理容器的工具主体，以及处理 Kubernetes 的 GUI。  
如下图所示，它作为Server-client类型运行，其中Docker客户端运行一个名为Docker daemon的服务器。 默认情况下，Docker 守护进程启动一个更高级别的容器运行时，称为 containerd。 containerd 管理 Docker 镜像并运行一个名为 runC 的低级容器运行时，用于启动和运行 Docker 容器（[配置详情](https://lethediana.sakura.ne.jp/tech/archives/overview-ja/2054/)）。

![Docker内部構成](https://res.cloudinary.com/zenn/image/fetch/s--a8jpxYry--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://lethediana.sakura.ne.jp/tech/wp-content/uploads/2023/03/f527d84598d47468495ba5ea8bd13a41.png)

### 2.2 什么是容器式虚拟化？

它总结了Docker正在做的Container虚拟化是什么。 首先，虚拟化是一种技术，可以将实际不存在的事物当作确实存在的事物来对待。 因此，可以根据什么被视为“存在”来组织和理解虚拟化技术具有什么样的特征。 众所周知，计算机虚拟化技术可以分为以下五个层次。  

1. 指令集架构 (ISA) 级别
   - 虚拟化CPU指令集
   - 示例：使 32 位 Windows 指令在 64 位 Windows 上运行的技术
2. 硬件抽象层 (HAL) 级别
   - I/O设备、内存等物理设备与操作系统之间的虚拟化
   - 示例：使用管理程序的虚拟机
3. 操作系统 (OS) 级别
   - 虚拟化以在操作系统和应用程序之间进行抽象
   - 示例：容器
4. 图书馆级别
   - 用户和图书馆之间的虚拟化
   - 示例：一种将Windows程序调用的库替换为Mac库并运行的技术
5. 应用层
   - 仅虚拟化一个应用程序
   - 示例：在不依赖外部库的情况下运行应用程序的技术

容器式虚拟化对应操作系统级虚拟化，实现了操作系统和应用程序之间的抽象。 换句话说，操作系统级虚拟化技术是一种技术，它创造了一种情况，就好像内核有多个用户空间实例，进程运行在完全不同的执行环境中。  
通过使用它，您可以在 OS 上创建一个隔离环境，因此即使实际上只有一台物理机，您也可以获得多个用户操作多台 PC 的环境，并且您可以创建应用程序可以实现这样的环境如果它在专用机器上运行。 使用Hypervisor等进行HAL级别的虚拟化也可以做到这一点，但最大的优势是可以更轻量级地运行（其他级别的虚拟化详见[这里](https://lethediana.sakura.ne.jp/tech/archives/summary-ja/2040/)）。

![Hypervisor vs. Container virtualization design](https://res.cloudinary.com/zenn/image/fetch/s--qM1fkEg0--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://cloudacademy.com/wp-content/uploads/2015/09/12.jpg)



## 3. Docker时代结束的迹象
很难想象一直引领容器技术的Docker在未来变得无法使用，但有迹象表明它将不再是唯一的解决方案。 本章介绍了一些示例。

### 3.1 Docker 桌面许可

Visual Studio 和 Unity 等软件，以前只需要付费许可，现在有了可以免费使用的许可，这有望起到增加新用户进入市场的效果。（Docker EE）。 上一章提到，Docker 在 2017 年实施了这一战略，但由于 Kubernetes 的普及导致销售不景气，Mirantis 在 2019 年收购了 Docker EE 业务。 截至 2023 年，Docker 的主要付费产品是 Docker Desktop 和订阅容器注册服务 Docker Hub。 由于只供个人免费使用的价格设定，如果有一定规模的公司想用Docker来管理容器，第一个候选者是支付Docker Desktop的许可费并使用它。 然而现实中，一个不熟悉虚拟化技术的公司在做这件事的时候，往往很难解释和说服Container技术的优越性，而且费用往往是一个阻碍，不可否认会流向其他开源容器运行时（由于 Docker CE 以开源方式运行，因此可以在 Windows 上启动 WSL 并几乎免费使用 Docker Engine）。

### 3.2 Docker漏洞

Docker 安全性经常被认为是一个问题，2020 年甚至有一份报告称“Docker 是网络犯罪分子的主要目标”（Docker 和 Kubernetes 特定的恶意软件也成为一种趋势）。
一个漏洞的例子是 Docker 默认以 root 身份运行容器。 以 root 用户身份运行 Container 会为 Container 主机提供比其需要更多的权限。 因此，如果Container存在漏洞（即使运行在Container中的应用是健壮的，也有可能是Container所基于的Image经常被引用为问题），并且攻击者拥有root权限。如果你得到，对运行Container的主机PC进行文件访问和命令执行的可能性就会出现。 另一个问题是 Docker 守护进程以 root 身份运行。 Docker 也采取了一些措施，例如在 Dockerfile 中添加不允许 root 用户运行容器等设置，以及对 Docker daemon 实现 rootless 模式，但是通过切换到 Podman 等其他 Container Runtime，在某些情况下，可能会被认为可以减轻面临的安全问题。

> 备注：
> 除了安全性之外，Docker 的功能弱点还包括难以处理的监控功能和基于 GUI 的应用程序。

### 3.3 Kubernetes 弃用 Docker

Kubernetes 从发布之初就正式支持 Docker，但截至 2022 年 4 月，将完全停止支持并弃用 Docker。 这是因为上一章提到的 CRI 的出现，使得不通过 Docker 直接与 Container Runtime 通信成为可能。 换句话说，就 Docker 而言，现在可以直接使用 Docker 使用的 containerd。 因此，弃用 Docker 并不意味着如果您正在使用 Docker，Kubernetes 将无法工作（除非您正在使用依赖于 dockershim 的系统）。 然而，有了这种对 Kubernetes 的支持，至少在假设使用 Kubernetes 的系统情况下，使用 Docker 的意义不大，**毫无疑问，Docker 已经更多地成为开发人员的工具**。

### 3.4 非 Docker 容器运行时的增长

目前，许多 Container Runtime 都是开源的，并且根据 Runtime 的不同，存在各种类型的 Container 可执行文件。 由于Docker已经完全称霸，它们都以Docker为标杆兼容Kubernetes、轻量级、高安全性等特点，让您可以选择适合自己环境和需求的Container执行环境（第5章：详细描述了有什么样的Container Runtime）。  
下图总结了一些 Container Runtime 类型及其关系的概述（并未涵盖每个工具的所有配置）。 在这里，我们展示了从 Kubernetes 组件 Kubelet 和 Container Engine 到运行 Container 的流程以及使用的工具之间的关系。 在图中，Kata Container 和 gVisor 都包含在 Container Engine 中，但它们往往被画成 low-level Container Runtime（这次将稍后介绍的 kata-runtime 和 runsc 放在那里）。

![Container Runtime関係性](https://res.cloudinary.com/zenn/image/fetch/s--3EF8nw1W--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/4a8cc1f385c157250c87b99a.png%3Fsha%3D524e996fdd698f9fb6adb455c63b54edabb4f782)



## 4. 容器技术相关历史

在本章中，我们将关注操作系统级虚拟化技术的发展以及Docker的定位。

![Container技術関連史](https://res.cloudinary.com/zenn/image/fetch/s--_dBnKtxe--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/7735189819a351457ce5168f.png%3Fsha%3Dda0537d4201786aed74adc19780b74745f03d3a7)

### 4.1 OS级虚拟化技术的兴起（1970-2004年左右）

在 1960 和 70 年代，世界上的计算机并不像今天这样丰富，而且价格昂贵且价值不菲，因此在多个用户之间共享少量物理机的虚拟化技术受到关注。 在虚拟化技术中，操作系统级虚拟化的起源是系统调用“chroot”，它出现在 1979 年的 Unix version 7 中，改变当前进程及其子进程的根目录。 通过执行该命令，可以分离每个进程的文件访问，因此每个进程创建一个在称为 Jail 的虚拟环境中执行的情况。 Chroot 后来于 1982 年被正式添加到 4.2版的 BSD 中，使用户可以使用此功能。  
大约在这个时候，出现了追求系统稳定性的应用程序去中心化的需求，但随着廉价小型计算机（8 位个人计算机）的出现，增加机器数量而不是虚拟化技术的想法被提出采用、传播。 但是，随着公司机器数量的增加，管理成本也随之增加，虚拟化技术将再次被曝光。 在这样的背景下，当涉及到虚拟化时，模拟 hypervisor 等硬件的想法成为当日明星，本应集成的进程之间逐渐发现安全漏洞等问题也随之而来。 在这种情况下，2000 年 FreeBSD 4.0 中设计并引入了一种名为 jail 的工具，作为一种新的应用程序级虚拟化技术。 在监狱中创建的虚拟环境也称为监狱，每个监狱都分配有一个 IP 地址、根目录等，因此可以实现比 chrooted 虚拟环境复杂得多的环境。

> 备注：
> 据我研究，进程虚拟环境上下文中的 Jails 通常指的是 FreeBSD Jails。 为了清楚地区分使用 chroot 和 FreeBSD 创建的虚拟环境，前者通常称为 chroot jails，后者通常称为 FreeBSD Jails。

在 FreeBSD Jails 出现后不久，Linux VServer 于 2001 年推出，用于构建几乎等同于 Linux 的虚拟环境（请注意，Linux VServer 是一个独立于 Linux Virtual Server 的项目）。 在 Linux VServer 中，创建了一个名为 security context 的分区，里面的虚拟化系统称为 Virtual Private Server (VPS)。
2004 年，发布了 Solaris Containers 的第一个公开测试版，它是 Solaris 的虚拟化工具，Oracle 的操作系统（使用称为 Zones 的操作系统实例处理虚拟环境），2005 年，RHEL OpenVZ（OpenVZ 中的虚拟环境实例被多个操作系统调用） VPS、Virtual Environment (VE)、Container（有时在 OpenVZ 文档中缩写为 CT）等术语发布。操作系统级别的虚拟化在环境中成为可能。

### 4.2 Container的诞生（2004-2012年前后）

大约从2004年开始，谷歌就开始专注于操作系统级虚拟化技术的开发，并于2006年开发了一种名为进程容器的功能，用于隔离和限制计算机资源（CPU、RAM、网络、I/O等）。 进程容器是进程的集合，而不是虚拟环境。 上一节简要提到，Container这个词开始在虚拟环境的意义上使用，所以在2007年改名为cgroup（control group的缩写）以避免混淆，次年2008年Linux Built进入内核。

> 笔记
> 2007 年 5 月 Linux 新闻网站上介绍的 Container 定义：  
>A "container" is a group of processes which shares a set of parameters used by one or more subsystems.
> 
>至今仍在使用的 cgroup 定义：  
> A cgroup is a collection of processes that are bound to a set of limits or parameters defined via the cgroup filesystem.
>
> 对比一下，两者是同一个概念。

同年，2008年，LXC（Linux Container）作为一个使用cgroups和Linux命名空间的虚拟环境出现，即只由Linux内核驱动，不需要任何补丁的虚拟环境。
2006年前后，云计算技术开始普及，谷歌CEO提到云，公有云的先行者Amazon Web Service推出服务。 在云计算环境中，虚拟机（VM）的使用以其能够灵活改变系统规模、省去本地环境中管理的麻烦等优势而备受关注。内部系统向云端的迁移将逐步进行。
同时，在2011年，VMware的开源PaaS（Platform as a Service）Cloud Foundry的Warden提供了一个工具，它提供了使用LXC管理虚拟环境的API（参见Warden文档描述为隔离环境而不是虚拟环境） , 2012年RedHat发布容器执行和管理工具OpenShift，2013年谷歌推出类似LXC的cgroups和命名空间，最初由谷歌开发，lmctfy（Let Me Contain That For You）作为容器开源结合的系统。

> 备注：
> 谷歌从2007年左右开始，内部基础设施一直在使用隔离环境，但是当基础设施重建时，可以分离出Container系统，我也很看好开放。好像lmctfy已经发布了

此外，需要为每个 VM 单独管理操作系统文件和设置数据，以及需要大量资源（如内存和存储）的事实开始被视为一个严重的问题。 ，其操作更轻量级，将作为解决方案出现。 到目前为止，容器主要是系统容器，其管理方式与在内部运行操作系统的虚拟机相同，但应用程序容器正在引起人们的注意，每个容器运行一个进程或服务。

### 4.3 容器=Docker时期（2012-2015年左右）

2012年前后，IoT（物联网）普及，4G到来，智能手机拥有率达到10%（见下图），对软件的需求越来越大。 随之而来的是，为了更高效地开发和维护服务器应用，特别是考虑到云平台上的运行，微服务架构、DevOps（Development和Operations的组合词）、CI/CD（Continuous Integration）/Continuous Delivery）开始流行起来.

> 备注
> 在使用VM的应用程序开发中，准备和维护硬件的成本几乎消失，与传统开发相比可以说有了很大的进步。但仍然存在难以诊断资源（内存）是否可用等问题。 , DB, etc) 被正确使用。 对此，人们对Application Container抱有很高的期望，它可以为每个应用程序提供一个虚拟化环境。

在这种情况下，2013 年 5 月，PaaS 提供商 dotCloud 开源了一个名为 Docker 的容器运行时。 Container by Docker 相对于 Jail 最具革命性的一点是，它通过将应用程序在 Container 中执行所必需的信息写入一个名为 Dockerfile 的文件中，从而方便了 Container 的共享。在这个事实的支持下，Docker 项目迅速成长，虚拟化通过容器一次传播。 Docker刚发布的时候默认使用LXC作为执行环境，但是随着2014年5月的版本升级，加入了一个从lmcfty派生出来的名为libcontainer的GO语言配置的环境，并在其中执行Containers。替换掉了（只有默认的）执行环境发生了变化，它并没有停止与 LXC 的工作。见下图）。
并且在同年7月，由于Docker的壮大，dotCloud更名为Docker，并将dotCloud的服务卖给了cloudControl。 此外，谷歌在 11 月发布了提供 Docker 容器的 Google Container Engine (GKE)，AWS 发布了支持 Docker 容器的容器管理服务 Amazon EC2 Container Service。随着 10 月发布的 docker-compose ver1.0，这使得处理多个 Docker 容器变得更加容易，Container = Docker 的认可度越来越高。

![Docker execdriver diagram](https://res.cloudinary.com/zenn/image/fetch/s--XI1JnkPA--/https://www.docker.com/wp-content/uploads/2014/03/docker-execdriver-diagram.png.webp)

与其他Container Runtimes一样，CoreOS于2014年12月发布的Rocket（后称rkt）和Canonical于2015年2月发布的LXD，其事实标准的地位并未动摇。
随着各种Container Runtimes的出现，2015年6月Docker、CoreOS、Google、AWS等成立了Open Container Initiative（OCI），建立容器的开放标准规范（最初是Open Container Project，后来成为Linux基础项目）。 OCI开始制定Container Runtime和Container Image的规范，此时Docker使用的libcontainer被提供给OCI作为Container Runtime的参考，同时也提供了一个新的工具runC给OCI，创建为wrapper围绕 libcontainer 提供附加功能，例如读取 OCI 定义的 JSON 类型规范的能力。 runC 今天仍然作为根据 OCI 规范处理容器的工具进行管理。

> OCI的创始成员如下：
>
> CoreOS, Amazon Web Services, Apcera, Cisco, EMC, Fujitsu, Goldman Sachs, Google, HP, Huawei, IBM, Intel, Joyent, Mesosphere, Microsoft, Pivotal, Rancher Labs, Red Hat, VMware, Docker

### 4.4 Container=Docker+K8s时期（2014-2018年左右）

随着Docker的出现，通过将服务分割成更小的元素并在容器上执行，可以实现微服务架构，这是一种比传统单体架构更灵活的系统结构。对Container Orchestration的需求越来越大，一种将部署在网络中的大量容器作为一个组（通常称为集群）进行管理的方法。
2014 年 6 月，正如上一节所述，当 Docker 越来越受欢迎时，谷歌内部用于容器管理的平台
我们用 GO 语言将 Borg 和 Omega 重构为 Kubernetes 并开源。

> 备注：
> 在 2015 年 1 月的博客文章中，Google 表示：
> 我们相信 Kubernetes 将有助于创建更好的基于容器的应用程序和更少的操作开销，这将加速容器采用的趋势，谷歌云平台将更多地用作基于 Kubernetes 的应用程序的运行环境。它表示它正在想象成为喜欢的策略。

2015 年 10 月，Docker 发布了 Docker Swarm 作为 Docker 容器的编排工具（Docker 从 2014 年左右开始预发布了一个名为 Swarm 的系统），2016 年 7 月，Mesosphere 的集群资源管理 Apache Mesos（2011 年发布），执行容器编排，已经发布了支持Docker等容器的1.0版本。 大约在这个时候，出现了各种编排工具，例如AWS的Amazon ECS（弹性容器服务）和RedHat的OpenShift升级到版本3来处理Docker和Kubernetes。Kubernetes、Docker Swarm和Apache Mesos Strong瞄准了事实上的标准地位。

> 备注：
> 前面提到的 docker-compose 也是处理多个容器的好工具，但它与 Docker Swarm 的区别很大，它只能在单机上运行。 其他容器编排工具，例如 Kubernetes，也可以将集群的容器分布到多台机器上。

其中，Kubernetes 所占份额最大，成为最活跃的开源社区。 这有几个可能的原因，包括部署功能和多云支持的便利性。  
另外，Kubernetes 最初只支持 Docker 作为容器运行时，但 CoreOS 想加入对 rkt 的支持，于是在 2016 年 CRI（Container Runtime 引入了一个名为 OCI Interface 的插件接口）并启动了一个名为 OCID（OCI daemon ) 通过上述 OCI。 OCID 是一个后来更名为 CRI-O 的项目，它允许 Kubernetes 直接运行一个符合 OCI 规范的容器运行时，有效地消除了 Kubernetes 对容器运行时的依赖，并允许 Kubernetes 社区独立运作。  
此外，Kubernetes 是 CNCF（云原生计算基金会）的第一个项目，该基金会成立于 2015 年，旨在普及云原生计算，这是一种在云计算环境中构建、部署和管理现代应用程序的方法。还提到。 由于CNCF作为社区的枢纽，许多与Kubernetes高度兼容的周边工具（典型的例子有系统监控工具Prometheus和服务网格Istio）相继诞生，Kubernetes社区不断壮大、对它是一种可持续工具的认识已经取得进展。  
为了响应 Kubernetes 的这一运动，Docker 也于 2016 年将 containerd（管理由 runc 执行的 Container 的镜像）作为高阶 Container Runtime 开源，并于 2017 年加入 CNCF 项目。不过，containerd 被认证为2019年毕业阶段。 此外，通过使用实现 Kubernetes CRI 的名为 cri 的容器插件，包括 Docker 在内的容器运行时可以直接与 Kubernetes 进行互操作。 还值得注意的是，最近 Docker 本身正在成为使用 runC 和 containerd 的微服务。

> 备注：
> CNCF的初始成员有：  
> Google, CoreOS, Mesosphere, Red Hat, Twitter, Huawei, Intel, Cisco, IBM, Docker, Univa, VMware

随着2017年10月Microsoft AKS（Azure Container Service）、2017年11月Amazon EKS（Amazon Elastic Container Service for Kubernetes）、2018年5月GKS（Google Kubernetes Engine）的发布，三大公有云都支持Kubernetes， Kubernetes 已经成为容器编排的事实标准。

### 4.5 Docker/Kubernetes 近期趋势（2017 年前后）

2017年到2023年，Kubernetes的成功加上数字化转型（DX）的强烈热潮，让云原生这个关键词更加火爆。  
为了进一步推广Container技术，Docker于2017年在DockerCon2017上发布了创建轻量级Linux VM的LinuxKit，以及包含它的开源项目Moby，并将Github docker/docker仓库迁移至moby/moby。 Moby 包括 Docker 中使用的组件，如 containerd 和 SwarmKit，通过像乐高一样组合它们，您可以创建一个适合您环境的独立容器系统。 Docker工具分为基于Moby的开源版Docker CE（社区版）和付费版Docker EE（企业版）。 对于Moby项目的未来趋势，路线图上列出了五项内容：containerd、build tools/Dockerfile、非root用户的daemon启动、Moby项目的测试功能、更精细的组件化。  
In addition, WASM (WebAssembly), which abstracts the programming language, is emphasized as a technology that complements Docker. Appropriate selection of VM, Container, and WASM according to the weight required for the service improves the quality of the application. It is said那 对Docker创始人之一的Solomon Hykes来说，如果WASM和WASI（WebAssembly System Interface，WASM使用系统资源的接口）在2008年就已经存在，就没有必要创建Docker了。可以说，这个WASM有对 Docker 影响很大，随着 2022 年 Docker Desktop 4.15 的更新，出现了运行 WASM runtime 而不是 containerd 管理下的 runC 的 Docker + WASM beta 版本。将在 docker/roadmap Issue 讨论中进一步加强。  
在幕后，与 Docker 一起引领容器相关技术的 Kubernetes 继续成长。  
**第一个趋势是 Kubernetes 本身正在变得更加功能化。**  
特别是在 2017 年 6 月的版本更新（Kubernetes 1.7）中，Kubernetes 实现了一个名为 Custom Controller 的功能。 这使得实现 CoreOS 在 2016 年提出的 Operator Pattern 成为可能。 这种设计模式被称为 Kubernetes Operator，它是一种将如何操作集群（例如，何时以及如何在多个容器之间同步数据和备份）纳入软件的模式。 由于开发人员头脑中的专有技术可以转化为软件并内置到 Operator 中，这对系统自动化非常有用，并且可以共享专有技术。  
**第二个趋势是让 Kubernetes 更容易使用。**  
Helm 是一个典型的工具示例，它可以更轻松地管理 Kubernetes 上的集群。 Deis（2017 年 4 月被微软收购）帮助企业迁移到 Kubernetes 和云原生计算，当时正在开发开源项目 Helm。 Helm 是一款专注于让用户轻松打包应用程序并安装到 Kubernetes 上的工具，发布为 Helm2，2018 年参与 CNCF 项目，2019 年更新为 Helm3，2020 年获得 CNCF 毕业认证。 使用这个 Helm，部署到 Kubernetes 的组件可以被视为一个称为图表的单一组，从而降低管理成本。 这对于管理上述包含各种组件的 Operators 非常有用。 此外，Rancher 是一种可以更轻松地管理配置了 Kubernetes 的系统本身的方法。 Rancher 是一种允许您在任何云（包括私有云）上部署 Kubernetes 集群的工具，并且由于其直观的 GUI 管理而受到欢迎。  
**第三个趋势是扩大 Kubernetes 的使用范围。**  
到目前为止，Kubernetes 的主要执行环境一直在云端，但已经采取措施将这种实用性引入物联网设备和边缘计算设备。 这种趋势在资源（内存、CPU 等）有限、要求实时性能的可能性、安全措施以及边缘设备（嵌入式设备）特有的其他问题上不是。 但是，2019 年 Rancher 发布了 k3s，一个用于边缘的轻量级 Kubernetes，2020 年 CNCF 认证了一个在边缘使用 Kubernetes 的工具，叫做 KubeEdge，进入了孵化阶段，一个工具已经出现。 此外，GPU厂商NVIDIA宣布推出NVIDIA GPU Operator，FPGA/SoC厂商Xilinx和Intel开始为Kubernetes提供设备插件。  
在研究领域，有使用 Kubernetes 管理机器人和无人机控制器的例子。



## 5. 各种容器运行时
### 5.1 高级容器运行时

#### 5.1.1 containerd

![containerd](https://res.cloudinary.com/zenn/image/fetch/s--N5WtTCeZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://containerd.io/img/logos/footer-logo.png)

containerd 是一个行业标准的高级容器运行时，专注于简单性、健壮性和可移植性。 到目前为止，它是由 Docker 开发的，从 2016 年开始在内部使用，然后捐赠给 CNCF。 它具有 CNCF 项目的毕业地位，根据 Sysdig 2022 云原生安全和使用报告，据报道它的份额仅次于 Docker。 它旨在让用户将 containerd 集成到更大的系统（例如 Docker 或 Kubernetes）中，而不是孤立地使用它。 它基本上作为一个守护程序运行，底层容器运行时默认使用 runC，并且由于其被内置到 Docker 中的记录而获得了稳定的流行。

![containerd architecture](https://res.cloudinary.com/zenn/image/fetch/s--1yeyD_Yr--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://github.com/containerd/containerd/raw/main/docs/historical/design/architecture.png)

https://github.com/containerd/containerd

#### 5.1.2 CRI-O

![CRI-O](https://res.cloudinary.com/zenn/image/fetch/s--GfXJxUI7--/https://github.com/cri-o/cri-o/raw/main/logo/crio-logo.svg)

CRI-O 是 RedHat 于 2017 年发布的轻量级 Kubernetes 优化容器运行时。 主要目标是取代 Docker 作为 Kubernetes 实现（例如 OpenShift 容器平台）的容器引擎。 在CRI（Container Runtime Interface）的基础上加上OCI的O，据说是CRI-O。 强烈意识到在 Kubernetes Pod 中稳定运行，在 CRI-O 版本和 Kubernetes 版本相同的情况下保证兼容性（CRI-O 1.x.y 和 Kubernetes 1.x.y 是兼容的）。 当然，它符合 OCI 标准，并于 2019 年获得 CNCF 孵化资格。 由于它是 Openshift 的默认容器运行时，根据 Sysdig 2022 Cloud-Native Security and Usage Report，据报道它的份额仅次于 containerd。 与 Docker 整个系统由一个大的守护进程管理不同，每个 Container 都由一个名为 conmon 的进程管理和监控，因此可以说是健壮的。 RunC 默认用于低级容器运行时。

![CRI-O architecture](https://res.cloudinary.com/zenn/image/fetch/s--yXeuxHcE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://cri-o.io/assets/images/architecture.png)

https://github.com/cri-o/cri-o

#### 5.1.3 Podman

![Podman](https://res.cloudinary.com/zenn/image/fetch/s--Wadt0eHD--/https://podman.io/images/podman.svg)

Podman 是 2019 年由 Red Hat 主导发布的轻量级 Container Runtime。 Pod manager的缩写，它有管理Kubernetes中引入的Pods（容器组）的思想（标志的主题是Selkie，一种像爱尔兰美人鱼的神话生物。标志似乎是一个海豹，并且Selkie 有形成称为 pods 的组的习惯。） 它基于 libpod，一个用于容器生命周期管理的库，默认情况下使用 runC 作为低级容器运行时。 由于它不像 Docker 那样有守护进程，而是像 CRI-O 一样使用 conmon 来管理容器，所以它具有轻量级和能够灵活响应配置变化的优点。 此外，具有无根模式等高安全性也是其魅力之一。 Podman Desktop 将于 2022 年发布，进一步加强其作为 Docker 竞争对手的地位。

![Podman architecture](https://res.cloudinary.com/zenn/image/fetch/s--slfelT1k--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://developers.redhat.com/blog/wp-content/uploads/2019/01/podman-pod-architecture.png)

https://developers.redhat.com/blog/2019/01/15/podman-managing-containers-pods#podman_pods__what_you_need_to_know

https://github.com/containers/podman

> 备注：
> CRI-O 和 Podman 也是轻量级的，因为它们专门管理容器和容器镜像。 因此，往往需要使用Container的构建工具Buildah和Container Image的复制验证工具Skopeo来进行功能的补充。  
> Red Hat 提到了一个无守护进程的工具生态系统，它使用 Buildah 构建容器，使用 Podman 运行容器，并使用 Skopeo 传输容器镜像。

!["Buildah"](https://res.cloudinary.com/zenn/image/fetch/s--T1kuDhXN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://buildah.io/images/buildah.png)

!["Skopeo"](https://res.cloudinary.com/zenn/image/fetch/s--WSE607Kt--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://camo.githubusercontent.com/edb2cf198aad5b6bc776e245ac1f2a703b5aabcf11521bdb3a3b641b62dc8b0e/68747470733a2f2f63646e2e7261776769742e636f6d2f636f6e7461696e6572732f736b6f70656f2f6d61696e2f646f63732f736b6f70656f2e737667)

### 5.2 低レベルContainer Runtime

#### 5.2.1 runC

![runC](https://res.cloudinary.com/zenn/image/fetch/s--TmaVWP7K--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://www.cncf.io/wp-content/uploads/2020/08/runc-logo-5.png)

runc 是一个 CLI 工具，用于创建和运行 OCI 规范的 Linux 容器，于 2015 年从 Docker 中分离出来。 目前由OCI维护，用Go语言编写。 它是轻量级和高度可移植的，并且它内置于 Docker 中并像 containerd 一样默认使用，因此它是一个经过高度验证和可靠的 Container Runtime。  
https://github.com/opencontainers/runc

#### 5.2.2 crun

![crun](https://res.cloudinary.com/zenn/image/fetch/s--_l0L67Ra--/https://github.com/containers/crun/raw/main/docs/crun.svg)

2021 年发布了 1.0 版的新低级容器运行时。 主要由 Red Hat 维护。 与runc最大的区别是crun是用c语言写的。 Go 有一些问题，比如它不能正确支持 fork/exec 模型，runc 的开发添加了很多聪明的 hack 来让它工作，但这仍然是 Go 的一个限制。它被认为是一个问题受限于 它基本上比 runc 更轻，但即使 Openshift 4.12 于 2023 年 1 月发布，在生产环境中使用 crun 也已被弃用。  
https://github.com/containers/crun

#### 5.2.3 gVisor

![gVisor](https://res.cloudinary.com/zenn/image/fetch/s--6WCHwQqH--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://github.com/google/gvisor/raw/master/g3doc/logo.png)

gVisor 是一个容器运行时，提供沙盒容器，由谷歌于 2018 年推出。 当 runc 和 crun 使用主机内核来执行容器时，gVisor 接受容器的系统调用作为访客内核。 换句话说，gVisor 认为只要 Container 机制共享一个宿主内核，就无法逃脱宿主内核的漏洞，通过使用一个名为 Gofer 的组件，它负责提供文件系统，并创建一个guest kernel作为代理层，容器与host kernel分离，提高安全性，提高footprint flexibility（但是，由于guest kernel的存在，需要注意到操作的开销）。 runsc用于执行Container，Container符合OCI。

> ![gVisor Architecture](https://res.cloudinary.com/zenn/image/fetch/s--qzVyEpbS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://gvisor.dev/docs/Sentry-Gofer.png)

> 备注：
> 计算机安全上下文中的沙箱意味着一个隔离的过程，以减轻系统故障和软件漏洞的传播。 据说这个名字来源于这样一个事实，即给不擅长与朋友玩耍的孩子的个人游戏空间被称为沙盒。 gVisor 的观点是，Linux Container 不能称为沙箱，因为它们共享宿主操作系统的内核。
> 还有一个叫 Nabla 的 Container Runtime 也是类似的思路。

https://github.com/google/gvisor

#### 5.2.4 Kata container

![Kata container](https://res.cloudinary.com/zenn/image/fetch/s--O-RGWHKa--/https://www.vectorlogo.zone/logos/katacontainersio/katacontainersio-official.svg)

Kata Container 是 OpenStack 基金会于 2018 年发布的 Container Runtime，旨在将 VM 的安全性与容器的速度和可管理性结合起来。 它是一种创建超轻量级VM并在其上运行容器的方法，并且由于其隔离性可以说安全性非常高。 它结合了英特尔2015年发布的[Intel Clear Containers](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-clear-containers-1-the-container-landscape.html)和Hyper 2017年9月发布的[runV](https://github.com/hyperhq/runv)的技术构建而成，目前实现了OCI兼容的Container Runtime kata-kontainer，也通过插件兼容CRI。 kata-runtime 通过为每个容器或 Pod 创建一个 QEMU/KVM（Quick Emulator/Kernel-based Virtual Machine）并在其中运行一个名为 kata-agent 的守护进程来管理容器。  
类似思路还有一个叫[Firecracker](https://firecracker-microvm.github.io/)的Container Runtime（根据[@inductor](https://zenn.dev/inductor)的说明在修正中）。

> ![Kata container architecture](https://res.cloudinary.com/zenn/image/fetch/s--mdGCfnvS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://katacontainers.io/static/a43936f549270231f0f81e52b99494f9/43a2d/kata-explained1%25402x.png)

> 备注：
> 在 Kata 容器项目公布时，除了 Intel 和 Hyper 之外，参与的公司还有：
> 99cloud、AWcloud、Canonical、中国移动、城网、CoreOS、Dell/EMC、EasyStack、烽火、谷歌、华为、京东、Mirantis、NetApp、Red Hat、SUSE、腾讯、Ucloud、UnitedStack、中兴。

## 6. 结论

Container的使用范围不断扩大，以Web服务为核心，最终到达物联网应用和边缘计算。 于是，对Container的期望需求变得多样化，各种Container和Orchestration工具应运而生。 因此，为了在适当的环境中处理容器，掌握本文所见的情况和背景，并赶上未来的趋势是极其重要的。

## 参考

https://www.sokube.ch/post/the-kubernetes-containers-runtime-jungle

https://www.cloud-for-all.com/azure/blog/history-and-background.html

https://blogs.itmedia.co.jp/itsolutionjuku/2017/10/1it_1.html

https://mkdev.me/posts/dockerless-part-1-which-tools-to-replace-docker-with-and-why

https://www.redhat.com/en/blog/paas-kubernetes-cloud-services-looking-back-10-years-red-hat-openshift

https://betterprogramming.pub/why-docker-isnt-free-for-everyone-anymore-856f849b5c2c

https://zenn.dev/ttnt_1013/articles/f36e251a0cd24e