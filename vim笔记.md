# vim笔记

**大感谢**

[Vim中如何快速进行光标移动？](https://harttle.land/2015/11/07/vim-cursor.html)

[vim正则表达式很强大](https://www.cnblogs.com/penseur/archive/2011/02/25/1964522.html)

[简明vim](https://coolshell.cn/articles/5426.html#%E7%AC%AC%E4%BA%8C%E7%BA%A7_%E2%80%93_%E6%84%9F%E8%A7%89%E8%89%AF%E5%A5%BD)

***

**vim博大精深，私以为初学者应掌握三点，这三点分别是`光标移动`、`查找替换`、`正则表达`**，然后应该再了解一下代码高亮，自动填充等内容。

![](https://harttle.land/assets/img/blog/vim-key.png)

## 1. 光标移动

### 1.上下左右

用方向键或者`hjkl`(其中j是朝下，您看j的模样是不是朝下的)

### 2.行尾行首

`0`行首

`^`非空格行首

`$`行尾

`g_`非空格行尾

![1586327010819](/tmp/1586327010819.png)

### 3.词首词尾

`w`下一个词头

`e`下一个词尾

`b`上一个词头

`ge`上一个词尾

### 3.复制黏贴

`dd`删除并复制当前行

`yy`复制当前行

`p`黏贴当前行

`ye`当前位置拷贝到本单词的最后一个字符

### 4.自由跳转

N`G`到第 N 行

`gg`到第一行

`G`到最后一行

`ggyG`复制全篇

### 5.可视化

`v`可视化选择

`gu`变小写

`gU`大写

```
命令行模式下：
:history 查看所有命令行模式下输入的命令历史
:history search或 / 或？ 查看搜索历史

普通模式下：
q/ 查看使用/输入的搜索历史
q? 查看使用？输入的搜索历史
q: 查看命令行历史
```

### 历史命令

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200411202036872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1eXVlcw==,size_16,color_FFFFFF,t_70)

## 2.正则

### 1.断言

[断言](http://notes.maxwi.com/2015/12/13/vim-regexp/)

`/tu\(壤\)\@=`是查找后面是壤的tu的意思



***

**补充**

显示高亮  `set lhsearch`  

取消高亮的词汇`noh`

显示行号`set nu`

取消行号`set nonu`

快速保存退出`ZZ`

代码高亮 `syntax on `