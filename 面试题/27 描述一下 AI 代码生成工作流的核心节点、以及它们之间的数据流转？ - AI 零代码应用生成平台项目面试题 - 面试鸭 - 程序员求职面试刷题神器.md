
# 描述一下 AI 代码生成工作流的核心节点、以及它们之间的数据流转？

代码生成工作流是将一个复杂的网站生成任务分解为一系列有序的、自动化的步骤。这些步骤被定义为工作流中的节点，数据则通过一个共享的状态对象 `WorkflowContext` 在节点之间进行传递。

1）图片收集节点 (`image_collector`)

-   输入：从 `WorkflowContext` 中获取 `originalPrompt`。
-   处理：调用 AI 分析提示词，并发地使用图片搜索、插画搜索、Logo生成等工具，收集网站所需的图片素材。
-   输出：将收集到的图片信息列表 `imageList` 更新回 `WorkflowContext`。

2）提示词增强节点 (`prompt_enhancer`)

-   输入：从 `WorkflowContext` 中获取 `originalPrompt` 和 `imageList`。
-   处理：将图片素材的信息（比如描述和URL）整合成一段格式化的文本，并追加到原始提示词的末尾，形成一个信息更丰富的“增强提示词”。
-   输出：将 `enhancedPrompt` 更新回 `WorkflowContext`。

3）智能路由节点 (`router`)

-   输入：从 `WorkflowContext` 中获取 `originalPrompt`。
-   处理：调用 AI 智能路由服务，根据用户需求的复杂度，决策出最合适的代码生成类型（HTML、多文件或Vue工程）。
-   输出：将 `generationType` 更新回 `WorkflowContext`。

4）代码生成节点 (`code_generator`)

-   输入：从 `WorkflowContext` 中获取 `enhancedPrompt` 和 `generationType`。
-   处理：根据指定的生成类型，调用相应的 AI Service，使用增强后的提示词来生成网站代码。如果质检失败，还会接收 `qualityResult` 进行代码修复。
-   输出：将生成代码所在的目录路径 `generatedCodeDir` 更新回 `WorkflowContext`。

5）代码质量检查节点 (`code_quality_check`)

-   输入：从 `WorkflowContext` 中获取 `generatedCodeDir`。
-   处理：读取生成的所有代码文件，拼接后调用一个专门的 AI 服务来检查是否存在语法错误或结构问题。
-   输出：将检查结果 `qualityResult` (包含是否通过、错误列表、改进建议) 更新回 `WorkflowContext`。

6）项目构建节点 (`project_builder`)

-   输入：从 `WorkflowContext` 中获取 `generatedCodeDir` 和 `generationType`。
-   处理：如果生成的是 Vue 工程，则在此节点执行 `npm install` 和 `npm run build` 命令进行打包。
-   输出：将最终可供部署的目录路径 `buildResultDir` 更新回 `WorkflowContext`。

完整流程如图：

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/yck6fSOR_202508221400439_mianshiya.png)
