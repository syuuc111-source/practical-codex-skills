---
name: research-paper-writing
description: Use when drafting, revising, or reviewing research papers, improving section structure and paragraph flow, or checking that claims are supported by evidence, especially for ML, CV, and NLP papers.
---

# Research Paper Writing

Read and follow `<SOURCE_ROOT>/research-paper-writing/SKILL.md` for the current writing task.

Resolve the original skill's relative references against `<SOURCE_ROOT>/research-paper-writing/`. For example, its paper-review guide is `<SOURCE_ROOT>/research-paper-writing/references/paper-review.md`. Read only the references needed for the section or review requested by the user.

If the source files are unavailable, report the missing path and ask for the repository's new location.

Upstream source: https://github.com/Master-cai/Research-Paper-Writing-Skills (MIT), adapted from Prof. Peng Sida's open research notes.

## Portable source location

Resolve `<SOURCE_ROOT>` as `<CODEX_HOME>/skill-sources/research-paper-writing`; when `CODEX_HOME` is unset, use `~/.codex/skill-sources/research-paper-writing`. Expand the home directory and environment variable before reading files. If the user supplies another checkout, use it. Dependencies are installed separately following the repository README; never assume the source is bundled.
