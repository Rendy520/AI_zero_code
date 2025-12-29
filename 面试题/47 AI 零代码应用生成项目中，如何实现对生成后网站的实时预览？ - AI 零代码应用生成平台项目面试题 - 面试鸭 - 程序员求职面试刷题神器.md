
# AI 零代码应用生成项目中，如何实现对生成后网站的实时预览？

## 

12065\. AI 零代码应用生成项目中，如何实现对生成后网站的实时预览？ 

项目通过在前端页面内嵌 `iframe` 并结合后端静态文件服务的方式，实现对生成后网站的实时预览。

1.  在应用对话页面的右侧区域，放置一个 `iframe` 元素，作为承载生成后网站的容器。
2.  后端提供了一个专门的静态资源接口，该接口可以根据传入的路径，从服务器的特定文件目录中读取并返回对应的静态文件。后端在生成代码时，会根据应用的 `codeGenType` 和 `appId` 将文件保存在一个唯一的子目录下。
3.  当前端通过 SSE 接收完所有AI生成的代码片段后，后端会完成文件的保存。此时，前端会根据当前应用的 `appId` 和 `codeGenType` 构造一个指向后端静态资源服务的URL。对于Vue工程项目，URL会指向构建产物 `dist` 目录下的 `index.html`。
4.  最后，前端将这个构造好的 `previewUrl` 赋值给 `iframe` 的 `src` 属性。`iframe` 接收到新的 `src` 后，会向该URL发起请求，后端静态资源接口则返回 `index.html` 及其引用的资源，最终在 `iframe` 中渲染出完整的网站预览效果。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/OrFoDX5u_1752762717001-aae8ab1f-7965-43b5-8fa7-3f8041530c90_mianshiya.png)

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
