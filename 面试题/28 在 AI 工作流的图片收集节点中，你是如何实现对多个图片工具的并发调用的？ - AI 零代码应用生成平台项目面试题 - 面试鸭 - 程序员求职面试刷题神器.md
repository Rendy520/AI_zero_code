
# 在 AI 工作流的图片收集节点中，你是如何实现对多个图片工具的并发调用的？

在图片收集节点中，为了最大化效率，避免串行调用多个耗时的图片 API，我采用了基于 Java `CompletableFuture` 的并发执行方案，内容图片搜索、插画搜索、架构图生成和 Logo 生成这 4 个任务可以同时进行，显著缩短了图片素材的准备时间。

实现流程如下：

1）节点首先会调用一个 **图片收集规划 AI 服务**。这个服务会分析用户的原始提示词，并返回一个结构化的 `ImageCollectionPlan` 对象。这个计划对象中详细列出了需要执行的各类图片任务及其参数，比如需要搜索哪些关键词的内容图片、需要生成什么描述的 Logo 等。

2）拿到这个计划后，程序会遍历计划中的各项任务，并为每个任务创建一个 `CompletableFuture.supplyAsync()` 异步任务。比如，遍历 `contentImageTasks` 列表，为每个任务都提交一个调用 `imageSearchTool.searchContentImages()` 的异步请求到线程池中。所有这些 `CompletableFuture` 对象被收集到一个列表中。

3）使用 `CompletableFuture.allOf(futures.toArray(new CompletableFuture[0])).join()` 来阻塞等待任务列表中的所有异步任务执行完毕。

4）所有任务都完成后，遍历 `CompletableFuture` 列表，通过 `get()` 方法获取每个任务返回的图片资源列表，并将它们全部汇总到一个最终的 `collectedImages` 列表中。

5）最后，将这个包含了所有并发收集到的图片的 `collectedImages` 列表更新到工作流的 `WorkflowContext` 中，供后续节点使用。

通过这种模式，将原本需要依次等待多个网络请求和 AI 计算的串行流程，变成了一个并行的流程，实测这种优化将图片收集的耗时缩短了 70 多秒。

当然，我还实践过利用 LangGraph4j 的并发分支（和子图）能力实现并发。只需要把每个图片收集工具定义成工作节点（或子图），然后配置线程池，就可以并发执行这些工具，最后再统一汇总结果。

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/83l3TckC_202508221401070_mianshiya.png)
