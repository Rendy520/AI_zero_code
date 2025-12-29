
# AI 零代码应用生成项目中，哪些部分被抽象成了可复用的 Vue 组件？举例说明你开发一个组件时的主要思路。

## 

12069\. AI 零代码应用生成项目中，哪些部分被抽象成了可复用的 Vue 组件？举例说明你开发一个组件时的主要思路。 

项目中，为了提升代码的可维护性和复用性，一些独立的功能模块被抽象成了可复用的 Vue 组件：

-   AppCard：用于在主页列表中展示单个应用信息的卡片。
-   AppDetailModal：用于在对话页或管理页点击查看应用详情时弹出的模态框。
-   DeploySuccessModal：用于在应用部署成功后，展示部署成功信息和访问链接的弹窗。
-   MarkdownRenderer：一个专门用于解析和渲染 AI 返回的 Markdown 文本，并支持代码高亮的组件。

以 `AppDetailModal` 为例，开发这个组件时的主要思路是遵循受控组件和单一职责原则：

1.  明确职责：该组件的唯一职责就是根据传入的数据展示应用详情，自身不负责获取数据或执行业务逻辑。
2.  通过 Props 接收数据：组件通过 props 接收父组件传递来的数据，比如 `app` 对象用于展示应用信息，`showActions` 布尔值用于决定是否显示“修改”、“删除”等操作按钮。这使得组件的行为可以由外部灵活控制。
3.  通过 Emits 通知父组件：当用户在弹窗内点击修改或删除按钮时，组件会通过 `$emit` 触发相应的事件。它只负责通知父组件用户的意图，具体的业务逻辑则由父组件来完成，实现逻辑的解耦。
4.  通过 v-model 控制显隐：组件的显示和隐藏状态通过 `v-model:open` 与父组件的状态进行双向绑定，使父组件可以完全控制弹窗的生命周期。

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
