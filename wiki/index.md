# Wiki Index

> 最后更新：2026-09-12 | 页面总数：22 | 素材总数：13

本索引是 LLM 的内容导向导航入口。每次 ingest / query / lint / publish 操作后都会更新。

## Entities
- [[Obsidian]] - 基于本地 markdown 的知识管理工具 (sources: 1)
- [[Dify]] - 低代码 AI 应用开发平台，支持 RAG/Agentic workflow (sources: 1)
- [[Milvus]] - 生产首选的分布式向量数据库 (sources: 1)
- [[Ollama]] - 本地大模型推理运行时，agent 推理底座 (sources: 1)
- [[ComfyUI]] - 本地可视化 AI 生成工作流工具 (sources: 1)
- [[多租户架构]] - 主库+多租户库，动态数据源按请求路由 (sources: 1)

## Concepts
- [[RAG]] - 检索增强生成，LLM + 文档交互的主流方式 (sources: 2)
- [[双链网络]] - `[[双链]]` 交叉引用的知识网络 (sources: 1)
- [[AI应用开发]] - 以调 LLM 为核心的工程化应用开发方向 (sources: 1)
- [[Agent]] - AI 智能体：调工具、能续跑的自主执行体 (sources: 2)
- [[AI智能体分权治理]] - agent 按角色分层与变更治理闭环 (sources: 1)
- [[显示与存储单位分离]] - 展示/存储单位解耦，避免精度丢失 (sources: 1)
- [[本地优先AI创作]] - 本地 ComfyUI 管线 + 硬件预算内调度 (sources: 1)
- [[自愈闭环设计]] - 无 docker.sock 的自愈改码/重启通道 (sources: 2)
- [[意图路由与服务端兜底]] - 正则+粘性路由，LLM 失败时服务端确定性补卡 (sources: 1)

## Summaries
- [[Obsidian-LLM-Wiki实操指南-摘要]] - LLM Wiki 模式完整实操阐述 (2026-09-12)
- [[RAG检索增强生成学习笔记]] - RAG 全链路与 Agent 记忆层五层栈 (2026-09-12)
- [[为什么转岗AI大模型应用]] - AI 应用层转岗定位与赛道时机 (2026-09-12)
- [[GitHub-AI周报2026-08-09]] - 技能层占增量 50%，本地推理双雄 (2026-09-12)
- [[个人协作偏好与工程方法论-摘要]] - 本地记忆脱敏提炼的协作纪律与工程实践 (2026-09-12)

## Comparisons
- [[LLM-Wiki-vs-RAG]] - LLM Wiki 模式与传统 RAG 的对比分析 (sources: 1)

## Synthesis
- [[个人AI工程方法论]] - 三重验证、最小 diff、证据优先的工程实践沉淀
- [[AI智能体工程方法论]] - 部署铁律、token 预算、协作契约等智能体工程原则

---

## 使用提示

- LLM 通过本文件快速定位相关页面
- 起步期（<300 页）：单文件 index.md 完全够用
- 中期（300-1000 页）：可拆分为 `index-entities.md`、`index-concepts.md` 等分类子索引
- 后期（1000+ 页）：引入全文检索/语义搜索，index 降级为辅助导航