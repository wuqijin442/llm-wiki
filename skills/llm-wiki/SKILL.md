# LLM Wiki Skill

> 该 Skill 是 LLM 维护本知识库时按需加载的工作流规则。
> 它是 `AGENTS.md`（Schema）的补充细则。主规范以 `AGENTS.md` 为准。

## 定位

在 LLM Wiki 模式中：Obsidian 是"IDE"，LLM 是"程序员"，wiki 是"代码库"。
人类在 Obsidian 中浏览和审阅，LLM 负责编写和维护。

## 触发时机

当用户下达以下指令时加载本 Skill：
- `ingest <素材>` — 消化一篇素材
- `query <问题>` / `查一下 <问题>` — 基于已编译知识回答
- `lint` / `做一次健康检查` — 定期维护检查
- `publish <主题> 为 <类型>` — 产出成品
- `refresh <主题>` — 联网重校验

## 执行前置

每次操作前：
1. 读取 `AGENTS.md`（根目录）确认 Schema
2. 读取 `wiki/index.md` 了解当前索引
3. 读取 `wiki/log.md` 了解最近操作

## 执行后置

每次操作后：
1. 更新 `wiki/index.md`（新增/变更页面）
2. 更新 `wiki/log.md`（追加操作记录）

## 页面质量检查清单

创建或更新任一 wiki 页面时自检：
- [ ] 包含完整 frontmatter（title/aliases/tags/category/created/updated/sources/description）
- [ ] `category` 取值合法
- [ ] 至少与一个其他页面建立 `[[双链]]`
- [ ] `sources` 指向真实存在的 raw 素材
- [ ] `description` 不超过 100 字
- [ ] `updated` 已刷新为当前日期