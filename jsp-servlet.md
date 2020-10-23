# Jsp与Servlet面试题

<https://blog.csdn.net/zhaojw_420/article/details/70880968>

<https://blog.csdn.net/u013842976/article/details/52275344>

## 一、jsp有哪些内置对象作用分别是什么

答:JSP共有以下9种基本内置组件（可与ASP的6种内部组件相对应）：
request 用户端请求，此请求会包含来自GET/POST请求的参数
response 网页传回服务器端的回应
pageContext 网页的属性是在这里管理
session 与请求有关的会话期
application ： 服务器启动时创建，服务器关闭时停止，为多个应用程序保存信息
out 用来传送回应的输出
config servlet 的构架部件
page JSP网页本身
exception 针对错误网页，未捕捉的例外

## 二、jsp有哪些动作作用分别是什么

答:JSP共有以下6种基本动作
jsp:include：在页面被请求的时候引入一个文件。
jsp:useBean：寻找或者实例化一个JavaBean。
jsp:setProperty：设置JavaBean的属性。
jsp:getProperty：输出某个JavaBean的属性。
jsp:forward：把请求转到一个新的页面。
jsp:plugin：根据浏览器类型为Java插件生成OBJECT或EMBED标记

## 三、JSP中动态INCLUDE与静态INCLUDE的区别？

答：动态INCLUDE用jsp:include动作实现
它总是会检查所含文件中的变化，适合用于包含动态页面，并且可以带参数
静态INCLUDE用include伪码实现,定不会检查所含文件的变化，适用于包含静态页面

## 四、 Servlet生命周期

Servlet生命周期包括三部分：
初始化：Web容器加载servlet，调用init()方法
处理请求：当请求到达时，运行其service()方法。service()自动派遣运行与请求相对应的doXXX（doGet或者doPost）方法。
销毁：服务结束，web容器会调用servlet的distroy()方法销毁servlet。

## 五、get提交和post提交有何区别

（1）get一般用于从服务器上获取数据，post一般用于向服务器传送数据
（2）请求的时候参数的位置有区别，get的参数是拼接在url后面，用户在浏览器地址栏可以看到。post是放在http包的包体中。
比如说用户注册，你不能把用户提交的注册信息用get的方式吧，那不是说把用户的注册信息都显示在Url上了吗，是不安全的。
（3）能提交的数据有区别，get方式能提交的数据只能是文本，且大小不超过1024个字节，而post不仅可以提交文本还有二进制文件。
所以说想上传文件的话，那我们就需要使用post请求方式。
（4）servlet在处理请求的时候分别对应使用doGet和doPost方式进行处理请求。

## 六、JSP与Servlet有什么区别

Servlet是服务器端的程序，动态生成html页面发送到客户端，但是这样程序里会有很多out.println(),Java与html语言混在一起很乱，所以后来sun公司推出了JSP。其实JSP就是Servlet，每次运行的时候JSP都首先被编译成servlet文件，然后再被编译成.class文件运行。有了jsp，在MVC项目中servlet不再负责动态生成页面，转而去负责控制程序逻辑的作用，控制jsp与javabean之间的流转。

## 七、doGet与doPost方法的两个参数是什么

HttpServletRequest：封装了与请求相关的信息
HttpServletResponse：封装了与响应相关的信息

## 八、request.getAttribute()和request.getParameter

（1）有setAttribute，没有setParameter方法
（2）getParameter获取到的值只能是字符串，不可以是对象，而getAttribute获取到的值是Object类型的。
（3）通过form表单或者url来向另一个页面或者servlet传递参数的时候需要用getParameter获取值；getAttribute只能获取setAttribute的值
（4）setAttribute是应用服务器把这个对象放到该页面所对应的一块内存当中，当你的页面服务器重定向到另一个页面的时候，应用服务器会把这块内存拷贝到另一个页面对应的内存当中。通过getAttribute可以取得你存下的值，当然这种方法可以用来传对象。用session也是一样的道理，这是说request和session的生命周期不一样而已。

## 九、四种会话跟踪技术

