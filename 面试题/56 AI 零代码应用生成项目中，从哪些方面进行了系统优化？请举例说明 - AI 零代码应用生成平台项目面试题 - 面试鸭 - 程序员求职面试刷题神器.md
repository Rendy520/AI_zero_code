
# AI 零代码应用生成项目中，从哪些方面进行了系统优化？请举例说明

系统主要从性能、实时性、安全性、稳定性和成本这 5 个方面进行了全面优化。

下面以性能优化中的解决 AI 并发调用瓶颈为例详细说明：

最初，系统存在一个严重的性能瓶颈：当多个用户同时请求AI生成代码时，这些请求会被串行处理，后来的用户必须等待前面的请求全部完成后才能开始执行。通过分析发现，根本原因在于 LangChain4j 框架中的 `ChatModel` 实例被配置为单例。尽管流式接口 `StreamingChatModel` 使用了响应式编程，但其底层的 HTTP 客户端在解析数据流时仍然是同步阻塞的，导致整个 AI 调用链无法真正实现并发。

解决方案是采用多例模式，每次 AI 调用都使用一个全新的、独立的 `ChatModel` 实例，避免共享实例带来的阻塞。这个方案相比于自己编写工厂类来创建实例，更能利用 Spring 框架的特性，代码更简洁且易于维护。 为了验证优化效果，项目编写了并发测试用例，利用 Java 21 的虚拟线程同时发起多个 AI 调用请求。测试结果显示，所有请求几乎同时开始和结束，证明了请求现在是并行处理的，成功解决了这个并发性能瓶颈。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/owojNOyh_1755763305473-752e6473-5d7d-4952-9382-0fafd4692d7e_mianshiya.png)

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/VRIRwfhm_1755763310415-b773588e-11e1-4c78-b6cd-e1f0bf916912_mianshiya.png)
