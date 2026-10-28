---
name: scientific-agen
description: Use for scientific research, literature review, experiment design, scientific data analysis, and publication figures through the locally downloaded K-Dense scientific-agent-skills collection.
---

# Scientific research skills

This is the user's single Codex entry point for the K-Dense scientific-agent-skills collection.

The source repository is `<SOURCE_ROOT>`.

1. Search or read `<SOURCE_ROOT>/docs/skills.md` for the capabilities relevant to the user's task.
2. Read the matching `<SOURCE_ROOT>/skills/<skill-directory>/SKILL.md` before applying it. Load only the skills needed for the current task.
3. Resolve each selected skill's relative references, scripts, and assets against that selected skill's original directory, not this entry point. Use absolute paths when invoking its scripts from a different working directory.
4. Apply the selected workflow within the user's task and available tools. Check the selected skill's dependencies when needed; loading this entry point does not install packages, configure credentials, or enable external services.

If the source repository is missing, explain which path is unavailable and ask for its new location. Do not claim to have used instructions that could not be read.

Upstream source: https://github.com/K-Dense-AI/scientific-agent-skills (MIT).

## Portable source location

Resolve `<SOURCE_ROOT>` as `<CODEX_HOME>/skill-sources/scientific-agen`; when `CODEX_HOME` is unset, use `~/.codex/skill-sources/scientific-agen`. Expand the home directory and environment variable before reading files. If the user supplies another checkout, use it. Dependencies are installed separately following the repository README; never assume the source is bundled.
