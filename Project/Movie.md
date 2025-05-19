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
