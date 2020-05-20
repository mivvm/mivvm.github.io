---
title: tomcat安装细节
date: 2019-06-19 15:08:53
tag: java,tomcat
description: Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，它早期的名称为catalina，后来由Apache、Sun 和其他一些公司及个人共同开发而成，并更名为Tomcat。

---

- 安装jdk（最好装在C盘）

  - 安装结果展示

    ​		![](tomcat/pic_1.png)

    

  - 配置环境变量 

    - JAVA_HOME，JDK的安装路径
    - CLASSPATH，.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
    - PATH，%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
    - JRE_HOME，JRE的安装路径

  - 监测是否安装成功

    1.cmd输入java：

  ​		![](tomcat/pic_2.png)
  ​	
    2.cmd输入javac：

  ​		![](tomcat/pic_3.png)

  

    

    

    

  

- 安装tomcat路径

  - 配置环境变量 

    - CATALINA_BASE，tomcat的安装路径
    - CATALINA_HOME，tomcat的安装路径

  - 修改端口：conf/server.xml

  - tomcat不能启动测试的小诀窍

    ​	在cmd输入startup.bat会有相应的错误提示

  - 测试tomcat是否安装成功

    ​	1.cmd输入startup.bat

    ​	2.直接双击startup.bat

    ​		![](/tomcat/pic_4.png)

