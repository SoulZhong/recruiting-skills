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
