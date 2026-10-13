# karpathy-skill

A Muse skill that answers with **Andrej Karpathy's distilled mental models, workflows, and tone** — for questions about using LLMs, doing AI research, and building with AI agents.

> Distilled, not impersonated: this skill captures how Karpathy thinks and works, not a parody of how he talks.

## What's inside

| File | Content |
|---|---|
| `SKILL.md` | Lean trigger + 6-step workflow (route to a mental model → conclusion first → mechanism → workflow → honest uncertainty → bilingual tone) |
| `references/mental-models.md` | 7 mental models: lossy-zip-file view of LLMs, ghosts-not-animals, jagged intelligence via verifiability, LLM-as-OS, Software 1.0/2.0/3.0, build-to-understand, power-to-the-people |
| `references/how-he-uses-llms.md` | Daily LLM workflows: context-window discipline, thinking vs non-thinking models, LLM council, deep research, code interpreter, vibe → agentic engineering, LLM wiki pattern — plus the **5 X posts verified Oct 2026** (capability-funnel, "Land or Water?" eval, bespoke video explainers, Opus 5 three.js render, voice-ramble "mind meld") |
| `references/autoresearch-loop.md` | The overnight autonomous-research loop (program.md / train.py / prepare.py, 5-min timebox, single comparable metric, elegance preference, anti-cagy measures) |
| `references/signature-phrasing.md` | His sentence DNA: openers, uncertainty register, anti-patterns |

## How to trigger it

Tell your agent: `karpathy mode`, 「用 karpathy 的视角看…」, or "from Karpathy's perspective". It does not fire on ordinary AI questions.

## Install

Copy this repo's contents into your skills directory:

```bash
cp -r karpathy-skill ~/.claude/skills/karpathy/
```

## Source provenance

Every reference file marks its sources: **primary** (his blog posts, YouTube lectures, podcast appearances), **X digest** (his recent @karpathy posts, directly verified 2026-10-13), and **secondary** (news/forum coverage of his X threads, clearly labeled). All quotes are brief excerpts used for distillation.

## Related

- [idea-digger](https://github.com/chendahe666/idea-digger) — an overnight autonomous idea-discovery skill built on Karpathy's autoresearch design (same author)
