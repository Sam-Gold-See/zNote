# Online-Study 项目笔记

## JWT 令牌拦截器

技术关键词：**JWT**、**常量解析密钥**、

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.6</version>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.6</version>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.6</version>
</dependency>
```

### 常量解析密钥

```yml application.yml
online-study:
  jwt:
    # 设置jwt签名加密时使用的秘钥
    admin-secret-key: study
    # 设置jwt过期时间，单位毫秒
    admin-ttl: 7200000
    # 设置前端传递过来的令牌名称，键
    admin-token-name: token
```

```java JwtProperties.java
@Component
@ConfigurationProperties(prefix = "online-study.jwt")
@Data
public class JwtProperties {

    /**
     * B端用户生成jwt令牌相关配置
     */
    private String adminSecretKey;
    private long adminTtl;
    private String adminTokenName;
}
```

```java JwtUtil.java
/**
 * jwt令牌工具类
 */

public class JwtUtil {

    /**
     * 常量解析密钥
     *
     * @param secretKey JWT 密钥，至少为256位
     * @param token     需要解析的 JWT Token
     * @return 解析后的 Claims
     */
    public static Claims parseJWT(String secretKey, String token) {
        Jws<Claims> jws = Jwts.parser()// 创建JWT解析器
                .verifyWith(Keys.hmacShaKeyFor(secretKey.getBytes(StandardCharsets.UTF_8)))// 设置JWT签名验证密钥
                .build()// 构建解析器
                .parseSignedClaims(token); // 解析JWT Token，返回Jws<Claims>对象

        return jws.getPayload(); // 从Jws<Claims>对象中提取Claims（载荷数据）
    }
}
```

```java JwtClaimsConstant.java
/**
 * jwt载荷常量类
 */

public class JwtClaimsConstant {

    public static final String ADMIN_ID = "adminId";
}
```

```java JwtTokenAdminInterceptor.java
/**
 * jwt令牌校验拦截器
 */

@Component
@Slf4j
public class JwtTokenAdminInterceptor implements HandlerInterceptor {

    @Autowired
    private JwtProperties jwtProperties; // 注入配置类，获取 JWT 配置（如令牌名称和秘钥）

    /**
     * 校验 JWT 令牌
     *
     * @param request  当前请求
     * @param response 当前响应
     * @param handler  当前处理的 handler（通常是 Controller 方法）
     * @return 返回 true 表示请求通过，false 表示请求被拦截
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        //判断当前拦截到的是Controller的方法还是其他资源
        if (!(handler instanceof HandlerMethod)) {
            //当前拦截到的不是动态方法，直接放行
            return true;
        }

        //1. 从请求头中获取令牌
        String token = request.getHeader(jwtProperties.getAdminTokenName());

        //2. 校验令牌
        try {
            log.info("jwt校验：{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getAdminSecretKey(), token);
            Long adminUserId = Long.valueOf(claims.get(JwtClaimsConstant.ADMIN_ID).toString());
            log.info("当前B端用户id：{}", adminUserId);
            return true;
        } catch (Exception ex) {
            //4. 不通过，响应401状态码（未经授权）
            response.setStatus(401);
            return false;
        }
    }
}

```

```java WevMvcConfiguration.java
/**
 * 配置类，注册Web层相关组件
 */

@Configuration
@Slf4j
public class WebMvcConfiguration extends WebMvcConfigurationSupport {

    @Autowired
    private JwtTokenAdminInterceptor jwtTokenAdminInterceptor;

    /**
     * 自定义拦截器*/
    protected void addInterceptors(InterceptorRegistry registry) {
        log.info("开始注册自定义拦截器");
        registry.addInterceptor(jwtTokenAdminInterceptor)
                .addPathPatterns("/admin/**")
                .excludePathPatterns("/admin/user/login");
    }
}
```

### 生成 Jwt 令牌

```java JwtUtil.java
/**
 * jwt令牌工具类
 */

public class JwtUtil {

