# 第 5.9 节 压缩与解压

## zip

安装 zip 压缩文件：

```
# pkg install zip
```

或者

```
# cd /usr/ports/archivers/zip/
# make install clean
```

zip 解压的话，基本系统自带 `unzip`，不用安装。

```sh
# zip test.zip test # 压缩成 zip 文件

# unzip test.zip # 解压 zip 文件
```

## tar/xz

基本系统自带 tar、xz 相关工具，同样也不用安装。

```sh
# tar -cvf test.tar test # 压缩成 tar 格式文件

# tar -xvf test.tar # 解压 tar 格式文件

# tar -zcvf test.tar.gz test # 压缩成 gzip 格式文件

# tar -zxvf test.tar.gz # 解压 gzip 格式文件

# tar -jcvf test.tar.bz2 test # 压缩成 bzip2 格式文件

# tar -jxvf test.tar.bz2 # 解压 bzip2 格式文件

# tar -Jcvf test.tar.xz test # 压缩成 xz 格式文件

# tar -Jxvf test.tar.xz # 解压 xz 格式文件

# xz -z -k test.tar # 压缩成 xz 格式文件；如不带参数 -k，命令执行后将删除原文件

# xz -d -k test.tar.xz # 解压 xz 格式文件；如不带参数 -k，命令执行后将删除原文件
```

## 7z/7za

FreeBSD 操作系统下，7z 和 7za 命令均应通过下载 `7-zip` 使用。

安装：

```
# pkg install 7-zip
```

或者

```
# cd /usr/ports/archivers/7-zip/
# make install clean
```

示例如下：

```sh
# 7z a test.7z test # 压缩成 7z 文件
# 7z x test.7z # 解压 7z 文件
# 7za a test.7z test # 压缩成 7za 文件
# 7za x test.7z # 解压 7za 文件
```
