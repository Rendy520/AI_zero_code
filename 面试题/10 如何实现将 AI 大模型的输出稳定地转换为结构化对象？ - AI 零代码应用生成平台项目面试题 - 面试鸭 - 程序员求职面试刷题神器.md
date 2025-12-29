
# 如何实现将 AI 大模型的输出稳定地转换为结构化对象？

为了让程序能方便地处理 AI 生成的代码和描述，我利用了 LangChain4j 的结构化输出功能，将 AI 返回的自由文本自动转换为预定义的 Java 对象，比如 `HtmlCodeResult` 和 `MultiFileCodeResult`。

实现方式很简单，只需要将 AI Service 接口方法的返回值类型从 `String` 改为对应的 Java 类即可，LangChain4j 框架会自动在发送给大模型的提示词中追加指令，要求其以 JSON 格式返回数据。

但在实践过程中，我发现 AI 并不能 100% 稳定地返回符合预期的 JSON，主要遇到了 AI 返回非 JSON 格式或 JSON 结构错误的问题，导致程序解析失败。

为了解决这个问题，我采取了以下几种优化措施：

1.  设置 `max_tokens`：在模型配置中，我增大了 `max-tokens` 的值，比如设置为 8192，以防止 AI 生成的 JSON 内容因为超出长度限制而被截断，从而导致 JSON 格式不完整。
2.  开启模型的 JSON 模式：我查阅了 DeepSeek 模型的官方文档，发现它支持通过 `response-format` 参数强制开启 JSON 输出模式。于是我在`application.yml`配置文件中添加了 `response-format: json_object`，这极大地提高了输出的稳定性。
3.  为字段添加详细描述：为了让 AI 更好地理解期望的JSON结构，我使用了 LangChain4j 提供的 `@Description` 注解，为结果类及其每个字段都添加了清晰的中文描述，比如 `@Description("HTML代码")`。这些描述会被框架自动转换并附加到提示词中，为 AI 提供了更明确的指引。
4.  在提示词中补充 JSON 格式输出的要求

通过这些手段，结构化输出的成功率得到了显著提升。对于偶尔出现的失败情况，LangChain4j 框架自带的默认重试机制也能够处理大部分问题。
