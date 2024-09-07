[【返回首页】](../README.md) | [【返回上一页】](../README.md)

---

<h1 align="center" style="text-align:center;vertical-align:middle;">
  Java学习笔记
</h1>

## Java学习笔记

### Java注意点

#### 【0001】入参存在告警，“未注解的形参重写 @NonNullApi 形参”

* 需要增加标记是否可能为null的注解，比如@Nullable或@NotNull
  * 使用@Nullable注解表明这个方法可能返回null
  * 使用@NotNull注解表明这个方法不会返回null
* @Nullable和@NotNull的好处
  * 提高代码可读性：通过明确标识哪些引用可能为null，其他开发者可以更快地理解代码的逻辑，并避免因为不了解某个引用可能为null而导致的错误。 
  * 增强代码安全性：使用这些注解可以配合静态代码分析工具，在编译时检测可能的null引用错误，从而尽早发现和修复问题。 
  * 更好的IDE支持：现代IDE（如IntelliJ IDEA和Android Studio）通常会对这些注解提供丰富的支持，包括代码检查、自动补全和快速修复建议等，进一步提高了开发效率

#### 【0002】使用yuyue-java-spring-swagger的注意事项

* yuyue-java-spring-swagger是一个基于springdoc-openapi的swagger增强工具，用于生成swagger文档，使用的版本是swagger3
* 可以通过注解@EnableOpenApi启用swagger
  * value配置的值，在单体模式下，用于指定path；在微服务模式下，用于区分服务
  * isMicro配置的值，为true且nacos存在服务时，进入微服务模式
* 首先需要区分是单体版本还是微服务版本
  * 当使用单体版本时
    * 除了需要`io.github.yuyue98:yuyue-java-spring-swagger`外，还需要`org.springdoc:springdoc-openapi-starter-webmvc-ui`，将前端文件引入项目
    * 请注意，为了统一Authorize的URL，需要配置环境变量GATEWAY_HOST与GATEWAY_PORT为单体服务的HOST和PORT
    * 访问swagger页面，请使用`http://${GATEWAY_HOST}:${GATEWAY_PORT}/path/swagger-ui/index.html`
  * 当使用微服务版本是
    * 除了需要`io.github.yuyue98:yuyue-java-spring-swagger`外，还需要`org.springdoc:springdoc-openapi-starter-webflux-ui`，将前端文件引入项目
    * 请注意，微服务版本，依赖nacos服务，需要将所有需要使用swagger的服务注册到nacos上，且存在gateway服务进行统一管理
    * 访问swagger页面，请使用`http://${GATEWAY_HOST:yuyue-gateway}:${GATEWAY_PORT:9999}/swagger-ui/index.html`
* 为了增强swagger的适用性，可以引入knife4j，作为前端页面，即`com.github.xiaoymin:knife4j-openapi3-ui`
  * 当使用单体版本时
    * 访问knife4j页面，请使用`http://${GATEWAY_HOST}:${GATEWAY_PORT}/path/doc.html`
  * 当使用微服务版本时
    * 访问knife4j页面，请使用`http://${GATEWAY_HOST:yuyue-gateway}:${GATEWAY_PORT:9999}/doc.html`

