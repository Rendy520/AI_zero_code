
# 在实现 AI 的对话记忆时，为什么选择 Redis 进行持久化？

## 

12038\. 在实现 AI 的对话记忆时，为什么选择 Redis 进行持久化？ 

在项目中，我选择使用 Redis 进行对话记忆持久化，主要是考虑以下几点：

-   性能优势：Redis 作为一个内存数据库，其读写性能远高于传统的 MySQL 数据库。对于需要频繁读写的对话记忆场景，使用 Redis 可以提供更快的响应速度。
-   避免内存溢出：如果直接使用内存来存储会话记忆，当应用数量和对话轮次增多时，会占用大量 JVM 内存，容易导致内存溢出（OOM）问题。而 Redis 作为独立的持久化服务，可以有效避免这个问题。
-   数据一致性与重启恢复：与内存记忆相比，Redis 可以在服务重启后依然保留对话状态，保证了数据的持久性。
-   与业务数据隔离：虽然数据库中也有对话记忆，但是 `chat_history` 表存储的是完整的、用于长期追溯的对话记录，包含了其他业务字段。而 AI 的对话记忆只需要纯粹的对话上下文，将两者分开存储，可以避免直接将业务数据表暴露给 LangChain4j 的记忆组件管理，使架构更清晰。

作者：![](https://pic.code-nav.cn/user_avatar/1772087337535152129/thumbnail/8stgwMmT-50265-logo.3c8859f8.png)

分享最高赚 ¥64.5

![微信小程序](https://www.mianshiya.com/assets/svg/miniapp.svg)

[![Ray丶丶 的用户头像](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eqhf9NQyvibXhNGyhziaOsh2cB0FTiaLuXaS0FWdzw2lnYq1bHIheOW8Oue3CCZ46f6S4WiaicazuTiaQLQ/132)](https://www.mianshiya.com/user/1821431596438986753)

-   [![hjx 的用户头像](https://pic.code-nav.cn/user_avatar/1951107463568228354/thumbnail/lrGd3Onum59E2Twh.png)](https://www.mianshiya.com/user/1951107463568228354)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel3.png&w=96&q=75)
    
    我来回答一下这个问题。 在实现 AI 的对话记忆时，我选择使用 Redis 进行持久化，主要是考虑以下几个方面：
    
    1.  性能优势 Redis 作为一个内存数据库，其读写性能远高于传统的 MySQL 数据库。对于需要频繁读写的对话记忆场景，使用 Redis 可以提供更快的响应速度。比如，每次对话都需要
    
-   [![心在路上 的用户头像](https://thirdwx.qlogo.cn/mmopen/vi_32/UIrPVwK16j7txV04H47jVf1cEom01SWNtogra3v7NyF5SHtHkBXfvbTl7EneMNjO833lvoDvsibjyYUTBxt1gctFBhHlpIhKiaicHQ1cyPQXUs/132)](https://www.mianshiya.com/user/1817038876463783937)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel1.png&w=96&q=75)
    
    在实现 AI 的对话记忆时，为什么选择 Redis 进行持久化?
    
    1.  redis 基于内存，读写非常快，性能比较好
    2.  避免内存溢出，langchain4j 默认实现是基于内存的，如果对话轮次比较多的情况下，会产生OOM
    3.  数据一致性和重启恢复：与内存记忆相比，redis 在服务重启之后也会
    
-   [![yyc 的用户头像](https://pic.code-nav.cn/user_avatar/1916159505222971394/thumbnail/PSM9Rzkngc9fvC59.jpg)](https://www.mianshiya.com/user/1916159505222971394)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel2.png&w=96&q=75)
    
    在实现 AI 的对话记忆时，为什么选择 Redis 进行持久化?
    
    选择 Redis 进行对话记忆持久化基于以下考虑：
    
    1.  **性能优势**：Redis 作为内存数据库，读写性能远高于 MySQL，满足对话记忆频繁读写的响应速度需求。
        
    2.  **避免内存溢出**：直接使用内存存储会话记忆，应用
        
    
-   [![axing 的用户头像](https://pic.code-nav.cn/user_avatar/1962348711956758529/thumbnail/cERF6MkiAwhQKbNq.jpeg)](https://www.mianshiya.com/user/1962348711956758529)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel3.png&w=96&q=75)
    
    在实现 **AI 对话记忆** 时，我选择用 **Redis 来做持久化**，主要考虑到以下几点：
    
    1.  **性能优势**：Redis 是内存数据库，读写速度远快于 MySQL，非常适合频繁访问的对话记忆场景。
    2.  \*\*避免
    
-   [![给我一个lemon 的用户头像](https://pic.code-nav.cn/user_avatar/1834514345856004098/thumbnail/UU1McX5z3gFPoTin.jpg)](https://www.mianshiya.com/user/1834514345856004098)
    
    #### 
    
    ![等级图标](https://www.mianshiya.com/_next/image?url=%2Fassets%2Flevel%2Flevel3.png&w=96&q=75)
    
    ## 在实现AI的对话记忆时，为什么选择Redis进行持久化？
    
    -   性能优势：Redis作为一个内存数据库，读写速度非常快。对于频繁需要读写的对话记忆场景，使用Redis可以提供更快的响应速度
    -   避免内存溢出：如果直接使用内存来存储会话记忆，当应用数量和对话轮次增多时，会占用大量JVM内存，容易导
