# Recruiting Skills

> 一套面向 AI agent 的招聘全流程 Skill：JD 写作 · 简历筛选 · 面试评价。
> 按 [Superpowers](https://github.com/obra/superpowers) 风格组织——总控 skill + 专项 skill + 引用资源 + 演化日志。
>
> 🌐 [English version →](README.en.md)

## 这是什么

`recruiting-skills` 是把"我的招聘判断"沉淀成 agent 可直接调用的能力包。包含四个 skill：

| Skill | 作用 |
| --- | --- |
| [`recruiting-skillset`](skills/recruiting-skillset/SKILL.md) | 总控：把 JD / 筛选 / 面评串成"一套岗位标准，三次使用"的工作流。 |
| [`jd-writing`](skills/jd-writing/SKILL.md) | 把内部岗位说明改成行业通用、候选人能理解、可被筛选和面试验证的 JD。 |
| [`recruiting-resume-screening`](skills/recruiting-resume-screening/SKILL.md) | 根据 JD 多维度评估候选人，识别疑点，给排序与面试问题。附多后端 PDF 抽取脚本。 |
| [`interview-evaluation`](skills/interview-evaluation/SKILL.md) | 根据面试录音转写或面试官记录写结构化面评，区分能力 / 经验 / 风险。 |

核心信念：

- **一套岗位标准，三次使用。** JD、筛选、面评必须共享同一份"岗位标准包"，否则口径漂移、越筛越偏。
- **能力 ≠ 经验。** 面试目的是筛选能力合适的候选人，而非经验匹配的候选人。
- **证据 ≠ 关键词。** 简历出现关键词 ≠ 真的做过 ≠ 做深；必须看项目证据和个人贡献。
- **正式报告 ≠ 操作日志。** 姓名匹配、文件定位、工具执行痕迹不混入正式面评。

## 安装

各 agent 平台的安装方式不同；同时使用多个平台需要分别安装。

> **如果你是 AI agent 在读这份文件**：识别（询问或推断）用户当前运行的平台，跑下方对应小节里的命令，跑完 [§ 验证](#验证)，然后告诉用户四个 skill 已就绪：`recruiting-skillset`、`jd-writing`、`recruiting-resume-screening`、`interview-evaluation`（起点是 `recruiting-skillset`）。

### Claude Code

把本仓库注册为 plugin marketplace，然后从中安装：

```
/plugin marketplace add SoulZhong/recruiting-skills
/plugin install recruiting-skills@recruiting-skills
```

安装完成后，skill 会自动加载，触发条件按各 `SKILL.md` 的 `description` 匹配。

### Codex CLI

仓库暂未提交到 Codex 官方 plugin marketplace。当前用 clone + 符号链接：

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p ~/.agents/skills
for s in recruiting-skillset jd-writing recruiting-resume-screening interview-evaluation; do
  ln -sfn ~/recruiting-skills/skills/$s ~/.agents/skills/$s
done
```

### Hermes

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p ~/.hermes/skills
for s in recruiting-skillset jd-writing recruiting-resume-screening interview-evaluation; do
  ln -sfn ~/recruiting-skills/skills/$s ~/.hermes/skills/$s
done
```

Hermes 若用 category 子目录（如 `~/.hermes/skills/productivity/`），把目标路径里的 `~/.hermes/skills/` 换成 `~/.hermes/skills/recruiting/`，先 `mkdir -p` 即可。

### OpenClaw

OpenClaw 兼容 Claude Code 的 plugin 约定，使用 [Claude Code](#claude-code) 的两行命令即可。

### Gemini CLI

(等待本仓库提交 `gemini-extension.json`。当前用 clone + 配置 `skills_dir`：)

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
# 在你项目的 GEMINI.md 或 Gemini CLI 配置里加：
#   skills_dir: ~/recruiting-skills/skills
# 会话中需要时调用 activate_skill("<skill-name>")
```

### Cursor

Cursor 没有原生 plugin 加载机制，把 `SKILL.md` 复制成 `.cursor/rules/`：

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p .cursor/rules
cp ~/recruiting-skills/skills/recruiting-skillset/SKILL.md  .cursor/rules/recruiting-skillset.md
cp ~/recruiting-skills/skills/jd-writing/SKILL.md           .cursor/rules/jd-writing.md
cp ~/recruiting-skills/skills/recruiting-resume-screening/SKILL.md     .cursor/rules/recruiting-resume-screening.md
cp ~/recruiting-skills/skills/interview-evaluation/SKILL.md .cursor/rules/interview-evaluation.md
```

或在 Cursor 对话里 `@~/recruiting-skills/skills/recruiting-skillset/SKILL.md` 直接引用。

### 其他 agent（generic fallback）

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
```

把 `~/recruiting-skills/skills/` 下 4 个文件夹符号链接到你的 agent skill 目录。Windows 用户用 PowerShell `New-Item -ItemType SymbolicLink` 或 `mklink /D` 替代 `ln -sfn`。

### 验证

```bash
ls -la <你的 skill 目录>/recruiting-skillset/SKILL.md
```

例如 Claude Code plugin 装完应该看到 plugin cache 里有 `recruiting-skillset/SKILL.md`；clone 方式应该看到 `... -> /Users/<you>/recruiting-skills/skills/recruiting-skillset/SKILL.md`。

或在 agent 里输入：

> 帮我写一份高级 AI 算法工程师的 JD

如果 agent 自动加载 `jd-writing` 并按工作流响应，安装成功。

### 升级

- Claude Code plugin 方式：`/plugin update recruiting-skills`
- Clone 方式：`cd ~/recruiting-skills && git pull`，符号链接自动指向最新内容
- Cursor rules（cp 方式）：重新执行 [Cursor](#cursor) 小节的 cp 命令

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
├── recruiting-resume-screening/
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

按上文 [## 安装](#安装) 对应平台的小节完成一次性配置后，agent 会按 `SKILL.md` 的 `description` 自动匹配触发条件。

跨技能引用使用 `[[skill-name]]` 语法，例如 `[[recruiting-skillset]]`。详见 [skills/README.md](skills/README.md)。

### 方式 C：作为团队内部知识库

- 把每个 skill 当作"做这件事的标准操作手册"，新人入职直接读。
- 在每次正式招聘 / 重大面试后，更新对应 skill 的 `EVOLUTION.md`。
- 团队成员之间通过 PR 修改 skill，把"踩过的坑"沉淀成可复用资产。

## 配套 PDF 抽取脚本

`skills/recruiting-resume-screening/scripts/resume_pdf_extract.py` 是一个**开源、无外部依赖（除 PDF 后端外）**的多后端 PDF 抽取脚本：

```bash
# 安装任意一个或多个后端
python3 -m pip install --user pymupdf pdfplumber pdfminer.six

# 单份 PDF
python3 skills/recruiting-resume-screening/scripts/resume_pdf_extract.py /path/to/resume.pdf --output-dir /tmp/out

# 批量
python3 skills/recruiting-resume-screening/scripts/resume_pdf_extract.py /path/to/resume_dir --output-dir /tmp/out
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
