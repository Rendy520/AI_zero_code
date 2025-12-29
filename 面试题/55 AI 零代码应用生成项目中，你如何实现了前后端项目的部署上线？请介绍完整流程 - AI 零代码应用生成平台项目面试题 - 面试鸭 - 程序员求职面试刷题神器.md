
# AI 零代码应用生成项目中，你如何实现了前后端项目的部署上线？请介绍完整流程

## 

12073\. AI 零代码应用生成项目中，你如何实现了前后端项目的部署上线？请介绍完整流程 

项目的上线部署是一个系统性工程，涉及环境准备、项目打包、服务部署和 Nginx 配置等多个环节。我采用的是 服务器面板（1Panel） + Nginx 的部署方案，完整流程如下：

1）服务器准备与环境安装

-   服务器初始化：首先准备一台 Linux 服务器，并通过 `1Panel` 等现代化运维面板进行管理，方便后续操作。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/1JiIf5fI_1755834516840-87fe05ce-62f5-4988-85f9-a15e813984eb_mianshiya.png)

-   安装核心依赖：在服务器上安装项目运行所需的基础环境：
    
-   -   数据库：通过面板安装 MySQL 8.x，并创建项目所需的数据库和用户。
    -   缓存：安装 Redis，用于分布式 Session 和业务缓存。
    -   Web 服务器：安装 Nginx (OpenResty)，用于托管前端静态资源和反向代理。
    -   运行环境：JDK 21、Node.js、Chrome 浏览器及中文字体。

2）后端部署

-   生产环境配置：在本地项目中创建 `application-prod.yml` 配置文件，填入服务器上数据库、Redis、对象存储以及各大模型服务的真实连接信息和 API Key。
-   打包：在本地使用 Maven 将 Spring Boot 项目打包成一个可执行的 JAR 文件。
-   上传与运行：将打包好的 JAR 文件上传到服务器的指定目录。然后通过 `nohup java -jar ... &` 命令，以后台进程方式启动后端服务。

3）前端部署

-   生产环境配置：在本地项目中创建 `.env.production` 环境变量文件，将 API 地址配置为相对路径，以便通过 Nginx 代理，解决跨域问题。
-   打包：在本地运行 `npm run build` 命令，将 Vue3 项目打包成 `dist` 目录下的静态文件。
-   上传：将 `dist` 目录下的所有文件上传到服务器上由 Nginx 托管的网站根目录。

4）Nginx 核心配置

-   前端服务配置：
    
-   -   配置 Nginx 监听 80 端口，并将网站根目录 `root` 指向前端 `dist` 文件所在的路径。
    -   配置 `try_files $uri $uri/ /index.html;` 规则，解决 Vue 单页应用刷新页面时出现 404 的问题。所有找不到的路径都会被重定向到 `index.html`，交由前端路由处理。
-   后端反向代理：
    
-   -   配置 `location /api/ { ... }` 规则，将所有以 `/api` 开头的请求，通过 `proxy_pass` 转发到后端服务实际运行的 `http://localhost:8123` 地址。
    -   增加 `proxy_read_timeout` 等配置，延长超时时间，保证 SSE 流式接口不会被意外中断。
-   生成应用部署服务：
    
-   -   额外配置 `location /dist/ { ... }` 规则，使用 `alias` 指令将 `/dist` 路径映射到服务器上存放 AI 生成应用的目录。这样，用户就可以通过 `http://{域名}/dist/{deployKey}` 的地址访问到自己部署的应用。

通过以上流程，我们便完成了整个平台的上线部署，用户可以通过服务器 IP 或域名直接访问我们的 AI 零代码应用生成平台。

![img](https://pic.code-nav.cn/mianshiya/question_picture/markdown/GfhpIZYE_1755834581600-1b9af5c3-9dcd-417c-941a-6af64dbe73f5_mianshiya.png)

作者：![](https://pic.code-nav.cn/user_avatar/1772087337535152129/thumbnail/8stgwMmT-50265-logo.3c8859f8.png)

分享最高赚 ¥64.5

![微信小程序](https://www.mianshiya.com/assets/svg/miniapp.svg)

[![Ray丶丶 的用户头像](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eqhf9NQyvibXhNGyhziaOsh2cB0FTiaLuXaS0FWdzw2lnYq1bHIheOW8Oue3CCZ46f6S4WiaicazuTiaQLQ/132)](https://www.mianshiya.com/user/1821431596438986753)
