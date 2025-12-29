
# 什么是 ARMS？如何利用 ARMS 实现系统监控？它主要监控了哪些指标？

ARMS 是阿里云提供的应用实时监控服务，采用了探针技术，**能够在不修改应用代码的情况下**，自动收集和分析应用性能数据，快速构建实时的监控能力。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/DwTALKsM_1755783662222-715836e9-c4f2-4aa4-8cd6-ebc40bd7fbe0_mianshiya.png)

ARMS 的接入过程非常简单，完全不需要修改任何业务代码，它的核心是 Java Agent 探针技术。

我们只需在应用的启动命令中，通过 `-javaagent` 参数挂载 ARMS 提供的探针 JAR 包，并配置好应用的唯一 License Key 和名称。应用启动时，JVM 会加载这个探针，它会自动通过字节码增强技术，在关键的方法前后动态注入监控代码。这些监控代码会收集性能数据，并将其上报到 ARMS 控制台，实现了对应用的全面监控。

ARMS 为我们提供了开箱即用的、多维度的监控视图，主要覆盖了以下几类关键指标：

1）应用性能指标：

-   接口监控：自动发现并监控所有 Spring MVC 接口的请求数、错误率、平均耗时，以及 P50/P99 等响应时间百分位数。
-   调用链追踪：记录单个请求在系统内部的完整调用链路，清晰地展示请求从进入 Controller 到访问数据库、缓存、外部 AI 模型的每一步耗时，是定位性能瓶颈的利器。
-   异常分析：自动捕获并聚合应用中的所有异常，提供详细的堆栈信息，方便快速定位错误根源。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/QyogyZXx_1754925074445-1eaa6b6b-595a-4a7a-8b7c-fdf603f5e456_mianshiya.png)

2）外部依赖监控：

-   数据库调用：监控所有 SQL 查询的执行次数、耗时，并能自动识别和告警慢 SQL。
-   NoSQL 调用：监控对 Redis 的调用情况，包括操作命令、耗时等。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/JPdONHqH_1754924642853-e390c508-d9fe-4621-9af7-01594f636295_mianshiya.png)

3）JVM 和系统资源监控：

-   JVM 监控：深入监控 JVM 的内部状态，包括堆内存使用情况、GC（垃圾回收）频率与耗时、线程数和线程池状态等。
-   系统资源：监控服务器的 CPU 使用率、内存占用、磁盘 I/O 和网络流量等基础设施指标。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/MvN20IhM_1754925652119-aad032ed-49b6-4c2c-a62a-7c43337b7175_mianshiya.png)
