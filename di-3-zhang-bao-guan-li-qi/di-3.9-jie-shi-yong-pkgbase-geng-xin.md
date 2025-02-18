# 第 3.9 节 使用 pkgbase 更新 FreeBSD

现在 FreeBSD 的系统更新是与第三方软件更新的分离的（现在使用 `freebsd-update`），pkgbase 是目的就是将其合并起来统一使用 `pkg` 命令进行管理（学习 Linux？）。因为现在只有一级架构的 RELEASE 才有 `freebsd-update` 可用。pkgbase 早在 2016 年就有了，原计划在 FreeBSD 14 就进入系统替代 `freebsd-update`，但是现在推迟到了 15。另外个人感觉 `freebsd-update` 体验非常差，非常慢（网络无关）。

**pkgbase 的设计初衷是为了让 stable、current 和 release（BETA、RC 等）都能使用一种二进制工具进行更新。当下，stable、current 只能通过完全编译源代码的方式来更新。**

目前不清楚是否让系统组件成为可选的部分（比如 cron、ntp 等第三方软件），这样就可以在 `bsdinstall` 安装的时候选择系统组件了。

>**警告**
>
>**存在风险，可能会丢失所有数据！**

## 配置软件源


| **分支** | **更新频率** | **URL 地址** |
| :---: | :---: | :--- |
| main（15.0-CURRENT） | 每天两次：08:00、20:00 | <https://pkg.freebsd.org/${ABI}/base_latest> |
| main（15.0-CURRENT） | 每周一次：星期日 20:00 | <https://pkg.freebsd.org/${ABI}/base_weekly> |
| stable/14 | 每天两次：08:00、20:00  | <https://pkg.freebsd.org/${ABI}/base_latest> |
| stable/14 | 每周一次：星期日 20:00 | <https://pkg.freebsd.org/${ABI}/base_weekly> |
| releng/14.0（RELEASE） | 每天两次：08:00、20:00 | <https://pkg.freebsd.org/${ABI}/base_release_0> |
| releng/14.1（RELEASE） | 每天两次：08:00、20:00 | <https://pkg.freebsd.org/${ABI}/base_release_1> |

**以上表格的时间已转换为北京时间，即东八区时间。为 FreeBSD 官方镜像站时间。**

以 FreeBSD 15.0-CURRENT 为例（当下仅支持 14 和 15）

创建文件夹：

```sh
# mkdir -p /usr/local/etc/pkg/repos/
```

编辑配置文件：

```sh
# ee /usr/local/etc/pkg/repos/FreeBSD-base.conf 
```

写入以下内容，保存退出：


### 南京大学开源镜像站 NJU

```sh
FreeBSD-base: {
  url: "https://mirrors.nju.edu.cn/freebsd-pkg/${ABI}/base_latest",
  enabled: yes
}
```

### 中国科学技术大学开源镜像站 USTC 

```sh
FreeBSD-base: {
  url: "https://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/base_latest",
  enabled: yes
}
```

### 网易开源镜像站 163

```sh
FreeBSD-base: {
  url: "https://mirrors.163.com/freebsd-pkg/${ABI}/base_latest",
  enabled: yes
}
```

### FreeBSD 官方镜像站

```sh
FreeBSD-base: {
  url: "pkg+https://pkg.FreeBSD.org/${ABI}/base_latest",
  mirror_type: "srv",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled: yes
}
```

## 第一次安装更新

更新系统：

```sh
root@ykla:~ #  pkg update 
Updating FreeBSD repository catalogue...
Fetching meta.conf:   0%
FreeBSD repository is up to date.
Updating FreeBSD-base repository catalogue...
Fetching meta.conf: 100%    178 B   0.2kB/s    00:01    
Fetching data.pkg: 100%   40 KiB  40.6kB/s    00:01    
Processing entries: 100%
FreeBSD-base repository update completed. 527 packages processed.
All repositories are up to date.
```

```sh
# pkg install -r FreeBSD-base -g 'FreeBSD-*'
```

<details> 
<summary>点击此处展开 pkg 输出</summary>

