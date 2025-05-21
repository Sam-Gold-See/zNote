# Movie

## 接口文档

导入 Knife4j 依赖,自带 Swagger3

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
    <version>${knife4j}</version>
</dependency>
```

- `@Tag` 注解标注在接口类上，用于标注在 swagger 文档中显示的标签。

- `@Operation` 注解标注在接口方法上，用于标注在 swagger 文档中显示的接口名称，同时会根据`RequestMapping`系列注解中的接收请求方式生成不同的接口。

  - `summary` 用于显示接口名称。

  - `description` 用于显示接口描述。

- `@Parameter` 注解标注在接口方法的参数上，用于标注在 swagger 文档中显示的参数名称、类型、是否必填、默认值等信息。

  - `name` 用于显示参数名称。

  - `description` 用于显示参数描述。

  - `required` 布尔值，用于显示参数是否必填。

  - `example` 用于显示参数示例。

  - `schema` 用于显示参数类型。

  - `defaultValue` 用于显示参数默认值。

  - `in` 用于显示参数位置，可选值有`ParameterIn.QUERY` - `@RequestParam`、`ParameterIn.PATH` - `@PathVariable`、`ParameterIn.HEADER` - `@RequestHeader`、`ParameterIn.COOKIE` - `@CookieValue`。

- `@Schema` 注解标注在接口方法的参数和返回值上，用于标注在 swagger 文档中显示的类型、描述等信息，主要用于`Post`的请求体JSON数据描述。

  - `description` 用于显示类型描述。

  - `title` 用于显示类型名称。

  - `requiredMode` 用于显示类型是否必填。

  - `example` 用于显示类型示例。

```java
@Data
@Schema(description = "全局统一响应结果", title = "Result")
public class Result<T> implements Serializable {

    @Schema(description = "状态码")
    private Integer code;

    @Schema(description = "报错信息")
    private String msg;

    @Schema(description = "响应数据")
    private T data;
}
```

- `@ApiResponse` 注解标注在接口方法的返回值上，用于标注在 swagger 文档中显示的返回值名称、类型、描述等信息。

  - `responseCode` 用于显示返回值状态码。

  - `description` 用于显示返回值描述。

  - `content` 用于显示返回值类型。

```java
@ApiResponse(responseCode = "200", description = "成功", content = @Content(
        schema = @Schema(implementation = Result.class)
))
```

- 对于封装数据的`Result<UserVO>`类统一响应对象，需要在`Controller`中新建静态类，然后`content`中的`schema`指向该静态类

```java
@Schema(name = "UserVOResult", description = "包含UserVO的统一响应对象")
public static class UserVOResult extends Result<UserVO> {}

@ApiResponse(responseCode = "200", description = "成功", content = @Content(
        mediaType = Me
        schema = @Schema(implementation = UserVOResult.class)
))
```

## 日志

导入 logback 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
</dependency>
```

## Thymeleaf

导入 Thymeleaf 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

在服务器模块中的`resources`目录下创建`templates`文件夹，在此放置`html`模板文件。

静态文件的引用需要使用 Thymeleaf 中的`th:src`属性。对于例如这类文件`resources/static/css/index.css`需要使用`th:src="@{css/index.css}"`来指向

## mail

导入 `commons-email` 依赖

```xml pom.xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-email</artifactId>
    <version>1.5</version>
</dependency>
```

配置 yml 文件

```yml application.yml
spring:
  mail:
    host:
    port:
    username:
    password:
    protocal:
    default-encoding: UTF-8
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true
            required: true
          ssl:
            enable: true
```

新建 `EmailConfigProperties` 类配置邮箱相关信息

```java EmailConfigProperties.java
@Data
@Component
@ConfigurationProperties(prefix = "spring.mail")
public class EmailConfigProperties {

    // 邮件服务器地址
    private String host;

    // 邮件服务器端口
    private int port;

    // 发送邮件的邮箱账号
    private String username;

    // 邮箱授权码
    private String password;

    // 发送邮件协议
    private String protocol;
}
```

实现邮件发送工具类，因为是静态工具类，所以容器注入时需要使用构造方法注入。

```java EmailUtils.java
@Component
public class EmailUtils {

    private static EmailConfigProperties emailConfigProperties;

    public EmailUtils(EmailConfigProperties emailConfigProperties) {
        EmailUtils.emailConfigProperties = emailConfigProperties;
    }

        public static void sendVerificationCode(String toEmail, String verificationCode) {
        try{
            HtmlEmail email = new HtmlEmail();
            email.setHostName(emailConfigProperties.getHost()); // SMTP 服务器
            email.setCharset("UTF-8");
            email.setAuthentication(emailConfigProperties.getUsername(), emailConfigProperties.getPassword());
            email.setSSLOnConnect(true); // 启用SSL
            email.addTo(toEmail); // 设置收信人
            email.setSubject("登录验证码"); // 邮件主题
            email.setMsg("尊敬的用户，您好！您本次操作的验证码是：" + verificationCode + " ，请在" + AccountConstant.VERIFICATION_CODE_TTL + "分钟内尽快使用。"); // 邮件内容
            email.send();
        }catch (Exception e){
            throw new EmailException(MessageConstant.SEND_EMAIL_FAIL);
        }
    }
}
```
