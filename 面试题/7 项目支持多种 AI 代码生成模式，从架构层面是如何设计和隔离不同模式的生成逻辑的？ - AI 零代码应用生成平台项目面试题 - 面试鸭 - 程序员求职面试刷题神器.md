
# 项目支持多种 AI 代码生成模式，从架构层面是如何设计和隔离不同模式的生成逻辑的？

为了支持原生 HTML、多文件和 Vue 工程这三种不同的代码生成模式，我在架构设计上采用了门面模式 + 策略模式 + 工厂模式相结合的方式，实现了逻辑的清晰隔离和灵活扩展。

核心设计如下：

1）我创建了一个 `AiCodeGeneratorFacade` 门面类，它对外提供一个统一的 `generateAndSaveCodeStream` 方法。这个方法接收用户消息和代码生成类型作为参数，屏蔽了内部不同生成模式的实现细节。

2）在 `AiCodeGeneratorService` 接口中，我为每种生成模式定义了不同的方法，比如 `generateHtmlCodeStream` 和 `generateVueProjectCodeStream`。每种方法都通过 `@SystemMessage` 注解关联一个独立的系统提示词文件，确保 AI 在执行任务时遵循该模式特定的规则和约束。

3）在 `AiCodeGeneratorServiceFactory` 中，我实现了一个工厂方法 `getAiCodeGeneratorService`，它会根据传入的 `CodeGenTypeEnum` 来动态构建和配置 `AiCodeGeneratorService` 实例。

-   对于 Vue工程模式，它会配置使用能力更强的推理模型，并注入文件写入等一系列工具，因为这种模式依赖于工具调用来生成项目文件结构。
-   对于原生 HTML 和多文件模式，它会配置使用成本更低的对话模型，并且不注入任何工具，因为这两种模式是直接通过解析 AI 输出的 Markdown 代码块来保存文件的。

4）由于不同模式下 AI 的流式输出格式不同，我设计了 `StreamHandlerExecutor` 执行器，它会根据生成类型选择对应的流处理器来解析和处理 AI 的实时响应，实现了处理逻辑的隔离。

通过这种方式，每种生成模式的核心逻辑都被封装在各自的提示词、AI Service 配置和流处理器中，当未来需要支持新的生成模式时，我只需要新增相应的枚举、提示词文件，并在工厂类和执行器中增加一个分支即可，无需改动现有逻辑。

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/7UOKm56D_202508221358362_mianshiya.png)
