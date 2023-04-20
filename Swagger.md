# Swagger

# 1:介绍

相信无论是前端还是后端开发，都或多或少地被接口文档折磨过。前端经常抱怨后端给的接口文档与实际情

况不一致。后端又觉得编写及维护接口文档会耗费不少精力，经常来不及更新。其实无论是前端调用后端，

还是后端调用后端，都期望有一个好的接口文档。但是这个接口文档对于程序员来说，就跟注释一样，经常

会抱怨别人写的代码没有写注释，然而自己写起代码起来，最讨厌的，也是写注释。所以仅仅只通过强制来

规范大家是不够的，随着时间推移，版本迭代，接口文档往往很容易就跟不上代码了



Swagger是一个开源的软件框架，可以帮助开发人员设计、构建和使用Web服务，将代码与文档结合在一

起，完美的解决了上述问题，使开发人员将大部分精力集中到业务中，而不是文档的撰写



Swagger3在Swagger2的基础上进行了部分升级， 使用和Swagger2没有多少区别

一个重要的优化是依赖的引入，由之前的多个依赖变更为一个依赖，跟随springboot-starter风格

同时引入了新的开关注解 @EnableOpenApi 以代替@EnableSwagger2

因此，集成工作变得更加的简便了，必要工作只有两个

- 添加swagger3的starter依赖包
- 在springboot主程序类添加@EnableOpenApi开关注解



Swagger解决的痛点

传统方式提供文档有以下痛点：

1. 文档需要更新的时候，需要再次发送一份给前端，也就是文档更新交流不及时
2. 接口返回结果不明确
3. 不能直接在线测试接口，通常需要使用工具，比如postman
4. 接口文档太多，不好管理



Swagger存在的问题：`最明显的就是代码移入性比较强`



Swagger3的改变

Swagger3.0的改动，官方文档总结如下几点：

删除了对springfox-swagger2的依赖

删除所有@EnableSwagger2…注解

添加了springfox-boot-starter依赖项

移除了guava等第三方依赖；

文档访问地址改为http://ip:port/project/swagger-ui/index.html。




# 2:SpringBoot集成

SpringBoot集成Swagger3与SpringBoot集成其他框架的套路基本一致

通常包括：加入依赖、配置文件，创建配置类和使用

```xml
<dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-boot-starter</artifactId>
      <version>3.0.0</version>
</dependency>
```

# 3:基本使用

```java
@Api(tags = "教师管理")
@RestController
@RequestMapping("/guli/teacher")
public class TeacherController {
    
    @GetMapping("/one")
    public Teacher one(){
        Teacher teacher = new Teacher();
        teacher.setName("周志雄");
        return teacher;
    }
}
```

查看网页：http://localhost:8080/swagger-ui/index.html，可以看到我们的注解生效了

![image-20221018112359980](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018112359980.png)



# 4:常用的注解

```sh
@Api：用在Controller类上，表示对类的说明
    tags="说明该类的作用，可以在UI界面上看到的注解"

@ApiOperation：用在Controller方法上，说明方法的用途、作用
    value="说明方法的用途、作用"
    notes="方法的备注说明"

@ApiImplicitParams：用在Controller的方法上，表示一组参数说明
    @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
        name：参数名
        value：参数的汉字说明、解释
        required：参数是否必须传
        paramType：参数放在哪个地方
            · header --> 请求参数的获取：@RequestHeader
            · query --> 请求参数的获取：@RequestParam
            · path（用于restful接口）--> 请求参数的获取：@PathVariable
            · div（不常用）
            · form（不常用）    
        dataType：参数类型，默认String，其它值dataType="Integer"       
        defaultValue：参数的默认值

@ApiResponses：用在Controller的方法上，表示一组响应
    @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
        code：数字，例如400
        message：信息，例如"请求参数没填好"
        response：抛出异常的类

@ApiModel：用于响应类上【一般就是实体类】，表示一个返回响应数据的信息
            （这种一般用在post创建的时候，使用@RequestBody这样的场景，
            请求参数无法使用@ApiImplicitParam注解进行描述的时候）
            
@ApiModelProperty：用在属性上，描述响应类的属性
```



## 4.1:演示@Api

```java
@Api(tags = "教师管理")
@RestController
@RequestMapping("/guli/teacher")
public class TeacherController {
    
    @GetMapping("/one")
    public Teacher one(){
        Teacher teacher = new Teacher();
        teacher.setName("周志雄");
        return teacher;
    }
}
```

![image-20221018112359980](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018112359980.png)



## 4.2:演示@ApiOperation

```java
@Api(tags = "教师管理")
@RestController
@RequestMapping("/guli/teacher")
public class TeacherController {

    @ApiOperation(value = "展现教师信息",notes = "展现教师信息备注")
    @GetMapping("/one")
    public Teacher one(){
        Teacher teacher = new Teacher();
        teacher.setName("周志雄");
        return teacher;
    }
}
```

