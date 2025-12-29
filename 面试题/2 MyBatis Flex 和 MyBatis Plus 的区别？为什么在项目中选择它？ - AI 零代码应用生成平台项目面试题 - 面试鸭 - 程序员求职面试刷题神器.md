
# MyBatis Flex 和 MyBatis Plus 的区别？为什么在项目中选择它？

MyBatis Flex 和 MyBatis Plus 都是对原生 MyBatis 框架的增强工具，能够简化数据库操作、提高开发效率。但它们在设计理念和功能侧重点上有所不同。

- 轻量化： MyBatis Flex 核心设计的一大优势是更轻量。除了 MyBatis 自身，它没有任何第三方依赖，这带来了更高的自主性、可控性和稳定性。相比之下，MyBatis Plus 整合的功能更多，依赖也相对复杂一些。

- 性能： MyBatis Flex 在架构上进行了优化，在 SQL 执行过程中没有任何 MyBatis 拦截器和 SQL 解析，官方宣称能带来指数级的性能提升。

- 功能灵活性： MyBatis Flex 的 QueryWrapper 非常灵活，一个核心亮点是它原生支持多表关联查询，这在处理复杂业务场景时非常有用。而 MyBatis Plus 在多表查询方面则需要依赖其他插件或手写 SQL 来实现。

项目选型原因：

在这个项目中，我们选择 MyBatis Flex 主要基于以下考虑：

1. 技术栈更新与学习： 为了丰富技术栈，体验和学习新的、有竞争力的技术框架。

2. 性能优势： 看重其无拦截器、无 SQL 解析带来的性能潜力。

下图是官方提供的与 MyBatis Plus 的功能对比：
![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/kx0Ra3al_1755830827846-ed36f33c-3e4f-4930-b713-e12cb2085b2f_mianshiya.png)