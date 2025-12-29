
# AI 零代码应用生成项目中，为什么选择 Higress 而不是更常见的 Spring Cloud Gateway 作为微服务网关？它在项目中有哪些作用？

我在项目中选择 Higress 作为微服务网关，主要是因为它在性能、云原生特性以及和本项目高度相关的 AI 原生能力上具有明显优势。

选择 Higress 而非 Spring Cloud Gateway 的具体对比如下：

-   性能和资源占用：Higress 基于 C++ 实现的 Envoy 构建，相比于基于 JVM 和 Spring WebFlux 的 Spring Cloud Gateway，拥有更高的性能和更低的内存占用。
-   AI 原生支持：Higress 被定义为 AI 原生网关，它内置了对 AI 场景的优化支持，比如 AI 代理缓存、AI 令牌限流以及与大模型的直接对接能力，与我们 AI 代码生成平台的业务方向高度契合。
-   云原生设计：Higress 是为云原生环境设计的，提供了更丰富的可观测性指标和更便捷的控制台管理方式。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/OLBYVGVX_1755763046676-49a1923c-43c0-48ce-8be2-e8d7161acb6b_mianshiya.png)

在项目中，我利用 Higress 网关实现了统一调用入口和请求路由：

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/ZX7mYgel_1755609917638-49bdd8e1-998d-427e-9b7d-1386be187f40_mianshiya.png)

此外，还利用 Higress 自带的插件实现了全局跨域问题的解决、API 接口的防护和限流：

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/jroRYfG0_1755667474543-70ba7bdd-30df-46e9-b7ef-fa39cc59dcf8_mianshiya.png)
