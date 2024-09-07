[【返回首页】](../README.md) | [【返回上一页】](../README.md)

---

<h1 align="center" style="text-align:center;vertical-align:middle;">
  Java相关注意点
</h1>

## 【0001】入参存在告警，“未注解的形参重写 @NonNullApi 形参”

- 需要增加标记是否可能为null的注解，比如@Nullable或@NotNull
  - 使用@Nullable注解表明这个方法可能返回null
  - 使用@NotNull注解表明这个方法不会返回null
- @Nullable和@NotNull的好处
  - 提高代码可读性：通过明确标识哪些引用可能为null，其他开发者可以更快地理解代码的逻辑，并避免因为不了解某个引用可能为null而导致的错误。 
  - 增强代码安全性：使用这些注解可以配合静态代码分析工具，在编译时检测可能的null引用错误，从而尽早发现和修复问题。 
  - 更好的IDE支持：现代IDE（如IntelliJ IDEA和Android Studio）通常会对这些注解提供丰富的支持，包括代码检查、自动补全和快速修复建议等，进一步提高了开发效率
