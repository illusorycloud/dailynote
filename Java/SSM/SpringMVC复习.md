# SpringMVC复习

## servlet

传统servlet的执行过程分为如下几步： 
1、浏览器向服务器发送请求http://localhost:8080/demo/hello 
2、服务器接受到请求，并从地址中得到项目名称webproject 
3、然后再从地址中找到名称hello，并与webproject下的web.xml文件进行匹配 
4、在web.xml中找到一个` <url-pattern>hello</url-pattern>`的标签，并且通过他找到servlet-name进而找到`<servlet-class> `
5、再拿到servlet-class之后，这个服务器便知道了这个servlet的全类名，通过反射创建这个类的对象，并且调用doGet/doPost方法 

6、方法执行完毕，结果返回到浏览器。结束。

## SpringMVC

其中也配置了一个Servlet

配置的是org.springframework.web.servlet.DispatcherServlet，所有的请求过来都会找这个servlet （前端控制器）

DispatcherServlet继承了HttpServlet

### 运行过程分析：

1、  用户发送请求至前端控制器`DispatcherServlet`。

2、  `DispatcherServlet`收到请求调用`HandlerMapping`处理器映射器。

3、  处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)`HandlerExcutorChain`并返回给DispatcherServlet。

4、  `DispatcherServlet`调用`HandlerAdapter`处理器适配器。

5、  `HandlerAdapter`经过适配调用具体的处理器(Controller，也叫后端控制器)。

6、 ` Controller`执行完成返回`ModelAndView`。

7、  `HandlerAdapter`将controller执行结果`ModelAndView`返回给`DispatcherServlet`。

8、  `DispatcherServlet`将ModelAndView传给`ViewReslover`视图解析器。

9、  `ViewReslover`解析后返回具体`View`。

10、`DispatcherServle`t根据`View`进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet响应用户。

### 具体分析：

**1.建立Map<urls,controller>的关系**

在容器初始化时会建立`所有url和controller的对应关系`,保存到`Map<url,controller>`中.

`DispatcherServlet-->initApplicationContext`初始化容器 建立`Map<url,controller>`关系的部分 

tomcat启动时会通知spring初始化容器(加载bean的定义信息和初始化所有单例bean),然后springmvc会遍历容器中的bean,获取每一个controller中的所有方法访问的url,然后将url和controller保存到一个Map中;

**2.根据访问url找到对应controller中处理请求的方法.**

`DispatcherServlet-->doDispatch()`

有了前面的 Map 就可以根据 Request快速定位到controller,因为最终处理request的是controller中的方法,Map中只保留了url和controller中的对应关系,所以要根据request的url进一步确认controller中的method,这一步工作的原理就是拼接controller的url(controller上@RequestMapping的值)和方法的url(method上@RequestMapping的值),与request的url进行匹配,找到匹配的那个方法;　　

**3.反射调用处理请求的方法,返回结果视图**

　　确定处理请求的method后,接下来的任务就是参数绑定,把request中参数绑定到方法的形式参数上,这一步是整个请求处理过程中最复杂的一个步骤。springmvc提供了两种request参数与方法形参的绑定方法:

　　① 通过注解进行绑定,@RequestParam

　　② 通过参数名称进行绑定.
　　使用注解进行绑定,我们只要在方法参数前面声明@RequestParam("a"),就可以将request中参数a的值绑定到方法的该参数上.使用参数名称进行绑定的前提是必须要获取方法中参数的名称,Java反射只提供了获取方法的参数的类型,并没有提供获取参数名称的方法.springmvc解决这个问题的方法是用asm框架读取字节码文件,来获取方法的参数名称.asm框架是一个字节码操作框架,关于asm更多介绍可以参考它的官网.个人建议,使用注解来完成参数绑定,这样就可以省去asm框架的读取字节码的操作.

## @ModelAttribute

如果把@ModelAttribute放在方法的注解上时，代表的是：**该Controller的所有方法在调用前，先执行此@ModelAttribute方法**。

### 1. 用在没返回值的方法上

```java
@ModelAttribute()
public void aa(Model m) {
	m.addAttribute("key1", "value1");
}
```

执行当前`controller`的其他功能方法(有`@controller`的方法)之前会执行这个方法，可以做初始化工作。这里是把值方法model中。

### 2. 用在有返回值的方法上

SpringMVC 在调用目标方法前，将 @ModelAttribute 注解的 value 属性值作为 key , 返回值作为 value，存入到 model 中。

```java
@ModelAttribute()
public String aa(Model m) {
    return "value2";
}
```

  上面的代码相当于按照一的写法这样写

```java
@ModelAttribute()
public void aa(Model m) {
	m.addAttribute("string", "value2");//返回值类型首字母小写，当成key使用
}
```

### 3.用在方法参数上

```java
//就是从上一个Model中取出key2的值，给value2
public void aa(@ModelAttribute("key2") String value2) {
	System.out.pring(value2);
}
```

上面的代码相当于

```java
public void aa(Model m) {
  String value2=m.asMap().get("key2");
  System.out.pring(value2);
}
```

### 4. 用在功能方法上

写到方法上，并且方法上还有@RequestMapping注解，根据Spring官方文档，特意提及**此种写法完全没有任何意义**。

## @SessionAttribute

　默认情况下Spring MVC将模型中的数据存储到request域中。当一个请求结束后，数据就失效了。如果要跨页面使用。那么需要使用到session。而@SessionAttributes注解就可以使得模型中的数据存储一份到session域中。

```java
@SessionAttributes(value={"names"},types={Integer.class})
@Controller
public class Test {

    @RequestMapping("/test")
    public String test(Map<String,Object> map){
        map.put("names", Arrays.asList("caoyc","zhh","cjx"));
        map.put("age", 18);
        return "hello";
    }
}
```

上述代码中：Test这个controller中的方法每次执行前都会把`@SessionAttributes(value={"names"},types={Integer.class})`这里的names属性取出来存到session中，这样就可以在页面中获取。

@SessionAttribute是Controller类级别的注解，**作用是为了将指定名称或类型的隐含模型中的对象放置到Session作用域中，实现多次请求共享参数**，在每次请求的时候，使用@SessionAttributes添加的对象也会被添加到隐含模型对象中，我们可以通过@ModelAttribute来获取隐含模型中的对象。

**【注意】：@SessionAttributes注解只能在类上使用，不能在方法上使用**

## 参考：

https://www.cnblogs.com/heavenyes/p/3905844.html