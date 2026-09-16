# raw/articles — 网页剪藏落地目录

这里是**唯一**给浏览器剪藏工具写入的目录。素材进到这里后，由 LLM 按 `AGENTS.md` 执行 ingest，编译进 `wiki/`。

## 一、电脑端：Obsidian Web Clipper（推荐）

1. 浏览器安装扩展 **Obsidian Web Clipper**（Chrome / Edge / Firefox）。
2. 在扩展设置里指定：
   - **Vault**：`llmWiki`
   - **默认目录 / Note location**：`raw/articles`
   - **模板**：默认 Article 模板即可，建议保留 `{{title}}`、`{{author}}`、`{{url}}`、`{{date}}`
3. 看到好文章 → 点扩展图标 → **Add to Obsidian**，文章即以 Markdown 落到本目录。

**命名规范**（便于排序与追溯）：

```
YYYY-MM-DD-标题.md
```

例：`2026-09-12-LLM-Wiki自生长知识库.md`

## 二、手机端 / 碎片灵感

- **ima 知识管家**、WorkBuddy 小程序、微信收藏 → 先落到 `raw/00-Inbox/`，再统一 ingest。
- 聊天记录、会议纪要等私人资料**留在本机**，是否同步、同步到哪由你决定（Obsidian 本地优先）。

## 三、落地后做什么

在本目录新增素材后，对 Agent 说：

```text
请读取 raw/ 中新增的素材（可用 wiki/growth.md 的「未消化素材」定位），
严格按照 AGENTS.md 的规则执行增量 ingest。
```

> 也可以直接说 `ingest raw/articles/2026-09-12-xxx.md` 指定单篇。

## 四、注意

- `raw/` 是**证据层，LLM 只读**：绝不修改、删除、重命名原始素材。
- 剪藏后不要手工改内容（改了就失去"原始证据"的意义）。有想法请写在 `raw/01-Daily/` 的每日笔记里。
