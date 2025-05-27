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

通过`OpenApiConfig`类配置 swagger 文档信息。

```java
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("奶龙影视")
                        .description("奶龙影视网站的接口文档")
                        .version("1.0.0")
                        .contact(new Contact()
                                .name("SamGoldSee")
                                .url("https://github.com/Sam-Gold-See")
                                .email("chunxin.huang@m.scnu.edu.cn"))
                );
    }
}
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

- `@Schema` 注解标注在接口方法的参数和返回值上，用于标注在 swagger 文档中显示的类型、描述等信息，主要用于`Post`的请求体 JSON 数据描述。

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

- `@Hidden` 注解标注在接口方法上，用于隐藏该接口但是可供接口文档展示。

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

## Spring Security

导入 Spring Security 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

配置 `SecurityConfig` 类，配置 Spring Security 相关信息，同时注入`BCryptPasswordEncoder` 作为 `PasswordEncoder`

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                // 配置放行路径
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers(

                                // 页面请求
                                "/", "/toLogin", "/toRegister", "/email/sendCode", "/email/checkCode", "/user/register", "/user/login",

                                // knife4j接口文档路径
                                "/doc.html", "swagger-ui.html", "/swagger-resources/**", "/webjars/**", "/v2/api-docs", "/v3/api-docs", "/v3/api-docs/**", "/swagger-ui/**")

                        .permitAll().anyRequest().authenticated())

                // 配置登录相关属性
                .formLogin(form -> form
                        .loginPage("/toLogin")
                        .loginProcessingUrl("/user/login")
                        .usernameParameter("email")
                        .defaultSuccessUrl("/", true)
                        .permitAll())

                // 配置退出相关属性
                .logout(logout -> logout
                        .logoutUrl("/toLogout")
                        .logoutSuccessUrl("/toLogin"))

                .csrf(AbstractHttpConfigurer::disable);
        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

在`UserServiceImpl`类中继承接口`UserDetailsService` 并且实现方法 `loadUserByUsername`

```java
@Service
public class UserServiceImpl implements UserService, UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {

        // 校验验证码
        checkVerificationCodeStatus(email);

        // 检查用户是否存在
        User user = userMapper.getByEmail(email);
        if (user == null)
            throw new AccountException(MessageConstant.USER_NOT_EXISTS);

        return new org.springframework.security.core.userdetails.User(
                user.getEmail(),
                user.getPassword(),
                AuthorityUtils.commaSeparatedStringToAuthorityList("ROLE_USER")
        );
    }
}
```

为了存储用户 id、是否是 VIP 等信息，需要拓展`UserDetails`接口，并实现相关方法。

```java
@Data
public class UserSession implements UserDetails {

    private final Integer id;

    private final String email;

    private final String password;

    private final Boolean type;

    private final Collection<? extends GrantedAuthority> authorities;

    public UserSession(User user) {
        this.id = user.getId();
        this.email = user.getEmail();
        this.password = user.getPassword();
        this.type = user.getType();
        this.authorities = AuthorityUtils.createAuthorityList("ROLE_USER");
    }


    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return email;
    }
}
```

可以通过`Spring Security`的上下文容器`SecurityContextHolder`获取当前登录的用户信息。

```java
        UserSession userSession = (UserSession) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
```

## OSS 服务

### 配置依赖、属性

```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>${aliyun-oss}</version>
</dependency>
```

```yml
aliyun:
  oss:
    endpoint:
    access-key-id:
    access-key-secret:
    bucket-name:
```

```java
@Data
@ConfigurationProperties(prefix = "aliyun.oss")
@Configuration
public class OSSConfig{
    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;
}
```

### 实现业务层代码

```java
    /**
     * 阿里云OSS文件上传
     *
     * @param file       目标文件
     * @param bucketPath Bucket文件路径
     */
    @Override
    public String upload(MultipartFile file, String bucketPath) {

        // 获取相关配置
        String bucketName = ossConfig.getBucketName();
        String endpoint = ossConfig.getEndpoint();
        String accessKeyId = ossConfig.getAccessKeyId();
        String accessKeySecret = ossConfig.getAccessKeySecret();

        // 创建OSS对象
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        // 获取原生文件名
        String originalFilename = file.getOriginalFilename();

        // JDK8的日期格式
        LocalDateTime time = LocalDateTime.now();
        DateTimeFormatter dft = DateTimeFormatter.ofPattern("yyyy-MM-dd");

        // 获取用户名
        UserSession userSession = (UserSession) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        String username = userSession.getUsername().split("@")[0];

        // 处理非法字符
        String safeUsername = username.replaceAll("[^a-zA-Z0-9_-]", "_");

        // 提取文件扩展名
        String extension = originalFilename != null ? originalFilename.substring(originalFilename.lastIndexOf(".")) : null;

        // 拼接OSS文件名
        String datePart = dft.format(time);
        String uploadFileName = bucketPath + datePart + "-" + safeUsername + extension;

        try {
            PutObjectResult result = ossClient.putObject(bucketName, uploadFileName, file.getInputStream());
            // 拼装返回路径
            if (result != null)
                return "https://" + bucketName + "." + endpoint + "/" + uploadFileName;
        } catch (IOException ioe) {
            throw new FileException(MessageConstant.FILE_UPLOAD_ERROR + ":" + ioe.getMessage());
        } finally {
            // 关闭OSS服务，避免造成OOM
            ossClient.shutdown();
        }

        return null;
    }