![image-20221018112800628](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018112800628.png)



## 4.3:演示@ApiImplicitParams

```java
@ApiOperation(value = "添加教师姓名",notes = "添加教师姓名备注")
    @GetMapping("/add")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "name", value = "教师姓名", 
                              dataType = "string", example = "周志雄", required = true)
    })
    public Teacher add(String name){
        Teacher teacher = new Teacher();
        teacher.setName(name);
        return teacher;
    }
```

![image-20221018113652045](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018113652045.png)



## 4.4:演示@ApiResponses

```java
@ApiOperation(value = "添加教师姓名",notes = "添加教师姓名备注")
@GetMapping("/add")
@ApiResponses({
        @ApiResponse(code=400,message="请求参数没填好"),
        @ApiResponse(code=404,message="请求路径没有或页面跳转路径不对")
})
public Teacher add(String name){
    Teacher teacher = new Teacher();
    teacher.setName(name);
    return teacher;
}
```

![image-20221018114313123](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018114313123.png)



## 4.5:演示@ApiModel

```java
@ApiModel(value = "Teacher对象", description = "讲师")
public class Teacher implements Serializable {

    private static final long serialVersionUID = 1L;

    @ApiModelProperty("讲师ID")
    @TableId(value = "id", type = IdType.ASSIGN_ID)
    private String id;

    @ApiModelProperty("讲师姓名")
    @TableField("name")
    private String name;
}
```



## 4.6:演示@ApiModelProperty

```java
@ApiModel(value="user对象",description="用户对象user")
public class User implements Serializable{
    private static final long serialVersionUID = 1L;
    
     @ApiModelProperty(value="用户名",name="username",example="xingguo")
     private String username;
     @ApiModelProperty(value="状态",name="state",required=true)
      private Integer state;
      private String password;
      private String nickName;
      private Integer isDeleted;

      @ApiModelProperty(value="id数组",hidden=true)
      private String[] ids;
      private List<String> idList;
}
```

# 5:Swagger配置

在SpringBoot中，对Swagger的配置都是通过配置Docket对象来实现的

先来看看主页面

![1](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/1.png)

## 5.1:基本配置

```java
import springfox.documentation.service.Contact;


@Bean
public Docket api(){
    return new Docket(DocumentationType.OAS_30)
        	.enable(false) // 开关，我们只有在开发环境才会用到swagger
            //配置基本信息
            .apiInfo(apiInfo())//设置扫描接口
            .select()
        	.build();
}

@Bean
public ApiInfo apiInfo(){
    Contact contact = new Contact(
            "周志雄", // 作者姓名
            "www.baidu.com", // 作者网址
            "536509593@qq.com"); // 作者邮箱
    
    return new ApiInfoBuilder()
            .title("测试") // 标题
            .description("众里寻他千百度，慕然回首那人却在灯火阑珊处") // 描述
            .termsOfServiceUrl("https://www.baidu.com") // 跳转连接
            .version("1.0") // 版本
            .contact(contact)
            .build();
}
```

![image-20221018215733898](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018215733898.png)



## 5.2:Docket的select

使用过滤，必须先调用`select`方法

该方法返回了一个构造器`ApiSelectorBuilder`，用于构建API选择器`ApiSelector`。

在该构造器中，比较常用的两个方法是`apis()`和`paths()`

apis：扫描接口

paths：确实哪些接口会被添加API文档

## 5.3:apis扫描接口

```java
@Bean
public Docket api(){
    return new Docket(DocumentationType.OAS_30)
            //配置基本信息
            .apiInfo(apiInfo())//设置扫描接口
            .select()
        
            //配置如何扫描接口
            .apis(RequestHandlerSelectors
            	.any() // 扫描全部的接口，默认
            	.none() // 全部不扫描
            	.basePackage("com.zzx.guli.controller") // 扫描指定包下的接口，最为常用
            	.withClassAnnotation(RestController.class) // 扫描带有指定注解的类下所有接口
            	.withMethodAnnotation(PostMapping.class) // 扫描带有只当注解的方法接口
            ).paths(PathSelectors.none())
            .build();
}
```

在上述代码中，主要是通过以下两个方法来进行扫描接口配置的：

- selcet():是创建一个构造器，用于定义Swagger生成的文档中应该包含哪些接口和方法
- api():用来定义要包含的类(控制器和模型类)



方法api()参数为一个RequestHandoler对象

通常在使用的时候通过调用RequestHandlerSelectors中的方法来传递给api()参数

RequestHandlerSelectors中常用方法为：

- `any()`：扫描所有控制器

- `none()`：不扫描

