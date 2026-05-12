# Recruiting Skills

> 一套面向 AI agent 的招聘全流程 Skill：JD 写作 · 简历筛选 · 面试评价。
> 按 [Superpowers](https://github.com/obra/superpowers) 风格组织——总控 skill + 专项 skill + 引用资源 + 演化日志。
>
> [English version below ↓](#english)

---

## 这是什么

`recruiting-skills` 是把"我的招聘判断"沉淀成 agent 可直接调用的能力包。包含四个 skill：

| Skill | 作用 |
| --- | --- |
| [`recruiting-skillset`](skills/recruiting-skillset/SKILL.md) | 总控：把 JD / 筛选 / 面评串成"一套岗位标准，三次使用"的工作流。 |
| [`jd-writing`](skills/jd-writing/SKILL.md) | 把内部岗位说明改成行业通用、候选人能理解、可被筛选和面试验证的 JD。 |
| [`resume-screening`](skills/resume-screening/SKILL.md) | 根据 JD 多维度评估候选人，识别疑点，给排序与面试问题。附多后端 PDF 抽取脚本。 |
| [`interview-evaluation`](skills/interview-evaluation/SKILL.md) | 根据面试录音转写或面试官记录写结构化面评，区分能力 / 经验 / 风险。 |

核心信念：

- **一套岗位标准，三次使用。** JD、筛选、面评必须共享同一份"岗位标准包"，否则口径漂移、越筛越偏。
- **能力 ≠ 经验。** 面试目的是筛选能力合适的候选人，而非经验匹配的候选人。
- **证据 ≠ 关键词。** 简历出现关键词 ≠ 真的做过 ≠ 做深；必须看项目证据和个人贡献。
- **正式报告 ≠ 操作日志。** 姓名匹配、文件定位、工具执行痕迹不混入正式面评。

## 如何使用

### 方式 A：直接读 Markdown

每个 skill 都是纯 Markdown 文件，可以直接当文档读：

```
skills/
├── recruiting-skillset/
│   ├── SKILL.md                       # 主入口
│   ├── role-standard-packet.md        # 岗位标准包模板
│   └── EVOLUTION.md                   # 演化日志
├── jd-writing/
│   ├── SKILL.md
│   ├── title-conversion.md            # 内部 → 行业通用岗位名
│   ├── verifiable-requirements.md     # 能力 → JD 表达 → 面试验证
│   ├── role-type-playbooks.md         # 六类岗位写作侧重点
│   ├── positioning-templates.md       # 业务定位模板
│   └── EVOLUTION.md
├── resume-screening/
│   ├── SKILL.md
│   ├── five-dimension-rubric.md       # 五维评估完整定义
│   ├── red-flags-catalog.md           # 简历疑点清单
│   ├── output-templates.md            # 评估卡 / 排序表模板
│   ├── batch-screening.md             # 批量筛选纪律
│   ├── pdf-extraction.md              # PDF 提取与多后端 triage
│   ├── scripts/
│   │   └── resume_pdf_extract.py      # 开源版多后端 PDF 抽取脚本
│   └── EVOLUTION.md
└── interview-evaluation/
    ├── SKILL.md
    ├── capability-dimensions.md       # 16 项能力维度
    ├── attribution-errors.md          # 常见归因错误对照
    ├── probing-techniques.md          # 真实性 / 深度 / 风险追问
    ├── evaluation-template.md         # 空白面评模板
    ├── example-evaluation.md          # 完整面评示例
    └── EVOLUTION.md
```

### 方式 B：作为 agent skill 加载

把 `skills/` 文件夹放到你的 agent skill 目录（例如 Claude Code `~/.claude/skills/` 或 Codex `~/.agents/skills/`），agent 会按 `SKILL.md` 的 `description` 自动匹配触发条件。

跨技能引用使用 `[[skill-name]]` 语法，例如 `[[recruiting-skillset]]`。详见 [skills/README.md](skills/README.md)。

### 方式 C：作为团队内部知识库

- 把每个 skill 当作"做这件事的标准操作手册"，新人入职直接读。
- 在每次正式招聘 / 重大面试后，更新对应 skill 的 `EVOLUTION.md`。
- 团队成员之间通过 PR 修改 skill，把"踩过的坑"沉淀成可复用资产。

## 配套 PDF 抽取脚本

`skills/resume-screening/scripts/resume_pdf_extract.py` 是一个**开源、无外部依赖（除 PDF 后端外）**的多后端 PDF 抽取脚本：

```bash
# 安装任意一个或多个后端
python3 -m pip install --user pymupdf pdfplumber pdfminer.six

# 单份 PDF
python3 skills/resume-screening/scripts/resume_pdf_extract.py /path/to/resume.pdf --output-dir /tmp/out

# 批量
python3 skills/resume-screening/scripts/resume_pdf_extract.py /path/to/resume_dir --output-dir /tmp/out
```

输出：

- `<basename>.<backend>.txt`：每份 PDF 的最佳清洗文本。
- `summary.json`：含 `backend / chars / lines / keyword_hits / score / quality`。
- stdout 上一份 Markdown 表，可直接粘进会话。

## 设计取舍

- **不绑定公司 / 行业 / 岗位**：所有 skill 都泛化为通用方法论。如有特定公司 / 产品材料，请作为对话上下文输入。
- **不绑定平台**：skill 内容是纯 Markdown，能在 Claude Code、Codex、Gemini CLI 或任何 agent 里加载。
- **`description` 只写触发条件**：避免 agent 只读 description 就跳过 SKILL.md 正文。这是 Superpowers 框架的核心约定。
- **细节放进引用资源，SKILL.md 保持紧凑**：让 SKILL.md 只承担"路由 + 工作流 + 自检"，rubric、catalog、templates 这些细节放在同级 `.md` 文件里，agent 加载时按需读取。
- **演化日志独立成文件**：长期 evolution 不放进 SKILL.md，避免每次加载都把历史塞进上下文。

## 贡献

欢迎以 PR 形式贡献：

- 新增触发场景或反模式案例。
- 新增反向归因 / 追问技巧。
- 新增行业 / 岗位类型的写作 playbook。
- 修复脚本 bug、补 PDF 后端、改进清洗规则。

每次结构性变更请同步更新对应 skill 的 `EVOLUTION.md`。

## License

MIT — 见 [LICENSE](LICENSE)。

## 致谢

- 受 [Superpowers](https://github.com/obra/superpowers) 启发的 skill 结构与触发条件写法。
- 来自多个企业招聘流程的真实判断、踩坑、复盘——已脱敏。

---

<a id="english"></a>

# Recruiting Skills (English)

> An end-to-end recruiting skill pack for AI agents: JD writing · resume screening · interview evaluation.
> Organized in the [Superpowers](https://github.com/obra/superpowers) style — one umbrella skill + three focused skills + reference resources + evolution logs.

## What it is

`recruiting-skills` packages a recruiter's judgment so that AI agents can call it directly. Four skills:

| Skill | Purpose |
| --- | --- |
| [`recruiting-skillset`](skills/recruiting-skillset/SKILL.md) | Umbrella. Chains JD / screening / evaluation into one workflow built around a single "role standard packet". |
| [`jd-writing`](skills/jd-writing/SKILL.md) | Turn internal role notes into a candidate-readable, industry-titled JD that screening and interview can verify against. |
| [`resume-screening`](skills/resume-screening/SKILL.md) | Multi-dimensional resume evaluation against a JD: red flags, ranking, interview questions. Ships a multi-backend PDF extraction script. |
| [`interview-evaluation`](skills/interview-evaluation/SKILL.md) | Structured interview write-ups from transcripts or interviewer notes, separating capability / experience / risk. |

Core beliefs:

- **One role standard, used three times.** JD, screening, and evaluation must share the same role standard packet, or the bar drifts.
- **Capability ≠ experience.** The job of an interview is to filter candidates with the right capability, not the right resume keywords.
- **Evidence ≠ keywords.** A resume mentioning X does not mean the candidate did X, let alone did it deeply.
- **Formal reports ≠ operation logs.** Name-matching steps, file lookup, tooling traces stay out of the user-facing evaluation.

## How to use

### Option A — read as Markdown docs

Every skill is plain Markdown — readable as documentation:

```
skills/
├── recruiting-skillset/   # umbrella + role-standard-packet template
├── jd-writing/            # title conversion / verifiable requirements / role playbooks / positioning templates
├── resume-screening/      # 5-dim rubric / red flags / templates / batch discipline / PDF extraction script
└── interview-evaluation/  # 16 dimensions / attribution errors / probing techniques / template / full example
```

### Option B — load as agent skills

Drop `skills/` into your agent's skill directory (e.g. `~/.claude/skills/` for Claude Code, `~/.agents/skills/` for Codex). Agents will route to a skill based on the `description` field in each `SKILL.md`.

Cross-skill references use `[[skill-name]]` syntax. See [skills/README.md](skills/README.md).

### Option C — team knowledge base

- Treat each skill as the SOP for that step. Onboard new hires by reading the skills.
- After each significant hiring round or interview, update the relevant `EVOLUTION.md`.
- Iterate via PRs so lessons learned become reusable assets, not Slack messages that scroll away.

## PDF extraction script

`skills/resume-screening/scripts/resume_pdf_extract.py` is a self-contained, open-source multi-backend PDF extractor:

```bash
pip install --user pymupdf pdfplumber pdfminer.six   # any subset works
python3 skills/resume-screening/scripts/resume_pdf_extract.py /path/to/resume_dir --output-dir /tmp/out
```

Per file: cleans output, scores it, writes the best backend's text. Produces a `summary.json` and a Markdown triage table.

## Design tradeoffs

- **Company / industry / role agnostic.** Skills are generalized methodology; specific materials go in as task context.
- **Platform agnostic.** Pure Markdown — works in Claude Code, Codex, Gemini CLI, or any agent that supports skills.
- **Descriptions are triggers only.** Following Superpowers' convention to prevent agents from reading the description and skipping the body.
- **Heavy reference lives next to SKILL.md.** SKILL.md stays compact (routing + workflow + self-check). Rubrics, catalogs, templates are siblings loaded on demand.
- **Evolution logs are separate files.** Long-running history shouldn't bloat the SKILL.md that agents load every turn.

## Contributing

PRs welcome:

- New trigger conditions or anti-pattern cases.
- New attribution / probing techniques.
- New industry or role-type playbooks.
- Bug fixes for the script, new PDF backends, better cleaning heuristics.

Please update the affected `EVOLUTION.md` for any structural change.

## License

MIT — see [LICENSE](LICENSE).

## Acknowledgements

- Skill structure and trigger-condition style inspired by [Superpowers](https://github.com/obra/superpowers).
- Built from real hiring decisions, mistakes, and retrospectives across multiple teams (anonymized).
