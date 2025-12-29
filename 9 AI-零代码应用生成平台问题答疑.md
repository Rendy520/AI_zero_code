---
source: https://www.codefather.cn/course/1948291549923344386/section/1971140538623168514
---

# AI 零代码应用生成平台问题答疑 -

> ## Excerpt
> Bean named 'XXXXX' is expected to be of type 'dev.langchain4j.model.chat.XXXX' but was actually。编程导航教程分享，做您编程学习路上的导航员。

---
### Bean named 'XXXXX' is expected to be of type 'dev.langchain4j.model.chat.XXXX' but was actually of type 'dev.langchain4j.model.openai.XXXX'.

出现这个报错是因为 spring-boot-devtools ，解决办法就把下面这个依赖在 pom.xml 删除

```java
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

### 后端不停覆盖已经生成的文件，一直在调用 AI 生成

核心问题是因为 AI 忘记之前输出过的内容，具体解决办法可以参考第十一期教程：[https://www.codefather.cn/course/1948291549923344386/section/1955850416717950978](https://www.codefather.cn/course/1948291549923344386/section/1955850416717950978)

![](https://pic.code-nav.cn/course_picture/1608440217629360130/K3Crh606ZaggIzFV.webp)

这里的解决方法使用的是调大对⁠话记忆：     ⁠                           

修改 AiCodeGeneratorServiceFacto⁠ry#createAiCodeGener⁠atorService 对应最大 Message 数量                                

修改这个变成 maxMessages(100) 即可解决，一般生成时间小于 15min 大于可以直接停止后端服务

```java
private AiCodeGeneratorService createAiCodeGeneratorService(long appId, CodeGenTypeEnum codeGenType) {
        log.info("为 appId: {} 创建新的 AI 服务实例", appId);
        // 根据 appId 构建独立的对话记忆
        MessageWindowChatMemory chatMemory = MessageWindowChatMemory
                .builder()
                .id(appId)
                .chatMemoryStore(redisChatMemoryStore)
                // 关键修改的位置
                .maxMessages(100)
                .build();
        // 从数据库中加载对话历史到记忆中
        chatHistoryService.loadChatHistoryToMemory(appId, chatMemory, 20);
        return switch (codeGenType) {
            // Vue 项目生成，使用工具调用和推理模型
            case VUE_PROJECT -> {
                // 使用多例模式的 StreamingChatModel 解决并发问题
                StreamingChatModel reasoningStreamingChatModel = SpringContextUtil.getBean("reasoningStreamingChatModelPrototype", StreamingChatModel.class);
                yield AiServices.builder(AiCodeGeneratorService.class)
                        .chatModel(chatModel)
                        .streamingChatModel(reasoningStreamingChatModel)
                        .chatMemoryProvider(memoryId -> chatMemory)
                        .tools(toolManager.getAllTools())
                        // 处理工具调用幻觉问题
                        .hallucinatedToolNameStrategy(toolExecutionRequest ->
                                ToolExecutionResultMessage.from(toolExecutionRequest,
                                        "Error: there is no tool called " + toolExecutionRequest.name())
                        )
                        .maxSequentialToolsInvocations(20)  // 最多连续调用 20 次工具
                        .inputGuardrails(new PromptSafetyInputGuardrail()) // 添加输入护轨
//                        .outputGuardrails(new RetryOutputGuardrail()) // 添加输出护轨，为了流式输出，这里不使用
                        .build();
            }
            // HTML 和 多文件生成，使用流式对话模型
            case HTML, MULTI_FILE -> {
                // 使用多例模式的 StreamingChatModel 解决并发问题
                StreamingChatModel openAiStreamingChatModel = SpringContextUtil.getBean("streamingChatModelPrototype", StreamingChatModel.class);
                yield AiServices.builder(AiCodeGeneratorService.class)
                        .chatModel(chatModel)
                        .streamingChatModel(openAiStreamingChatModel)
                        .chatMemory(chatMemory)
                        .inputGuardrails(new PromptSafetyInputGuardrail()) // 添加输入护轨
//                        .outputGuardrails(new RetryOutputGuardrail()) // 添加输出护轨，为了流式输出，这里不使用
                        .build();
            }
            default ->
                    throw new BusinessException(ErrorCode.SYSTEM_ERROR, "不支持的代码生成类型: " + codeGenType.getValue());
        };
    }
```

### Redis 报错 redis.clients.jedis.exceptions.JedisDataException: ERR unknown command 'JSON.GET', with args beginning with: 00000

出现这个报错的原因是 Redis 没有 JSON 模块导致的，参考官方文档的说明 [https://github.com/RedisJSON/RedisJSON](https://github.com/RedisJSON/RedisJSON)

![](https://pic.code-nav.cn/course_picture/1608440217629360130/ypAsaEaseXqBHd3h.webp)

解决办法

1、安装 Redis8.0 ⁠以后版本     ⁠                           

2、仍然保持当前 Redis 版本手动安装 Redis JSON 模块，参考这个 issue [https://github.com/RedisJSON/RedisJSON/issues/214#issuecomment-1145465056](https://github.com/RedisJSON/RedisJSON/issues/214#issuecomment-1145465056)


