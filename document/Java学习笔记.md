[【返回首页】](../README.md) | [【返回上一页】](../README.md)

---

<h1 align="center" style="text-align:center;vertical-align:middle;">
  Java学习笔记
</h1>

## Java学习笔记

---

### Java注解使用

---

#### 【0001】@SuppressWarings注解基本用法及取值类型

【基本用法】

* @SuppressWarnings ("警告取值类型")批注允许您选择性地取消特定代码段（即，类或方法）中的警告。
* 其中的想法是当您看到警告时，您将调查它，如果您确定它不是问题，您就可以添加一个 @SuppressWarnings 批注，以使您不会再看到警告。
* 虽然它听起来似乎会屏蔽潜在的错误，但实际上它将提高代码安全性，因为它将防止您对警告无动于衷 — 您看到的每一个警告都将值得注意。

【取值类型】

| 警告取值类型                   | 用途                                                                                       | 用途（中文）                               |
|--------------------------|------------------------------------------------------------------------------------------|--------------------------------------|
| all                      | to suppress all warnings                                                                 | 抑制所有警告                               |
| boxing                   | to suppress warnings relative to boxing/unboxing operations                              | 抑制装箱/拆箱操作相关的警告                       |
| cast                     | to suppress warnings relative to cast operations                                         | 抑制类型转换操作相关的警告                        |
| dep-ann                  | to suppress warnings relative to deprecated annotation                                   | 抑制已废弃注解相关的警告                         |
| deprecation              | to suppress warnings relative to deprecation                                             | 抑制使用已废弃 API 的警告                      |
| fallthrough              | to suppress warnings relative to missing breaks in switch statements                     | 抑制 switch 语句中缺少 break 语句的警告          |
| finally                  | to suppress warnings relative to finally block that don’t return                         | 抑制 finally 块中返回值的警告                  |
| hiding                   | to suppress warnings relative to locals that hide variable                               | 抑制局部变量隐藏成员变量的警告                      |
| incomplete-switch        | to suppress warnings relative to missing entries in a switch statement (enum case)       | 抑制 switch 语句不完整覆盖所有枚举项的警告            |
| nls                      | to suppress warnings relative to non-nls string literals                                 | 抑制非 NLS 字符串字面量相关的警告                  |
| null                     | to suppress warnings relative to null analysis                                           | 抑制空分析相关的警告                           |
| rawtypes                 | to suppress warnings relative to un-specific types when using generics on class params   | 抑制使用泛型时未指定类型的警告                      |
| restriction              | to suppress warnings relative to usage of discouraged or forbidden references            | 抑制使用不推荐或禁止引用相关的警告                    |
| serial                   | to suppress warnings relative to missing serialVersionUID field for a serializable class | 抑制实现序列化接口的类没有声明 serialVersionUID 的警告 |
| staic                    | to suppress warnings relative to incorrect static                                        | 抑制静态方法中访问实例成员的警告                     |
| static-access            | to suppress warnings relative to incorrect static access                                 | 抑制不正确的静态访问相关的警告                      |
| synthetic-access         | to suppress warnings relative to unoptimized access from inner classes                   | 抑制内部类未优化访问相关的警告                      |
| unchecked                | to suppress warnings relative to unchecked operations                                    | 抑制未经检查或不安全的操作警告，如使用泛型时的未经检查的转换       |
| unqualified-field-access | to suppress warnings relative to field access unqualified                                | 抑制未限定字段访问相关的警告                       |
| unused                   | to suppress warnings relative to unused code                                             | 抑制未使用的局部变量、参数、类、方法或字段的警告             |
| path                     | to suppress warnings relative to file path                                               | 抑制对文件路径的警告                           |

---

### Java注意点

---

#### 【0001】入参存在告警，“未注解的形参重写 @NonNullApi 形参”

* 需要增加标记是否可能为null的注解，比如@Nullable或@NotNull
  * 使用@Nullable注解表明这个方法可能返回null
  * 使用@NotNull注解表明这个方法不会返回null
