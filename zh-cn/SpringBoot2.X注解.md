### 一、 Spring Bean相关

---

#### 1. @Autowired

自动导入对象到类中，被注入进的类同样要被Spring容器管理，如Service类注入到Controller类中

```java
@Service
public class UserService {

}

@RestController
@RequestMapping("/users")
public class UserController {
   @Autowired
   private UserService userService;

}
```



#### 2. @Component,@Repository,@Service,@Controller

一般使用@Autowired注解让Spring容器帮我们自动装配bean。要想把类标识称可用于@Autowired注解自动装配的bean的类，可以采用一下注解：

- @Component：通用的注解，可标注任意类为Spring组件。如果一个bean不知道属于哪个层，可以使用@Component注解标注。
- @Repository：对应持久层即Dao层，主要用于数据库相关操作。
- @Service：对应服务层，主要涉及一些复杂的逻辑，需要用到Dao层。
- @Controller：对应Spring MVC控制层，主要用于接受用户请求并调用Service层返回数据给前端页面。



#### 3. @RestController

@RestController注解是@Controller和@ResponseBody的合集，表示这是个控制器bean，并且是将函数的返回值直接填入HTTP响应体中，是REST风格的控制器。

单独使用@Controller不加@ResponseBody的话，一般是用在要返回一个视图的情况，这种情况属于比较传统的Spring MVC的应用，对应于前后端不分离的情况。@Controller+ResponseBody返回JSON或XML形式数据。

[关于@RestController和@Controller的比对](https://www.notion.so/e7031e0275cf4c17934cb38c26623961?pvs=21)



#### 4. @Scope

声明Spring Bean的作用域，使用方法：

```java
@Bean
@Scope("singleton")
public Person personSingleton(){
return new Person();
}
```

四种常见的Spring Bean的作用域：

- singleton：唯一的bean实例，Spring中的bean默认都是单例的。
- prototype：每次请求都会创建一个新的bean实例。
- request：每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- seesion：每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP session 内有效。



#### 5. @Configuration

一般用来声明配置类，可以使用@Component注解替代，不过使用@Configuration注解声明配置类更加语义化。

```java
@Configuration
public class AppConfig{
@Bean
public TransferService transferService(){
retrurn new TransferServiceImpl();
}
}
```

@[Configuration](https://so.csdn.net/so/search?q=Configuration&spm=1001.2101.3001.7020)、@ConfigurationProperties用法:

application.properties中配置的属性值：

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.46.24.png)

  ---

配置方式一：使用@Configuration+@Value方式：

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.48.52.png)



调用方式：

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.49.43.png)

运行结果：

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.49.59.png)



  ---

配置方式二：使用@ConfigurationProperties获取配置信息，通过@Configuration或@Component注解使spring @Component能够扫到该类；经操作@Component替换为@Configuration也可以获取同样结果。

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.50.17.png)

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.50.37.png)



### 二、 处理常见的HTTP请求类型

---

5中常见的请求类型：

- GET：请求从服务器获取特定资源。
- POST：在服务器上创建一个新的资源。
- PUT：更新服务器上的资源。
- DELETE：从服务器删除特定的资源。
- PATCH：更新服务器上的资源。

#### 1. GET请求

@GetMapping(”users”) 等价于@RequestMapping(value=”/users”,method=RequestMethod.GET)

```java
@GetMapping("/users")
public ResponseEntity<List<User>> getAllUsers() {
 return userRepository.findAll();
}
```

5中常见的请求类型：

- GET：请求从服务器获取特定资源。
- POST：在服务器上创建一个新的资源。
- PUT：更新服务器上的资源。
- DELETE：从服务器删除特定的资源。
- PATCH：更新服务器上的资源。

5中常见的请求类型：

- GET：请求从服务器获取特定资源。
- POST：在服务器上创建一个新的资源。
- PUT：更新服务器上的资源。
- DELETE：从服务器删除特定的资源。
- PATCH：更新服务器上的资源。



#### 2. POST请求

