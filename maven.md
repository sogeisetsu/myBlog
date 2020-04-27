# MAVEN基础



- *书籍资料*

  - 传智播客官方资料
    - https://drive.google.com/file/d/1PL091NdOWPGzVcs_2YZzDmlvOSu0w513/view?usp=sharing 谷歌云盘 (可在线查看， 对网络环境有要求)
    - https://dochub.com/suyuesheng01/M1qW4lR/maven基础讲义-pdf?dt=_KGqiqs6zYVq5sWw4A4j (可在线查看， 对网络环境有要求)
    - [https://gitee.com/sogeisetsu/myBlog/blob/master/Maven%E5%9F%BA%E7%A1%80%E8%AE%B2%E4%B9%89.pdf](https://gitee.com/sogeisetsu/myBlog/blob/master/Maven基础讲义.pdf)（ 不可在线阅读 需下载，对网络环境无要求）
  - 《maven应用实战》
    - https://www.jianguoyun.com/p/Dep89nEQyvP7BxjW3_8C （可在线查看，需注册坚果云账号，下载速度较快。缺点是在线查看清晰度较差）
    - https://pan.baidu.com/s/1bQ4dPiLT5rjiXR7c1kXfbw（百度云链接，缺点：速度极慢）
    - https://drive.google.com/file/d/1xHuzalmYL19RXk1njuqTCy5tsOB-_LTC/view?usp=sharing 谷歌云盘 (可在线查看， 对网络环境有要求)

- **初始化**

  - 下载http://archive.apache.org/dist/maven/maven-3/

  - 安装maven到一个没有中文的目录（解压操作）

    - 解压后目录结构如下：

      ![img](https://img.mubu.com/document_image/e95b3a3a-2257-4c72-ad43-ef3e555aa517-6002481.jpg)

  - 配置 MAVEN_HOME ，变量值就是你的 maven 安装 的路径（bin 目录之前一级目录），path 里添加 %MAVEN_HOME%/bin

    - 请检查以下 JAVA_HOME 是否为jdk安装路径

  - 在cmd 输入 mvn -v

    - 显示这个说明成功

      ![img](https://img.mubu.com/document_image/0a94fdc5-d128-4566-ac58-915cd412e963-6002481.jpg)

  - 到 %MAVEN_HOME%/conf/settings.xml 配置本地仓库位置

    - 地址自己设定

      ![img](https://img.mubu.com/document_image/037b1d49-c392-4671-aa08-2c0e1b14c3ce-6002481.jpg)

  - @ideamaven

- 工作流程

  - 

    ![img](https://img.mubu.com/document_image/9af3d445-9660-49bf-a78f-fedbc8bc92e0-6002481.jpg)

- 目录结构

  - 

    ![img](https://img.mubu.com/document_image/b1ba5e30-202b-4b98-bef0-b2a55b810141-6002481.jpg)

- 常用命令

  - **clean**

    - clean 是 maven 工程的清理命令，执行 clean 会删除 target 目录及内容。 （**拿到maven，先clean，因为开发环境的不一致，别人编译的文件不一定在另一台电脑运行**）

  - **compile**

    - compile 是 maven 工程的编译命令，作用是将 src/main/java 下的文件编译为 class 文件输出到 target 目录下。（**不会编译src/test/java下的代码**）

    - 执行成功的图片

      ![img](https://img.mubu.com/document_image/b35dc106-20c2-43ab-a9f0-f5475f9b7f65-6002481.jpg)

  - **test**

    - test 是 maven 工程的测试命令 mvn test，会执行 src/test/java 下的单元测试类，（**也会编译src/main/java下的代码**）

  - **package**

    - package 是 maven 工程的打包命令，对于 java 工程执行 package 打成 jar 包，对于 web 工程打成 war 包。 war包还是jar包取决于pom.xml里面的<packing></packing>

      ![img](https://img.mubu.com/document_image/6e327227-9889-4add-8338-ed0c25a05d06-6002481.jpg)

      ![img](https://img.mubu.com/document_image/951aedd9-2c72-4af3-b58f-145589749085-6002481.jpg)

    - **会编译src/\*/java ,并且会执行src/test/java测试类**

  - **install**

    - install 是 maven 工程的安装命令，执行 install 将 maven 打成 jar 包或 war 包发布到本地仓库。
    - 从运行结果中，可以看出： 当后面的命令执行时，前面的操作过程也都会自动执行。（即会将test编译并执行）

- 生命周期

  - **maven生命周期**

    ![img](https://img.mubu.com/document_image/23ab0aec-6708-4821-b9a0-629fc6578462-6002481.jpg)

  - **maven概念模型**

    ![img](https://img.mubu.com/document_image/9f343dfe-4519-4dd0-8714-77fa65a19f52-6002481.jpg)

  - 

- **IDEA maven** @ideamaven

  - \1. 基础配置

    - 在setting 里这样设置

      - 第一步

        ![img](https://img.mubu.com/document_image/cb939b9a-3608-4970-8842-be79429e0110-6002481.jpg)

      - 第二步

        - 在 runner 设置 **-DarchetypeCatalog=internal**

          ![img](https://img.mubu.com/document_image/14fbe1ea-c1ba-43b6-ba32-2cc5a6cbb4dc-6002481.jpg)

  - 2.开启一个maven项目

    - 视频教程https://www.cnblogs.com/sogeisetsu/articles/12578737.html
    - 1.建造一个 project
      - 选择是否 create from archetype(使用骨架) 和 create from archetype的种类
    - 2.指定 groupid 和artifactid
    - 3.检查maven地址
    - 4.补全缺失的文件夹

- **pom.xml**

  - scope

    ![img](https://img.mubu.com/document_image/43e57119-88a0-438c-9217-4445a1b4998c-6002481.jpg)

  - 坐标和路径的对应关系

    ![img](https://img.mubu.com/document_image/a858d379-fe9a-46cd-b7c5-f5d7585f10f8-6002481.jpg)

  - mvn test 中文乱码

    - https://blog.csdn.net/wenxindiaolong061/article/details/54343870

    - 网页截图

      ![img](https://img.mubu.com/document_image/59a6b55e-ba3e-4887-aed0-c6d660a71767-6002481.jpg)