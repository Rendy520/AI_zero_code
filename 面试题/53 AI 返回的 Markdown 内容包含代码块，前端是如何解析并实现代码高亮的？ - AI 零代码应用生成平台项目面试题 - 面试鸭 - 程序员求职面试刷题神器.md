
# AI 返回的 Markdown 内容包含代码块，前端是如何解析并实现代码高亮的？

## 

12071\. AI 返回的 Markdown 内容包含代码块，前端是如何解析并实现代码高亮的？ 

为了优化 AI 在对话页面返回内容的展示效果，前端通过组合使用 `markdown-it` 和 `Highlight.js` 这两个库来解析 Markdown 并实现代码高亮。

这个功能的实现被封装在一个可复用组件中，具体解析并高亮代码的流程：

1.  引入库：在组件中引入 `markdown-it` 用于 Markdown 的解析，以及 `Highlight.js` 用于代码的语法高亮。
2.  组件封装：`MarkdownRenderer` 组件接收一个 `content` prop，即后端返回的包含 Markdown 语法的原始字符串。
3.  解析与渲染：组件内部逻辑会使用 `markdown-it` 实例将传入的 `content` 字符串解析成 HTML。在解析过程中，`Highlight.js` 会自动识别并处理 Markdown 中的代码块，为不同语言的代码添加相应的语法高亮样式。
4.  使用组件：在应用对话页面中，直接使用 `<MarkdownRenderer :content="message.content" />` 组件来渲染 AI 的回复。这样，所有关于 Markdown 解析和代码高亮的复杂逻辑都被隔离在 `MarkdownRenderer` 组件内部，让对话页面的代码保持整洁。

作者：![](https://pic.code-nav.cn/user_avatar/1772087337535152129/thumbnail/8stgwMmT-50265-logo.3c8859f8.png)

分享最高赚 ¥64.5

![微信小程序](https://www.mianshiya.com/assets/svg/miniapp.svg)

[![Ray丶丶 的用户头像](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eqhf9NQyvibXhNGyhziaOsh2cB0FTiaLuXaS0FWdzw2lnYq1bHIheOW8Oue3CCZ46f6S4WiaicazuTiaQLQ/132)](https://www.mianshiya.com/user/1821431596438986753)

![面试鸭](https://www.mianshiya.com/_next/image?url=%2Flogo.png&w=96&q=75)

##### 面试鸭

##### 友情链接

##### 联系我们

##### 关注我们

![微信公众号 | 面试鸭](https://www.mianshiya.com/_next/image?url=%2Fassets%2Fwxmpqrcode.jpg&w=256&q=75)扫码关注  
面试鸭公众号