    /**
     * 生成jwt
     * 使用Hs256算法, 私匙使用固定秘钥
     *
     * @param secretKey jwt秘钥
     * @param ttlMillis jwt过期时间(毫秒)
     * @param claims    设置的信息
     * @return jwt令牌字符串
     */
    public static String createJWT(String secretKey, long ttlMillis, Map<String, Object> claims) {
        // 生成符合 JJWT 规范的 HMAC-SHA 密钥
        Key key = Keys.hmacShaKeyFor(secretKey.getBytes(StandardCharsets.UTF_8));

        // 计算过期时间
        long expMillis = System.currentTimeMillis() + ttlMillis;
        Date exp = new Date(expMillis);

        // 创建 JWT 令牌
        return Jwts.builder()
                .claims(claims)     // 设置自定义载荷
                .expiration(exp)    // 设置过期时间
                .signWith(key)      // 使用密钥进行签名
                .compact();         // 生成 JWT 字符串
    }
}
```

### 结合 ThreadLocal 优化 Jwt 拦截器

```java BaseContext.java
/**
 * 线程上下文管理
 */

public class BaseContext {

    /**
     * ThreadLocal 变量存储当前线程的用户ID
     */
    public static ThreadLocal<Long> threadLocal = new ThreadLocal<>();

    /**
     * 获取当前线程的用户ID
     *
     * @return 当前用户ID
     */
    public static Long getCurrentId() {
        return threadLocal.get();
    }

    /**
     * 设置当前线程的用户ID
     *
     * @param id 当前用户ID
     */
    public static void setCurrentId(Long id) {
        threadLocal.set(id);
    }

    /**
     * 移除当前线程的用户ID，防止内存泄露
     */
    public static void removeCurrentId() {
        threadLocal.remove();
    }
}
```

```java JwtTokenAdminInterceptor.java
/**
 * B端jwt令牌校验拦截器
 */

@Component
@Slf4j
public class JwtTokenAdminInterceptor implements HandlerInterceptor {

    @Autowired
    private JwtProperties jwtProperties; // 注入配置类，获取 JWT 配置（如令牌名称和秘钥）

    /**
     * 校验 JWT 令牌
     *
     * @param request  当前请求
     * @param response 当前响应
     * @param handler  当前处理的 handler（通常是 Controller 方法）
     * @return 返回 true 表示请求通过，false 表示请求被拦截
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        //判断当前拦截到的是Controller的方法还是其他资源
        if (!(handler instanceof HandlerMethod)) {
            //当前拦截到的不是动态方法，直接放行
            return true;
        }

        //1. 从请求头中获取令牌
        String token = request.getHeader(jwtProperties.getAdminTokenName());

        //2. 校验令牌
        try {
            log.info("jwt校验：{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getAdminSecretKey(), token);
            Long adminUserId = Long.valueOf(claims.get(JwtClaimsConstant.ADMIN_ID).toString());
            log.info("当前B端用户id：{}", adminUserId);
            BaseContext.setCurrentId(adminUserId);
            return true;
        } catch (Exception ex) {
            //4. 不通过，响应401状态码（未经授权）
            response.setStatus(401);
            return false;
        }
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        // 清除当前线程中的用户 ID
        BaseContext.removeCurrentId();
    }
}
```

## 序列化和反序列化

### 自定义对象映射器

```java JacksonObjectMapper.java
/**
 * 对象映射器：基于jackson将Java对象转为json，或者将json转为Java独享
 * 将JSON解析为Java对象的过程称为【从JSON反序列化Java对象】
 * 从Java对象生成JSON的过程称为【序列化Java对象到JSON】
 */

public class JacksonObjectMapper extends ObjectMapper {

    public static final String DEFAULT_DATE_FORMAT = "yyyy-MM-dd";
    public static final String DEFAULT_DATE_TIME_FORMAT = "yyyy-MM-dd HH:mm:ss";
    public static final String DEFAULT_TIME_FORMAT = "HH:mm:ss";

    // 对象映射器
    public JacksonObjectMapper() {
        super();
        // 收到未知属性时不报异常
        this.configure(FAIL_ON_UNKNOWN_PROPERTIES, false);

        SimpleModule simpleModule = new SimpleModule()
                .addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)))
                .addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)));

        // 注册功能模块，例如，可以添加自定义序列化器和反序列化器
        this.registerModule(simpleModule);
    }
}
```

```java WebMvcConfiguration.java
/**
 * 配置类，注册Web层相关组件
 */

