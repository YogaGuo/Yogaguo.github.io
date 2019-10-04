---
title: Request与Response对象介绍
date: 2019-09-11 18:38:45
tags: Request  
categories: [Servlet,request] 
---

##### Servlet体系结构  
```
Servlet-- 接口  
|  
GenericServlet-- 抽象类  
|  
HttpServlet-- 抽象类  
```
- GernericServlet:将Servlet接口中的方法做了默认空实现，只将Service()方法作了抽象，将来定义Servlet类时，可以继承GernericServlet，实现Service()  
- HttpServlet:对http协议的一种封装，为了简化操作  
   - 定义类继承HttpServlet  
   - 复写doGet()/doPost()  
##### Servlet相关配置  
- urlpartten:Servlet访问路径：一个Servlet可以定义多个访问路径  
```  
@WebServlet({"/demo05","/demo005"})  
```
- 路径的定义规则  
  - 1 /xxx  
  - 2 /xxx/xxx:多层路径
  - 3 *.do(*为通配符)  
##### HTTP:  
- 概念：**Hyper Text Transfer Protocol 超文本传输协议**  
- 传输协议：定义了客户端和服务器端通信时，发送数据的格式  
- HTTP协议特点：
  - 1 基于TCP/IP的高级协议  
  - 2 默认端口为80  
  - 3 基于请求/响应模型：一次请求对应一次响应  
  - 4 无状态协议：每次请求之间相互独立，不能交互数据  
##### 请求消息数据格式  
###### 请求行  
- 请求方式  请求url  请求协议/版本  
      GET/Demo.html HTTP/1.1  
  - HTTP有7中请求方式，常用的由2中  
    - **GET:==请求参数在请求行中==，即在url后,请求的url是有限制的，不太安全**  
    - **Post:==请求参数在请求体中==，请求的url没有限制，相对安全**
###### 请求头  
请求头名称:请求头值  
- 常见的请求头  
   - User-Agent:浏览器告诉服务器，我访问你使用的浏览器版本信息  
   - Referer:http://localhost/index.html-> 告诉服务器，我（当前请求）从哪里来  
     - 防止别人盗取链接
     - 做些统计的工作
###### 请求空行
- 空行：分割Post请求的请求头和请求体的  
###### 请求体（正文）  
- 封装Post请求消息的请求参数的
 {% asset_img 5.jpg  %}
   
```xml
            
   POST /index.html HTTP/1.1
Host: localhost:8888
Connection: keep-alive
Content-Length: 25
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh-TW;q=0.9,zh;q=0.8,en-US;q=0.7,en;q=0.6

uname=yogaguo
```
##### Requset  
###### request和response对象的基本原理
- 1 requset和response对象是由服务器创建的，我们只是使用  
- 2 request对象是获取请求消息，response对象是设置响应消息的  

###### request对象继承体系结构  

```
ServletRequest --接口 
|  
HttpServletRequest --接口  
|  实现
org.apache.catalina.connector.RequestFacade(类)
```
###### request功能  
- 1 获取请求消息数据  
   - 获取请求行数据:```Get/TestDemo02/demo02?name=yogaguo HTTP/1.1```  
   - 方法：  
     - 1.获取请求方式：```String getMethod()```  
     - ==2.获取虚拟目录==(TestDemo02)```String getContextPath```  
     - 3.获取Servlet路径(/demo02)```String ServletPath()```  
     - 4.获取Get方式的请求参数```String getQueryString```  
     - 5.==获取请求的url== ```/TestDemo02/demo02)``` ```String getRequestURI()```:返回的是 ```/TestDemo02/demo```,==StringBuffer getRequestURL()==,返回的是```http://localhost/TestDemo02/demo02```  
       - URL:统一资源定位符：```http://loclahost/TestDemo02/demo02```  
       - URI:统一资源标识符：```/TestDemo02/demo02```
     - 6.获取客户机的IP地址：```String getRemoteAddr()```
   - 获取请求头数据:  
     - 1==String getHeader(String name)== 通过请求头的名称获取请求头的值  
     - 2 ```Enumeration<String> getHeaderNames()``` :获取所有请求头的名称 **(Enumeration<String>类似迭代器)**
   - 获取请求体数据  
     - 只有==Post==请求方式才有请求体;在请求体中封装了Post请求的请求参数  
     - 获取步骤：  
        - 1.获取流对象  
           - 1.```BufferedReader getReader()``` ,获取字符输入流  
           - 2.```ServletInputStream getInputStream()``` ,获取字节输入流
        - 2.再从流对象中拿数据  
        ```  java
         BufferedReader reader=req.getReader();
        String line=null;
        while((line=reader.readLine())!=null){
            System.out.println(line);
        }  
        ```
    - 其他方法：  
      - 1.获取请求参数的通用方法  
         - String getParamater(String name):根据参数名获取参数值；String getParameterValues(String name):根据参数名获取参数值的数组  

   ```java
    Enumeration<String> parameterNames=req.getParameterNames();
                 while(parameterNames.hasMoreElements()){
                    String name= parameterNames.nextElement();
                     System.out.println(name);
                     String value=req.getParameter(name);
                     System.out.println(value);
                     System.out.println("---------------------");
                 }  
   ```
   ```markdown
   {% asset_img 6.jpg  %} 
   ```
     - 中文乱码问题：  
         -  Get方式：Tomcat8已经将中文乱码问题解决了
         - Post方式：会乱码，解决办法，在获取参数时，设置request的编码```req.setCharacterEncoding("utf-8");```
         
   - 2.请求转发:一种在服务器内部的资源跳转方式
    - 步骤  
        - 1.通过request对象获取请求转发器对象: ```RequestDispatcher getRequestDispatcher(String path)```  
        - 2.通过RequestDispatcher对象来进行转发：```forward(ServletRequest request,ServletResponse response)```  
    - **转发特点**：
        - 1.**浏览器地址栏路径不发生变化**  
        - 2.**只能转发到当前服务器内部的资源中**  
        - 3.**转发是一次请求**
        
