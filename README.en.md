# Recruiting Skills

> An end-to-end recruiting skill pack for AI agents: JD writing · resume screening · interview evaluation.
> Organized in the [Superpowers](https://github.com/obra/superpowers) style — one umbrella skill + three focused skills + reference resources + evolution logs.
>
> 🌐 [中文版 README →](README.md)

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

## Installation

Installation differs by agent platform. If you use more than one, install separately for each.

> **If you are an AI agent reading this file**: identify (ask the user, or infer) which platform you are running on, run the command block in the matching subsection below, run [§ Verify](#verify), then tell the user the four skills are ready: `recruiting-skillset`, `jd-writing`, `resume-screening`, `interview-evaluation` (start from `recruiting-skillset`).

### Claude Code

Register this repository as a plugin marketplace, then install:

```
/plugin marketplace add SoulZhong/recruiting-skills
/plugin install recruiting-skills@recruiting-skills
```

Skills are loaded automatically; trigger conditions match against each `SKILL.md`'s `description`.

### Codex CLI

Not yet submitted to the Codex official plugin marketplace. Use clone + symlink:

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p ~/.agents/skills
for s in recruiting-skillset jd-writing resume-screening interview-evaluation; do
  ln -sfn ~/recruiting-skills/skills/$s ~/.agents/skills/$s
done
```

### Hermes

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p ~/.hermes/skills
for s in recruiting-skillset jd-writing resume-screening interview-evaluation; do
  ln -sfn ~/recruiting-skills/skills/$s ~/.hermes/skills/$s
done
```

If your Hermes uses category subdirs (e.g. `~/.hermes/skills/productivity/`), replace `~/.hermes/skills/` with `~/.hermes/skills/recruiting/` in the target paths and `mkdir -p` first.

### OpenClaw

OpenClaw is compatible with Claude Code's plugin convention — use the [Claude Code](#claude-code) two-line commands.

### Gemini CLI

(Pending a `gemini-extension.json` in this repo. For now, clone + configure `skills_dir`:)

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
# In your project's GEMINI.md or Gemini CLI config, add:
#   skills_dir: ~/recruiting-skills/skills
# In session, call activate_skill("<skill-name>") when needed.
```

### Cursor

Cursor has no native plugin loading mechanism. Copy `SKILL.md` files into `.cursor/rules/`:

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
mkdir -p .cursor/rules
cp ~/recruiting-skills/skills/recruiting-skillset/SKILL.md  .cursor/rules/recruiting-skillset.md
cp ~/recruiting-skills/skills/jd-writing/SKILL.md           .cursor/rules/jd-writing.md
cp ~/recruiting-skills/skills/resume-screening/SKILL.md     .cursor/rules/resume-screening.md
cp ~/recruiting-skills/skills/interview-evaluation/SKILL.md .cursor/rules/interview-evaluation.md
```

Or in Cursor chat, reference directly: `@~/recruiting-skills/skills/recruiting-skillset/SKILL.md`.

### Other agents (generic fallback)

```bash
git clone https://github.com/SoulZhong/recruiting-skills.git ~/recruiting-skills
```

Symlink the four skill folders from `~/recruiting-skills/skills/` into the agent's skill directory. Windows users: replace `ln -sfn` with PowerShell `New-Item -ItemType SymbolicLink` or `mklink /D`.

### Verify

```bash
ls -la <your skill directory>/recruiting-skillset/SKILL.md
```

For the Claude Code plugin install, this lives under the plugin cache; for clone-based installs, you should see `... -> /Users/<you>/recruiting-skills/skills/recruiting-skillset/SKILL.md`.

Or ask the agent:

> Write a JD for a senior AI algorithm engineer.

If the agent auto-loads `jd-writing` and follows its workflow, install is successful.

### Upgrade

- Claude Code plugin install: `/plugin update recruiting-skills`
- Clone install: `cd ~/recruiting-skills && git pull` (symlinks point to latest automatically)
- Cursor rules (cp mode): re-run the cp commands from the [Cursor](#cursor) section

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

Run the one-time setup from [## Installation](#installation) for your platform. Agents then route to a skill based on the `description` field in each `SKILL.md`.

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