@PostMapping("users") 等价于@RequestMapping(value="/users",method=RequestMethod.POST)

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody UserCreateRequest userCreateRequest) {
 return userRespository.save(userCreateRequest);
}
```



#### 3. PUT请求

@PutMapping("/users/{userId}") 等价于@RequestMapping(value="/users/{userId}",method=RequestMethod.PUT)

```java
@PutMapping("/users/{userId}")
public ResponseEntity<User> updateUser(@PathVariable(value = "userId") Long userId,
  @Valid @RequestBody UserUpdateRequest userUpdateRequest) {
  ......
}
```



#### 4. DELETE请求

@DeleteMapping("/users/{userId}")等价于@RequestMapping(value="/users/{userId}",method=RequestMethod.DELETE)

```java
@DeleteMapping("/users/{userId}")
public ResponseEntity deleteUser(@PathVariable(value = "userId") Long userId){
  ......
}
```



### 三、前后端传值

---

#### 1. @Pathvariable和@RequestParam

@PathVariable用户获取路径参数，@RequestParam用于获取查询参数

```java
@GetMapping("/klasses/{klassId}/teachers")
public List<Teacher> getKlassrelatedTeachers(
@PathVariable("klassId") Long klassId,
@RequestParam(value="type",required=false) String type){
···
}
```

如果请求url是：/klasses/123456/teachers?type=web

那么获取到的数据就是：klassId=123456，type=web



#### 2. @RequestBody

用于读取Request请求的body部分，并且Content-Type为application/json格式的数据，接受到数据之后会自动将数据绑定到Java对象上去。系统会使用HttpMessageConverter或自定义的HttpMessageConverter将请求的body中的json字符串转换为java对象。

发送POST请求到这个接口，并且body携带JSON数据：

```java
{"userName":"coder","fullName":"shuangkou","password":"123456"} 
```

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-04-10%252022.52.49.png)

注意：一个请求方法只可以有一个@RequestBody，但是可以有多个@RequestParam和@PathVariable。



### 四、读取配置信息

---

例如有数据源：application.yml内容如下：

```yaml
wuhan2020: 2020年初武汉爆发了新型冠状病毒，疫情严重，但是，我相信一切都会过去！武汉加油！中国加油！

my-profile:
  name: Guide哥
  email: koushuangbwcx@163.com

library:
  location: 湖北武汉加油中国加油
  books:
    - name: 天才基本法
      description: 二十二岁的林朝夕在父亲确诊阿尔茨海默病这天，得知自己暗恋多年的校园男神裴之即将出国深造的消息——对方考取的学校，恰是父亲当年为她放弃的那所。
    - name: 时间的秩序
      description: 为什么我们记得过去，而非未来？时间“流逝”意味着什么？是我们存在于时间之内，还是时间存在于我们之中？卡洛·罗韦利用诗意的文字，邀请我们思考这一亘古难题——时间的本质。
    - name: 了不起的我
      description: 如何养成一个新习惯？如何让心智变得更成熟？如何拥有高质量的关系？ 如何走出人生的艰难时刻？
```



#### 1. @Value(常用)

使用@Value(”${property}”)读取比较简单的配置信息：

```java
@Value("${wuhan2020}")
String wuhan2020;
```

> 说明：有时候会这样用：@Value("${wuhan2020:12345}"),加上冒号时，如果配置文件中值不存在的话，会获取冒号后那个值



#### 2. @ConfigurationProperties(常用)

通过@ConfigurationProperties读取配置信息并与bean绑定。

```java
@Component
@ConfigurationProperties(prefix = "library")
class LibraryProperties {
    @NotEmpty
    private String location;
    private List<Book> books;

    @Setter
    @Getter
    @ToString
    static class Book {
        String name;
        String description;
    }
  省略getter/setter
  ......
}
```

可以像使用普通的Spring bean一样，将其注入到类中使用。



#### 3. @PropertySource(不常用)

@PropertySource读取指定properties文件

```java
@Component
@PropertySource("classpath:website.properties")
class WebSite{
@Value("${url}")
private String url;
···
}
```



### 五、全局处理Controller层异常

---

1. @ControllerAdvice：注解敌营全局异常处理类
2. @ExceptionHandler：注解声明异常处理方法

```java
@ControllerAdvice
@ResponseBody
public class globalExceptionHandler{
/**
*请求参数异常处理
*/
public ResponseEntity<?> handleMethodArgumentNotValidException(MethodArgumentNotValidExcption ex,
HttpServletRequest request){
···
}
}
```