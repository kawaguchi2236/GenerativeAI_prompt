# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is a prompt library, not a software project — there is no source code, build system, package manifest, linter, or test suite. The repository stores reusable Japanese-language prompt templates (`.md` files) intended to be pasted into LLM-based tools (Microsoft Copilot for PowerPoint, ChatGPT, Claude, etc.) for two recurring business tasks:

1. Redesigning/improving PowerPoint (PPTX) decks.
2. Analyzing Wakucone PC-usage-monitoring CSV exports and turning them into a customer-facing report plus a follow-up PPTX-generation prompt.

There is nothing to compile, lint, or test. "Working in this repo" means editing or adding Markdown prompt files.

## Structure

```
Copilot/
  pptxリメイク.md                              Standalone prompt: redesign an existing PPTX deck
  wakucone分析.md                              Master instruction doc (v4): Wakucone CSV → Markdown report + PPTX prompt
  ver/wakucone_stage1_markdown_analysis_prompt.md   An earlier/alternate variant of the Wakucone stage-1 prompt
```

- `pptxリメイク.md` is self-contained: role, goals, a slide-improvement methodology (one message per slide, chart/table/color rules, layout flow), and required final output sections.
- `wakucone分析.md` and `ver/wakucone_stage1_markdown_analysis_prompt.md` are two versions of the same "stage 1" prompt. `wakucone分析.md` is the more complete/current version (it adds sample-data detection, a `pandas`-based CSV reading snippet, additional banned-phrase rules, and a fuller PowerPoint-prompt template at the end). Treat `wakucone分析.md` as canonical when reconciling differences; `ver/...` appears to be a superseded draft kept for reference.

## Conventions when editing these prompts

- **Language**: all prompt content is Japanese. Keep additions/edits in Japanese and match the existing tone (formal, business-consulting register — です/ます調).
- **Two-stage Wakucone pipeline is load-bearing**: stage 1 (`wakucone分析.md`) explicitly must *not* produce a PPTX — it only produces a Markdown analysis report plus a PowerPoint-generation prompt for a separate stage 2 (Copilot for PowerPoint). Don't blur this boundary when modifying the prompt.
- **Banned/required phrasing rules are part of the spec, not incidental text**: e.g. customer-facing output must avoid accusatory/surveillance language (`問題です`, `違反です`, `怪しいです`, `監視対象です`, etc.) and use hedged alternatives (`〜の可能性があります`, `〜の傾向が見られます`); the heading "顧客と確認すべき仮説" is explicitly disallowed in favor of "確認すべき仮説" / "今後の確認ポイント". When editing these files, preserve and extend these allow/deny phrase lists rather than overriding them.
- **Structural templates are exact specs**: section orders (the final Markdown report structure, the recommended slide deck structure, the embedded PowerPoint-prompt template) are explicit deliverable contracts other prompts/users depend on — don't reorder or drop sections silently.
- **Embedded Python snippets** (`pandas`-based CSV readers in `wakucone分析.md`) are illustrative reference code for the LLM to follow when a code interpreter is available, not an executable module in this repo — there's no Python project/dependencies to install.

## Working with git

No CI, hooks, or branch protections are configured beyond normal git. Make focused commits to the relevant `.md` file(s) under `Copilot/`.
