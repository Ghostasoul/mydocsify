## SpringBoot2.X笔记

1. 修改springboot默认的banner：resources下新建banner.txt,里边填写上banner就行。

2. 所有的类应该和应用主类：Application.java是同级目录或者所有类的上级目录和应用主类是同级目录。也就是说其他的类最好是被包含在应用主类下。这样Spring Boot的应用主类会自动扫描`root package`以及所有子包下的所有类来进行初始化。

3. 如果，我们一定要加载非`root package`下的内容怎么办呢？使用`@ComponentScan`注解指定具体的加载包，比如：

   ```java
   @SpringBootApplication
   @ComponentScan(basePackages="com.example")
   public class Bootstrap {
   
       public static void main(String[] args) {
           SpringApplication.run(Bootstrap.class, args);
       }
   
   }
   ```

4. 可以用@Value注解来加载application.properties中的配置信息,例如:

   ```java
   @Value("${EHR.client_id}")
   String client_id;
   ```

5. 可以在application.properties中使用random来生成随机数字，然后用@Value注解调用即可，例如：

   ```java
   //随机生成字符串
   EHR.code=${random.value}
   
   //随机生成整型
   EHR.code=${random.int}
   
   //随机生成uuid
   EHR.code=${random.uuid}
   ```

6. @RestController和@Controller的区别

   1. @RestController注解，相当于@Controller+@ResponseBody两个注解的结合，返回json数据不需要在方法前面加@ResponseBody注解了，但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面
   2. @RestController 注解是从 Spring 4.0 版本开始添加进来的，主要用于更加方便的构建 RESTful Web 服务
   3. 如果使用@**Controller来实现返回数据返回json，那么方法一般要加上：**@ResponseBody，如果不加@ResponseBody的话，只用@Controller注解，返回的就是一个jsp页面或者html页面。

7. @RequestMapping与@GetMapping、@PostMapping的区别

   1. @GetMapping用于将HTTP get请求映射到特定处理程序的方法注解
      具体来说，@GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)的缩写。
   2. @GetMapping这个注解 是spring4.3版本引入，同时引入的还有@PostMapping、@PutMapping、@DeleteMapping和@PatchMapping，一共5个注解
   3. 如果同时用@RequestMapping和@GetMapping注解在一个方法上的话，请求只会进@RequestMapping注解，相当于@GetMapping无效

8. 命令行启动SpringBoot项目：点击idea中右上角maven中的packpage，把整个项目打包，然后使用命令java -jar /User/XXX/XXX/xxx.jar(打包项目的绝对路径)。例如：

  > java -jar /Users/gaosite/Documents/JavaProject/SpringBoot/target/SpringBootDemo-0.0.1-SNAPSHOT.jar


9. 启动的时候通过命令来设置参数：`java -jar xxx.jar --server.port=8888`，直接以命令行的方式，来设置server.port属性，另启动应用的端口设为8888。连续的两个减号`--`就是对`application.properties`中的属性值进行赋值的标识。所以，`java -jar xxx.jar --server.port=8888`命令，等价于我们在`application.properties`中添加属性`server.port=8888`。

10. 

