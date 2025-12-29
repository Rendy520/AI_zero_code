
# AI 零代码应用生成项目中，如何实现不同应用之间的对话记忆隔离？

为了确保每个应用的对话历史和上下文是完全独立的，我采用了 **基于应用 ID** 的 AI Service 实例隔离策略。

具体实现方式如下：

1）我并没有在 Spring 容器中注册一个单例的 `AiCodeGeneratorService`，而是通过一个 `AiCodeGeneratorServiceFactory` 工厂类来动态创建服务实例。

2）在工厂的 `createAiCodeGeneratorService` 方法中，会根据传入的 `appId` 来创建一个专属的 `MessageWindowChatMemory` 实例。这个记忆实例的 `id` 被设置为 `appId`，并且它会使用配置好的 `RedisChatMemoryStore` 进行持久化。

3）由于每个 `MessageWindowChatMemory` 都有一个唯一的 `id`，LangChain4j 在 Redis 中存储数据时，会生成类似 `langchain4j:chat_memory:{appId}` 这样的 Key，在物理上保证了不同应用对话记忆的隔离。

通过这种方式，每次与特定应用进行对话时，都会获取到一个绑定了该应用专属对话记忆的 AI Service 实例，实现了彻底的对话隔离。