* @Nullable和@NotNull的好处
  * 提高代码可读性：通过明确标识哪些引用可能为null，其他开发者可以更快地理解代码的逻辑，并避免因为不了解某个引用可能为null而导致的错误。 
  * 增强代码安全性：使用这些注解可以配合静态代码分析工具，在编译时检测可能的null引用错误，从而尽早发现和修复问题。 
  * 更好的IDE支持：现代IDE（如IntelliJ IDEA和Android Studio）通常会对这些注解提供丰富的支持，包括代码检查、自动补全和快速修复建议等，进一步提高了开发效率

---

#### 【0002】使用yuyue-java-spring-swagger的注意事项

* yuyue-java-spring-swagger是一个基于springdoc-openapi的Swagger增强工具，用于生成Swagger文档，使用的版本是Swagger3
* 可以通过注解@EnableOpenApi启用Swagger
  * value配置的值，在单体模式下，用于指定path；在微服务模式下，用于区分服务
  * isMicro配置的值，为true且nacos存在服务时，进入微服务模式
* 首先需要区分是单体版本还是微服务版本
  * 当使用单体版本时
    * 除了需要`io.github.yuyue98:yuyue-java-spring-swagger`外，还需要`org.springdoc:springdoc-openapi-starter-webmvc-ui`，将前端文件引入项目
    * 请注意，为了统一Authorize的URL，需要配置环境变量GATEWAY_HOST与GATEWAY_PORT为单体服务的HOST和PORT
    * 访问Swagger页面，请使用`http://${GATEWAY_HOST}:${GATEWAY_PORT}/path/swagger-ui/index.html`
  * 当使用微服务版本是
    * 除了需要`io.github.yuyue98:yuyue-java-spring-swagger`外，还需要`org.springdoc:springdoc-openapi-starter-webflux-ui`，将前端文件引入项目
    * 请注意，微服务版本，依赖nacos服务，需要将所有需要使用Swagger的服务注册到nacos上，且存在gateway服务进行统一管理
    * 访问Swagger页面，请使用`http://${GATEWAY_HOST:yuyue-gateway}:${GATEWAY_PORT:9999}/swagger-ui/index.html`
* 为了增强Swagger，可以引入knife4j，作为前端页面，即`com.github.xiaoymin:knife4j-openapi3-ui`
  * 当使用单体版本时
    * 访问knife4j页面，请使用`http://${GATEWAY_HOST}:${GATEWAY_PORT}/path/doc.html`
  * 当使用微服务版本时
    * 访问knife4j页面，请使用`http://${GATEWAY_HOST:yuyue-gateway}:${GATEWAY_PORT:9999}/doc.html`

---

#### 【0003】Swagger3的注意事项

【注意点】

1. Swagger3注解的包路径为`io.swagger.v3.oas.annotations.`
2. Swagger3入参相关注解
   * 实体类作为query入参时，如果需要将实体类的参数展开，需要使用注解`@ParameterObject`，或者开启配置`springdoc.default-flat-param-object=true`
   * 如果使用yuyue-java-spring-swagger，实体类的参数展开默认开启，也可以关闭配置`swagger.default-flat-param-object=false`
   * 实体类作为query入参时，需要使用注解`@Parameter(description = "名称")`，配置参数的Swagger3信息
   * 实体类作为body入参时，需要使用注解`@Schema(title = "名称")`，配置参数的Swagger3信息

【Swagger2与Swagger3注解区分】

| Swagger2           | Swagger3(OpenAPI3)                                              | 注解位置                         |
|:-------------------|:----------------------------------------------------------------|:-----------------------------|
| @Api               | @Tag(name = “接口类描述”)                                            | Controller 类上                |
| @ApiOperation      | @Operation(summary =“接口方法描述”)                                   | Controller 方法上               |
| @ApiImplicitParams | @Parameters                                                     | Controller 方法上               |
| @ApiImplicitParam  | @Parameter(description=“参数描述”)                                  | Controller 方法上 @Parameters 里 |
| @ApiParam          | @Parameter(description=“参数描述”)                                  | Controller 方法的参数上            |
| @ApiIgnore         | @Parameter(hidden = true) 或 @Operation(hidden = true) 或 @Hidden | -                            |
| @ApiModel          | @Schema                                                         | DTO类上                        |
| @ApiModelProperty  | @Schema                                                         | DTO属性上                       |

---

【注解信息】

