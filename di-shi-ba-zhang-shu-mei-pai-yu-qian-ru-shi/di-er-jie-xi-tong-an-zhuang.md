# 第二节 系统安装

>**注意**
>
>**FreeBSD 默认提供的 IMG 镜像使用 UFS 文件系统。想使用 ZFS 的用户可在存储卡上刻录好 img 正常启动，再插入 U 盘，运行命令 `bsdinstall` 正常安装（安装位置选择 U 盘）即可。如果想在存储卡上使用  ZFS，反过来用 U 盘进行安装即可。**


自树莓派 3B+ 开始，无需任何改动，系统即可从 U 盘启动，经过测试了 FreeBSD 12/13/14 都是支持的，但是速度非常慢，一方面树莓派使用 USB2.0 极大的限制的总线速度，另一方面可能是玄学问题。（笔者用的是东芝（TOSHIBA）64GB USB3.0 U 盘 U364 高速迷你车载 U 盘）。

因此不建议使用 U 盘启动，慢的我要打年年猫，年年猫是谁？是一只调皮的野生狸花猫而已。

我们所有要准备的有树莓派 4 板子一块，网线一段，存储卡一枚。从 FreeBSD.org 下载适用于树莓派 4 的镜像:

https://download.freebsd.org/ftp/releases/arm64/aarch64/ISO-IMAGES/13.0/FreeBSD-13.0-RELEASE-arm64-aarch64-RPI.img.xz

下载后解压缩。使用 rufus 刻录。插入网线，将存储卡插入树莓派，通电等待约五分钟，查看路由器后台获取 IP 。

**注意：** 刻录完需要挂载 FAT 分区 替换里面的所有文件，否则会启动花屏，替换的文件路径为：

https://github.com/FreeBSD-Ask/FreeBSD-rpi4-firmware
