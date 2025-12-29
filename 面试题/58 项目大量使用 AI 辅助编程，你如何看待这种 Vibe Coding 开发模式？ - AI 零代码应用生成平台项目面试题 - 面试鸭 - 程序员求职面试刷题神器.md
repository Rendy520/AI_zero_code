
# 项目大量使用 AI 辅助编程，你如何看待这种 Vibe Coding 开发模式？

## 

12076\. 项目大量使用 AI 辅助编程，你如何看待这种 Vibe Coding 开发模式？ 

我认为 Vibe Coding 是一种高效的开发范式，开发者从传统的代码编写者转变为架构设计师和 AI 指挥官。在这个项目中，我充分使用了这种模式：

1.  对于模式化的、重复性高的工作，Vibe Coding 的效率极高。比如，在项目中开发用户管理模块的前后端 CRUD 代码、创建包含表单和校验规则的前端页面时，通过向 AI 提供清晰的需求和上下文，可以在几分钟内生成原本需要数小时手动编写的代码。
2.  在项目初始化阶段，全局的前端布局，包括头部、底部和路由视图等，都是通过一个详细的 Prompt 让 AI 生成的。这样项目的初始框架搭建速度非常快，让我可以迅速进入核心业务逻辑的开发。
3.  开发者无需再纠结于具体的语法细节或API用法，而是将更多精力投入到更高层次的思考上，比如系统架构设计、业务流程梳理以及如何 crafting 一个能够让 AI 准确理解意图的 Prompt 。

不过这种模式也有一些关键点和难点：

1.  对Prompt质量要求极高：AI的输出质量直接取决于输入的Prompt质量。一个模糊的指令会导致 AI 生成无用甚至错误的代码。一个高质量的 Prompt 需要包含清晰的需求、角色定义、上下文，甚至是具体的输出格式约束。
2.  要求开发者具备很强的甄别和调试能力：AI生成的代码并非完美，有时会包含逻辑错误或不符合最佳实践。比如，项目中曾遇到 AI 生成的代码导致前端 ID 精度丢失、SSE 连接地址错误等问题。这要求开发者必须能快速读懂、评审并调试 AI 生成的代码，不能盲目信任。
3.  存在非确定性：同一个Prompt，AI 每次的输出可能不完全相同，这就导致代码的稳定性和一致性比较差。因此，在接受AI生成的代码后，一定要进行版本控制和充分测试。

Vibe Coding 模式是开发领域的一大进步，但它对开发者的抽象思维、需求表达能力和代码审查能力提出了更高的要求。

作者：![](https://pic.code-nav.cn/user_avatar/1772087337535152129/thumbnail/8stgwMmT-50265-logo.3c8859f8.png)

分享最高赚 ¥64.5

![微信小程序](https://www.mianshiya.com/assets/svg/miniapp.svg)

[![Ray丶丶 的用户头像](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eqhf9NQyvibXhNGyhziaOsh2cB0FTiaLuXaS0FWdzw2lnYq1bHIheOW8Oue3CCZ46f6S4WiaicazuTiaQLQ/132)](https://www.mianshiya.com/user/1821431596438986753)

-   [![axing 的用户头像](https://pic.code-nav.cn/user_avatar/1962348711956758529/thumbnail/cERF6MkiAwhQKbNq.jpeg)](https://www.mianshiya.com/user/1962348711956758529)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel3.png&w=96&q=75)
    
    我认为 Vibe Coding 是一种高效的开发范式，开发者从传统的代码编写者转变为架构设计师和 AI 指挥官。在这个项目中，我充分使用了这种模式：
    
    比如一些模式化、重复性的任务，像 CRUD 接口、前端表单页
    

![面试鸭](https://www.mianshiya.com/_next/image?url=%2Flogo.png&w=96&q=75)

##### 面试鸭

##### 友情链接

##### 联系我们

##### 关注我们

![微信公众号 | 面试鸭](https://www.mianshiya.com/_next/image?url=%2Fassets%2Fwxmpqrcode.jpg&w=256&q=75)扫码关注  
面试鸭公众号
