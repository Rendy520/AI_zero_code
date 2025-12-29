
# AI 零代码应用生成项目的对话历史表的索引是如何设计的？

`chat_history` 表的索引设计是为了高效地支持对话历史的查询，特别是游标分页。

```lua
-- 对话历史表
create table chat_history
(
    -- ... 其他字段
    INDEX idx_appId (appId),                       -- 提升基于应用的查询性能
    INDEX idx_createTime (createTime),             -- 提升基于时间的查询性能
    INDEX idx_appId_createTime (appId, createTime) -- 游标查询核心索引
) comment '对话历史' collate = utf8mb4_unicode_ci;
```

我主要设计了以下几个索引：

1）`idx_appId`：在 `appId` 字段上建立索引。这个索引主要用于快速筛选出特定应用的所有对话历史，比如在删除应用时需要关联删除其所有对话记录的场景。

2）`idx_createTime`：在 `createTime` 字段上建立索引。这个索引主要服务于管理员查询所有对话历史并按时间排序的需求。

3）`idx_appId_createTime`：这是最核心的一个索引，它同时包含了 `appId` 和 `createTime` 两个字段。这个复合索引是专门为游标分页查询优化的。当执行类似 `SELECT * FROM chat_history WHERE appId = ? AND createTime < ? ORDER BY createTime DESC LIMIT 10` 的查询时，数据库可以利用这个索引：

-   首先快速定位到特定 `appId` 的数据范围。
-   然后在 `appId` 相同的数据内部，继续利用索引快速找到 `createTime` 小于游标值的位置。
-   最后，由于索引本身是按 `createTime` 排序的，数据库可以直接从该位置顺序读取所需的10条记录，避免了全表扫描和文件排序，提升查询效率。
