# jQuery学习的几个不是问题的问题

[jQuery api](http://jquery.cuishifeng.cn/)

**1. 引号问题**

	- 如果是atr，那么key就不用加引号

![image-20200321111043749](jQuery%E5%AD%A6%E4%B9%A0%E7%9A%84%E5%87%A0%E4%B8%AA%E4%B8%8D%E6%98%AF%E9%97%AE%E9%A2%98%E7%9A%84%E9%97%AE%E9%A2%98/image-20200321111043749.png)

`		$($('#pu')[0]).attr({value: "恢复"})`**这样的情况就不加引号**

	- css 所有都要加引号

![image-20200321111226173](jQuery%E5%AD%A6%E4%B9%A0%E7%9A%84%E5%87%A0%E4%B8%AA%E4%B8%8D%E6%98%AF%E9%97%AE%E9%A2%98%E7%9A%84%E9%97%AE%E9%A2%98/image-20200321111226173.png)

 - 选择器的引号

   ​	![image-20200321111345118](jQuery%E5%AD%A6%E4%B9%A0%E7%9A%84%E5%87%A0%E4%B8%AA%E4%B8%8D%E6%98%AF%E9%97%AE%E9%A2%98%E7%9A%84%E9%97%AE%E9%A2%98/image-20200321111345118.png)

   ​	首先为了不引起歧义，要单引号和双引号混合着用`$('div[title="test"]')`。第二，从图上可以看出，属性的名称不用加引号