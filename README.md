# AIcoding 自主巡航系统

> 说需求 → 自动出完整可交付项目。零人工干预。

**AIcoding** 是一套让 AI 从任务描述直接产出企业级项目的工作流引擎：

```
Plan → Todo → 并行探索 → Agent 实施 → 严格验证 → 企业级交付
```

你只提供一句话需求，AI 完成从 0 到 1 的所有工作，交付一个可立即 clone、运行、部署的生产级项目。

---

## 核心原则

| 原则 | 说明 |
|------|------|
| **零干预** | 交付前绝不要求澄清/批准（Plan 模式最多 1-2 个关键问题） |
| **并行探索** | 多个子 Agent 同时调研技术选型、架构、最佳实践 |
| **严格验证** | 每次编辑跑 linter，全量测试，Canvas 评审架构 |
| **企业级标准** | 80%+ 测试覆盖、CI/CD、Docker、监控、完整 README |
| **无废话交付** | 不解释过程，直接给可运行产物 |

---

## 目录结构

```
aicoding/
├── skill/                    # 核心 Skill 定义（给 AI Agent 用）
│   ├── SKILL.md              # 工作流引擎完整规则（160 行）
│   ├── reference.md          # 企业交付清单 + CI 模板
│   └── aicoding-workflow.mdc # Cursor 规则版
├── docs/
│   ├── 快速上手.md            # 5 分钟跑通
│   ├── 架构流程图.md          # 工作流可视化
│   ├── 实战案例.md            # 真实交付案例
│   ├── 常见坑.md              # 踩坑记录
│   └── aicoding_*.plan.md    # 原始计划文档
└── templates/                 # 企业交付模板包
    ├── Dockerfile            # 多阶段构建模板
    ├── docker-compose.yml    # 本地 + 生产编排
    ├── ci.yml                # GitHub Actions 模板
    └── README.template.md    # 项目 README 骨架
```

---

## 5 分钟上手

1. **装进你的 AI 工具**（Cursor / Claude Code / Codex）：

```bash
# Cursor: 把 skill/ 目录内容放到 .cursor/skills/aicoding/
# Claude Code: 把 SKILL.md 放到 ~/.claude/skills/aicoding/
```

2. **说需求**：

```
"帮我做一个完整的项目：机器人远程监控面板，
FastAPI 后端 + WebSocket 实时推送 + Vue3 前端，
支持多设备接入，带 Docker 部署"
```

3. **AI 自动执行**：Plan → 并行调研 → 写代码 → 跑测试 → 打包 → 交付

4. **验收**：`git clone && docker compose up` 一条命令跑起来

---

## 适用场景

- ✅ Web 应用（前后端分离、SSR、SPA）
- ✅ 后端服务（REST API、WebSocket、微服务）
- ✅ 全栈项目（含 CI/CD、Docker、监控）
- ✅ 从零到一的完整系统

不适用：只有一句话没有约束的需求（先在 Plan 模式对齐）。

---

*License: MIT*
