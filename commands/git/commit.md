---
description: "{{ Haiku }} Generate a commit message. If nothing staged, stage all changes."
model: claude-haiku-4-5
disable-model-invocation: true
---

## Steps
1. If no changes staged, stage all. Otherwise use existing staging.
2. Generate commit message per conventional commits format.
3. Show message and await approval:
    - If approved, push to upstream
    - If changes requested, revise and repeat

<template format-reference="https://www.conventionalcommits.org/en/v1.0.0/">
  `type(scope?): description\n\nbody (optional)\n\nBREAKING CHANGE: footer (if applicable)`
</template>
