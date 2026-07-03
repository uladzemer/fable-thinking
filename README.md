# fable-thinking

> **Claude Fable 5's operating discipline, packaged as a Claude Code skill for Opus, Sonnet and Haiku.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-orange)](https://code.claude.com/docs/en/skills)
[![EN + RU](https://img.shields.io/badge/languages-EN%20%2B%20RU-blue)](README.ru.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/uladzemer/fable-thinking/pulls)

🇷🇺 [Русская версия README](README.ru.md) • [Русская версия скилла](ru/)

## Who this is for

You've noticed that smaller models don't *work* the way the frontier one does: they say "done" without checking, patch symptoms instead of causes, drift off the goal ten tool calls in. But running the top model for everything burns through tokens fast.

This skill is for exactly that trade-off. It transfers Fable 5's **working discipline** to the models below it — so Opus, Sonnet and Haiku can handle the same everyday work with frontier-style rigor, at a fraction of the token cost.

## Install in 5 seconds

```bash
git clone https://github.com/uladzemer/fable-thinking ~/.claude/skills/fable-thinking
```

That's it. Claude Code picks it up automatically on non-trivial tasks, or on demand: *"think like Fable"*, *"fable-mode"*.

## What it does

No prompt makes a model smarter — weights don't change. What a skill *can* transfer is **discipline**: most agentic failures aren't intelligence failures, they're discipline failures. This skill enforces 8 procedures + 2 deep-dive protocols that kill the classic ones:

| Failure mode you know | What the skill does |
|---|---|
| *"Done!"* — but nothing was verified | **PROOF**: no completion claims without running the check and reading the output |
| Patch on patch on patch, each "almost worked" | **TWO STRIKES**: 2 failed attempts on one hypothesis → full stop, root-cause protocol |
| Fixed one copy of duplicated code | **READ**: grep for twins and call sites before the first edit |
| Solved a different task than you asked | **GOAL**: one verifiable "done when…" line, re-anchored mid-session |
| Quietly guessed instead of asking | **CALIBRATION**: every claim tagged Verified / Inferred / Assumed |
| "Improved" things nobody asked for | **STEPS**: stop when the goal is met; scope boundary rules |

One design rule shaped every line: *"think again"* without new external evidence makes models **worse** ([Huang et al., ICLR 2024](https://arxiv.org/abs/2310.01798)) — so every self-check here is anchored to an external signal: a command, an output, code actually read. This kind of scaffolding is also why mid-tier models post double-digit gains on agentic benchmarks ([Shinn et al., 2023](https://arxiv.org/abs/2303.11366)).

## What's inside

```
SKILL.md                    # the 8 procedures + calibration by task size (English)
references/debugging.md     # full root-cause protocol (activated after "two strikes")
references/self-review.md   # three-lens hostile review of your own diff
ru/                         # the full Russian original (installable as fable-thinking-ru)
```

## Usage

**As a session skill** — installed as above, it triggers on non-trivial tasks automatically.

**For subagents (the sweet spot):** when dispatching a subagent on a cheaper model, add one line to its prompt:

```
Before doing anything else, read ~/.claude/skills/fable-thinking/SKILL.md
and follow its discipline throughout this task.
```

**Per-project install:** drop the folder into `.claude/skills/fable-thinking/` inside the repo and commit — the whole team gets it.

## When NOT to use it

- **Trivial edits** (rename, typo, one-liner) — the skill says so itself; ceremony is calibrated to task size.
- **On Fable 5 itself.** Anthropic's guide is explicit: skills written for prior models are *"often too prescriptive for Claude Fable 5 and can degrade output quality"*. This skill is for the models below it.

This is discipline transfer, not capability transfer — it won't add knowledge or extend a model's planning horizon. The payoff is largest on long, messy, ambiguous tasks.

## Provenance

Written by Claude Fable 5, distilled from the official [Prompting Claude Fable 5 guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5), the official [Claude Code prompt library](https://code.claude.com/docs/en/prompt-library) (each ritual item maps to an officially blessed pattern — the mapping table is in SKILL.md), and the Iron-Law patterns popularized by [obra/superpowers](https://github.com/obra/superpowers).

*Community project. Not affiliated with or endorsed by Anthropic. Claude is a trademark of Anthropic, PBC.*

## License

[MIT](LICENSE). Use it, fork it, ship it with your team.

---

**If your Opus/Sonnet sessions stopped saying "done" for things that weren't — that's the skill working. Star ⭐ the repo so others find it.**
