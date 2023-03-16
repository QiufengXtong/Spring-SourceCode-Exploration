# Servlet 详解
每一个Web应用程序,只有一个`ServletContext`。
Servlet通过ServletContext实现数据共享。

## 概念
Servlet （Server Applet）是 Java Servlet的简称，称为小服务器或服务器连接器，用Java编写的服务器端程序，具有独立于平台和协议的特性,主要功能在于交互式的浏览和生成数据，生成动态Web内容，狭义的Servlet是指Java语言实现的一个接口，广义的Servlet运行于支持Java的应用服务器中，从原理上讲，Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器。

## 快速入门
1. 创建一个JavaEE项目，并引入`javax.servlet.servlet-api.jar`包。
2. 定义一个类，并实现Servlet。
```java
public class DemoServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("初始化 DemoServlet");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("执行 DemoServlet");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {
        System.out.println("销毁 DemoServlet");
    }
}
```
3. `web.xml`中配置Servlet
```xml
<servlet>
    <servlet-name>demo</servlet-name>
    <servlet-class>cn.xtong.example.DemoServlet</servlet-class>
    <!--项目启动加载-->
    <!--<load-on-startup>0</load-on-startup>-->
</servlet>

<servlet-mapping>
    <servlet-name>demo</servlet-name>
    <url-pattern>/demo</url-pattern>
</servlet-mapping>
```
4. 配置`tomcat`，并启动项目,访问`http://localhost:8080/demo`。

## 执行原理

1. 当服务器接收到客户端浏览器请求后，会解析请求URL路径，获取访问的Servlet的资源路径。
2. 查询`web.xml`文件，是否有对应的`url-pattern`。
3. 如果有通过`servlet-name`，找到于其对应的`servlet-class`。
4. `tomcat`会根据`servlet-class`内的类路径地址，找到对应的class文件并将class文件加载进内存，并创建其对象，执行`init(ServletConfig servletConfig)`方法初始化。
5. 执行`service(ServletRequest servletRequest, ServletResponse servletResponse)`方法。

## Servlet 中方法的介绍
- `init(ServletConfig servletConfig)`：只执行一次，默认情况下，第一次访问时执行，`load-on-startup`为0时，项目启动时加载。
- `service(ServletRequest servletRequest, ServletResponse servletResponse)`：执行多次，每一次访问`DemoServlet`，`service（)`
- `destroy()`：Servlet之前执行，通常只有服务器关闭时，才会执行。