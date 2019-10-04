---
title: Web相关知识入门
date: 2019-09-11 17:07:56
tags: Web  
categories: Servlet
---

## Web服务入门介绍 
- web相关概念回顾
- web服务器软件:Tomcat
- Servlet入门学习  
### web相关概念回顾  
- 1.软件架构  
  - 1.C/S:客户端/服务器端    
  - 2.B/S:浏览器/服务器端 (重点) 
- 2.资源分类  
  - 1.静态资源:所有用户访问后，得到的结果都是一样的,静态资源可以直接被浏览器解析 * 如:html,css,Javascript  
  - 2.动态资源:每个用户访问相同资源后，得到的结果可能不一样 ,动态资源被访问后需要先转为静态资源，再返回给浏览器(Response) *如 servlet/jsp,php  
- 3.网络通讯三要素  
  - 1.IP:电子设备在网络中的唯一标识 
  - 2.端口:应用程序在计算机中的唯一标识 0-65535
  - 3.传输协议:规定了数据传输的规范
    - 1.基础协议:  
      - 1.tcp:安全的协议，三次握手确认双方都在线，在进行传输 ，速度稍慢 
      - 2.udp:不安全协议，速度快  
### web服务器软件  
- 1.服务器：安装了服务器软件的计算机  
- 2.服务器软件:接受用户的请求，处理请求，做出响应  
- web服务器软件：接受用户的请求，处理请求，做出响应   
     - 1.在web服务器软件中可以部署web项目，让用户通过浏览器来访问这些项目  
     - 2.web容器  
- 3.常见java相关的web服务器软件  
  - 1.webLogic:oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的  
  - 2.webSphere:IBW公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的    
  - 3.JBOSS:JBOSS公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的  
  - 4.Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范(servlet/jsp),开源的，免费的  
- 4.JavaEE:Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范  
### Tomcat:web服务器软件  
- 1.下载: [官网地址](http://tomcat.apache.org/)  
- 2.安装: 解压压缩包 (**注意：安装目录建议不要有中文和空格**) 
- 3.卸载: 删除目录就行  
- 4.启动:bin/startup.bat双击该文件  
  - 1.访问自己：浏览器输入：(http://localhost:8888)(我自己该的端口号，默认8080) 回车访问  
  - 2.访问别人:(http://别人ip:8080)  
  - 3.可能遇到的问题:  
    - 1.黑窗口一闪而过：没有正确配置JAVA_HOME环境变量  
    - 2.启动报错：
      - 1.暴力解决->找到占用的端口号，并且找到对应进程，杀死该进程.  
      - 2.温柔解决:修改自身端口号,   conf/server.xml     服务器的主配置文件   **注意：改动此文件时先备份，以防改错**  
      ```
       <Connector port="8888" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />  
      ```
- 5.关闭:
  
  - 1.bin目录命令窗口直接敲  shutdown/catalina stop 二者都可以,或者直接双击shutdown文件
- 6.配置:
  - 1.部署项目的方式：  
    - 1.直接将项目放到webapps目录下即可  
      - 1. /hello:项目的访问路径--->虚拟目录  
      - 2.简单部署：将项目打包成war包,再将war包放置到webapps目录下，war包会自动解压  
    - 1.配置conf/server.xml文件  
    - 1.在conf\Catalina\localhost创建任意名称的xml文件，在文件中编写  
  - 1.静态项目和动态项目  
    - 1.目录结构  
      - 1.Java动态项目的目录结构  
        - 项目根目录  
          - WEB-INF目录：  
            - web.xml:web项目核心配置文件  
            - class目录：放置字节码文件  
            - lib目录：放置依赖的jar包  
   - 将Tomcat集成到IDEA中，并且创建JavaEE的项目,部署项目  
     - Run->Edit configurations->Templates->Tomcat server->Local  
     - 配置Tomcat路径  
     - ![如图](![](http://ww1.sinaimg.cn/large/006VKolAly1g6dcz3t5ywj30lv043glj.jpg))  
     - 热部署：保证每次更新资源，Tomcat会自动启动
     - ![](![](http://ww1.sinaimg.cn/large/006VKolAly1g6dd6hai4nj30bc03ka9w.jpg))  
     - 


