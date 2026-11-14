# Clammerly

Grammarly for clankers. A Claude Code skill that finds the writing tics that make text read as machine-written, plus vague wording, hidden actions and buried points, and rewrites each line plainly.

It works on product copy, emails, prompts, docs and drafts. Give it a path, a diff, pasted text, or a plain description like "the front page" or "all user-facing copy".

## Install

Copy the folder into your Claude Code skills directory:

```bash
git clone https://github.com/dadukhankevin/clammerly ~/.claude/skills/clammerly
```

Then run `/clammerly` in Claude Code, optionally with a target and `--fix` or `--strict`. See [SKILL.md](SKILL.md) for the full list of patterns and how it reports.

## Credit

Built from informative X posts on the flaws of AI writing that have been distilled into a skill.