@Configuration
@Slf4j
public class WebMvcConfiguration extends WebMvcConfigurationSupport {
    /**
     * 拓展Spring MVC的消息转化器
     *
     * @param converters 消息转化器类对象
     */
    @Override
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        // 创建一个消息转换器对象
        MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
        // 需要为消息转换器设置一个对象转换器，对象转换器可以将Java对象序列为Json数据
        converter.setObjectMapper(new JacksonObjectMapper());
        // 将自己的消息转化器加入容器中
        converters.add(0, converter);
    }
}
```

## AOP

### 公共字段自动注入

```java OperationType.java
/**
 * 数据库操作类型
 */
public enum OperationType {

    /**
     * 更新操作
     */
    UPDATE,

    /**
     * 插入操作
     */
    INSERT
}
```

```java AutoFill.java
/**
 * 自定义注解，用于标识需要进行公共字段自动填充处理的方法
 */

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AutoFill {
    // 指定数据库的操作类型：UPDATE INSERT
    OperationType value();
}
```

```java AutoFillConstant.java
/**
 * 公共字段自动填充相关常量
 */
public class AutoFillConstant {

    /**
     * 实体类中的方法
     */
    public static final String SET_CREATE_TIME = "setCreateTime";
    public static final String SET_UPDATE_TIME = "setUpdateTime";
    public static final String SET_CREATE_USER = "setCreateUser";
    public static final String SET_UPDATE_USER = "setUpdateUser";
}
```

```java AutoFillAspect.java
/**
 * 自定义切面，实现公共字段自动填充处理
 */

@Aspect
@Component
@Slf4j
public class AutoFillAspect {

    /**
     * 切入点
     */
    @Pointcut("execution(* com.study.mapper.*.*(..)) && @annotation(com.study.annotation.AutoFill) ")
    public void autoFillPointCut() {
    }

    @Before("autoFillPointCut()")
    public void autoFill(JoinPoint joinPoint) {
        // 获取当前被拦截的方法上的数据库操作类型
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();// 方法签名对象
        AutoFill autoFill = signature.getMethod().getAnnotation(AutoFill.class);// 获得方法上的注释对象
        OperationType operationType = autoFill.value();// 获得数据库操作类型

        // 获取到当前被拦截的方法的参数实体对象
        Object[] args = joinPoint.getArgs();// 约定所有方法，将实体对象作为第一个参数
        if (args == null || args.length == 0)
            return;
        Object entity = args[0];

        // 准备赋值的数据
        LocalDateTime now = LocalDateTime.now();
        Long currentId = BaseContext.getCurrentId();

        // 根据不同的操作类型，为对应的属性通过反射赋值
        if (operationType == OperationType.INSERT)
            try {
                Method setCreateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_TIME, LocalDateTime.class);
                Method setCreateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_USER, Long.class);
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                setCreateTime.invoke(entity, now);
                setCreateUser.invoke(entity, currentId);
                setUpdateTime.invoke(entity, now);
                setUpdateUser.invoke(entity, currentId);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        else if (operationType == OperationType.UPDATE)
            try {
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                setUpdateTime.invoke(entity, now);
                setUpdateUser.invoke(entity, currentId);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
    }
}
```

## 全局统一响应

### Result 和 PageResult

```java Result.java
/**
 * 后端统一响应结果
 */
@Data
public class Result<T> implements Serializable {

    private Integer code;
    private String msg;
    private T data;

    public static <T> Result<T> success() {
        Result<T> result = new Result<>();
        result.code = 1;
        return result;
    }

    public static <T> Result<T> success(T object) {
        Result<T> result = new Result<>();
        result.code = 1;
        result.data = object;
        return result;
    }

    public static <T> Result<T> error(String msg) {
        Result<T> result = success();
        result.msg = msg;
        result.code = 0;
        return result;
    }
}
```

```java PageResult.java
/**
 * 封装分页查询结果
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class PageResult<T> implements Serializable {

    private long total; //总记录数

    private List<T> records; //当前页数据集合

}
```

## 统一异常处理

```java BaseException.java
/**
 * 自定义业务异常基类
 */
public class BaseException extends RuntimeException {

    public BaseException() {
    }

