# Contributing to Recruiting Skills

> 欢迎 PR。本仓库的核心是"招聘判断 → agent 可调用 skill"的沉淀；任何能让这套判断更准、更可复用、更跨平台的贡献都欢迎。
>
> 提交前请扫一眼下方约定。

## What we welcome

- **新的触发条件 / 反模式案例**：在对应 SKILL.md 的 _When to Use_ 或 _Anti-Patterns_ 加条目。
- **新的归因校准 / 追问技巧**：放到 `skills/interview-evaluation/attribution-errors.md` 或 `probing-techniques.md`。
- **新的行业 / 岗位 playbook**：放到 `skills/jd-writing/role-type-playbooks.md` 或 `positioning-templates.md`。
- **PDF 抽取脚本 bug 修复、新后端、更好的清洗规则**：在 `skills/recruiting-resume-screening/scripts/`。
- **示例（脱敏）**：完整脱敏的 JD / 评估卡 / 面评示例，能让首次读者一眼看懂产出形态。
- **跨语言 / 跨平台适配**：英文版 skill、Codex / Gemini CLI 适配说明等。

## What we don't take

- 把仓库写窄成"某公司专用"的内部规则——本仓库刻意保持公司 / 行业 / 岗位无关。
- **任何未脱敏的真实姓名、公司名、客户数据**。所有示例必须脱敏到不可反向识别。
- 与招聘流程无关的方法论（飞书自动化、项目管理 等等）——超出范围。

## Repository conventions

每个 skill 一个文件夹：

```
skills/<skill-name>/
├── SKILL.md          # 主入口
├── EVOLUTION.md      # 演化日志
└── *.md / scripts/   # 按主题拆分的引用资源
```

### SKILL.md frontmatter

```yaml
---
name: <skill-name-with-hyphens>
description: Use when <trigger conditions only — NOT a workflow summary>
---
```

`description` 必须是 **触发条件**，不要写工作流总结。否则 agent 可能只读 description 就跳过 SKILL.md 正文。

### 跨技能引用

用 `[[skill-name]]` 语义链接，必要时配 `**REQUIRED:**` 标记。**不要**用 `@path` 强加载语法。

### 结构性变更必须同步 EVOLUTION.md

只要你修改了：

- SKILL.md 的工作流、自检、反模式、触发条件，
- 引用资源的章节标题或归因 / 校准规则，
- 输出契约 / 模板的结构，

就**必须**在对应 skill 的 `EVOLUTION.md` 顶部加一条新条目，注明日期 + 变更摘要 + 原因。这是本仓库的可追溯性保证。

仅修正 typo / 链接 / 标点不需要 EVOLUTION 条目。

## PR 流程

1. Fork → 在 feature branch 工作。
2. 一个 PR 只解决一类问题（不要把多个 skill 的多个变更塞进同一个 PR）。
3. PR 描述说明：解决什么问题、影响哪些 skill、是否更新了对应 EVOLUTION.md。
4. 若新增 `scripts/` 或修改脚本，本地验证可执行：`python3 path/to/script --help`。
5. 中英文 README 任一更新时，另一种语言也同步更新（保持双语一致）。

## Sensitive material

所有示例（JD、评估卡、面评）必须**完全脱敏**：

- 公司名 → "某 AI 产品公司" / "某行业解决方案公司"
- 候选人姓名 → "候选人 A / 候选人 B"
- 具体业务数字 → 量级或比例（"数千万粉丝" / "成本下降约 2/3"）
- 内部项目名、代号、产品名 → 通用化描述
- 内部工具栈 / 缓存路径（Hermes、内部脚本路径等）→ 改成通用形式

提交前自查一遍是否仍能从多个细节叠加反向识别到真实人或公司。

## Asking before building

不确定要不要做？开一个 issue 先讨论。比直接交付被退回 PR 更高效。

License: MIT — 所有贡献按 MIT 授权。
