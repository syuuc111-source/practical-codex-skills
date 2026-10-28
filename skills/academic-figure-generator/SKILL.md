---
name: academic-figure-generator
description: Use when preparing academic paper figures, architecture diagrams, method pipelines, or detailed English prompts for scientific image generation, using the local Academic Figure Generator skills.
---

# Academic Figure Generator

Read and follow `<SOURCE_ROOT>/academic-figure-prompt/SKILL.md` for the user's figure task.

When the user specifically requests the pastel variant, read `<SOURCE_ROOT>/academic-figure-prompt-pastel/SKILL.md` instead. Resolve relative resources against the selected original skill directory.

These upstream skills produce academic figure prompts. For actual image generation, use an available image-generation capability when the user requests it. Do not claim that installing this entry point provides an image-generation API or runs the repository's web application.

If the source files are unavailable, report the missing path and ask for the repository's new location.

Upstream source: https://github.com/LigphiDonk/academic-figure-generator (MIT).

## Portable source location

Resolve `<SOURCE_ROOT>` as `<CODEX_HOME>/skill-sources/academic-figure-generator`; when `CODEX_HOME` is unset, use `~/.codex/skill-sources/academic-figure-generator`. Expand the home directory and environment variable before reading files. If the user supplies another checkout, use it. Dependencies are installed separately following the repository README; never assume the source is bundled.
