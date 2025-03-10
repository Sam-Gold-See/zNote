# Online-Study 项目笔记

## JWT 令牌拦截器

技术关键词：**JWT**、**常量解析密钥**、

### 引入依赖（`0.12.6`）

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
            Long empId = Long.valueOf(claims.get(JwtClaimsConstant.ADMIN_ID).toString());
            log.info("当前员工id：{}", empId);
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
