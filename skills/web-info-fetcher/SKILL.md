---
name: web-info-fetcher
description: 使用 cursor-ide-browser MCP 打开指定网页、按指令获取信息并存储到 info 目录。适用于自动化抓取文档、帮助页、知识库等网页内容。主会话调用 web-info-fetcher subagent 时应引用本 skill。
---

# 网页信息获取

按本 skill 执行网页信息抓取，确保操作规范、结束条件明确、存储结构清晰。

---

## 1. 工具与能力

使用 **cursor-ide-browser** MCP 的以下工具：

| 工具 | 用途 |
|------|------|
| browser_navigate | 打开指定 URL |
| browser_wait_for | 等待页面加载（text/textGone/time） |
| browser_snapshot | 获取页面可访问性快照（非截图） |
| browser_click | 点击元素（需 ref） |
| browser_scroll | 滚动页面（direction/deltaY/scrollIntoView） |
| browser_lock / browser_unlock | 操作前锁定、完成后解锁 |

**SPA 页面**：SAP Help Portal、文档站等多为 JS 渲染，需等待 "Loading…" 类文本消失后再 snapshot。

---

## 2. 工作流程

### 2.1 标准流程

1. **打开页面**：browser_navigate(url)
2. **等待加载**：browser_wait_for(textGone: "Loading Topic…" 或 time: 5)
3. **获取结构**：browser_snapshot()，若输出到文件则读取提取正文
4. **按指令操作**：点击目录、切换章节、滚动等
5. **汇总与存储**：按 [storage.md](storage.md) 写入 info 目录
6. **解锁**：browser_unlock()

### 2.2 多章节抓取

- 从 snapshot 中找目录/导航的 `ref`
- browser_click(ref) 切换章节
- 每次切换后等待 2–3 秒再 snapshot
- 逐章提取内容，合并为结构化总结

---

## 3. 结束条件（强制）

**必须满足以下任一条件才能结束任务**：

| 类型 | 说明 | 示例 |
|------|------|------|
| 数量限制 | 用户明确指定抓取数量 | 「抓取 2 个章节」「最多 5 页」 |
| 内容匹配 | 用户指定目标内容 | 「抓取与 MRP 相关的章节」 |
| 结构边界 | 页面结构天然边界 | 目录已到底、无更多链接 |
| 时间/步数上限 | 防止无限循环 | 默认最多 10 次章节切换 |

**若用户未给出结束条件**：

1. **立即停止执行**，不开始抓取
2. **返回主会话**，提示用户补充：
   - 「请明确结束条件，例如：抓取 N 个章节、抓取到某关键词、或指定最大步数」
3. 主会话将提示转达给用户，待用户确认后再重新调度

---

## 4. 输出与存储

- **返回主会话**：结构化摘要（来源、章节列表、核心要点、存储路径）
- **写入 info 目录**：按 [storage.md](storage.md) 的目录与命名规则

详见 [storage.md](storage.md)。
