---
name: AIcoding 自主巡航系统
overview: 基于现有 openclaw 自主Agent框架和 Cursor skills 系统，增强实现 AIcoding 技能：用户下达任务描述后，系统自动完成从规划、探索、编码、测试、CI/CD 到完整企业级可交付项目的全流程，无任何人工干预。支持多类型项目，交付包含完整测试套件、Docker、监控和部署脚本。
todos:
  - id: create-aicoding-skill
    content: 使用 create-skill 指南创建 ~/.cursor/skills/aicoding/SKILL.md，集成 TodoWrite、SwitchMode、parallel Task 和 enterprise delivery templates
    status: completed
  - id: enhance-openclaw
    content: 分析并扩展 .openclaw/workspace/AGENTS.md、MEMORY.md、HEARTBEAT.md 以支持全自动巡航模式
    status: completed
  - id: define-delivery-checklist
    content: 创建企业级交付模板，包括 CI/CD workflows、Docker、tests、monitoring 和 Canvas 架构评审
    status: completed
  - id: implement-workflow-engine
    content: 构建核心自主引擎：plan->explore(并行)->agent-implement->validate->deliver 闭环
    status: completed
  - id: test-with-sample-task
    content: 使用示例任务（如全栈认证系统）验证端到端零干预交付
    status: completed
isProject: false
---

## 架构设计

AIcoding 将作为新 skill 集成到现有系统中，扩展 [.openclaw/workspace/AGENTS.md](.openclaw/workspace/AGENTS.md) 的 memory/heartbeat/ bootstrap 机制。

核心工作流：
1. **任务解析**：接收自然语言任务，使用 Task(explore) 并行分析需求。
2. **规划模式**：使用 CreatePlan + TodoWrite 拆解任务，AskQuestion 澄清范围。
3. **Agent模式执行**：并行使用 explore/generalPurpose subagents 调研、Read/Grep 分析现有代码、Shell 构建项目。
4. **交付阶段**：生成完整项目结构（含 tests、.github/workflows CI/CD、Dockerfile、monitoring 配置）、运行测试、提交 git、生成 Canvas 架构评审。
5. **零干预**：严格遵守 plan/agent 模式切换 (SwitchMode)、linter 自动修复 (ReadLints)、企业级 checklist。

Mermaid 架构图：

```mermaid
graph TD
    UserTask[用户任务描述] --> Parser[AIcoding Parser<br/>Task(explore)]
    Parser --> PlanMode[Plan Mode<br/>CreatePlan + TodoWrite]
    PlanMode --> Clarify[AskQuestion 澄清<br/>1-2问题]
    Clarify --> AgentMode[Agent Mode<br/>SwitchMode]
    AgentMode --> ParallelAgents[并行Subagents<br/>explore + generalPurpose]
    ParallelAgents --> Build[项目构建<br/>Write/StrReplace + Shell(git/npm)]
    Build --> Validate[验证循环<br/>ReadLints + Tests + Canvas]
    Validate --> Deliver[企业级交付<br/>README + CI/CD + Docker + Monitoring]
    Deliver --> Heartbeat[openclaw HEARTBEAT/MEMORY 更新]
```

关键文件引用：
- [C:\Users\20392\.cursor\skills-cursor\create-skill\SKILL.md](C:\Users\20392\.cursor\skills-cursor\create-skill\SKILL.md)：指导创建 aicoding/ 目录及 SKILL.md（name: aicoding, description 包含触发词如'完整项目'、'全自动交付'、'企业级'）。
- [C:\Users\20392\.cursor\skills-cursor\canvas\SKILL.md](C:\Users\20392\.cursor\skills-cursor\canvas\SKILL.md)：架构评审、交付总结使用 Canvas（避免 markdown 表格，使用 Stat/Grid/Table）。
- create-rule 用于持久规则（如企业交付标准）。
- .openclaw/workspace/* (SOUL.md, MEMORY.md, BOOTSTRAP.md)：增强自主性。

## 实施步骤

**Phase 1: 技能基础 (使用 create-skill 工作流)**
- 在 ~/.cursor/skills/aicoding/ 创建 SKILL.md（<500行，third-person description，progressive disclosure）。
- 集成 TodoWrite 进行任务管理（complex tasks 3+ steps 必须使用）。
- 严格模式切换：计划阶段只读，实施阶段全工具。

**Phase 2: 自主工作流引擎**
- 定义多类型项目模板（web、backend、fullstack）。
- 企业交付 checklist：完整测试 (Jest/Pytest)、GitHub Actions CI/CD、Docker + docker-compose、Prometheus/Grafana 监控基本配置、详细 README with deployment steps。
- 反馈循环：linter 修复、测试通过、canvas 评审后才交付。

**Phase 3: 增强 openclaw**
- 更新 AGENTS.md 添加 AIcoding 触发逻辑。
- 实现 self-improving：使用 MEMORY.md 记录成功项目模式。

**成功标准**
- 输入："创建一个用户认证系统" → 输出完整可 clone 运行的企业级 repo（无人工修改）。
- 零 slop：遵循所有 SKILL.md 规则（no emojis unless requested, concise, etc.）。
- 可扩展：支持后续任务如"优化性能"继续在同一项目上迭代。

此计划保持简洁，聚焦高影响力决策。用户确认后切换到 Agent 模式实施。