```sh
root@ykla:~ # pkg install -r FreeBSD-base -g 'FreeBSD-*'
Updating FreeBSD-base repository catalogue...
FreeBSD-base repository is up to date.
All repositories are up to date.
Updating database digests format: 100%
The following 527 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
	FreeBSD-acct: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-acct-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-acct-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-acpi: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-acpi-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-acpi-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-apm: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-at: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-at-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-at-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-audit-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-autofs: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-autofs-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-autofs-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bhyve: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bhyve-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bhyve-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-blocklist-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bluetooth-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bootloader: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsdinstall: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsdinstall-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsdinstall-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-bsnmp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-caroot: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ccdconfig: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ccdconfig-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ccdconfig-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-certctl: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-certctl-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clang: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clang-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clang-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clang-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-clibs-man-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-console-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-console-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-console-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cron: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cron-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cron-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-csh: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-csh-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-csh-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ctf-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ctf-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ctf-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cxgbe-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cxgbe-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-cxgbe-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-devmatch-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dhclient: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dhclient-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dhclient-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dma: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dma-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dma-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtb: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dtrace-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dwatch: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-dwatch-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ee: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ee-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ee-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-efi-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-elftoolchain: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-elftoolchain-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-elftoolchain-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-examples: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fetch-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftpd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftpd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ftpd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fwget: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-fwget-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-games: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-games-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-games-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-geom: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-geom-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-geom-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-geom-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-geom-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ggate: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ggate-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ggate-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hast: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hast-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hast-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hostapd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hostapd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hostapd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hyperv-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hyperv-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-hyperv-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-inetd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-inetd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-inetd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipf: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipf-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipf-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipfw: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipfw-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ipfw-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-iscsi: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-iscsi-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-iscsi-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-jail: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-jail-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-jail-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kerberos-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic-mmccam: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic-mmccam-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic-nodebug: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-kernel-generic-nodebug-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lib9p-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libarchive-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbegemot-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libblocksruntime-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsdstat-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbsm-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libbz2-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcasper-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcompat-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcompat-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcompat-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcompiler_rt-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcompiler_rt-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libcuse-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libdwarf-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libevent1-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libexecinfo-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libldns-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liblzma-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libmagic-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libopencsd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libopencsd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libopencsd-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libpathconv-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librpcsec_gss-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-librss-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsdp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libsqlite3-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdbuf-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libstdthreads-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libthread_db-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libucl-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libvmmapi: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libvmmapi-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-libvmmapi-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liby-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-liby-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lld: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lld-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lld-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lldb: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lldb-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lldb-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-locales: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-lp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mlx-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mlx-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mlx-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mtree: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mtree-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-mtree-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-natd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-netmap-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-newsyslog: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-newsyslog-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-newsyslog-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nfs: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nfs-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nfs-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ntp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ntp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ntp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nuageinit: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nvme-tools: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nvme-tools-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-nvme-tools-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-lib-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-openssl-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-periodic: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-periodic-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pf: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pf-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pf-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pf-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pkg-bootstrap: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pkg-bootstrap-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-pkg-bootstrap-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ppp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ppp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ppp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-quotacheck: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-quotacheck-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-quotacheck-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rc: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rc-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rc-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rcmds: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rcmds-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rcmds-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rdma: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rdma-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rdma-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-rescue: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-resolvconf: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-resolvconf-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-runtime-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-sendmail-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-smbutils-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-src: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-src-sys: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ssh-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-syscons: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-syslogd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-syslogd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-syslogd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tcpd-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-telnet: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-telnet-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-telnet-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tests: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tests-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tests-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-tests-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-ufs-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-unbound-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-utilities-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-vi: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-vi-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-vi-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-vt-data: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-wpa: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-wpa-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-wpa-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-yp: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-yp-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-yp-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-dbg: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-dbg-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-dev: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-dev-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-lib32: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zfs-man: 15.snap20240806043323 [FreeBSD-base]
	FreeBSD-zoneinfo: 15.snap20240806043323 [FreeBSD-base]

Number of packages to be installed: 527

The process will require 5 GiB more space.
1 GiB to be downloaded.

Proceed with this action? [y/N]:
```
</pre> </details>

## 第一次安装 pkgbase 后的准备工作（仅需这一次，以后可能不再需要）

还原配置文件（**如果不做这一步，你的密码都会被清零**）：

```
# cp /etc/ssh/sshd_config.pkgsave /etc/ssh/sshd_config
# cp /etc/master.passwd.pkgsave /etc/master.passwd
# cp /etc/group.pkgsave /etc/group
# pwd_mkdb -p /etc/master.passwd
# service sshd restart
# cp /etc/sysctl.conf.pkgsave /etc/sysctl.conf
```

- 比对旧系统文件（这谁比的完？）：

```sh
# find / -name "*.pkgsave" -type f -exec sh -c "f='{}'; echo '==== OLD ===='; ls -l \${f}; md5sum \${f}; echo '==== NEW ===='; ls -l \${f%.pkgsave}; md5sum \${f%.pkgsave}; cp -vi \${f} \${f%.pkgsave}" \;
```
这是不可行的。

故，经过实际测试，按照 WIKI，直接删掉旧系统的文件就行：

>**注意**
>
>我测试的是纯净系统，没有任何多余配置及第三方程序（除了 pkg），仅开了 SSH 服务。

```
# find / -name \*.pkgsave -delete
# rm /boot/kernel/linker.hints
```


>**警告**
>
>**存在风险，可能会丢失所有数据！**

>**技巧**
>
>**经测试，以后更新直接 `pkg upgrade` 即可，不再需要此节内容**

## 参考文献

- [wiki/PkgBase](https://wiki.freebsd.org/PkgBase)

