
# AI 零代码应用生成项目中，你如何实现工具调用信息的流式输出？

针对 Vue 工程这种复杂场景，我利用了 LangChain4j 框架的工具调用机制，并对其流式输出进行了深度定制，在前端实现实时反馈。

整个流程的核心是将 AI 在工具调用过程中的不同阶段，封装成统一格式的流式消息对象，再通过 SSE 推送给前端。

1）定义了一个包含 `type` 字段的 `StreamMessage` 基类和几个子类，用来区分是普通的 AI 文本响应，还是工具调用的请求，或是工具执行完毕的结果。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/stTZwyXp_1753103456340-3307d403-73b1-4114-981c-780cd52085e0_mianshiya.png)

2）由于 LangChain4j 的 `Flux` 响应式流默认不直接暴露工具调用的中间状态，我使用了功能更强大的 `TokenStream`。通过参考 GitHub Issues 上的源码，我通过覆盖类路径的方式为 `TokenStream` 添加了 `onPartialToolExecutionRequest`等关键事件的监听器。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/1dufTX6O_1755783100163-ab6e1fc7-0844-46ee-b639-52491897e7a6_mianshiya.png)

并且，我将 TokenStream 返回的实时数据统一封装为了 JSON 格式的消息（如 AI 思考、发起工具调用、工具执行结果），便于下游处理：

```bash
{type="ai_response", data="为你生成代码"}

{type="tool_request", index=0, id="call_0", name="writeFile", arguments="流式参数"}
{type="tool_request", index=0, id="call_0", name="writeFile", arguments="流式参数"}
{type="tool_request", index=0, id="call_0", name="writeFile", arguments="流式参数"}
{type="tool_executed", index=0, id="call_0", name="writeFile", arguments="完整参数"}

{type="tool_request", index=1, id="call_1", name="writeFile", arguments="流式参数"}
{type="tool_request", index=1, id="call_1", name="writeFile", arguments="流式参数"}
{type="tool_request", index=1, id="call_1", name="writeFile", arguments="流式参数"}
{type="tool_executed", index=1, id="call_1", name="writeFile", arguments="完整参数"}

{type="ai_response", data="生成代码结束"}
```

3）我编写了一个 `JsonMessageStreamHandler` 处理器。它负责处理 `TokenStream` 封装好的 JSON 消息格式，并根据情况返回给前端或者保存对话记忆到数据库中。

为了简化逻辑，我还做了一个优化。对于同一个工具的多次流式调用请求，只在第一次出现时向前端推送一个 “选择工具” 的提示，后续具体的参数流不再实时透传，而是在工具执行完毕后，将完整的调用信息一次性格式化后推送。

整个 AI 流式处理过程如图：

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/x1tIULGA_202508221402275_mianshiya.png)

通过这个流程，前端就能根据收到的不同类型的消息，实时地向用户展示 AI 的完整工作流，比如 “【选择工具】写入文件”、“【调用工具】写入文件 `App.vue`”，实现了复杂工具调用过程的清晰、实时的流式输出。