```

## 支付宝沙箱支付

### 配置依赖、属性

```xml
<dependency>
    <groupId>com.alipay.sdk</groupId>
    <artifactId>alipay-sdk-java</artifactId>
    <version>${alipay-sdk}</version>
</dependency>

<dependency>
    <groupId>com.alipay.sdk</groupId>
    <artifactId>alipay-easysdk</artifactId>
    <version>${alipay-easysdk}</version>
</dependency>
```

`app_id`，`merchant_private_key`，`alipay_public_key`需要在支付宝沙箱申请，并配置到`application.yml`文件中。

```yml
alipay:
  app_id:
  merchant_private_key:
  alipay_public_key:
  notify_url: https://公网地址/alipay/notify
  return_url: https://公网地址/toProfile
  sign_type: RSA2
  charset: utf-8
  gateway_url: https://openapi-sandbox.dl.alipaydev.com/gateway.do
```

实现 `AliPayConfig.java` 类读取支付宝支付相关配置属性以及初始化支付宝 SDK

```java
@Slf4j
@ConfigurationProperties(prefix = "alipay")
@Configuration
@Data
public class AliPayConfig {

    // 应用ID
    public String appId;

    // 商户私钥
    public String merchantPrivateKey;

    // 支付宝公钥
    public String alipayPublicKey;

    // 服务器[异步通知]页面路径
    public String notifyUrl;

    // 返回路径
    public String returnUrl;

    // 签名方式
    private String signType;

    // 字符编码格式
    private String charset;

    // 支付宝网关
    public String gatewayUrl;

    @PostConstruct
    public void init() {
        // 设置参数
        Config config = new Config();
        config.protocol = "https";
        config.gatewayHost = "openapi.alipay.com";
        config.signType = this.signType;
        config.appId = this.appId;
        config.merchantPrivateKey = this.merchantPrivateKey;
        config.alipayPublicKey = this.alipayPublicKey;
        config.notifyUrl = this.notifyUrl;
        Factory.setOptions(config);
        System.out.println("=======支付宝SDK初始化成功=======");
    }
}
```

`SecurityConfig.java` 类放行支付宝支付相关接口 `/alipay/**`

```java
http
                // 配置放行路径
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers(

                                // 静态资源
                                "/css/**", "/img/**", "/js/**", "/scss/**", "/vendor/**",

                                // 页面请求
                                "/toLogin", "/toRegister", "/toForgotPassword", "/email/sendCode", "/email/checkCode", "/user/register", "/user/login", "/user/updatePassword", "/alipay/**",

                                // knife4j接口文档路径
                                "/doc.html", "/swagger-ui.html", "/swagger-resources/**", "/webjars/**", "/v2/api-docs", "/v3/api-docs", "/v3/api-docs/**", "/swagger-ui/**")

                        .permitAll().anyRequest().authenticated())
```

`AliPay.java`支付订单实体类

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Schema(description = "支付订单对象", title = "AliPay")
public class AliPay implements Serializable {

    @Serial
    private static final long serialVersionUID = 1L;

    // 订单编号
    private String traceNo;

    // 商品金额
    private Double totalAmount;

    // 商品名称
    private String subject;

    // 订单追踪号
    private String alipayTraceNo;
}
```

### 发起支付请求

`Controller` 层实现支付接口 `/alipay/pay`

```java
/**
 * 发起支付请求
 *
 * @param aliPayDTO    支付宝订单对象
 * @param httpResponse response
 */
