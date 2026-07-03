# fable-thinking

> **Claude Fable 5 wrote down how it thinks — so Opus, Sonnet and Haiku can borrow the discipline.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-orange)](https://code.claude.com/docs/en/skills)
[![EN + RU](https://img.shields.io/badge/languages-EN%20%2B%20RU-blue)](README.ru.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/uladzemer/fable-thinking/pulls)

**Install in 5 seconds:**

```bash
git clone https://github.com/uladzemer/fable-thinking ~/.claude/skills/fable-thinking
```

That's it. Claude Code picks it up automatically. Say *"think like Fable"* or just start a non-trivial task on Opus/Sonnet — the skill loads by its description.

🇷🇺 [Русская версия README](README.ru.md) • [Русская версия скилла](ru/)

---

## The story

A user asked Claude Fable 5 (Anthropic's Mythos-class frontier model): *"Make a skill that teaches Opus 4.8 and Sonnet 5 to think like you."*

Fable 5 took the request literally — and honestly:

1. **Fact-checked the premise.** There is **no official mechanism** for "transferring Fable's thinking" to smaller models (verified against Anthropic's announcements and docs — the rumor is likely a distortion of the refusal-fallback, where requests Fable 5 declines get served by Opus 4.8).
2. **Found what actually transfers**: *operating discipline*. Anthropic's official [Prompting Claude Fable 5 guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5) is effectively a catalog of Fable 5's behaviors — including one technique with a measured result (auditing progress claims against tool results *"virtually eliminated fabricated status reports"* in Anthropic's tests).
3. **Distilled its own operating patterns** into 8 enforceable procedures + 2 deep-dive protocols (debugging, self-review).
4. **A/B-tested it** on Sonnet 5 subagents with scripted grading.
5. **Survived five adversarial reviews** — fresh-context self-critique, a security pass, a "read it as the target audience" pass by Sonnet itself, plus **cross-model reviews by GPT-5.5 (Codex) and Gemini 3.5**. Every confirmed finding was fixed, including a YAML bug that would have silently prevented the skill from loading at all.

## What it actually does (the honest part)

No prompt makes a model smarter — weights don't change. What a skill *can* transfer is **discipline**: most agentic failures aren't intelligence failures, they're discipline failures. This skill kills the classic ones:

| Failure mode you know | What the skill does |
|---|---|
| *"Done!"* — but nothing was verified | **PROOF**: no completion claims without running the check and reading the output |
| Patch on patch on patch, each "almost worked" | **TWO STRIKES**: 2 failed attempts on one hypothesis → full stop, root-cause protocol |
| Fixed one copy of duplicated code | **READ**: grep for twins and call sites before the first edit |
| Solved a different task than you asked | **GOAL**: one verifiable "done when…" line, re-anchored mid-session |
| Quietly guessed instead of asking | **CALIBRATION**: every claim tagged Verified / Inferred / Assumed |
| "Improved" things nobody asked for | **STEPS**: stop when the goal is met; scope boundary rules |

Why this works — the research the skill is built on:

- Scaffolding gives mid-tier models double-digit gains: **Reflexion lifts HumanEval 80%→91%** on the same model ([Shinn et al., 2023](https://arxiv.org/abs/2303.11366)); GPT-3.5 in an agentic loop (95.1%) beats GPT-4 zero-shot (67%).
- The critical caveat that shaped every line: **"think again" without new external evidence makes models *worse*** ([Huang et al., ICLR 2024](https://arxiv.org/abs/2310.01798)). Every self-check in this skill is therefore anchored to an external signal — a command, an output, code actually read. Never re-think; re-check.

## What's inside

```
SKILL.md                    # the 8 procedures + calibration by task size (English)
references/debugging.md     # full root-cause protocol (activated after "two strikes")
references/self-review.md   # three-lens hostile review of your own diff
ru/                         # the full Russian original (installable as fable-thinking-ru)
```

## Usage

**As a session skill** — installed as above, it triggers on non-trivial tasks automatically, or on demand: *"think like Fable"*, *"fable-mode"*.

**For subagents (the sweet spot):** when dispatching a subagent on a cheaper model, add one line to its prompt:

```
Before doing anything else, read ~/.claude/skills/fable-thinking/SKILL.md
and follow its discipline throughout this task.
```

**Per-project install:** drop the folder into `.claude/skills/fable-thinking/` inside the repo and commit — the whole team gets it.

## When NOT to use it

- **Trivial edits** (rename, typo, one-liner) — the skill says so itself; ceremony is calibrated to task size.
- **On Fable 5 itself.** Anthropic's guide is explicit: skills written for prior models are *"often too prescriptive for Claude Fable 5 and can degrade output quality"*. This skill is for the models below it.

## Honest limitations

- On short, well-specified tasks Sonnet 5 is already disciplined — our A/B runs saturated (both arms ~10/10). The payoff is on **long, messy, ambiguous tasks** — consistent with Anthropic's own framing: *"the longer and more complex the task, the larger Fable 5's lead."*
- This is discipline transfer, not capability transfer. It won't add knowledge or extend a model's planning horizon.

## Provenance

Distilled from: the official [Prompting Claude Fable 5 guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5), the official [Claude Code prompt library](https://code.claude.com/docs/en/prompt-library) (each ritual item maps to an officially blessed pattern — the mapping table is in SKILL.md), the Iron-Law patterns popularized by [obra/superpowers](https://github.com/obra/superpowers), and the research cited above.

*Community project. Not affiliated with or endorsed by Anthropic. Claude is a trademark of Anthropic, PBC.*

## License

[MIT](LICENSE). Use it, fork it, ship it with your team.

---

**If your Opus/Sonnet sessions stopped saying "done" for things that weren't — that's the skill working. Star ⭐ the repo so others find it, and open an issue with your before/after stories: real-world reports are what improves the next version.**
