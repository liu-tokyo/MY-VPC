# VMware下ubuntu与Windows实现文件共享

更新到最新版的**Ubuntu 22.04**之后，发现原来的宿主机共享的目录不可用，尝试以下办法可以解决问题。

## 解决办法-1

- 参照资料：  
  https://kb.vmware.com/s/article/74650?lang=zh_CN  
  属于VMware官方的解决案例，不过稍显复杂。

具体操作步骤如下：

1. **/etc/systemd/system/mnt-hgfs.mount**

   创建该文件：

   ```
   sudo nano /etc/systemd/system/mnt-hgfs.mount
   ```

   包含以下内容的文件：

   ```ini
   [Unit]
   Description=VMware mount for hgfs
   DefaultDependencies=no
   Before=umount.target
   ConditionVirtualization=vmware
   After=sys-fs-fuse-connections.mount
   
   [Mount]
   What=vmhgfs-fuse
   Where=/mnt/hgfs
   Type=fuse
   Options=default_permissions,allow_other
   
   [Install]
   WantedBy=multi-user.target
   ```

2. **/etc/modules-load.d/open-vm-tools.conf**

   打开该文件：

   ```bash
   sudo nano /etc/modules-load.d/open-vm-tools.conf
   ```

   添加如下内容：

   ```bash
   fuse
   ```

   ※如果该文件已存在，请将该行添加到该文件中。

3. 使用以下命令启用系统服务：

   ```bash
   sudo systemctl enable mnt-hgfs.mount
   ```

   这可确保重新引导后会挂载 hgfs 目录。

4. 确保已加载“fuse”模块：

   ```bash
   sudo modprobe -v fuse
   ```

5. 在 Workstation 或 Fusion 中，启用“Virtual Machine Settings”>“Options”中的“Shared Folders”，并设置要共享的文件夹。

6. 共享文件夹应显示在目录 /mnt/hgfs 中。否则，使用以下命令启动服务：

   ```bash
   sudo systemctl start mnt-hgfs.mount
   ```

   或重新引导。

**注意**：供应商可能在未来版本的 open-vm-tools 软件包中提供该功能。如果这样，您可以移除手动添加的文件。

## 解决办法-2

- 参照资料：  
  https://communities.vmware.com/t5/VMware-Workstation-Player/Shared-Folder-in-linux/td-p/2853182  
  VMware论坛里面的资料，估计也属于官方资料，该办法相对简单。

具体操作步骤如下：

1. 打开 **/etc/fstab** 文件：

   ```bash
   sudo nano /etc/fstab
   ```

   在最后一行，填写如下内容：

   ```bash
   vmhgfs-fuse   /mnt/hgfs    fuse    defaults,allow_other    0    0
   ```

2. 重新引导系统（重启虚拟机）后就可以在 /mnt/hgfs 下面看到共享目录。

## 解决办法-3

参照资料：https://markdevel.hatenablog.com/entry/2018/10/04/094105

- 执行如下命令：

  ```bash
  sudo vmhgfs-fuse -o allow_other -o auto_unmount .host:/ /mnt/hgfs
  ```

  需要每次重新开机之后都要执行该指令。

- 启动时自动挂载：

  ```bash
  sudo nano /etc/fstab
  ```

  将以下内容添加到 /etc/fstab 的末尾。 根据需要添加 uid 和 gid 选项。
  
  ```ini
  .host:/ /mnt/hgfs fuse.vmhgfs-fuse allow_other,auto_unmount,defaults 0 0
  ```


## 解决办法-4

模拟VirtualBox的共享办法，直接把共享目录挂载到文件管理中，这样找起来更方便。

1. 创建 /media/host-share 目录

   ```bash
   sudo mkdir  /media/host-share
   ```

   类似于把共享数据作为CD-ROM一样进行挂载。

2. 启动时自动挂载：

   ```bash
   sudo nano /etc/fstab
   ```

   将以下内容添加到 /etc/fstab 的末尾。 根据需要添加 uid 和 gid 选项。

   ```ini
   .host:/ /media/host-share fuse.vmhgfs-fuse allow_other,auto_unmount,defaults 0 0
   ```
   
   当然，添加下面这一行也是一样的：
   
   ```ini
   vmhgfs-fuse   /media/host-share    fuse    defaults,allow_other    0    0
   ```
   
   

