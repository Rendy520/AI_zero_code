
# 描述一下 AI 大模型生成 Vue 工程项目的过程？

生成 Vue 工程项目比生成单个 HTML 文件要复杂得多，因为它涉及到多个文件和目录的创建。为此，我设计了一套基于 AI 工具调用的自动化流程。

分为以下几个关键步骤：

1）AI 规划与工具调用：当用户请求生成一个 Vue 工程时，系统会使用一个专门为 Vue 项目设计的、包含详细约束的系统提示词来调用大模型。这个提示词要求 AI 必须通过调用我提供的 `writeFile` 工具来逐个创建项目文件。AI 会首先在内部规划出需要创建的文件列表，如 `package.json`、`vite.config.js`、`src/main.js`、`src/App.vue` 等，然后按顺序、多次调用 `writeFile` 工具。

2）后端文件写入：我在后端实现了一个 `FileWriteTool` 工具类。当 AI 调用此工具时，LangChain4j 框架会捕获该请求并执行 `writeFile` 方法。该方法会根据 AI 提供的相对路径和文件内容，在一个以 `vue_project_{appId}` 命名的专属目录下创建或覆写相应的文件，保证每个应用的文件隔离。

3）流式过程反馈：为了让用户能实时看到生成进度，整个工具调用过程是流式返回给前端的。我设计了一套自定义的 JSON 消息格式，用来区分不同的事件类型，如 `AI_RESPONSE`、`TOOL_REQUEST`和 `TOOL_EXECUTED`。前端接收到这些消息后，会解析并展示给用户。

4）自动化构建：当 AI 完成所有文件的写入后，`JsonMessageStreamHandler` 会在流结束时异步触发 `VueProjectBuilder` 服务。该服务会自动在服务器上对应的项目目录中执行 `npm install` 和 `npm run build` 命令，安装依赖并打包构建项目。

5）预览与部署：构建成功后会生成一个 `dist` 目录。前端的预览 `iframe` 和最终的应用部署，都会指向这个 `dist` 目录，让用户能够看到最终可运行的应用效果。

整个过程如图：

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/9NWQ1VIM_202508221359572_mianshiya.png)