会话作用域ServletsJSP 页面描述
page：是代表与一个页面相关的对象和属性。一个页面由一个编译好的 Java servlet 类（可以带有任何的 include 指令，但是没有 include 动作）表示。这既包括 servlet 又包括被编译成 servlet 的 JSP 页面
request：是代表与 Web 客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个 Web 组件（由于 forward 指令和 include 动作的关系）
session是是代表与用于某个 Web 客户机的一个用户体验相关的对象和属性。一个 Web 会话可以也经常会跨越多个客户机请求
application是是代表与整个 Web 应用程序相关的对象和属性。这实质上是跨越整个 Web 应用程序，包括多个页面、请求和会话的一个全局作用域
page：当前页面，也就是只要跳到别的页面就失效了
request：一次会话，简单的理解就是一次请求范围内有效
session：浏览器进程，只要当前页面没有被关闭（没有被程序强制清除），不管怎么跳转都是有效的
application：服务器，只要服务器没有重启（没有被程序强制清除），数据就有效

## 十、forward和redirect的区别

转发与重定向
（1）从地址栏显示来说
forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址。

redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL。

（2）从数据共享来说
forward:转发页面和转发到的页面可以共享request里面的数据.
redirect:不能共享数据.

（3）从运用地方来说
forward:一般用于用户登陆的时候,根据角色转发到相应的模块.
redirect:一般用于用户注销登陆时返回主页面和跳转到其它的网站等.

（4）从效率来说
forward:高.
redirect:低.

## 十一、Servlet的线程安全问题

实例变量不正确的使用是造成Servlet线程不安全的主要原因。

从Servlet的调用过程可以看出，当客户端第一次请求Servlet的时候,tomcat会根据web.xml配置文件实例化servlet
当又有一个客户端访问该servlet的时候，不会再实例化该servlet，也就是多个线程在使用这个实例。

JSP/Servlet容器默认是采用单实例多线程(这是造成线程安全的主因)方式处理多个请求的，这种默认以多线程方式执行的设计可大大降低对系统的资源需求，提高系统的并发量及响应时间。

Servlet本身是无状态的，一个无状态的Servlet是绝对线程安全的，无状态对象设计也是解决线程安全问题的一种有效手段。

所以，servlet是否线程安全是由它的实现来决定的，如果它内部的属性或方法会被多个线程改变，它就是线程不安全的，反之，就是线程安全的。

如何控制Servlet的线程安全性？

避免使用实例变量
避免使用非线程安全的集合
在多个Servlet中对某个外部对象(例如文件)的修改是务必加锁（Synchronized，或者ReentrantLock），互斥访问。
属性的线程安全：ServletContext、HttpSession是线程安全的；ServletRequest是非线程安全的。

设计线程安全的Servlet

1.实现 SingleThreadModel 接口
该接口指定了系统如何处理对同一个Servlet的调用。如果一个Servlet被这个接口指定，那么在这个Servlet中的service方法将不会有两个线程被同时执行，当然也就不存在线程安全的问题。但是，如果一个Servlet实现了SingleThreadModel接口，Servlet引擎将为每个新的请求创建一个单独的Servlet实例，这将引起大量的系统开销，在现在的Servlet开发中基本看不到SingleThreadModel的使用，这种方式了解即可，尽量避免使用。

2.同步对共享数据的操作
使用synchronized 关键字能保证一次只有一个线程可以访问被保护的区段，可以通过同步块操作来保证Servlet的线程安全。如果在程序中使用同步来保护要使用的共享的数据，也会使系统的性能大大下降。这是因为被同步的代码块在同一时刻只能有一个线程执行它，使得其同时处理客户请求的吞吐量降低，而且很多客户处于阻塞状态。另外为保证主存内容和线程的工作内存中的数据的一致性，要频繁地刷新缓存,这也会大大地影响系统的性能。所以在实际的开发中也应避免或最小化Servlet 中的同步代码。

3.避免使用实例变量
线程安全问题很大部分是由实例变量造成的，只要在Servlet里面的任何方法里面都不使用实例变量，那么该Servlet就是线程安全的。

在Servlet中避免使用实例变量是保证Servlet线程安全的最佳选择。

Java 内存模型中，方法中的临时变量是在栈上分配空间，而且每个线程都有自己私有的栈空间，所以它们不会影响线程的安全。

---



二、JSP&Servlet技术
1. 描述JSP和Servlet的区别、共同点、各自应用的范围
2. 在Web开发中需要处理HTML标记时，应做什么样的处理，要筛选那些字符（< > &“”）
3. 在JSP中如何读取客户端的请求，如何访问CGI变量，如何确定某个Jsp文件的真实路径。
4. 描述Cookie和Session的作用，区别和各自的应用范围，Session工作原理。
5. 列出Jsp中包含外部文件的方式，两者有何区别。
6. 说明Jsp中errorPage的作用，应用范围。
7. 介绍在Jsp中如何使用JavaBeans。
8. 简单介绍JSP的标记库
9. Jsp和Servlet中的请求转发分别如何实现。

