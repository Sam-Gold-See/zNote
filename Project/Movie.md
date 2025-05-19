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

`@Tag` 注解标注在接口类上，用于标注在 swagger 文档中显示的标签。

`@Operation` 注解标注在接口方法上，用于标注在 swagger 文档中显示的接口名称，同时会根据`RequestMapping`系列注解中的接收请求方式生成不同的接口。

`@Parameter` 注解标注在接口方法的参数上，用于标注在 swagger 文档中显示的参数名称、类型、是否必填、默认值等信息。

`@RequestBody` 注解标注在接口方法的参数上，用于标注在 swagger 文档中显示的请求体参数。

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
