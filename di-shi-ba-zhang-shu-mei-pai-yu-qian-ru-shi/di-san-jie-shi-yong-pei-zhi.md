# 第三节 使用配置

使用 XShell 即可登录树莓派。用户名密码均为 `freebsd` 。root 需要登录后输入 `su`，密码为 `root` 。

可通过更改 `/etc/ssh/sshd_config` 文件来开启 root 账户的 ssh 远程登录权限。

方法：

编辑 `/etc/sh/sshd_config` （注意是 sshd 不是 ssh ！这是两个文件），修改或者加入

```
PermitRootLogin yes #允许 root 登录
PasswordAuthentication yes # 设置是否使用口令验证。
```

（也可以把对应行前面的注释 # 去掉，注意 `PermitRootLogin` 一行默认是 no，去掉后要改成 yes 。即 `PermitRootLogin yes` ）。

然后重启服务：

```
# server sshd restart
```

然后就是时间设置问题，树莓派没有板载的纽扣电池确保 CMOS 时钟准确。所以完全依靠 NTP 服务来校正时间，如果时间不准确，将影响很多服务的运行，比如无法执行 portsnap auto 命令。

方法很简单：

在 `/etc/rc.conf` 中加入

```
ntpd_enable="YES"
ntpdate_enable="YES"
ntpdate_program="ntpdate"
ntpdate_flags="0.cn.pool.ntp.org"
```

然后开启时间服务器：

```
# service ntpdate start
```

输入 `# date` 查看时间，完成校时。我国使用 UTC+8 北京时，虽然不更改不会影响软件使用，但看起来不方便，可通过 `# bsdconfig` 命令将地区调整到亚洲 /中国 /上海。

树莓派应该会自动接通互联网，所以不必考虑联网问题。

CPU 频率调整（600MHZ 到 1500MHZ）：

```
# sysrc powerd_enable="YES"
# service powerd restart
```