- 基本信息注解
  - `@OpenAPIDefinition`
    - 描述：用于定义整个 API 文档的基本信息。
    - 可用于：类、接口。
    - 属性：
      - info：指定 `@Info` 注解的对象，用于描述 API 文档的基本信息。
  - `@Info`
    - 描述：用于定义 API 文档的基本信息。
    - 可用于：类、接口。
    - 属性：
      - title：API 的标题。
      - description：API 的描述。
      - version：API 的版本号。
      - termsOfService：服务条款的 URL。
      - contact：指定 `@Contact` 注解的对象，用于描述联系人信息。
      - license：指定 `@License` 注解的对象，用于描述许可证信息。
  - `@Contact`
    - 描述：用于定义 API 文档中的联系人信息。
    - 可用于：类、接口。
    - 属性：
      - name：联系人的名称。
      - url：联系人的网址。
      - email：联系人的电子邮件地址。
  - `@License`
    - 描述：用于定义 API 文档中的许可证信息。
    - 可用于：类、接口。
    - 属性：
      - name：许可证的名称。
      - url：许可证的网址。
- 分组注解
  - `@Tag`
    - 描述：用于给 API 分组，用途类似于为 API 文档添加标签。
    - 可用于：方法、类、接口。
    - 属性：
      - name：分组的名称。
- 请求方法注解
  - `@Operation`
    - 描述：用于描述 API 的操作。
    - 可用于：方法。
    - 属性：
      - summary：操作的摘要信息description：操作的详细描述。
      - description：操作的详细描述。
      - tags：指定 @Tag 注解的对象数组，用于将操作归类到特定的分组。
      - parameters：指定 `@Parameter` 注解的对象数组，用于描述操作的输入参数。
      - responses：指定 `@ApiResponse` 注解的对象数组，用于描述操作的响应结果。
      - requestBody：指定 `@RequestBody` 注解的对象，用于描述操作的请求体。
  - `@Parameter`
    - 描述：用于描述操作的输入参数。
    - 可用于：方法。
    - 属性：
      - name：参数的名称。
      - in：参数的位置，可以是 path、query、header、cookie 中的一种。
      - description：参数的描述。
      - required：参数是否必需，默认为 false。
      - schema：指定 `@Schema` 注解的对象，用于描述参数的数据类型。
  - `@RequestBody`
    - 描述：用于描述操作的请求体。
    - 可用于：方法。
    - 属性：
      - required：请求体是否必需，默认为 false。
      - content：指定 `@Content` 注解的对象数组，用于描述请求体的内容。
  - `@ApiResponse`
    - 描述：用于描述操作的响应结果。
    - 可用于：方法。
    - 属性：
      - responseCode：响应的状态码。
      - description：响应的描述。
      - content：指定 `@Content` 注解的对象数组，用于描述响应的内容。
  - `@Content`
    - 描述：用于描述请求体或响应的内容。
    - 可用于：方法。
    - 属性：
      - mediaType：内容的媒体类型。
      - schema：指定 `@Schema` 注解的对象，用于描述内容的数据类型。
  - `@Schema`
    - 描述：用于描述数据模型的属性。
    - 可用于：方法、类、接口。
    - 属性：
      - title：数据模型的标题。
      - description：数据模型的描述。
      - type：数据模型的类型。
      - format：数据模型的格式。
- 路径注解
  - `@Path`
    - 描述：用于定义路径参数。
    - 可用于：方法。
    - 属性：
      - value：路径参数的名称。
  - `@PathVariable`
    - 描述：用于描述路径参数。
    - 可用于：方法的参数。
    - 属性：
      - value：路径参数的名称。
  - `@RequestParam`
    - 描述：用于描述查询参数。
    - 可用于：方法的参数。
    - 属性：
      - value：查询参数的名称。
      - required：查询参数是否必需，默认为 false。
  - `@RequestBody`
    - 描述：用于描述请求体。
    - 可用于：方法的参数。
- 响应注解
  - `@ApiResponse`
    - 描述：用于描述响应结果。
    - 可用于：方法。
    - 属性：
      - responseCode：响应的状态码。
      - description：响应的描述。
      - content：指定 `@Content` 注解的对象数组，用于描述响应的内容。

---