10.JSP的常用指令



---

<https://blog.csdn.net/Jeff_Seid/article/details/80761076>

# Servlet方面

1、说一说Servlet的生命周期?

Servlet有良好的生存期的定义，包括加载和实例化、初始化、处理请求以及服务结束。这个生存期由javax.servlet.Servlet接口的init,service和destroy方法表达。 Servlet被服务器实例化后，容器运行其init方法，请求到达时运行其service方法，service方法自动派遣运行与请求对应的doXXX方法（doGet，doPost）等，当服务器决定将实例销毁的时候调用其destroy方法。

与cgi的区别在于Servlet处于服务器进程中，它通过多线程方式运行其service方法，一个实例可以服务于多个请求，并且其实例一般不会销毁，而CGI对每个请求都产生新的进程，服务完成后就销毁，所以效率上低于Servlet。

## 2、JAVA SERVLET API中forward() 与redirect()的区别？

forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后把这些内容再发给浏览器，其实客户端浏览器只发了一次请求，所以它的地址栏中还是原来的地址，session,request参数都可以获取。

redirect就是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址，相当于客户端浏览器发送了两次请求。

## 3、Servlet的基本架构

```java
public class ServletName extends HttpServlet {

  public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException  {
      }

  public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException  {
      }

}
```

## 4、什么情况下调用doGet()和doPost()？

JSP页面中的form标签里的method属性为get时调用doGet()，为post时调用doPost()；超链接跳转页面时调用doGet()

## 5、servlet的生命周期

web容器加载servlet，生命周期开始。通过调用servlet的init()方法进行servlet的初始化。通过调用service()方法实现，根据请求的不同调用不同的do*()方法。结束服务，web容器调用servlet的destroy()方法。

## 6、页面间对象传递的方法

request，session，application，cookie等

## 7、JSP和Servlet有哪些相同点和不同点，他们之间的联系是什么？

JSP是Servlet技术的扩展，本质上是Servlet的简易方式，更强调应用的外表表达。JSP编译后是"类servlet"。Servlet和JSP最主要的不同点在于，Servlet的应用逻辑是在Java文件中，并且完全从表示层中的HTML里分离开来。而JSP的情况是Java和HTML可以组合成一个扩展名为.jsp的文件。JSP侧重于视图，Servlet主要用于控制逻辑。

## 8、四种会话跟踪技术

会话作用域ServletsJSP 页面描述

1）page否是代表与一个页面相关的对象和属性。一个页面由一个编译好的 Java servlet 类（可以带有任何的 include 指令，但是没有 include 动作）表示。这既包括 servlet 又包括被编译成 servlet 的 JSP 页面

2）request是是代表与 Web 客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个 Web 组件（由于 forward 指令和 include 动作的关系）

3）session是是代表与用于某个 Web 客户机的一个用户体验相关的对象和属性。一个 Web 会话可以也经常会跨越多个客户机请求

4）application是是代表与整个 Web 应用程序相关的对象和属性。这实质上是跨越整个 Web 应用程序，包括多个页面、请求和会话的一个全局作用域

## 9、Request对象的主要方法

setAttribute(String name,Object)：设置名字为name的request的参数值

getAttribute(String name)：返回由name指定的属性值

getAttributeNames()：返回request对象所有属性的名字集合，结果是一个枚举的实例

getCookies()：返回客户端的所有Cookie对象，结果是一个Cookie数组

getCharacterEncoding()：返回请求中的字符编码方式

getContentLength()：返回请求的Body的长度

getHeader(String name)：获得HTTP协议定义的文件头信息

getHeaders(String name)：返回指定名字的request Header的所有值，结果是一个枚举的实例

getHeaderNames()：返回所以request Header的名字，结果是一个枚举的实例

getInputStream()：返回请求的输入流，用于获得请求中的数据

getMethod()：获得客户端向服务器端传送数据的方法

getParameter(String name)：获得客户端传送给服务器端的有name指定的参数值

getParameterNames()：获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例

getParameterValues(String name)：获得有name指定的参数的所有值

