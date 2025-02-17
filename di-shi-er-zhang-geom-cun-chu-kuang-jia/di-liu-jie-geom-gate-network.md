# 第六节 GEOM Gate Network

在 Linux 上，网络块设备（NBD）是一个网络协议，可以用来将一个块设备（通常是硬盘或分区）从一台机器转发到另一台机器。举例来说，一台本地机器可以访问连接到另一台计算机的硬盘驱动器。

该协议最初是为 Linux 2.1.55 开发的，并在 1997 年发布。在 2011 年，该协议被修订，正式记录在案，现在被发展为一个合作的开放标准。有几个可互操作的客户端和服务器。

有 Linux 兼容的 NBD 实现，用于 FreeBSD 和其他操作系统。术语“网络块设备”有时也被笼统地使用。

从技术上讲，一个网络块设备是由三个部分实现的：服务器部分、客户端部分和它们之间的网络。在作为设备节点的客户机上，一个内核驱动程序控制着该设备。每当一个程序试图访问该设备时，内核驱动程序就会将请求（如果客户机部分没有在内核中完全实现，可以在用户空间程序的帮助下完成）转发到服务器机上，数据就在服务器机上。在服务器机上，来自客户端的请求由一个用户空间程序处理。

网络块设备服务器通常作为一个用户空间程序在通用计算机上运行。所有针对网络块设备服务器的功能都可以驻留在用户空间程序中，因为该程序通过传统的套接字与客户端进行通信，并通过传统的文件系统接口访问存储。

网络块设备客户端模块可以在类 Unix 操作系统上使用，包括 Linux 和 Bitrig。由于服务器是一个用户空间程序，它在理论上可以在每个类 Unix 平台上运行；例如，NBD 的服务器部分已经被移植到Solaris。

——<https://en.wikipedia.org/wiki/Network_block_device>

GEOM Gate Network 就是这么一个东西（此处存疑）。

## 参考资料

 - <https://handbook.bsdcn.org/di-19-zhang-geom-mo-kuai-hua-ci-pan-zhuan-huan-kuang-jia/19.6.-geom-gate-wang-luo-she-bei.html>
