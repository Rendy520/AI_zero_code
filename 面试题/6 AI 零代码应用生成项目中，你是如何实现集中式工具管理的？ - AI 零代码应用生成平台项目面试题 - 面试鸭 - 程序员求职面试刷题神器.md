
# AI 零代码应用生成项目中，你是如何实现集中式工具管理的？

在项目中，随着提供给 AI 的工具越来越多，如果分散管理，会导致代码混乱且难以维护。

因此，我们设计了一套集中式的工具管理机制，其核心是 `ToolManager` 类 + Spring 框架的依赖注入 + 设计模式。

整体架构如下：

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/D0CcS65H_1755833556997-4d832cb2-c719-4e3e-b4c4-61be35819871_mianshiya.png)

具体实现步骤：

1）定义工具基类：我们首先创建了一个抽象的 `BaseTool` 基类。 这个基类定义了所有工具都必须具备的通用接口和行为，例如 `getToolName()`和 `getDisplayName()`，以及生成不同阶段向用户展示信息的统一方法。

2）实现具体工具：项目中的每一个具体工具，如 `FileWriteTool`、`FileReadTool`、`FileModifyTool` 等，都继承自 `BaseTool` 基类，并被声明为 Spring 的 Bean。 这样，它们就可以被 Spring 容器自动扫描和管理。

3）创建工具管理器：这是集中管理的核心。

-   `ToolManager` 本身也是一个 Spring Bean。
-   它通过 `@Resource` 注解，注入了一个 `BaseTool` 类型的**数组**。这是关键一步，Spring 会自动将容器中所有继承了 `BaseTool` 的 Bean 实例收集起来，并注入到这个数组中。
-   在 `@PostConstruct` 初始化方法中，`ToolManager` 会遍历这个工具数组，将每个工具实例以其 `toolName` 为键，存入一个内部的 `Map` 中，方便后续通过名称快速查找。

4）统一使用：

-   `ToolManager` 提供了 `getTool(String toolName)` 方法，可以根据名称快速获取任何一个已注册的工具实例。
-   它还提供了 `getAllTools()` 方法，直接返回包含所有工具实例的数组，这个方法专门用于为 LangChain4j 的 AI Service 统一注册所有可用工具。

通过这种方式，我们实现了工具的自动注册和集中管理。未来如果需要新增一个工具，我们只需要创建一个新的类继承 `BaseTool` 并加上 `@Component` 注解即可，无需修改任何现有管理和注册逻辑的代码。