getProtocol()：获取客户端向服务器端传送数据所依据的协议名称

getQueryString()：获得查询字符串

getRequestURI()：获取发出请求字符串的客户端地址

getRemoteAddr()：获取客户端的IP地址

getRemoteHost()：获取客户端的名字

getSession([Boolean create])：返回和请求相关Session

getServerName()：获取服务器的名字

getServletPath()：获取客户端所请求的脚本文件的路径

getServerPort()：获取服务器的端口号

removeAttribute(String name)：删除请求中的一个属性

## 10、我们在web应用开发过程中经常遇到输出某种编码的字符，如iso8859-1等，如何输出一个某种编码的字符串？

```java
public String translate (String str) {

    String tempStr = "";
    try {
      tempStr = new String(str.getBytes("ISO-8859-1"), "GBK");
      tempStr = tempStr.trim();
    }catch (Exception e) {
      System.err.println(e.getMessage());
    }
    return tempStr;
}
```

## 11、Servlet执行时一般实现哪几个方法？

public void init(ServletConfig config)

public ServletConfig getServletConfig()

public String getServletInfo()

public void service(ServletRequest request,ServletResponse response)

public void destroy()

## 12、描述Cookie和Session的作用，区别和各自的应用范围，Session工作原理。

1）cookie 是一种发送到客户浏览器的文本串句柄，并保存在客户机硬盘上，可以用来在某个WEB站点会话间持久的保持数据。

2）session其实指的就是访问者从到达某个特定主页到离开为止的那段时间。 Session其实是利用Cookie进行信息处理的，当用户首先进行了请求后，服务端就在用户浏览器上创建了一个Cookie，当这个Session结束时，其实就是意味着这个Cookie就过期了。

注：为这个用户创建的Cookie的名称是aspsessionid。这个Cookie的唯一目的就是为每一个用户提供不同的身份认证。

3）cookie和session的共同之处在于：cookie和session都是用来跟踪浏览器用户身份的会话方式。

4）cookie 和session的区别是：cookie数据保存在客户端，session数据保存在服务器端。

5）session工作原理：session技术中所有的数据都保存在服务器上，客户端每次请求服务器的时候会发送当前会话的sessionid，服务器根据当前sessionid判断相应的用户数据标志，以确定用户是否登录或具有某种权限。

## 13、什么是Servlet？

Servlet是用来处理客户端请求并产生动态网页内容的Java类。Servlet主要是用来处理或者是存储HTML表单提交的数据，产生动态内容，在无状态的HTTP协议下管理状态信息。

## 14、说一下Servlet的体系结构。

所有的Servlet都必须要实现的核心的接口是javax.servlet.Servlet。每一个Servlet都必须要直接或者是间接实现这个接口，或者是继承javax.servlet.GenericServlet或者javax.servlet.http.HTTPServlet。最后，Servlet使用多线程可以并行的为多个请求服务。

## 15、Applet和Servlet有什么区别？

Applet是运行在客户端主机的浏览器上的客户端Java程序。而Servlet是运行在web服务器上的服务端的组件。applet可以使用用户界面类，而Servlet没有用户界面，相反，Servlet是等待客户端的HTTP请求，然后为请求产生响应。

## 16、GenericServlet和HttpServlet有什么区别？

GenericServlet是一个通用的协议无关的Servlet，它实现了Servlet和ServletConfig接口。继承自GenericServlet的Servlet应该要覆盖service()方法。最后，为了开发一个能用在网页上服务于使用HTTP协议请求的Servlet，你的Servlet必须要继承自HttpServlet。这里有Servlet的例子。

## 17、解释下Servlet的生命周期。

