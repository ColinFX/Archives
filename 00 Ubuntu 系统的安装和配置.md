# Ubuntu 系统的安装和配置

本文阐述了如何安装 Ubuntu 系统到空硬盘上以实现双系统，以及安装完成后的一系列常用基础配置。

安装系统：Ubuntu 18.04.6 LTS

本机电脑：DELL XPS 15-9570

目标硬盘：SAMSUNG 500GB Portable SSD T5

U盘启动盘：SanDisk USB 2.0 16GB

快照日期：2021年9月11日

## 准备工作

### 目标硬盘分区

1. 在 Windows 系统中下载并安装 Disk Genius，下载链接详见文末。

2. 插入目标硬盘，通过 Disk Genius 软件对硬盘进行完全格式化，对所有的分区逐一右击，选择`删除当前分区`。请注意数据备份。若原硬盘有密码或指纹锁定，请取消设置。

3. 通过 Disk Genius 软件在空硬盘中划分分区，在界面上方横条显示“空闲”处右击选择`建立新分区`，依次添加以下分区：

    |分区类型|文件系统类型|推荐大小|卷标|备注|
    |---|---|---:|---|---|
    |主分区|exFAT|100GB|`Samsung_T5`|通用存储区，在任何操作系统下均可读取|
    |主分区|FAT32|1GB|`efi`|系统引导分区|
    |主分区|Linux swap|20GB|`primary`|系统缓存分区，需略大于本机硬件 RAM 存储空间以使用休眠功能|
    |主分区|EXT4|379GB|`/`|新系统的主要存储空间|
    
    其中 Linux swap 分区可以在多个系统之间共享，因此如果本机连接有其他带有 Linux swap 分区的硬盘，在这里也可以选择不创建该分区。

    也可以选择将 `/` 卷和 `/home` 卷分离开来，以便于在大版本更新 Ubuntu 系统时（如从 18.04.6 LTS 更新到 20.04.4 LTS）保留个人系统配置文件，分区方式如下：
    
    |分区类型|文件系统类型|推荐大小|卷标|
    |---|---|---:|---|
    |主分区|exFAT|100GB|`Samsung_T5`|
    |主分区|FAT32|1GB|`efi`|
    |主分区|Linux swap|20GB|`primary`|
    |拓展分区|EXT4|50GB|`/`|
    |拓展分区|EXT4|329GB|`/home`|
    
    其中由于硬盘总分区数的限制，需要先创建一个整体的拓展分区，然后再对拓展分区进行二次分区。

### 启动盘制作

1. 在 Windows 系统中下载并安装 UltraISO，下载链接详见文末。

2. 下载 Ubuntu 18.04.6 LTS 镜像文件，下载链接详见文末，注意选择 Desktop 版本，并且确认本机处理器为 AMD 架构。Ubuntu 18.04.6 LTS 暂不支持 ARM 架构处理器，清选择 Ubuntu 20.04.4 LTS。

3. 插入U盘，通过 UltraISO 软件安装系统镜像到U盘上，在界面左下角选择镜像文件所在文件目录，在界面右下角双击镜像文件。 

4. 在界面上方菜单栏选择`启动`，`写入镜像`。

5. 在弹出窗口中选择正确的硬盘驱动器，点击`格式化`，格式化完成后点击`写入`。请注意数据备份。

## 安装系统

1. 关机，插入u盘启动盘和目标硬盘。

2. 按下开机键，连按 F2 进入 Bios 设置界面，在 `Secure Boot`、`Secure Boot Enable` 下选择 `Disabled`。不同的主机厂商所对应的 Bios 设置按键不同，请提前查询。

3. 点击 `Apply` 后重启电脑，连按 F12 进入 Boot 界面，选择插入的U盘启动盘。不同的主机厂商所对应的 Boot 界面按键不同，请提前查询。

4. 选择 `Try Ubuntu Without Installing`。

5. 加载完成后在桌面双击 `Install Ubuntu 18.04.6 LTS`。

6. 选择语言为 `English`，设置键盘布局为 `English (US)`。

7. 选择 `Normal installation`，勾选 `Install third-party software`。

8. 在 Installation Type 界面选择 `Something else`，手动分配分区。

9. 按上文分区表逐一设置 `efi` 分区、 `Linux Swap` 分区和 `/` 分区的文件系统类型、大小和卷标。

10. 选择 Device for boot loader installation 为前文 `efi` 分区。

11. 继续安装，选择时区，设置用户名和密码。

12. 安装完成后选择重新启动，拔出u盘启动盘。

13. (可选)对u盘启动盘重新全盘格式化，恢复正常使用。

14. 稍后若需要切换启动的系统，请按 F12 进入 Boot 界面。

## 后续常用基础配置

### 设置中文输入法

1. 打开 Settings，选择 Region & Language，选择 Mnage Installed Languages，完成新系统初始化后添加 Chinese Simplified。

2. `colinfx@local:~$ sudo apt-get install ibus-pinyin`

3. 重新启动电脑。

4. 打开 Settings，选择 Region & Language，添加键盘 Chinese(Intelligent Pinyin)。

5. 设置模糊音和补充词典。

### 设置双系统时钟问题

Windows 系统将硬件时间（Hardware clock）作为本地时间，而 Ubuntu 系统将硬件时间作为 UTC 标准时间。为了解决该冲突，可以切换 Ubuntu 系统下的时间模式。

1. `colinfx@local:~$ timedatectl set-local-rtc 1`

2. 重新启动电脑。

### 拓展 apt 下载链接列表

`colinfx@local:~$ sudo add-apt-repository ppa:deadsnakes/ppa`

### 安装 python 3.7

`colinfx@local:~$ suo apt install python 3.7`

### 设置任务栏时间格式

```
colinfx@local:~$ gsettings set org.gnome.desktop.interface clock-show-date true
colinfx@local:~$ gsettings set org.gnome.desktop.interface clock-show-seconds true
```

## 相关链接

参考教程：https://blog.csdn.net/qq_42699580/article/details/104312383

Disk Genius 下载地址：https://blog.csdn.net/qq_42699580/article/details/104312383

UltraISO 下载地址：https://cn.ultraiso.net/xiazai.html

Ubuntu 18.04.6 LTS 镜像文件下载地址：https://releases.ubuntu.com/18.04/

