
# 什么是 LangChain4j 的护轨机制？你是如何利用输入护轨来防止恶意 Prompt 输入和注入攻击的？

LangChain4j 的护轨机制是一套用于保障 AI 应用安全和稳定性的拦截器系统，类似于 Web 应用中的过滤器或拦截器。它分为两种：

-   输入护轨 (InputGuardrail)：在用户的输入发送给大模型之前进行检查。
-   输出护轨 (OutputGuardrail)：在大模型返回响应之后，但在内容呈现给用户之前进行检查。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/bgQYoLYi_1755762526746-44c39aa1-4afa-49d4-abb9-128ff0bf7bf1_mianshiya.png)

在项目中，我主要利用输入护轨来防御恶意的 Prompt 输入和常见的注入攻击：

1）创建自定义护轨：我实现了一个 `PromptSafetyInputGuardrail` 类，它继承了 `InputGuardrail` 接口。

2）实现多维度校验：在这个类的 `validate` 方法中，我定义了多层校验逻辑来审查用户的输入内容：

-   长度限制：检查输入是否超过预设的最大长度，防止超长 Prompt 消耗过多资源。
-   敏感词过滤：维护一个静态的敏感词列表，包含如“忽略之前的指令”、“破解”、“越狱”等词汇。如果输入中包含这些词，请求将被拒绝。
-   注入模式检测：定义了一系列正则表达式，用于匹配常见的 Prompt 注入攻击模式，比如试图让 AI 忘记身份、扮演其他角色或遵循新指令的语句。如果匹配成功，同样会拦截请求。

3）拦截与响应：如果任何一项检查失败，`validate` 方法会返回 `fatal("错误信息")`，这会立即中断 AI 调用流程并抛出异常，阻止恶意输入被发送到大模型。

4）集成到 AI Service：最后，在 `AiCodeGeneratorServiceFactory` 中构建 AI Service 实例时，通过 `.inputGuardrails()` 方法将这个自定义的护轨注册进去。