对每一个客户端的请求，Servlet引擎载入Servlet，调用它的init()方法，完成Servlet的初始化。然后，Servlet对象通过为每一个请求单独调用service()方法来处理所有随后来自客户端的请求，最后，调用Servlet(译者注：这里应该是Servlet而不是server的destroy()方法把Servlet删除掉。

## 18、doGet()方法和doPost()方法有什么区别？

doGet：GET方法会把名值对追加在请求的URL后面。因为URL对字符数目有限制，进而限制了用在客户端请求的参数值的数目。并且请求中的参数值是可见的，因此，敏感信息不能用这种方式传递。

doPOST：POST方法通过把请求参数值放在请求体中来克服GET方法的限制，因此，可以发送的参数的数目是没有限制的。最后，通过POST请求传递的敏感信息对外部客户端是不可见的。

## 19、什么是Web应用程序？

Web应用程序是对Web或者是应用服务器的动态扩展。有两种类型的Web应用：面向表现的和面向服务的。面向表现的Web应用程序会产生包含了很多种标记语言和动态内容的交互的web页面作为对请求的响应。而面向服务的Web应用实现了Web服务的端点(endpoint)。一般来说，一个Web应用可以看成是一组安装在服务器URL名称空间的特定子集下面的Servlet的集合。

## 20、什么是服务端包含(Server Side Include)？

服务端包含(SSI)是一种简单的解释型服务端脚本语言，大多数时候仅用在Web上，用servlet标签嵌入进来。SSI最常用的场景把一个或多个文件包含到Web服务器的一个Web页面中。当浏览器访问Web页面的时候，Web服务器会用对应的servlet产生的文本来替换Web页面中的servlet标签。

## 21、什么是Servlet链(Servlet Chaining)？

Servlet链是把一个Servlet的输出发送给另一个Servlet的方法。第二个Servlet的输出可以发送给第三个Servlet，依次类推。链条上最后一个Servlet负责把响应发送给客户端。

## 22、如何知道是哪一个客户端的机器正在请求你的Servlet？

ServletRequest类可以找出客户端机器的IP地址或者是主机名。getRemoteAddr()方法获取客户端主机的IP地址，getRemoteHost()可以获取主机名。看下这里的例子。

## 23、HTTP响应的结构是怎么样的？

HTTP响应由三个部分组成：

状态码(Status Code)：描述了响应的状态。可以用来检查是否成功的完成了请求。请求失败的情况下，状态码可用来找出失败的原因。如果Servlet没有返回状态码，默认会返回成功的状态码HttpServletResponse.SC_OK。

HTTP头部(HTTP Header)：它们包含了更多关于响应的信息。比如：头部可以指定认为响应过期的过期日期，或者是指定用来给用户安全的传输实体内容的编码格式。如何在Serlet中检索HTTP的头部看这里。

主体(Body)：它包含了响应的内容。它可以包含HTML代码，图片，等等。主体是由传输在HTTP消息中紧跟在头部后面的数据字节组成的。

## 24、什么是cookie？session和cookie有什么区别？

cookie是Web服务器发送给浏览器的一块信息。浏览器会在本地文件中给每一个Web服务器存储cookie。以后浏览器在给特定的Web服务器发请求的时候，同时会发送所有为该服务器存储的cookie。下面列出了session和cookie的区别：

无论客户端浏览器做怎么样的设置，session都应该能正常工作。客户端可以选择禁用cookie，但是，session仍然是能够工作的，因为客户端无法禁用服务端的session。

在存储的数据量方面session和cookies也是不一样的。session能够存储任意的Java对象，cookie只能存储String类型的对象。

## 25、浏览器和Servlet通信使用的是什么协议？

浏览器和Servlet通信使用的是HTTP协议。

## 26、什么是HTTP隧道？

HTTP隧道是一种利用HTTP或者是HTTPS把多种网络协议封装起来进行通信的技术。因此，HTTP协议扮演了一个打通用于通信的网络协议的管道的包装器的角色。把其他协议的请求掩盖成HTTP的请求就是HTTP隧道。

## 27、sendRedirect()和forward()方法有什么区别？

sendRedirect()方法会创建一个新的请求，而forward()方法只是把请求转发到一个新的目标上。重定向(redirect)以后，之前请求作用域范围以内的对象就失效了，因为会产生一个新的请求，而转发(forwarding)以后，之前请求作用域范围以内的对象还是能访问的。一般认为sendRedirect()比forward()要慢。

## 28、什么是URL编码和URL解码？

URL编码是负责把URL里面的空格和其他的特殊字符替换成对应的十六进制表示，反之就是解码。

## 29、什么是Scriptlets？

JSP技术中，scriptlet是嵌入在JSP页面中的一段Java代码。scriptlet是位于标签内部的所有的东西，在标签与标签之间，用户可以添加任意有效的scriplet。

## 30、声明(Decalaration)在哪里？

声明跟Java中的变量声明很相似，它用来声明随后要被表达式或者scriptlet使用的变量。添加的声明必须要用开始和结束标签包起来。

## 31、什么是表达式(Expression)？

【列表很长，可以分上、中、下发布】

JSP表达式是Web服务器把脚本语言表达式的值转化成一个String对象，插入到返回给客户端的数据流中。表达式是在<%=和%>这两个标签之间定义的。

## 32、隐含对象是什么意思？有哪些隐含对象？

JSP隐含对象是页面中的一些Java对象，JSP容器让这些Java对象可以为开发者所使用。开发者不用明确的声明就可以直接使用他们。JSP隐含对象也叫做预定义变量。下面列出了JSP页面中的隐含对象：

• application
• page
• request
• response
• session
• exception
• out
• config
• pageContext

# [CGI与Servlet的区别和联系](https://www.cnblogs.com/MuyouSome/p/3938203.html)

1. 定义：

CGI(Common Gateway Interface 公共网关接口)是HTTP服务器与你的或其它机器上的程序进行“交谈”的一种工具，其程序须运行在网络服务器上。

 2.功能：

绝大多数的CGI程序被用来解释处理杰自表单的输入信息，并在服 务器产生相应的处理，或将相应的信息反馈给浏览器。CGI程序使 网页具有交互功能。

 

\3. 运行环境：

CGI程序在UNIX操作系统上CERN或NCSA格式的服务器上运行。 在其它操作系统（如：windows NT及windows95等）的服务器上 也广泛地使用CGI程序，同时它也适用于各种类型机器。

 

\4. CGI处理步骤：

　　⑴通过Internet把用户请求送到服务器。

　　⑵服务器接收用户请求并交给CGI程序处理。

　　⑶CGI程序把处理结果传送给服务器。

　　⑷服务器把结果送回到用户。

 

 

Servlet是一种服务器端的Java应用程序，具有独立于平台和协议的特性,可以生成动态的Web页面。 它担当客户请求（Web浏览器或其他HTTP客户程序）与服务器响应（HTTP服务器上的数据库或应用程序）的中间层。 Servlet是位于Web 服务器内部的服务器端的Java应用程序，与传统的从命令行启动的Java应用程序不同，Servlet由Web服务器进行加载，该Web服务器必须包含支持Servlet的Java虚拟机。

工作模式：客户端发送请求至服务器；服务器启动并调用Servlet，Servlet根据客户端请求生成响应内容并将其传给服务器；服务器将响应返回客户端。

 

## Java Servlet与CGI (Common Gateway Interface 公共网关接口)的比较

　　与传统的CGI和许多其他类似CGI的技术相比，Java Servlet具有更高的效率，更容易使用，功能更强大，具有更好的可移植性，更节省投资。在未来的技术发展过程中，Servlet有可能彻底取代CGI。

　　在传统的[CGI](http://baike.baidu.com/view/32614.htm)中，每个请求都要启动一个新的进程，如果CGI程序本身的执行时间较短，启动进程所需要的开销很可能反而超过实际执行时间。而在Servlet中，每个请求由一个轻量级的Java线程处理(而不是重量级的操作系统进程)。

　　在传统CGI中，如果有N个并发的对同一CGI程序的请求，则该CGI程序的代码在内存中重复装载了N次；而对于Servlet，处理请求的是N个线程，只需要一份Servlet类代码。在性能优化方面，Servlet也比CGI有着更多的选择。

　　* 方便 　

　　Servlet提供了大量的实用工具例程，例如自动地解析和解码HTML表单数据、读取和设置[HTTP](http://baike.baidu.com/view/9472.htm)头、处理[Cookie](http://baike.baidu.com/view/835.htm)、跟踪会话状态等。

　　* 功能强大

　　在Servlet中，许多使用传统CGI程序很难完成的任务都可以轻松地完成。例如，Servlet能够直接和[Web](http://baike.baidu.com/view/3912.htm)服务器交互，而普通的CGI程序不能。Servlet还能够在各个程序之间共享数据，使得[数据库](http://baike.baidu.com/view/1088.htm)连接池之类的功能很容易实现。

　　* 可移植性好

Servlet用Java编写，Servlet [API](http://baike.baidu.com/view/16068.htm)具有完善的标准。因此，为IPlanet Enterprise Server写的Servlet无需任何实质上的改动即可移植到[Apache](http://baike.baidu.com/view/28283.htm)、[Microsoft ](http://baike.baidu.com/view/2422.htm)IIS或者WebStar。几乎所有的主流服务器都直接或通过插件支持Servlet。