    public BaseException(String message) {
        super(message);
    }
}
```

```java GlobalExceptionHandler.java
/**
 * 全局异常处理器，处理项目中抛出的业务异常
 */
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    /**
     * 捕获业务异常
     *
     * @param e BaseException 业务异常类
     * @return Result响应对象类
     */
    @ExceptionHandler
    public Result<String> exceptionHandler(BaseException e) {
        log.error("异常信息：{}", e.getMessage());
        return Result.error(e.getMessage());
    }
}
```

### 优化统一异常处理

对于`SQLIntegrityConstraintViolationException`异常，为了更好的报错显示，使用正则表达式匹配读取信息并返回

```java GlobalExceptionHandler.java
/**
 * 全局异常处理器，处理项目中抛出的业务异常
 */
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    /**
     * 处理 SQL 违反约束异常 (`SQLIntegrityConstraintViolationException`)
     *
     * @param ex SQLIntegrityConstraintViolationException SQL 唯一约束或外键约束异常
     * @return Result<String> 返回封装的错误信息
     */
    @ExceptionHandler(SQLIntegrityConstraintViolationException.class)
    public Result<String> exceptionHandler(SQLIntegrityConstraintViolationException ex) {
        // 获取异常信息
        String message = ex.getMessage();

        // 记录日志，方便调试和排查问题
        log.error("SQL 约束异常: {}", message);

        // 处理唯一约束冲突问题
        // 'Duplicate entry 'admin' for key 'admin_user.username'
        if (message.contains("Duplicate entry")) {
            // 使用正则表达式提取冲突值，如 `admin`
            Pattern pattern = Pattern.compile("Duplicate entry '(.*?)' for key");
            Matcher matcher = pattern.matcher(message);
            if (matcher.find()) {
                String username = matcher.group(1); // 获取冲突字段的值
                String msg = username + MessageConstant.ALREADY_EXISTS; // 格式化提示
                return Result.error(msg); // 返回全局统一响应对象
            }
        }
        // 其他未知 SQL 约束异常，返回通用错误信息
        return Result.error(MessageConstant.UNKNOWN_ERROR);
    }
}
```

## 邮箱发送验证码

```xml pom.xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-email</artifactId>
    <version>1.5</version>
</dependency>
```

### 配置 + 发送邮件工具类

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

```java EmailConfigProperties.java
/**
 * 邮件配置类
 */

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

```java EmailUtil.java
/**
 * 邮件发送工具类（静态）
 */
@Component
public class EmailUtils {

    private static EmailConfigProperties emailConfigProperties;

    /**
     * 由 Spring 初始化 EmailConfigProperties
     */
    public EmailUtils(EmailConfigProperties emailConfigProperties) {
        EmailUtils.emailConfigProperties = emailConfigProperties;
    }

    /**
     * 发送验证码邮件（163邮箱）
     *
     * @param toEmail          接收邮箱
     * @param verificationCode 验证码
     */
    public static void sendVerificationCode(String toEmail, String verificationCode) {
        try {
            HtmlEmail email = new HtmlEmail();
            email.setHostName(emailConfigProperties.getHost()); // SMTP 服务器
            email.setCharset("UTF-8");
            email.setAuthentication(emailConfigProperties.getUsername(), emailConfigProperties.getPassword()); // 认证信息
            email.setFrom(emailConfigProperties.getUsername(), "OnlineStudy"); // 发件人
            email.setSSLOnConnect(true); // 启用SSL
            email.addTo(toEmail); // 收件人
            email.setSubject("登录验证码"); // 邮件主题
            email.setMsg("尊敬的用户，您好！您本次操作的验证码是：" + verificationCode + " ，请尽快使用。"); // 邮件内容
            email.send();
        } catch (Exception e) {
            throw new EmailException(MessageConstant.SEND_EMAIL_FAIL);
        }
    }
}
```

## 分页查询

```xml pom.xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>2.1.0</version>
</dependency>
```

```java ClientUserServiceImpl.java
@Service
public class ClientUserServiceImpl implements ClientUserService {

    @Autowired
    private ClientUserMapper clientUserMapper;

        /**
     * C端用户分页查询
     *
     * @param clientUserPageQueryDTO C端用户分页查询DTO对象
     * @return PageResult<AdminUser> ClientUser类的分页查询对象
     */
    @Override
    public PageResult<ClientUser> getClientListPage(ClientUserPageQueryDTO clientUserPageQueryDTO) {
        PageHelper.startPage(clientUserPageQueryDTO.getPage(), clientUserPageQueryDTO.getPageSize());

        Page<ClientUser> page = clientUserMapper.getListPage(clientUserPageQueryDTO);

        return new PageResult<>(page.getTotal(), page.getResult());
    }
}
```