```java
@WebServlet("/requestDemo7")
public class RequestDemo07 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo7被访问了。。。");
        //转发到demo8资源
        req.getRequestDispatcher("/requestDemo8").forward(req,resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         this.doPost(req,resp);
    }
}  

@WebServlet("/requestDemo8")
public class RequestDemo08 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Demo8被访问了");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         this.doGet(req,resp);
    }
}  
demo7被访问了。。。
Demo8被访问了


```


- 3.共享数据:
  - 域对象：一个有作用范围的对象，可以在范围内共享数据
  - request域：代表一次请求的范围，所以request域一般用于请求转发的多个资源中取共享数据==  
  - 方法：  
     - 1.setAttribute(String name,Object obj):存储数据  
     - 2.Object getAttribute(String name):通过键获取值  
     - 3.void removeAttribute(String name):通过键移除键值对
  - 4.获取ServletContext  

##  案例：用户登录  
### 编写login.html登录页面  

~~~html
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
        <form action="/day14_test/loginServlet" method="post">
            <input type="text" name="username"><br>
            <input type="password" name="password"><br>
            <input type="submit" value="登录">
  
        </form>
  </body>
  </html>
  ```
~~~
###  使用Druid数据库连接池技术，操作mysql  
~~~java
  ```java
  package cn.Yogaguo.dao.Utils;
  
  import com.alibaba.druid.pool.DruidDataSourceFactory;
  
  import javax.sql.DataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.sql.Connection;
  import java.sql.SQLException;
  import java.util.Properties;
  @SuppressWarnings("all")
  /**
   * JDBC的工具类，使用的是Druid连接池
   */
  public class JDBCUtils {
      private static DataSource ds;
      static {
          Properties properties = new Properties();
          try {
              //加载配置文件
              properties.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
              //初始化连接池对象
             ds = DruidDataSourceFactory.createDataSource(properties);
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
      /**
       * 获取连接池对象
       */
      public static Connection getConnection() throws SQLException {
          return ds.getConnection();
      }
      /**
       * 获取连接Connection对象
        */
      public static DataSource getDataSource(){
          return ds;
      }
  }
  
  ```
~~~
###  使用JDBCTemplate技术封装JDBC  
~~~java
  ```java
  package cn.Yogaguo.dao;
  
  import cn.Yogaguo.dao.Utils.JDBCUtils;
  import cn.Yogaguo.domain.User;
  import org.springframework.dao.DataAccessException;
  import org.springframework.jdbc.core.BeanPropertyRowMapper;
  import org.springframework.jdbc.core.JdbcTemplate;
  
  /**
   * 操作数据库中User表
   */
  @SuppressWarnings("all")
  public class Userdao {
      /**
       * 声明JDBCTemplate对象共用
       */
      private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
  
      /**
       * 登录方法
       * @param loginUser，只有用户名和密码
       * @return user的全部信息
       */
      public User login(User loginUser){
          try {
              //编写sql
              String sql = "select * from user where username = ? and password = ?";
              //调用query方法
              User user = template.queryForObject(sql,
                      new BeanPropertyRowMapper<User>(User.class),
                      loginUser.getUsername(),loginUser.getPassword());
  
              return user;
          } catch (DataAccessException e) {
               return null;
          }
      }
  }
  
  ```
~~~
### 登录成功跳转到SuccessServlet展示：登录成功，用户名，欢迎你  
~~~java
  ```java
  package cn.Yogaguo.web.servlet;
  
  import cn.Yogaguo.dao.Userdao;
  import cn.Yogaguo.domain.User;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  @SuppressWarnings("all")
  @WebServlet("/loginServlet")
  public class LoginServlet extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //设置编码
          req.setCharacterEncoding("utf-8");
          String username = req.getParameter("username");
          String password = req.getParameter("PASSWORD");
          User loginUser = new User();
          loginUser.setUsername(username);
          loginUser.setPassword(password);
  
          Userdao dao = new Userdao();
          User user = dao.login(loginUser);
          if(user  == null){
              req.getRequestDispatcher("/failServlet").forward(req,resp);
          }else{
              req.setAttribute("user",user);
               req.getRequestDispatcher("/SuccessServlet").forward(req,resp);
          }
      }
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           this.doPost(req,resp);
      }
  }

  ```
~~~
### 跳转
```java
  package cn.Yogaguo.web.servlet;
  
  import cn.Yogaguo.domain.User;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  @SuppressWarnings("all")
  @WebServlet("/SuccessServlet")
  public class SuccessServlet extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //获取request域中共享的user对象
          User user = (User)req.getAttribute("user");
          if(user != null){
              //给页面写句话
              //设置页面编码
              resp.setContentType("text/html;charset = utf-8");
              //输出到页面上
              resp.getWriter().write("登录成功！"+user.getUsername()+",欢迎你");
          }
         
      }
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          this.doPost(req,resp);
      }
  }
  
```

### 登录失败，跳转到FailServlet展示：登录失败，用于名或密码错误

```java
 package cn.Yogaguo.web.servlet;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  @SuppressWarnings("all")
  @WebServlet("/failServlet")
  public class FailServlet extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //给页面写句话
          //设置页面编码
          resp.setContentType("text/html;charset = utf-8");
          //输出到页面上
          resp.getWriter().write("登录失败，用户名或密码错误");
      }

      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           this.doPost(req,resp);
      }
  }

```



 









 

 



 





~~~

~~~