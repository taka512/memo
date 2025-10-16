---
title: "AGENTS.md"
source: "https://agents.md/"
author:
published:
created: 2025-10-15
description: "AGENTS.md is a simple, open format for guiding coding agents, used by over 20k open-source projects. Think of it as a README for agents."
tags:
  - "clippings"
image: "https://agents.md/og.png"
---
## Why AGENTS.md?

README.md files are for humans: quick starts, project descriptions, and contribution guidelines.

AGENTS.md complements this by containing the extra, sometimes detailed context coding agents need: build steps, tests, and conventions that might clutter a README or aren’t relevant to human contributors.

We intentionally kept it separate to:

Give agents a clear, predictable place for instructions.

Keep READMEs concise and focused on human contributors.

Provide precise, agent-focused guidance that complements existing README and docs.

Rather than introducing another proprietary file, we chose a name and format that could work for anyone. If you’re building or using coding agents and find this helpful, feel free to adopt it.

## One AGENTS.md works across many agents

Your agent definitions are compatible with a growing ecosystem of AI coding agents and tools:

## Examples

```
# Sample AGENTS.md file ## Dev environment tips- Use \`pnpm dlx turbo run where <project_name>\` to jump to a package instead of scanning with \`ls\`.- Run \`pnpm install --filter <project_name>\` to add the package to your workspace so Vite, ESLint, and TypeScript can see it.- Use \`pnpm create vite@latest <project_name> -- --template react-ts\` to spin up a new React + Vite package with TypeScript checks ready.- Check the name field inside each package's package.json to confirm the right name—skip the top-level one. ## Testing instructions- Find the CI plan in the .github/workflows folder.- Run \`pnpm turbo run test --filter <project_name>\` to run every check defined for that package.- From the package root you can just call \`pnpm test\`. The commit should pass all tests before you merge.- To focus on one step, add the Vitest pattern: \`pnpm vitest run -t "<test name>"\`.- Fix any test or type errors until the whole suite is green.- After moving files or changing imports, run \`pnpm lint --filter <project_name>\` to be sure ESLint and TypeScript rules still pass.- Add or update tests for the code you change, even if nobody asked. ## PR instructions- Title format: [<project_name>] <Title>- Always run \`pnpm lint\` and \`pnpm test\` before committing.
```### [openai/codex](https://github.com/openai/codex/blob/-/AGENTS.md)

[

General-purpose CLI tooling for AI coding agents.

Rust

\+ 179

](https://github.com/openai/codex/blob/-/AGENTS.md)apache/airflow

Platform to programmatically author, schedule, and monitor workflows.

Python

\+ 3926

[View original](https://github.com/apache/airflow/blob/-/AGENTS.md)temporalio/sdk-java

Java SDK for Temporal, workflow orchestration defined in code.

Java

\+ 109

[View original](https://github.com/temporalio/sdk-java/blob/-/AGENTS.md)PlutoLang/Pluto

A superset of Lua 5.4 with a focus on general-purpose programming.

C++

\+ 8

[View original](https://github.com/PlutoLang/Pluto/blob/-/AGENTS.md)

[View 20k+ examples on GitHub](https://github.com/search?q=path%3AAGENTS.md&type=code)

## How to use AGENTS.md?

### 1\. Add AGENTS.md

Create an AGENTS.md file at the root of the repository. Most coding agents can even scaffold one for you if you ask nicely.

### 3\. Add extra instructions

Commit messages or pull request guidelines, security gotchas, large datasets, deployment steps: anything you’d tell a new teammate belongs here too.

### 4\. Large monorepo? Use nested AGENTS.md files for subprojects

Place another AGENTS.md inside each package. Agents automatically read the nearest file in the directory tree, so the closest one takes precedence and every subproject can ship tailored instructions. For example, at time of writing the main OpenAI repo has 88 AGENTS.md files.

## About

AGENTS.md emerged from collaborative efforts across the AI software development ecosystem, including [OpenAI Codex](https://openai.com/codex/),[Amp](https://ampcode.com/),[Jules from Google](https://jules.google/),[Cursor](https://cursor.com/), and [Factory](https://factory.ai/).

We’re committed to helping maintain and evolve this as an open format that benefits the entire developer community, regardless of which coding agent you use.