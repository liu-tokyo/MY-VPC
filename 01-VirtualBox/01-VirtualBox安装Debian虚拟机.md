# VirtualBox 安装 Debian 虚拟机



## 当前用户追加 sudo 权限

当在终端执行sudo命令时，系统提示“XX is not in the sudoers file”：

其实就是没有权限进行sudo，解决方法如下：

1. 切换到超级用户： 

   ```bash
   su root
   ```

2. 打开/etc/sudoers文件：

   ```bash
   vi /etc/sudoers
   ```

3. 修改文件内容：

   找到 `root ALL=(ALL:ALL) ALL` 一行，在下面插入新的一行，内容是 `lizh ALL=(ALL:ALL) ALL`，然后在vim键入命令 `:wq!` 保存并退出。

   *注：这个文件是只读的，不加“!”保存会失败。*

4. 退出超级用户：

   ```bash
   exit
   ```

   