- `basePackage(final String basePackage)`：扫描指定包名下的控制器
- `withClassAnnotation(RestController.class)`：扫描带有指定注解的类下所有接口
- `withMethodAnnotation(PostMapping.class) `：扫描带有只当注解的方法接口

抛开paths(PathSelectors.none())不看，apis是扫描的类，但是并不是一定真正被显示在UI界面的类



## 5.4:paths显示接口

这个时候就要使用`paths(PathSelectors.xxx)`了

在`PathSelectors`对象常用的方法有：

- `any()`:扫描所有请求路径
- `none()`:不扫描
- `ant(final String antPattern)`:匹配Ant样式的路径模式
- `regex(final String pathRegex)`:匹配正则指定的正则表达式路径

```java
@Bean
public Docket api(){
    return new Docket(DocumentationType.OAS_30)
            //配置基本信息
            .apiInfo(apiInfo())//设置扫描接口
            .select()
        
            //配置如何扫描接口
            .apis(RequestHandlerSelectors
            	.any() // 扫描全部的接口，默认
            	.none() // 全部不扫描
            	.basePackage("com.zzx.guli.controller") // 扫描指定包下的接口，最为常用
            	.withClassAnnotation(RestController.class) // 扫描带有指定注解的类下所有接口
            	.withMethodAnnotation(PostMapping.class) // 扫描带有只当注解的方法接口
            ).paths(PathSelectors.any())
            .build();
}
```



## 5.5:build()

最后需要调用`build()`方法来对将当前配置的接口扫描信息配置到当前的Docket对象中



## 5.6:配置API分组

一个Docket对象对应一个分组，可以通过创建多个Docket对象来实现创建多个分组。

在Swagger-UI中，页面右上角可以对分组进行选择

![image-20221018222445511](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018222445511.png)

只需要在调用`select()`方法之前调用`groupName("xxx")`就可以了



配置多个分组的目的就是将所有的API进行分类，每一个分组显示各自所扫描到的API信息

这里创建了两个Dokcet对象，两个Docket对象所扫描的包不同，扫描的路径也不同



# 6:再次总结注解

Swagger常用注解如下：

![image-20221018222729239](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/image-20221018222729239.png)



## 6.1:@Api和 @ApiOperation

```java
@RestController()
@Api(tags = "描述HelloController")
public class HelloController {

    @ApiOperation(value="描述getUser()",notes="对getUser()更详细的描述",response = User.class)
    @GetMapping("/getuser")
    public User getUser(String username, String password, int age){
        return new User(username,password,age);
    }
}
```

![1](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/1-16661036984521.png)



## 6.2:@ApiImplicitParams和@ApiImplicitParam

```java
@ApiOperation(value="描述getUser()",notes = "对getuser()更详细的描述",response = User.class)
@ApiImplicitParams({
      @ApiImplicitParam(name="username",
            //value：参数说明。
            value = "ApiImplicitParam-用户名",
            //dataType：参数数据类型。
            dataType = "string",
            //paramType：参数类型，取值为 path、query、body、header、form。
            paramType = "query",
            //required：是否必填。
            required = true,
            //defaultValue：默认值。
            defaultValue = "ApiImplicitParam-helo")
    })
@GetMapping("/getuser")
public User getUser( String username,String password,int age){
        return new User(username,password,age);
}
```

![2](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/2-16661039041802.png)



## 6.3:@ApiParam

```java
@ApiOperation(value="描述getUser()",notes = "对getuser()更详细的描述",response = User.class)
@GetMapping("/getuser")
 public User getUser( 
 	@ApiParam(value="ApiParam-描述username",required = true)String username,
 	@ApiParam(value="ApiParam-描述password",required = true)String password,
 	@ApiParam(value="ApiParam-描述age",required = true)int age){
    return new User(username,password,age);
}
```

![3](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/3.png)



## 6.4:@ApiModel和@ApiModelProperty

```java
@Data
@ToString
@ApiModel(value = "ApiModel-Value-User",description = "ApiModel-User具体说明")
public class User {
    @ApiModelProperty(value = "用户名")
    private String username;
    @ApiModelProperty(value = "用户密码")
    private String password;
    @ApiModelProperty(value = "用户年龄")
    private Integer age;

    public User(String username, String password, Integer age) {
        this.username = username;
        this.password = password;
        this.age = age;
    }
}
```

![5](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/5.png)



## 6.5:@ApiResponse和@ApiResponses

```java
@RestController()
@Api(tags = "描述HelloController")
@ApiResponses({@ApiResponse(code = 200,message = "@ApiResponse-OK",response = User.class)})
public class HelloController {

@ApiOperation(value="描述getUser()",notes = "对getuser()更详细的描述",response = User.class)
@GetMapping("/getuser")
public User getUser(String username, String password, int age){
     	 return new User(username,password,age);
	}
}
```

![6](https://zzx-note.oss-cn-beijing.aliyuncs.com/swagger/6.png)