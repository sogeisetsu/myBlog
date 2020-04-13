# Ubuntu如何设置中文man手册

**大感谢**

[/etc/profile、/etc/bashrc、~/.bash_profile、~/.bashrc 文件的作用](https://www.cnblogs.com/cwp-bg/p/8257843.html)

<https://github.com/man-pages-zh/manpages-zh>

首先下载中文环境，

`sudo apt install manpages-zh`

*知识小讲解：man -M path*

```
 -M 路径, --manpath=路径
              指定要使用的另一 manpath 路径。默认情况下 man 使用 manpath 得到的代码来确定搜索路径。此选项会覆盖 $SYSTEM  环
              境变量。

              指定为   manpath   的路径必须是划分为若干章节的手册页层次结构的根目录。章节描述见  man-db  手册(位于“手册页系
              统”)。要查看层次结构之外的手册页，请参阅 -l 选项。
```

然后造一个cman命令  

`alias cman='man -M /usr/share/man/zh_CN'`

这样就可以使用了

**但是，要把cman整成个默认的命令还有几步要做**

*小知识  su -l*

根据我个人理解，`su -l` 和 `su - `的区别就是前者后面不需要加用户名，`-`表示的是一种状态，一种不仅修改了用户还修改了用户环境的状态。如果不加`-`的话就只会切换用户而不会切换shell的状态

```
 -, -l, --login
           提供一个类似于用户直接登录的环境，用户可能会希望这样。

           When - is used, it must be specified before any username. For portability it is recommended to use it as last option, before any username. The other forms (-l and --login) do not have this restriction.

```

*小知识  bash* 

```
下面是在本机的几个例子： 
1. 图形模式登录时，顺序读取：/etc/profile和~/.profile 
2. 图形模式登录后，打开终端时，顺序读取：/etc/bash.bashrc和~/.bashrc 
3. 文本模式登录时，顺序读取：/etc/bash.bashrc，/etc/profile和~/.bash_profile 
4. 从其它用户su到该用户，则分两种情况： 
    （1）如果带-l参数（或-参数，--login参数），如：su -l username，则bash是lonin的，它将顺序读取以下配置文件：/etc/bash.bashrc，/etc/profile和~/.bash_profile。 
    （2）如果没有带-l参数，则bash是non-login的，它将顺序读取：/etc/bash.bashrc和~/.bashrc 
5. 注销时，或退出su登录的用户，如果是longin方式，那么bash会读取：~/.bash_logout 
6. 执行自定义的shell文件时，若使用“bash -l a.sh”的方式，则bash会读取行：/etc/profile和~/.bash_profile，若使用其它方式，如：bash a.sh， ./a.sh，sh a.sh（这个不属于bash shell），则不会读取上面的任何文件。 
7. 上面的例子凡是读取到~/.bash_profile的，若该文件不存在，则读取~/.bash_login，若前两者不存在，读取~/.profile。
```

根据以上，我们可以总结，只要登录，不管是图形还是用文本，不管是否打开shell。必定会`/etc/profile`发生作用，并会根据用户不同，不同的`.profile`发挥作用。    只要打开shell一定会有`/etc/bash.bashrc`发挥作用，根据longin方式和non-login方式会决定启动各自的`.bashrc`还是根据`.bash_profile`来发挥作用，凡是读取到`~/.bash_profile`的，若该文件不存在，则读取`~/.bash_login`，若前两者不存在，读取`~/.profile`。当同一变量在两个文件里有冲突时，以用户环境为准。

看个图片就明白

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200410094155649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

看到这，您应该根据自己的需求来将`alias cman='man -M /usr/share/man/zh_CN'`放在不同的位置，比如说您想root用户时有cman，那您就将`alias cman='man -M /usr/share/man/zh_CN'`这句话放在root账户的.bashrc。如果您是图形页面的使用者，并且您想在所有用户都有cman命令，那您就将`alias cman='man -M /usr/share/man/zh_CN'`放在/etc/bash.bashrc。如果您的想在图形页面只有root用户有cman，文字登录时所有用户都有cman(这个需求很奇葩啊)，您应该把`alias cman='man -M /usr/share/man/zh_CN'`命令放在`/etc/bash.bashrc`,把出root用户以外的所有用户都来个`unalias cman`就行了。如果想来大团结就在`/etc/profile`放入`alias cman='man -M /usr/share/man/zh_CN'`