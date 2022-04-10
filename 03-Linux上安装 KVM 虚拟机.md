# Linux上安装 KVM 虚拟机

测试环境：Ubuntu 20.04

来自于 [Red Hat](https://www.redhat.com/zh/topics/virtualization/what-is-KVM) 的介绍。

## 目录

- [Linux上安装 KVM 虚拟机](#linux上安装-kvm-虚拟机)
  - [目录](#目录)
  - [1. 检查 Ubuntu 虚拟化支持](#1-检查-ubuntu-虚拟化支持)
  - [2. 在 Ubuntu  上安装 KVM](#2-在-ubuntu--上安装-kvm)
  - [3. 在 Ubuntu 上创建虚拟机](#3-在-ubuntu-上创建虚拟机)
  - [4. 激活 default 虚拟网络](#4-激活-default-虚拟网络)
    - [4.1 主机配置](#41-主机配置)
    - [4.2 客户端配置](#42-客户端配置)
  - [5. 虚拟机驱动安装](#5-虚拟机驱动安装)
  - [6. 解除全画面显示](#6-解除全画面显示)
  - [7. 加快鼠标反应速度](#7-加快鼠标反应速度)
  - [参照](#参照)

---

## 1. 检查 Ubuntu 虚拟化支持

在 Ubuntu 上安装 KVM 之前，首先检查您的硬件是否支持 KVM。 安装 KVM 的最低要求是 AMD-V 和 Intel-VT 等 CPU 虚拟化扩展的可用性。

要检查您的 Ubuntu 系统是否支持虚拟化，请运行以下命令：

```shell
egrep -c '(vmx|svm)' /proc/cpuinfo
```

结果大于 0 表示支持虚拟化。 从下面的输出中，我们已经确认服务器运行正常。

要检查您的系统是否支持 KVM 虚拟化，请运行以下命令：

```shell
sudo kvm-ok
```

如果服务器上不存在“kvm-ok”实用程序，请运行 apt 命令进行安装：

```shell
sudo apt install cpu-checker
```

然后运行“kvm-ok”命令来探测系统：

```shell
sudo kvm-ok
```

执行信息如下：

```shell
liu@debian:~$ sudo kvm-ok
[sudo] liu 的密码：
INFO: /dev/kvm exists
KVM acceleration can be used
```



## 2. 在 Ubuntu  上安装 KVM

在验证您的系统可以支持 KVM 虚拟化后，安装 KVM。 要安装 KVM、virt-manager、bridge-utils 和其他依赖项，请运行以下命令：

```shell
sudo apt install -y qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager
```

上記のパッケージの簡単な説明：

- The **qemu** package (quick emulator) is an application that allows you to perform hardware virtualization.
- The **qemu-kvm** package is the main KVM package.
- The **libvritd-daemon** is the virtualization daemon.
- The **bridge-utils** package helps you create a bridge connection to allow other users to access a virtual machine other than the host system.
- The **virt-manager** is an application for managing virtual machines through a graphical user interface.

在继续之前，您需要确保虚拟化守护进程 (libvritd-daemon) 正在运行。 为此，请运行命令：

```shell
sudo systemctl status libvirtd
```

个人电脑的显示信息如下：

```shell
liu@debian:~$ sudo systemctl status libvirtd
● libvirtd.service - Virtualization daemon
     Loaded: loaded (/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-04-09 22:43:17 CST; 8min ago
TriggeredBy: ● libvirtd.socket
             ● libvirtd-admin.socket
             ● libvirtd-ro.socket
       Docs: man:libvirtd(8)
             https://libvirt.org
   Main PID: 4035 (libvirtd)
      Tasks: 19 (limit: 32768)
     Memory: 11.7M
        CPU: 258ms
     CGroup: /system.slice/libvirtd.service
             └─4035 /usr/sbin/libvirtd

4月 09 22:43:17 debian systemd[1]: Starting Virtualization daemon...
4月 09 22:43:17 debian systemd[1]: Started Virtualization daemon.
```

您可以运行以下命令使其在启动时作为服务随机启动：

```shell
sudo systemctl enable --now libvirtd
```

要检查 KVM 模块是否已加载，请运行以下命令：

```shell
lsmod | grep -i kvm
```

从输出中，您可以看到 kvm_intel 模块的存在。 这是针对英特尔处理器的。 对于 AMD CPU，请获取 kvm_intel 模块。

```shell
liu@debian:~$ lsmod | grep -i kvm
kvm_intel             327680  0
kvm                   921600  1 kvm_intel
irqbypass              16384  1 kvm
```

如上安装之后，需要**重新启动**一下电脑，这样后续安装虚拟机的时候，不至于出现问题。

检查 libvirtd 是否处于活动状态以及 QEMU 上是否启用了硬件虚拟化：

```shell
sudo systemctl is-active libvirtd
```



## 3. 在 Ubuntu 上创建虚拟机

成功安装 KVM 后，创建虚拟机。 有两种方法可以做到这一点。 在命令行上创建虚拟机或使用 KVMvirt-manager 图形界面。

**※ 最简单的就是在应用程序里面找到 KVM 虚拟机的图标直接启动。**

- 测试在 KVM 上安装 Ubuntu 20.04 LTS：
  - 相当的智能化，自动识别OS，所有设置几乎不需要任何改变，安装也非常顺利；相当的流畅，尤其是感觉比 VMware 里面运行的流畅。
- 测试在 KVM 上安装 Windows10 客户虚拟机：
  - 正常，也能够启动，但是效果不是很好。


后面内容引自其它资料，请图略随后的内容。经过个人认证，不适合于普通个人用户。

virt-install 命令行工具用于在终端上创建虚拟机。 创建虚拟机时，需要一些参数。

我用 Deepin ISO 镜像创建虚拟机的完整命令是：

```shell
sudo virt-install --name=deepin-vm --os-variant=Debian10 --vcpu=2 --ram=2048 --graphics spice --location=/home/Downloads/deepin-20Beta-desktop-amd64.iso --network bridge:vibr0 
```

-name 选项指定虚拟机的名称—— deepin-vm -os-variant 标志表示操作系统系列或虚拟机衍生产品。 由于 Deepin10 是 Debian 的衍生版本，因此我们将 Debian10 指定为变体。

或者如下的例子：

```shell
virt-install \
 --name vm001 \
 --vcpus 2 \
 --memory 2048 \
 --cdrom /vmdata/isos/AlmaLinux-8.4-x86_64-dvd.iso \
 --os-variant almalinux8 \　←ココで「短縮 ID」を指定する
 --disk path=/vmdata/vm001.qcow2 \
 --network network=bridge-br0 \
 --graphics vnc,port=5901,listen=0.0.0.0 \
 --boot uefi
```

运行命令以获取有关操作系统对象的其它信息：

```shell
## 安装 libosinfo-bin 软件包
sudo apt install libosinfo-bin

## 查询能够支持的 os 名称
osinfo-query os
```

-vcpu 选项表示 CPU 内核（本例中为 2 个内核），-ram 表示 2048MB 的 RAM。 -location 标志指向 ISO 映像的绝对路径，-network bridge 指定虚拟机使用的适配器。 运行命令后，虚拟机将启动，安装程序将启动，您将能够安装虚拟机。

virt-manager 实用程序允许用户使用 GUI 创建虚拟机。 要开始，请转到终端并运行命令。

```shell
virt-manager
```

或者，您可以使用 ssh 远程启动 virt-manager，如以下命令所示：

```shell
ssh -X host's address
[remotehost]# virt-manager
```

虚拟机管理器窗口如图所示弹出。



## 4. 激活 default 虚拟网络

安装结束，再次启动之后，启动虚拟机的时候，出现虚拟网络问题。

libvirt支持的网络配置：


1. 虚拟网络使用NAT
2. 直接分配物理pci设备或者SR-IOV(single root I/O virtualization)供虚拟网络使用
3. 桥接网络

在Host电脑，把该问题进行处理之后，虚拟机可以正确启动。

### 4.1 主机配置 

标准安装libvirt之后,默认的虚拟网络（default virtual network）采用的是NAT，可以通过virsh net-list --all 查看：

```shell
sudo virsh net-list --all
```

加入默认的虚拟网络丢失之后，可以采用下面的方法重新加载和激活：

```shell
sudo virsh net-define /usr/share/libvirt/networks/default.xml
```

标记默认网络自动启动：

```shell
sudo virsh net-autostart default
```

启动默认网络：

```shell
sudo virsh net-start default
```

libvirt 将会增加iptables规则以便保证虚拟机可以正常使用网络，记得检查 /etc/sysctf.conf中的net.ipv4.ip_forwart是否开启。

### 4.2 客户端配置

unix/linux一直沿用至今的“一切皆文件”的开发设计理念，为我们在配置的各种参数和性能时，提供了非常多的方便。这里将会说明如何查看和调整客户端的网络配置：
＃cd /etc/libvirt/qemu/
\#vim centos.xml (centos.xml是我安装的虚拟机<－－>客户端)
...........
...........
<interface>
      <source network='default'/>
</interface>
.............
............
下面将会看到使用bridge时，该字段的变化。－－－－：说明，可以在<interface>域内加入<mac address='xx:xx:...' />以便定义起mac地址，当然这不是必要的。

## 5. 虚拟机驱动安装

**Linux：**

尚未找到，但是显示一切正常。可能是开源的 **KVM**，我安装的版本 **Ubuntu 20.04 LTS** 能够自动安装驱动？

据介绍，KVM就是最原装的Linux，从2007年之后的版本，对KVM支持，都是无缝连接性质。

**Windows：**

但是，安装 Windows10 客户机的话，好像是有问题。驱动安装明显有问题，内存使用量才 1.7GB 左右，堂堂 Windows10 岂是这点儿内存能够跑起来的？

- 至少分配 **8GB** 内存，好像是虚拟机一定要保持 2GB 用于交换，如果是分配 4GB 内存，能用的只有 1.7GB 左右，导致运行极度缓慢。

  调整为 8GB 内存之后，可以发现内存使用量直接跳到了 6GB 左右，至少看更新速度快了很多。

- 下载并安装相应的驱动：[virtio-win packages dissection - Repology](https://repology.org/project/virtio-win/information)

  - 当前(2022年4月)，最新驱动版本是 **virtio-win-0.1.215.iso**

    安装驱动之后，虽然操作感觉好了一些，但是感觉还是有一点不太舒服的感觉；远没有安装的 **Ubuntu** 的情况流畅。

    也有可能是在 Linux 下面安装的，所以情况不太理想？



## 6. 解除全画面显示



全画面显示之后，如果想要解除全画面显示：

- 把鼠标移到**画面最上端的中间位置**，会出现 **退出全屏** 的图标，点击之后就能解除全画面显示。

  如果您查看屏幕的顶部中心，您会看到一点白色。将鼠标悬停在该条上，它将显示两个图标：左侧为“全屏”，右侧为“发送键组合”。您可能需要暂时不按Ctrl-Alt才能摆脱它。单击“全屏”以返回窗口视图。

- **Alt+ F10**或**Alt+用鼠标向下拖动**。

- 尚未找到合适的办法，所以一旦宕机，也是一个麻烦事儿，只能通过控制程序解决该问题。



**画面自动适应显示：**

菜单【**查看**】-【**缩放显示**】-【**自动调整虚拟系统窗口大小**】，标记该项目。



## 7. 加快鼠标反应速度

安装kvm创建windows虚拟机，通过novnc连接到虚拟机上发现存在鼠标位置偏移问题：

- **方法1：关闭客户机的鼠标加速**

  【**控制面板**】 -【**轻松使用**】 -【**轻松使用设置中心**】 -【**设置鼠标键**】，关闭鼠标加速。

  经测试，windows系列虚拟机关闭鼠标加速亦可缓解该问题，不过还存在比较小的偏移

  1.关闭鼠标加速，控制面板-硬件设备-鼠标-指针选项-去掉提高指针精确度勾选状态

  2.指针速度加速条拖到最慢然后保存选项，如下图 往左拖到底再保存

- **方法2：在宿主中修改kvm虚拟机的对应xml文件**

  1. 关闭kvm虚拟机
  
  2. 在/etc/libvirt/qemu下找到对应的xml配置文件
  
     在<devices>标签下添加
  
     ```xml
     <input type='tablet' bus='usb'/>
     ```
  
     执行如下指令：
  
     ```shell
     virsh define /etc/libvirt/qemu/**.xml
     ```
  
  3. 启动kvm虚拟机
  
  上述操作可能比较麻烦，可以选择如下的简单方式：
  
  - 【**虚拟机硬件详情**】-【**增加硬件**】，启动画面内选择【**输入**】-【**EvTouch USB图形数位板**】项目。

建议采用方法二可以得到有效解决。



## 参照

- [KVM をインストールして設定する - Qiita](https://qiita.com/tkarube/items/7e02d1f9e93d107c616b)
- [Driver for Windows Server Catalog](https://www.windowsservercatalog.com/results.aspx?text=Red+Hat&bCatID=1282&avc=10&ava=0&OR=5&=Go&chtext=&cstext=&csttext=&chbtext=)

