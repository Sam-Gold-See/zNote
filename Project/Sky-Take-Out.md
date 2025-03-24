# Sky-Take-Out 项目笔记

当前端提交的数据和实体类中对应的属性差别比较大时，建议使用**DTO**来封装数据

- 功能测试

  - 测试方式分为：接口文档测试、前后端联调测试

  - 由于开发阶段前后端是并行开发的，后端完成某个功能后，此时前端对应的功能可能还没有开发完成，导致无法进行前后端联调测试。

  - 在开发阶段，后端测试主要以接口文档测试为主

- **ThreadLocal**

  - ThreadLocal 并不是一个 Thread，而是 Thread 的局部变量

  - ThreadLocal 为每个线程提供单独一份存储空间，具有线程隔离的效果，只有在线程内才能获得到对应的值，线程外则不能访问

  - `public void set(T value)`设置当前现成的线程局部变量的值

  - `public T get()`返回当前线程所对应的线程局部变量的值

  - `public void remove()`移除当线程成的线程局部变量

- pagehelper

  - pagehelper 是 MyBatis 的分页插件，可以很方便的实现 MyBatis 的分页功能

  - 引入依赖：`pom.xml`中添加依赖

- 日期展示

  - 属性上加入注解，对日期进行格式化`@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")`

  - 在 WebMvcConfiguration 中拓展 Spring MVC 的消息转换器，同一对日期类型进行格式化处理

  ```java
  package com.sky.config;
  /**
   * 配置类，注册web层相关组件
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
        //创建一个消息转换器对象
        MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
        //需要为消息转换器设置一个对象转换器，对象转换器可以将Java对象序列化为Json数据
        converter.setObjectMapper(new JacksonObjectMapper());
        //将自己的消息转化器加入容器中
        converters.add(0, converter);
    }
  }
  ```

- 公共字段自动填充

  - 技术点：枚举、注解、AOP、反射

  - 自定义注解 `AutoFill` ，用于标识需要进行公共字段自动填充的方法

  - 自定义切面类 `AutoFillAspect` ，统一拦截加入了 `AutoFill` 注解的方法，通过反射为公共字段赋值

  - 在 `Mapper` 的方法上加上 `AutoFill` 注解，即可实现公共字段自动填充

```java
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
      @Pointcut("execution(* com.sky.mapper.*.*(..)) &&   @annotation(com.sky.annotation.AutoFill)")
      public void autoFillPointCut() {

      }

      @Before("autoFillPointCut()")
      public void autoFill(JoinPoint joinPoint) {
          //获取当前被拦截的方法上的数据库操作类型
          MethodSignature signature = (MethodSignature)   joinPoint.getSignature();//方法签名对象
          AutoFill autoFill =   signature.getMethod().getAnnotation(AutoFill.class);  //获得方法上的注释对象
          OperationType operationType = autoFill.value();//获 得数据库操作类型

          //获取到当前被拦截的方法的参数实体对象
          Object[] args = joinPoint.getArgs();//约定所有参数，  将实体对象作为第一个参数
          if (args == null || args.length == 0) {
              return;
          }
          Object entity = args[0];

          //准备赋值的数据
          LocalDateTime now = LocalDateTime.now();
          Long currentId = BaseContext.getCurrentId();

          //根据当前不同的操作类型，为对应的属性通过反射来赋值
          if (operationType == OperationType.INSERT) {
              //为四个公共字段赋值
              try {
                  Method setCreateTime =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_CREATE_TIME,  LocalDateTime.class);
                  Method setCreateUser =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_CREATE_USER, Long.class);
                  Method setUpdateTime =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_UPDATE_TIME,  LocalDateTime.class);
                  Method setUpdateUser =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_UPDATE_USER, Long.class);

                  //通过反射为对象属性赋值
                  setCreateTime.invoke(entity, now);
                  setCreateUser.invoke(entity, currentId);
                  setUpdateTime.invoke(entity, now);
                  setUpdateUser.invoke(entity, currentId);
              } catch (Exception e) {
                  throw new RuntimeException(e);
              }
          } else if (operationType == OperationType.UPDATE) {
              //为两个公共字段赋值
              try {
                  Method setUpdateTime =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_UPDATE_TIME,  LocalDateTime.class);
                  Method setUpdateUser =  entity.getClass().getDeclaredMethod(AutoFill Constant.SET_UPDATE_USER, Long.class);

                  //通过反射为对象属性赋值
                  setUpdateTime.invoke(entity, now);
                  setUpdateUser.invoke(entity, currentId);
              } catch (Exception e) {
                  throw new RuntimeException(e);
              }
          }

      }
  }
```

- 敲业务代码之前，理清业务逻辑、涉及的表查询等等准备，培养业务思维，提升开发效率

- 组件默认的 Bean 注册名字为小驼峰类名，所以在一个项目中不能同时存在两个同名组件，可以直接在`@Bean`及其子集的注解中传入组件名
