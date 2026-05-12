# Skills Catalog

> Skill 入口索引。每个 skill 一个文件夹，内含 `SKILL.md`（主入口）+ 引用资源。
>
> 推荐入口：[recruiting-skillset](recruiting-skillset/SKILL.md) — 招聘全流程的总控 Skill。

## 总控 Skill

| Skill | When to use |
| --- | --- |
| [recruiting-skillset](recruiting-skillset/SKILL.md) | 任何端到端招聘任务：定义岗位 / 写 JD / 筛简历 / 准备面试 / 写面评 / 复盘招聘标准。 |

## 专项 Skill

| Skill | When to use |
| --- | --- |
| [jd-writing](jd-writing/SKILL.md) | 写 JD、改 JD、把内部岗位说明改成对外招聘 JD。 |
| [resume-screening](resume-screening/SKILL.md) | 根据 JD 评估单份 / 批量简历，给排序、疑点、面试问题。 |
| [interview-evaluation](interview-evaluation/SKILL.md) | 根据面试录音转写或面试官记录写结构化面评。 |

## Skill 之间的关系

```
   recruiting-skillset (总控，维护"岗位标准包")
        │
        ├─► jd-writing          ──► 候选人外发 JD + 内部验证标准
        │
        ├─► resume-screening    ──► 个人评估卡 + 排序 + 疑点 + 面试问题
        │
        └─► interview-evaluation ─► 推进建议 + 优势 / 不足 / 风险 + 下一步

   反馈回流：投递质量 / 面试发现 / 入职表现 → 修正各 Skill 与岗位标准包
```

## Skill 内部结构约定

每个 skill 文件夹遵循同一约定：

- `SKILL.md`：主入口，包含 `name` + `description`（"Use when ..." 触发条件）+ 工作流 + 自检。
- `EVOLUTION.md`：演化日志。每次结构性变更或反馈回流追加一条。
- `*.md`：按主题拆分的引用资源（rubric、catalog、templates、playbooks 等）。
- `scripts/`：可选。`resume-screening` 下放了多后端 PDF 提取脚本。

## 跨技能引用

`[[skill-name]]` 是语义链接，指向同名 skill 的 `SKILL.md`。例如：

- `[[recruiting-skillset]]` → [recruiting-skillset/SKILL.md](recruiting-skillset/SKILL.md)
- `[[jd-writing]]` → [jd-writing/SKILL.md](jd-writing/SKILL.md)
- `[[resume-screening]]` → [resume-screening/SKILL.md](resume-screening/SKILL.md)
- `[[interview-evaluation]]` → [interview-evaluation/SKILL.md](interview-evaluation/SKILL.md)

## 触发风格

所有 SKILL.md 的 `description` 字段都遵循 Superpowers 约定：**只写触发条件，不总结工作流**。

正例：

```yaml
description: Use when reviewing one or more Chinese-language resumes against a JD, ...
```

反例（不要这样写）：

```yaml
description: 五步走简历筛选——抽文本、五维评估、识别疑点、综合评级、输出报告
```

原因：描述如果总结流程，未来的 agent 可能只读 description 就开始干活，跳过 SKILL.md 正文里的实际规则。
