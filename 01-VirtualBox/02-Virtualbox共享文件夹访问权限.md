# 解决virtualbox共享文件夹访问权限
Virtualbox是一款免费试用的虚拟机软件。基本功能完全可替代需要购买或破解的VMware。

## 1. 文件共享

在Windows主机上用Virtualbox搭建Linux虚拟机，虚拟机和主机之间传递文件最方便的方法就是共享文件夹。

- 假设将Windows下的share文件夹作为共享文件夹。设置好共享文件夹之后，进入虚拟机，共享文件夹的地址是/media/sf_share。

  但是进入该文件夹时，会发现共享文件夹无法访问，系统提示的原因是权限不足（Permission denied）。

  在虚拟机下查看共享文件夹的属性，发现该目录的所有者是root，所属组是vboxsf。而一般而言我们登录的用户和所属组都是<user>（你的用户名），所以确实没有权限。

  而共享文件夹的所有者和所属组是不能修改的。（不信的话可以切到root用户试一下 😛 ）

- 解决权限不足问题的方法就是将自己登录的用户，添加到vboxsf组中。

  具体做法是：

  （1）执行如下指令：

  ```bash
  sudo usermod -aG vboxsf $(whoami)
  ```

  这条指令的含义是：usermod -aG <group> <user>

  将用户<user>加入到（追加到）组<group>中，其中选项[-aG]是追加到组的意思。

  （2）重启虚拟机系统

  然后进入系统，共享文件夹已经可以正常使用。

## 2. 共享权限

- 如下指令也应该能够解决问题：

  ```
  sudo gpasswd -a $USER vboxsf
  ```

  