@Operation(summary = "发起支付请求")
@GetMapping("/pay")
public void pay(AliPayDTO aliPayDTO, HttpServletResponse httpResponse) throws Exception {
    UserSession userSession = (UserSession) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
    log.info("用户(id:{})正在提交升级账户权限账单:{}", userSession.getId(), aliPayDTO);
    aliPayService.pay(aliPayDTO, httpResponse);
}
```

`Service` 层实现支付相关业务需求

```java
/**
 * 发起支付请求
 *
 * @param aliPayDTO    支付宝订单对象
 * @param httpResponse response
 */
@Override
public void pay(AliPayDTO aliPayDTO, HttpServletResponse httpResponse) throws Exception {
    UserSession userSession = (UserSession) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
    AliPayOrder order = AliPayOrder.builder()
            .tradeNo(aliPayDTO.getTradeNo())
            .totalAmount(BigDecimal.valueOf(aliPayDTO.getTotalAmount()))
            .subject(aliPayDTO.getSubject())
            .userId(userSession.getId())
            .status(false)
            .build();
    aliPayOrderMapper.insert(order);
    AlipayClient alipayClient = new DefaultAlipayClient(
            AliPayConstant.GATEWAY_URL,
            aliPayConfig.getAppId(),
            aliPayConfig.getMerchantPrivateKey(),
            AliPayConstant.FORMAT,
            AliPayConstant.CHARSET,
            aliPayConfig.getAlipayPublicKey(),
            aliPayConfig.getSignType());
    AlipayTradePagePayRequest request = new AlipayTradePagePayRequest();
    request.setNotifyUrl(aliPayConfig.getNotifyUrl());
    request.setReturnUrl(aliPayConfig.getReturnUrl());
    JSONObject bizContent = new JSONObject();
    bizContent.put("out_trade_no", aliPayDTO.getTradeNo());
    bizContent.put("total_amount", aliPayDTO.getTotalAmount());
    bizContent.put("subject", aliPayDTO.getSubject());
    bizContent.put("product_code", "FAST_INSTANT_TRADE_PAY");
    request.setBizContent(bizContent.toJSONString());
    String form;
    try {
        // 调用SDK生成表单
        form = alipayClient.pageExecute(request).getBody();
    } catch (AlipayApiException e) {
        throw new AliPayException(MessageConstant.ALIPAY_ERROR + ":" + e.getMessage());
    }
    httpResponse.setContentType("text/html;charset=" + aliPayConfig.getCharset());
    httpResponse.getWriter().write(form);
    httpResponse.getWriter().flush();
    httpResponse.getWriter().close();
}
```

### 支付回调实现业务

支付宝支付回调接口 `/alipay/notify`

```java
/**
 * 支付宝支付异步回调
 *
 * @param request request
 */
@Operation(summary = "支付回调")
@PostMapping("/notify")
@ApiResponse(responseCode = "200", description = "成功", content = @Content(
        mediaType = MediaType.APPLICATION_JSON_VALUE,
        schema = @Schema(implementation = Result.class)
))
public Result<String> payNotify(HttpServletRequest request) throws Exception {
    User user = aliPayService.payNotify(request);
    log.info("用户(id:{})完成账号权限升级", user.getId());
    return Result.success();
}
```

实现业务

```java
/**
 * 支付宝支付异步回调
 *
 * @param request request
 */
@Override
public User payNotify(HttpServletRequest request) throws Exception {
    Map<String, String> params = new HashMap<>();
    Map<String, String[]> requestParams = request.getParameterMap();
    for (String name : requestParams.keySet())
        params.put(name, request.getParameter(name));
    log.info("支付宝异步回调参数: {}", params);

    String tradeStatus = params.get("trade_status");
    if (!"TRADE_SUCCESS".equals(tradeStatus)) {
        throw new AliPayException(MessageConstant.ALIPAY_ERROR + ":" + tradeStatus);
    }

    // 验签
    boolean signVerified = Factory.Payment.Common().verifyNotify(params);
    if (!signVerified)
        throw new AliPayException(MessageConstant.ALIPAY_ERROR + ": 回调验签失败");

    String aliPayTradeNo = params.get("out_trade_no");

    AliPayOrder aliPayOrder = aliPayOrderMapper.getByTradeNo(aliPayTradeNo);

    aliPayOrderMapper.update(AliPayOrder.builder()
            .id(aliPayOrder.getId())
            .aliPayTradeNo(aliPayTradeNo)
            .status(true)
            .build());

    User user = User.builder()
            .id(aliPayOrder.getUserId())
            .type(true)
            .build();
    userMapper.update(user);

    return user;
}
```