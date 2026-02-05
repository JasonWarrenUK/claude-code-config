---
description: "{{ OPUS }} Probe the project for weaknesses"
argument-hint: [focus of analysis]
model: claude-opus-4-5
---

<overview>
  Identify weaknesses in this codebase's implementation.
</overview>
<steps>
  1. Analyse the codebase as a developer would when reading unfamiliar code.
  2. If $ARGUMENTS contains content, focus analysis on that area.
  3. Provide practical overview of weaknesses in implemented code only (not missing features).
</steps>
<inputs>
  $ARGUMENTS
</inputs>
