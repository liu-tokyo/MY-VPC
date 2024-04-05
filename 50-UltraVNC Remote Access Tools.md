# 远程桌面连接

## 1. Ubuntu

### 1.1 UltraVNC

> 只兼容 `Windows` ，没有相关 `Linux` 的版本。估计需要在 `Linux` 终端上，开启 `VNC` 的相关协议。

下载：

- [Home - UltraVNC VNC OFFICIAL SITE, Remote Desktop Free Opensource (uvnc.com)](https://uvnc.com/)

  尤其是 KVM 建立的虚拟机，直接就兼容了 远程连接 的协议，只要下载了相应的客户端，就能够很容易实现远程连接。





### 1.2 XRDP

`XRDP`是一种开源工具，它允许用户通过`Windows RDP`访问`Linux`远程桌面。 除了`Windows RDP`外，`XRDP`工具还接受来自其他`RDP`客户端(如`FreeRDP`、`rdesktop`和`NeutrinoRDP` )的连接。 相较于`VNC`，`XRDP`更加的轻量级。下面简单几步实现`ubuntu XRDP+cpolar`内网穿透工具，实现`Windows远程桌面`控制`Ubuntu`。 

#### 1.2.1 安装 `XRDP`

- 更新 `APT` 包管理器

  ```
  sudo apt update
  ```

- 下载安装 `XRDP`

  ```
  sudo apt install xrdp
  ```

- 启动服务（如在启动提示错误,可能是端口冲突,重启设备再尝试）

  ```
  sudo systemctl start xrdp
  ```

- 查看状态（active表示成功）

  ```
  systemctl status xrdp
  ```

- 设置开机启动

  ```
  sudo systemctl enable xrdp
  ```

- 开启远程桌面开关  
  ![image-20231218021615101](C:\Users\KST_LIUZHIJUN\AppData\Roaming\Typora\typora-user-images\image-20231218021615101.png)

#### 1.2.2 局域网测试连接

- 查看ip地址：

  ```
  ip address
  ```

- 避免连接出现问题，先在防火墙中添加一个3389端口：

  ```
  sudo ufw allow from any to any port 3389 proto tcp
  ```

- 然后记得退出登录

  一定要记得，否则连接不上，这一步目的是让Ubuntu处于锁屏界面。

- 然后打开windwos远程连接工具

  输入我们上面查看的`ubuntu`局域网`ip地址,`，然后点击连接，输入相应的`用户名`和`登录口令`，即可正常连接。

  界面和直接登录操作的界面稍微不同，需要注意的是，操作结束之后，需要注销用户（恢复到锁屏状态）。

### 1.3 cpolar

> 参照网站：https://www.bilibili.com/read/cv25443498/

使用`cpolar`穿透本地`XRDP`服务,使得`Windwos远程桌面`可以远程进行访问。cpolar支持http/https/tcp协议，不限制流量，操作简单，无需`公网IP`，也无需路由器。

专业版的话，可以固定公网IP，便于制作自己的服务器。

cpolar（极点元）官网：https://www.cpolar.com/

