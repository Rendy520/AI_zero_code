
# 你如何在 AI 项目中实现自定义业务指标的监控？

我的业务指标监控方案是基于业界主流的 Prometheus + Grafana 技术栈构建的，并通过在应用代码中进行手动埋点来实现数据的精细化采集。整个方案遵循 **采集-存储-展示** 的经典监控流程。

1）采集

我使用了 `Micrometer` 这个指标采集门面库，它集成在 Spring Boot Actuator 中。Micrometer 提供了一套统一的 API 来定义和记录指标，并且能够将这些指标转换为多种监控系统所需的格式，其中就包括 Prometheus。

我创建了一个 `AiModelMetricsCollector` 类，在其中使用 Micrometer API 定义了我们关心的核心业务指标，主要包括：

-   `Counter`：用于统计累计值，如 AI 请求总数、Token 消耗总量、错误总数。
-   `Timer`：用于记录耗时分布，如 AI 响应时长。

利用 LangChain4j 框架提供的 `ChatModelListener` 监听器接口，创建了 `AiModelMonitorListener`。这个监听器会在 AI 调用的 `onRequest`, `onResponse`, `onError` 等关键生命周期节点被触发，然后调用 `AiModelMetricsCollector` 来实时记录上述定义的指标数据。

Spring Boot Actuator 会自动创建一个 `/actuator/prometheus` 端点，将 Micrometer 采集的所有指标以 Prometheus 要求的文本格式暴露出来。

2）存储

我部署了一个独立的 Prometheus 服务，并在它的配置文件中添加了一个抓取任务，让它定期来拉取后端应用暴露的 `/actuator/prometheus` 端点的数据。Prometheus 会将这些时序数据高效地存储在它内置的 TSDB 时序数据库中。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/ZK7U8JbI_1754975743800-29954d3b-80e4-457f-85fb-5bb75a1abd8c_mianshiya.png)

3）展示

我在 Grafana 中创建了一个自定义的监控仪表盘，通过编写 PromQL 查询语句，将存储在 Prometheus 中的原始指标数据聚合成有意义的可视化图表，比如：

-   模型调用趋势图
-   Token 消耗速率图
-   Top 10 Token 消耗用户排行

因为一个个搭建图表费时费力，所以我采用了一种更高效的方式：让 AI 帮我生成完整的看板 JSON 配置。需要给 AI 提供需求说明、数据样例和 Grafana 官方的 JSON 规范。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/kGStaQ5z_1755099021246-329ed3d8-b29c-4511-9cb1-0023058a933f_mianshiya.png)
