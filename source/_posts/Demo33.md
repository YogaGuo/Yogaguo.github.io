---
title: Servlet与request入门（一)
date: 2019-09-11 18:21:09
tags: servlet  
categories: [Servlet,Tomcat]
---

### Servlet  
- 概念：servlet applet,运行在服务器端的小程序,servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则 
- 将来我们自定义一个类，实现复写Servlet接口，复写方法.  
#### 快速入门：  
- 1.创建JavaEE的项目  
 {% asset_img 01.jpg  %}
- 2.定义一个类，实现Servlet接口，实现接口中的抽象方法  
```public class ServletDemo01 implements Servlet ```
- 3.配置Servlet，在web.xml中配置 
 ```
 <!--配置Servlet -->
    <servlet>
        <servlet-name>demo01</servlet-name>
        <servlet-class>cn.yogaguo.web.servlet.ServletDemo01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo01</servlet-name>
        <url-pattern>/demo01</url-pattern>
    </servlet-mapping>
</web-app>  
 ```
 - 4.执行原理:  
   - 当服务端接收到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径  
   - 查找web.xml文件，是否有对应的<url-pattern>标签体内容。如果有，则在找到对应的<servlet-class>全类名  
   - tomcat会将字节码文件加载到内存，并且创建其对象  
   - 调用其方法  
 #### Servlet中的生命周期:  
- 被创建:执行init方法,只执行一次  
  - Servlet什么时候被创建  
      - 默认情况下，第一次被访问，Servlet被创建  
      - 当然，可以配置Servlet的创建时机    
      **在<servlet>标签下配置**  
```  
 <servlet>
        <servlet-name>demo02</servlet-name>
        <servlet-class>cn.yogaguo.web.servlet.ServletDemo02</servlet-class>
        <!--指定Servlet的创建时机
               1.第一次被访问时，创建
                    * <load-on-startup>的值为负数，对应1情况（默认值为-1）
               2.在服务器启动时，创建
                    * <load-on-startup>的值为0或正整数
               -->
        <load-on-startup>5</load-on-startup>
    </servlet>  
```
  **Servlet的init方法只执行一次，说明了一个Servlet在内存中只存在一个对象，Servlet是单例的,所以，多个用户访问时，可能存在线程安全问题**  
  ~~尝试加锁，但是不行，影响了性能~~,***解决办法：尽量不要在Servlet中定义成员变量，即使定义了成员变量，也不要对其修改值，避免并发的操作***
- 提供服务:执行service方法，执行多次  
每次访问Servlet时，Service方法都会被调用一次
- 被销毁：执行destory方法，只执行一次  
Servlet被销毁时执行，即服务器正常关闭时，**注意：destory方法在Servlet被销毁之前执行，一般用于释放资源**  
#### 解决Servlet配置问题  
- Servlet3.0之后，支持注解配置，可以不需要web.xml  
 {% asset_img 02.jpg  %}
- 步骤：  
1.创建JavaEE的项目,选择Servlet的版本，3.0以上，可以不创建web.xml  
2.定义一个类，实现Servlet接口  
3.复写方法  
**4.在类上使用@WebServlet注解，进行配置**  
``` @WebServlet(urlPatterns = "/资源路径") ```  
#### IDEA与Tamcat的相关配置  
- 1.IDEA会为每一个tomcat部署的项目单独建立一份配置文件  
查看控制台的输出log:Using CATALINA_BASE:   "C:\Users\LENOVO\.IntelliJIdea2018.2\system\tomcat\_Test"  
- 2.工作空间目录和Tomcat部署的web项目  
   - **Tomcat真正访问的是“Tomcat部署的web项目，以我电脑为例 D:\IntelliJIDEA2018\Test\out\artifacts\TestDemo02_war_exploded”，Tomcat部署的web项目对应着工作空间项目的web目录下的所有资源，而且web-INFO下的classes对应的字节码文件是工作空间src下的Java文件被编译后放到里面的**  
   - ***WEB_INFO目录下的资源不能被浏览器直接访问，所以一般不把一些静态资源放到这下面***  
   - 在Tomcat如何断点调试
   使用“小虫子”启动  
 {% asset_img 03.jpg  %}








​    



 
