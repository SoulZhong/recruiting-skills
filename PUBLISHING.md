# Publishing

This repo ships in two formats. The source of truth is `skills/`. Publish artifacts are generated, never edited directly.

## Claude Code plugin (existing)

Use `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json` as already configured. No changes needed.

## ClawHub

ClawHub publishes each skill as its own slug; there is no "skill-pack" wrapper. The artifact lives at `dist/clawhub/` and contains the four skills with ClawHub-compatible frontmatter (`version: 0.1.0` added, conflicting `license:` removed — ClawHub publishes under MIT-0 by default).

### One-time setup

```bash
npm i -g clawhub        # or: brew install clawhub  (see https://clawhub.ai)
clawhub login           # GitHub account ≥1 week old
```

### Publish

From the repo root:

```bash
# dry-run first — prints what would be published
clawhub sync --root dist/clawhub --bump patch --dry-run

# real publish (all four skills)
clawhub sync --root dist/clawhub --bump patch
```

Or one skill at a time:

```bash
clawhub skill publish dist/clawhub/recruiting-skillset --version 0.1.0
clawhub skill publish dist/clawhub/jd-writing          --version 0.1.0
clawhub skill publish dist/clawhub/recruiting-resume-screening --version 0.1.0
clawhub skill publish dist/clawhub/interview-evaluation --version 0.1.0
```

### Regenerating the artifact

If you edit anything under `skills/`, refresh `dist/clawhub/` before publishing:

```bash
rm -rf dist/clawhub
mkdir -p dist/clawhub
rsync -a --exclude='.DS_Store' --exclude='__pycache__' --exclude='*.pyc' --exclude='.venv' skills/ dist/clawhub/
# then re-add `version:` and `homepage:` to each SKILL.md (and drop any `license:` keys)
```

### Bumping versions

Bump the `version:` field in each `SKILL.md` under `dist/clawhub/<skill>/` before publishing. `clawhub sync --bump patch` will auto-increment when given existing slugs.
