# 第六节 Postfix 服务器

Postfix 服务器即邮件服务器，和系统自带 sendmail 本质上是一类存在。在本书的参考文献之一的《鸟哥的 Linux 私房菜》服务器篇中有详细介绍。

## 安装

```
# pkg install mail/postfix
```

>**注意**
>
>由于参数较多，建议使用 ports 编译安装:
>
>```
>cd /usr/ports/mail/postfix/
>make install clean
>```


FreeBSD 上的 postfix 存储目录见 <https://www.freshports.org/mail/postfix/>。

## 参考资料

 - <https://www.freebsd.org/cgi/man.cgi?query=postfix&sektion=&manpath=freebsd-release-ports>
 - <https://www.pbdigital.org/post/2021-02-01-postfix-virtualhosts/>
