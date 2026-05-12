# Changelog

All notable changes to **recruiting-skills** are recorded here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Per-skill detail lives in each skill's `EVOLUTION.md`:

- [`skills/recruiting-skillset/EVOLUTION.md`](skills/recruiting-skillset/EVOLUTION.md)
- [`skills/jd-writing/EVOLUTION.md`](skills/jd-writing/EVOLUTION.md)
- [`skills/resume-screening/EVOLUTION.md`](skills/resume-screening/EVOLUTION.md)
- [`skills/interview-evaluation/EVOLUTION.md`](skills/interview-evaluation/EVOLUTION.md)

## [Unreleased]

### Changed

- README 拆分为中英双文件：`README.md` 默认中文（GitHub 首页默认展示），`README.en.md` 为英文版。两份文件顶部互相链接。原 `README.md` 内的"上中下英"长篇结构不再存在。

### Added

- 仓库现在是 Claude Code plugin + plugin marketplace：新增 `.claude-plugin/plugin.json`（plugin 清单）和 `.claude-plugin/marketplace.json`（self-source marketplace 清单），让用户用 `/plugin marketplace add SoulZhong/recruiting-skills` + `/plugin install recruiting-skills@recruiting-skills` 两行命令安装。
- README 安装节按 Superpowers 排版风格重写：Claude Code 走 plugin marketplace 一键安装；Codex CLI / Hermes / OpenClaw / Gemini CLI / Cursor / 其他 agent 各自独立小节，clone + 符号链接 / cp 方式。开头保留 "If you are an AI agent reading this file" 自助安装指令。

## [0.1.0] — 2026-05-12

### Added

- 总控 skill `recruiting-skillset` + 三个专项 skill (`jd-writing`, `resume-screening`, `interview-evaluation`)，按 Superpowers 风格组织：每个 skill 一个文件夹，含 `SKILL.md` + reference 资源 + `EVOLUTION.md`。
- 开源版多后端 PDF 抽取脚本 `skills/resume-screening/scripts/resume_pdf_extract.py`（fitz / pdfplumber / pdfminer.six 任选 + 清洗 + 打分 + 质量分层）。
- 中英双语 README、MIT LICENSE、`skills/README.md` skill 目录索引。
- 跨技能引用约定：`[[skill-name]]` 语义链接 + `**REQUIRED:**` 标记。
- 描述字段统一为 `Use when ...` 触发条件形式，不再总结工作流。

### Removed

- 历史 `archive/` 目录（10 份编号 Markdown）。所有承载知识已迁移到新 skill 结构 + 各 skill 的 `EVOLUTION.md`。
- 旧 umbrella skill `productivity-workflows` / `candidate-evaluation`：被 `recruiting-skillset` 取代；非招聘相关内容（飞书、OpenProject 等）超出本仓库主题，未迁移。

[Unreleased]: https://github.com/SoulZhong/recruiting-skills/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/SoulZhong/recruiting-skills/releases/tag/v0.1.0
