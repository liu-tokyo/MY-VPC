# VirtualBox的硬盘扩容

> [仮想化環境のディスク容量を拡張する - Avintonジャパン株式会社](https://avinton.com/academy/extending-virtualbox-virtual-drive/)

## 作业环境

- VirtualBox 6.1.32
- 虚拟机：Ubuntu 20.04 LTS

## 宿主机扩容作业

1. 关闭虚拟化环境；

2. 从 VirtualBox Manager 扩展目标客户操作系统的容量；

   菜单【**管理**】→【**虚拟介质管理**】→【**虚拟硬盘**】

   如果存在快照，则无法展开。 在删除所有快照的情况下执行以下操作。

3. 选择目标客户操作系统的虚拟硬盘，输入扩展容量，单击应用。 （在本例中，它将从 20GB 扩展到 64GB。）

## 虚拟机扩容作业

使用分区管理工具从客户操作系统扩展容量

1. 启动虚拟机，启动Ubuntu，启动终端。

2. 安装 GParted，一个分区管理软件。

   ```shell
   sudo apt update
   sudo apt install gparted
   ```

3. 以 root 权限运行 GParted。

   ```shell
   sudo gparted
   ```

4. 将鼠标光标移动到要扩展的分区上，右键单击并单击调整大小/移动。

   如果存在交换分区，需要先停止该交换功能，然后把交换分区调整到硬盘的末尾，然后把前面的分区调整到最大。

5. 确认所有修改，重新启动虚拟机，扩容结束。
