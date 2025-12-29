
# LangChain4j 在项目中有什么作用？你是如何将 AI 大模型能力集成到业务代码中的？

LangChain4j 在项目中是核心的 AI 开发框架，它的主要作用是简化和标准化与大模型交互的过程。它通过一套声明式的 API ，让我可以不用关心底层 HTTP 请求和复杂的 JSON 处理，更专注于业务逻辑的实现。

我主要是通过 AI Service 这种模式将大模型能力集成到业务代码中的，这个过程可以分为几个步骤：

1）定义服务接口：我创建了一个 `AiCodeGeneratorService` 接口，并在其中定义了与业务相关的方法，比如 `generateHtmlCodeStream` 和 `generateVueProjectCodeStream`。

2）使用注解驱动：在接口方法上，我使用了 LangChain4j 提供的注解来配置AI的行为。

-   `@SystemMessage`：用于关联一个系统提示词文件，设定AI的角色和任务约束。
-   `@UserMessage`：用于标记哪个方法参数是用户的输入。
-   `@MemoryId`：用于为不同的对话分配独立的记忆空间，这在实现按应用隔离对话记忆时非常关键。

3）创建服务实例：我编写了一个 `AiCodeGeneratorServiceFactory` 工厂类，使用 `AiServices.builder()` 来构建 `AiCodeGeneratorService` 接口的动态代理实现。在这个构建过程中，我可以注入配置好的 `ChatModel` 或 `StreamingChatModel`，还可以注册工具、配置对话记忆和护轨等。

4）业务层调用：最后，在我的业务逻辑中，我可以直接注入并调用 `AiCodeGeneratorService` 接口的方法，就像调用一个普通的 Java 方法一样，LangChain4j 框架会自动处理与大模型的所有通信细节。

除了基本的对话功能，我还利用了 LangChain4j 的其他核心特性，比如通过结构化输出将 AI 的 JSON 响应自动映射为 Java 对象，通过工具调用让 AI 能够使用我提供的 `FileWriteTool` 等工具来生成复杂的 Vue 工程项目。
