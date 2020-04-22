linux 学习之路

*原因：之前学过linux，如今忘了大半，遂决定重新学习，并将自己认为重要的或曾经在使用过程中遇到的问题记录下来*

## 1.常规

### 档案、目录、权限

ls

![1586242696644](/tmp/1586242696644.png)

[我曾经记得笔记](https://www.cnblogs.com/sogeisetsu/p/11388951.html)

ls 显示文件

| -a            |                   显示隐藏文件                   |
| ------------- | :----------------------------------------------: |
| -l            |                     显示具体                     |
| -A            |           显示隐藏文件但是不显示.和..            |
| -d            |                   只显示文件夹                   |
| --color=      |                       颜色                       |
| --time={}     |                       时间                       |
| -r            |       反向，比如默认从大到下，用它从小到大       |
| -h            |            人能读懂的方式显示文件大小            |
| --block-size= | 以什么作为单位，如果等于M，则显示以M作为文件单位 |

cd

![1586243813993](/tmp/1586243813993.png)

mkdir

```
-m ：设定档案的权限喔！直接设定，不需要看预设权限(umask) 的脸色～
-p ：帮助你直接将所需要的目录(包含上层目录)递回建立起来！
```

rmdir

```

-p ：
```

cp的一些常用的命令

| -a   | 相当于-drp                                               |
| ---- | -------------------------------------------------------- |
| -d   | 若为链接文件就复制链接而非文件本身                       |
| -r   | 复制文件夹                                               |
| -i   | 询问                                                     |
| -u   | 比较两文件，若要复制的文件比要被覆盖的文件新才会进行复制 |
| -f   | 强制                                                     |
| -p   | 把属性一同复制过去                                       |
**举例**

```
root@DESKTOP-FA1P4IO:~# ls -l --time={atime,ctime} 12.txt      #显示源文件信息日期是八月5号
-rwxrw-rw- 1 sys001 root 84 Aug  5 12:59 12.txt
root@DESKTOP-FA1P4IO:~# cp 12.txt test1/12.txt          # 复制
root@DESKTOP-FA1P4IO:~# ls -l --time={atime,ctime} 12.txt  #源文件的属性修改时间没有变
-rwxrw-rw- 1 sys001 root 84 Aug  5 12:59 12.txt
root@DESKTOP-FA1P4IO:~# ls -l test1/12.txt  # 显示复制过去的文件信息，显示八月21号（复制源文件过来的时间）创建
-rwxrw-rw- 1 root root 84 Aug 21 14:39 test1/12.txt
root@DESKTOP-FA1P4IO:~# cp -p 12.txt test1/12.txt  # 重新复制，采用-p
root@DESKTOP-FA1P4IO:~# ls -l test1/12.txt
-rwxrw-rw- 1 sys001 root 84 Jul 25 01:23 test1/12.txt
root@DESKTOP-FA1P4IO:~# ls -l --time={atime,ctime} test1/12.txt   # 复制过去的文件和源文件信息一致
-rwxrw-rw- 1 sys001 root 84 Aug 21 14:41 test1/12.txt
```
**-p用于文件备份很好用**

```
[root@study ~]# mv [-fiu] source destination
[root@study ~]# mv [options] source1 source2 source3 .... directory选项与参数：
-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
-i ：若目标文件 （destination） 已经存在时，就会询问是否覆盖！
-u ：若目标文件已经存在，且 source 比较新，才会更新 （updat
```

rm

![r1586247027537](/tmp/1586247027537.png)

tar

![1586250830803](/tmp/1586250830803.png)

**要删除一个文件，对该文件所在目录需要拥有write和execute权限，对该文件本身却没有write权限要求。** 

前缀 PS1

alias 创造命令

### zip 和 unzip

`unzip -o -d /home/sunny myfile.zip`   

```
把myfile.zip文件解压到 /home/sunny/
-o:不提示的情况下覆盖文件；
-d:-d /home/sunny 指明将文件解压缩到/home/sunny目录下；
```

`zip -r myfile.zip ./*`  将当前目录下的所有文件和文件夹全部压缩成myfile.zip文件,－r表示递归压缩子目录下所有文件.

### grep

`cat .bashrc | grep -P 'alias (?=l)' --color `       grep的断言

```
grep -E "[0-9]+" sentence.txt
-E 扩展的正则表达式
-P Perl正则表达式（支持一些高级用法，比如先行断言、后发断言、负向零宽断言等）
```

```
-v 反转查找
-w 只显示全字符合的列
-i 忽略字符大小写的差别
-o 只输出文件中匹配到的部分
-n 显示列号
-F 禁用正则表达式（用来搜索包含正则表达式特殊字符的的场景）
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409165158237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

### 语言

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409093019761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

ubuntu 修改  `/etc/default/locale`  可以修改默认设置  或者修改.bashrc

`xargs`

### ps

![image-20200413164855954](linux%E7%AC%94%E8%AE%B0/image-20200413164855954.png)

### find

```
最近发现find本身就是支持正则表达式的
find path -regex "xxx"
find path -iregex "xxx"

```

```
【find】命令
find 指令用于查找符合条件的文件示例：
find / -name “ins*” 查找文件名称是以 ins 开头的文件
find / -name “ins*” –ls
find / –user itcast –ls 查找用户 itcast 的文件
find / –user itcast –type d –ls 查找用户 itcast 的目录
find /-perm -777 –type d-ls 查找权限是 777 的文件
```

### service

`service --status-all`所有服务

### whereis 和 which

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409162728623.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

### wget

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409163657238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409164224548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409163828429.png)

### dpkg -l

显示所有软件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200410070758301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

### jobs

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409165907221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

### 软链接

ln -s 

```
ln -s 是linux中一个非常重要命令，一定要熟悉。它的功能是为某一个文件在另外一个位置建立一个同不的链接，这个命令最常用的参数是-s,
具体用法是：ln -s 源文件 目标文件。
当 我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后在其它的 目录下
用ln命令链接
（link）它就可以，不必重复的占用磁盘空间。例如：ln -s /bin/less /usr/local/bin/less
-s 是代号（symbolic）的意思。
这 里有两点要注意：第一，ln命令会保持每一处链接文件的同步性，也就是说，不论你改动了哪一处，其它的文件都会发生相同的变化；第二，ln的链接又软链接 和硬链接两种，软链接就是ln -s ** **,它只会在你选定的位置上生成一个文件的镜像，不会占用磁盘空间
硬链接ln ** **,没有参数-s, 它会在你选定的位置上生成一个和源文件大小相同的文件
无论是软链接还是硬链接，文件都保持同步变化。v
```

### [/etc/profile、/etc/bashrc、~/.bash_profile、~/.bashrc 文件的作用](https://www.cnblogs.com/cwp-bg/p/8257843.html)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200410033603232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

上面的例子凡是读取到~/.bash_profile的，若该文件不存在，则读取~/.bash_login，若前两者不存在，读取~/.profile。

<https://www.cnblogs.com/sogeisetsu/>

![](https://img2020.cnblogs.com/blog/1709672/202004/1709672-20200412102941438-1465577002.png)

### $ps1

https://www.jianshu.com/p/0ad354929baf

![image-20200422193755698](linux%E7%AC%94%E8%AE%B0.assets/image-20200422193755698.png)

### centos

yum  

*yum repolist* 列出已经配置的所有可用仓库

#### systemctl

systemctl 鸟哥17章

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200411211546139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

<http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html>

<https://www.jb51.net/article/136559.htm>

#### /etc/sudoers

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200412084844215.png)

