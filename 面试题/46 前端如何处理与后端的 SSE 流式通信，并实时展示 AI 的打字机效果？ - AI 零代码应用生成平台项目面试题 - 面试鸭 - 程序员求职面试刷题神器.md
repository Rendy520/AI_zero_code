
# 前端如何处理与后端的 SSE 流式通信，并实时展示 AI 的打字机效果？

前端主要通过浏览器内置的 `EventSource` API 来实现与后端的 SSE 流式通信和实时打字机效果。

实现细节：

1）建立连接：前端通过 `new EventSource(url, { withCredentials: true })` 创建一个实例，连接到后端的 SSE 接口。URL中会携带 `appId` 和用户消息等参数，`withCredentials: true` 确保请求会携带 Cookie 以供后端进行用户认证。

2）监听消息：通过为 `EventSource` 实例设置 `onmessage` 事件监听器来接收服务器推送的数据。后端为了防止空格丢失，会将每个数据块包装在一个JSON对象中（如 `{"d": "chunk"}`），前端在 `onmessage` 回调中解析这个JSON以获取真实的数据片段。

3）实现打字机效果：前端在组件内部维护一个字符串变量 `fullContent`，每当 `onmessage` 事件触发时，就将新接收到的数据块追加到 `fullContent` 变量的末尾。然后，将这个不断增长的 `fullContent` 赋值给Vue组件中用于渲染AI回复的响应式变量。通过这种方式，UI会随着数据的接收而持续更新，实现打字机效果。

4）处理流结束：后端在数据流完全结束后，会发送一个名为 `done` 的自定义事件。前端通过 `eventSource.addEventListener('done', ...)` 来监听此事件，一旦接收到，就调用 `eventSource.close()` 方法关闭连接，停止接收数据，并触发网站预览的更新。

5）错误处理：前端通过 `onerror` 事件监听器来处理网络中断等连接错误。我还专门监听一个自定义的 `business-error` 事件，用于处理后端在流建立前就抛出的业务异常，向用户展示准确的错误信息。
