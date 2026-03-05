---
title: "Claude Code Customization Guide: Rules vs Skills vs Subagents vs MCPs"
source: "https://marioottmann.com/articles/claude-code-customization-guide"
author:
  - "[[Mario Ottmann]]"
published: 2026-01-05
created: 2026-01-20
description: "Master Claude Code's four customization layers. Practical examples for design systems, security audits, backend architecture, and content workflows."
tags:
  - "clippings"
image: "https://marioottmann.com/images/articles/claude-code-customization-guide.webp"
---
## Claude Code Customization: When to Use Rules, Skills, Subagents, and MCPs

tl;dr

Claude Code has four internal configuration layers: **CLAUDE.md** (master config), **Rules** (domain knowledge), **Skills** (workflows), and **Subagents** (autonomous tasks). Plus **MCPs** as external bridges to tools like GitHub, databases, and Slack. Internal layers shape how Claude thinks; MCPs extend what Claude can reach.

Claude Code is powerful out of the box, but its real potential unlocks when you customize it for your specific workflow. After months of building with it, I've developed a mental model for when to use each customization layer.

The problem? Most developers throw everything into rules files and wonder why Claude ignores half of them. Or they skip customization entirely and re-explain their stack every session.

This guide breaks down the five customization layers with concrete examples from real projects.

Before diving into specifics, here's the mental model. Claude Code customization splits into two categories:

These shape how Claude thinks, what it knows, and how it works:

| Layer | Trigger | Best For | Example |
| --- | --- | --- | --- |
| **CLAUDE.md** | Always loaded | Master config, standards, quick reference | Framework conventions, skill/rule index |
| **Rules** | Path-based or always | Domain-specific knowledge | Design system, security, database patterns |
| **Skills** | User-invoked (`/skill`) | Repeatable workflows | Backend architecture, legal page generation |
| **Subagents** | Claude-invoked | Autonomous complex tasks | Security audits, codebase exploration |

MCPs extend what Claude can reach beyond your codebase:

| Layer | Trigger | Best For | Example |
| --- | --- | --- | --- |
| **MCPs** | Tool availability | External tool access | GitHub, Postgres, Slack, Linear |

Think of it this way: the four internal layers configure Claude's brain—its knowledge, workflows, and capabilities. MCPs are the hands that let Claude interact with the outside world.

The `CLAUDE.md` file is the foundation of your Claude Code setup. It's always loaded—every conversation, every task—making it the place for project-wide context and a quick reference to your other customizations.

**Perfect for:**

- Framework conventions (Next.js 14, React patterns)
- Code quality standards (DRY, documentation, testing)
- Accessibility requirements
- CI/CD expectations
- An index of available skills, rules, and subagents

**Not ideal for:**

- Deep domain knowledge (use Rules)
- Step-by-step workflows (use Skills)
- File patterns that only matter sometimes (use path-based Rules)

Here's a condensed version of a production CLAUDE.md:

```markdown
# Next.JS Rules

 

## Architecture & Routing

- Use Next.js 14 with App Router (not pages router)

- Use middleware for auth and security checks

- Prefer server-side rendering for SEO

 

## Server Components & Actions

- Use Server Actions for data mutations

- Add "use server" at top of action files

 

## Client Components

- Add "use client" to interactive components

- Use useMemo/useCallback for performance

 

# Skills & Subagents

 

## Available Skills

- \`/landing-page-builder\` - Hero sections, pricing, testimonials

- \`/backend-architecture\` - Supabase vs Vercel vs Server Actions

- \`/legal-pages\` - Imprint, terms, privacy (DE/EN)

 

## Project Rules (auto-loaded by path)

- \`security.md\` - Security headers, CSP, XSS prevention

- \`database-supabase.md\` - RLS, API security, rate limiting

- \`design-system.md\` - Layout, typography, colors

- \`tailwind-v4.md\` - v4 syntax and utilities

 

## Subagents

- \`security-auditor\` - Comprehensive security reviews

 

# Code Quality

 

- **DRY**: Refactor duplicate code into reusable functions

- **Documentation**: Document complex code with docstrings

- **Testing**: Unit tests first, integration tests for critical paths

- **Apostrophes**: Escape with &amp;apos; in JSX

 

# Accessibility

 

- All UI keyboard-navigable and screen-reader friendly

- Don&apos;t rely on color alone to convey information

- Ensure sufficient contrast ratios

- Label all form fields
```

Without a CLAUDE.md, you'll repeat yourself constantly:

- "Remember, we use Next.js 14 App Router..."
- "Don't forget the accessibility requirements..."
- "What skills do we have again?"

With a well-structured CLAUDE.md, Claude knows your stack, your standards, and where to find deeper knowledge (your Rules) from the first message.

| CLAUDE.md | Rules |
| --- | --- |
| Always loaded | Can be path-filtered |
| Project-wide standards | Domain-specific depth |
| Quick reference format | Detailed documentation |
| Single file | Multiple focused files |
| "How we work here" | "How to do X specifically" |

Think of CLAUDE.md as the employee handbook everyone reads on day one. Rules are the detailed documentation for specific departments.

Rules are markdown files in `.claude/rules/` that provide detailed, domain-specific context. Unlike CLAUDE.md (which is always loaded), rules can be **path-filtered** —only loading when you're working on relevant files.

Rules can specify which files trigger them using frontmatter:

```markdown
---

paths:

  - src/app/api/**/*

  - src/lib/supabase/**/*

  - supabase/**/*

  - "**/*.sql"

---

# Database & Supabase

 

## RLS Policies

Always enable RLS on ALL tables...
```

This rule only loads when Claude is working on database-related files. No need to clutter context when you're just editing CSS.

**Perfect for:**

- Design systems with full color palettes and component patterns
- Security requirements with code examples
- Database patterns, RLS policies, and query optimization
- Framework-specific details (Tailwind v4 utility mappings)

**Not ideal for:**

- Project-wide standards (use CLAUDE.md)
- Step-by-step workflows (use Skills)
- Complex multi-step operations (use Subagents)
- External tool access (use MCPs)

Here's a condensed version of a design system rule file:

```markdown
# Design System

 

## Layout Principles

- **Fixed Content Width**: Always use \`.constrained-layout\` wrapper

- Container: 56.25rem (900px) on desktop

- **Mobile-first**: Start with mobile styles, add \`md:\` / \`lg:\` breakpoints

 

## Typography

- **Body Font**: Geist (via next/font/google)

- **Headings**: Cooper BT (serif)

- **Hero**: Use \`.hero-title\` class (responsive scaling)

 

## Colors

### Brand Colors

- **Income Blue**: \`#3875f4\` - Primary CTAs, links

- **Vibe Orange**: \`#ff7a1c\` - Accents, highlights

 

### Background & Text

- **Background**: \`#181818\` (dark) or \`bg-brand-gradient\`

- **Text Primary**: \`#f2f5fa\` (light cream)
```

With this rule active, every UI task Claude performs follows these conventions. No need to re-explain "use income blue for buttons" every time.

Security rules ensure Claude never introduces vulnerabilities:

```markdown
# Security Requirements

 

## XSS Prevention

**CRITICAL:** Never use \`dangerouslySetInnerHTML\` without sanitization.

 

✅ Always sanitize with DOMPurify:

const cleanHtml = DOMPurify.sanitize(userContent, {

  ALLOWED_TAGS: [&apos;p&apos;, &apos;br&apos;, &apos;strong&apos;, &apos;em&apos;, &apos;a&apos;],

  ALLOWED_ATTR: [&apos;href&apos;, &apos;target&apos;, &apos;rel&apos;],

});

 

## Content Security Policy

When adding third-party services:

1. Identify required directives (script-src, frame-src, etc.)

2. Add domains explicitly (never use wildcards)

3. Test in browser console for CSP violations

4. Document why each domain is needed
```

Now Claude automatically sanitizes HTML and updates CSP headers when adding embeds.

Claude Code uses two locations: **project-level** (`.claude/`) and **global** (`~/.claude/`).

**Project-level** (`.claude/` in your repo):

```
.claude/
├── rules/
│   ├── design-system.md      # UI conventions (no path filter)
│   ├── security.md           # Security requirements (no path filter)
│   ├── database-supabase.md  # Database patterns (path: **/api/**, *.sql)
│   └── tailwind-v4.md        # Framework specifics (no path filter)
└── agents/
    └── linkedin-to-mdx-blog.md  # Project-specific agent
```

**Global** (`~/.claude/` shared across projects):

```
~/.claude/
├── CLAUDE.md                 # Master config (always loaded)
├── skills/
│   ├── backend-architecture/
│   │   ├── SKILL.md
│   │   ├── examples.md
│   │   └── reference.md
│   └── legal-pages/
│       ├── SKILL.md
│       ├── templates.md
│       └── questions.md
└── agents/
    ├── security-auditor.md
    └── code-refactorer.md
```

**Path filtering strategy:**

- **Always load**: Design system, security (you always want these)
- **Path-filtered**: Database rules (only when touching DB files)

Skills are slash commands that expand into full prompts. They're perfect for workflows you repeat but don't want running automatically.

**Perfect for:**

- Architecture decisions that need user confirmation
- Content generation workflows
- Legal/compliance page generation
- Deployment checklists
- Code review workflows

**Not ideal for:**

- Always-on context (use Rules)
- Fully autonomous operations (use Subagents)
- External API integrations (use MCPs)

Skills live in `~/.claude/skills/` (global) or `.claude/skills/` (project-level). Each skill is a **directory** containing a main `SKILL.md` file plus optional supporting files:

```
~/.claude/skills/
├── backend-architecture/
│   ├── SKILL.md       ← Main skill definition
│   ├── examples.md    ← Code templates and patterns
│   └── reference.md   ← Advanced patterns, edge cases
├── legal-pages/
│   ├── SKILL.md
│   ├── templates.md   ← Page templates
│   └── questions.md   ← Required information checklist
└── landing-page-builder/
    ├── SKILL.md
    ├── examples.md
    └── reference.md
```

Splitting skills into multiple files keeps each file focused and maintainable:

- **SKILL.md**: Core decision logic and quick reference (what Claude needs immediately)
- **examples.md**: Production-ready code templates (loaded when implementing)
- **reference.md**: Advanced patterns and edge cases (loaded when needed)
- **templates.md**: Reusable content structures
- **questions.md**: Information gathering checklists

This prevents bloated skill files and lets Claude load only what's needed for each step.

**Directory:**`~/.claude/skills/backend-architecture/`

```
backend-architecture/
├── SKILL.md       ← Decision framework, quick reference table
├── examples.md    ← 5 production-ready code templates
└── reference.md   ← Security patterns, testing strategies
```

**SKILL.md frontmatter:**

```yaml
---

name: backend-architecture

description: Decides whether to use Supabase Edge Functions, Vercel Functions, or Server Actions.

allowed-tools: Read, Grep, Write, Edit, Glob

---
```

**SKILL.md content structure:**

```markdown
# Backend Architecture Decisions

 

## When to Use This Skill

- Implementing new API endpoints

- Adding webhook handlers

- Creating database operations

- Unclear where server logic should live

 

## Quick Decision Tree

 

Need RLS bypass (service role)?

├─ YES → Supabase Edge Function

└─ NO ↓

 

Database trigger? (on signup, insert, update)

├─ YES → Supabase Edge Function

└─ NO ↓

 

Simple UI mutation? (form, toggle, like)

├─ YES → Server Action

└─ NO ↓

 

External API/webhook? (Stripe, SendGrid, OpenAI)

├─ YES → Vercel Function

└─ NO → Server Action (default)

 

## Quick Reference Table

| Scenario | Use | Why |

|----------|-----|-----|

| User signup hook | Supabase Edge | Database trigger, RLS bypass |

| Stripe webhook | Vercel Function | Needs public URL |

| Like button | Server Action | Simple mutation |

...
```

The `examples.md` file contains complete, production-ready implementations for each pattern (Supabase Edge Functions, Vercel Functions, Server Actions). The `reference.md` covers security checklists and advanced patterns.

Usage: Type `/backend-architecture` and Claude walks through the decision with you.

**Directory:**`~/.claude/skills/legal-pages/`

```
legal-pages/
├── SKILL.md        ← Generation workflow
├── templates.md    ← Page templates (Imprint, Terms, Privacy)
└── questions.md    ← Required information checklist
```

**SKILL.md frontmatter:**

```yaml
---

name: legal-pages

description: Generates Imprint, Terms & Conditions, and Privacy Policy pages with German and English versions.

allowed-tools: Read, Grep, Write, Edit, Glob

---
```

The skill analyzes your codebase first, identifies what legal pages exist, then asks only for missing required information before generating compliant pages.

**Directory:**`~/.claude/skills/landing-page-builder/`

```
landing-page-builder/
├── SKILL.md       ← Section options, process flow
├── examples.md    ← Component examples for each section type
└── reference.md   ← Design system integration patterns
```

**SKILL.md frontmatter:**

```yaml
---

name: landing-page-builder

description: Builds complete landing page sections including hero, features, testimonials, pricing, and FAQ.

allowed-tools: Read, Grep, Write, Edit, Glob

---
```

The main SKILL.md defines available sections (Hero, Features, Testimonials, Pricing, FAQ) and the generation process. Supporting files contain component templates that follow your design system.

Subagents are specialized Claude instances that handle complex, multi-step tasks autonomously. They're invoked by Claude (not you) when a task matches their specialty.

**Perfect for:**

- Security audits across entire codebase
- Codebase exploration and understanding
- Implementation planning
- Code refactoring at scale
- Content transformation workflows

**Not ideal for:**

- Simple, single-step operations
- Tasks requiring constant user input (use Skills)
- Persistent context (use Rules)

Claude Code ships with three built-in subagents:

| Subagent | Model | Purpose | When Claude Uses It |
| --- | --- | --- | --- |
| `Explore` | Haiku (fast) | Read-only codebase search | "How does X work?", "Find where Y is defined" |
| `Plan` | Sonnet | Implementation planning | Complex features needing architecture research |
| `General-purpose` | Sonnet | Multi-step complex tasks | Tasks requiring both exploration and modification |

**Explore** is optimized for speed—it uses Haiku and can only read files, not modify them. Claude uses it when you ask questions about your codebase.

**Plan** activates when you enter plan mode. It researches your codebase before presenting an implementation approach for approval.

**General-purpose** handles complex multi-step tasks that need both reading and writing. Claude delegates here when a task has interdependent steps.

The real power comes from defining your own specialized subagents. These are configured in your Claude Code settings and automatically invoked when tasks match their description.

**Example: Security Auditor Subagent**

```markdown
# security-auditor

 

Use this agent for comprehensive security review including:

- Server functions (Vercel and Supabase Edge Functions)

- Database queries and RLS policies

- Frontend API calls and exposed endpoints

- Authentication flows

- HTTPS headers and CSP

- Dependency vulnerabilities (npm audit)

 

Invoke after implementing features that touch authentication,

authorization, data access, or external integrations.
```

With this configured, after you say "I just implemented login," Claude recognizes the security implications and automatically launches your security auditor.

**Example: Content Transformer Subagent**

```markdown
# linkedin-to-mdx-blog

 

Transforms LinkedIn posts into comprehensive, AEO-optimized

evergreen guides. Uses the post as a seed idea, not content

to expand.

 

## Process

1. Fetch LinkedIn post content

2. Extract core concept

3. Research topic for comprehensive coverage

4. Generate 2000+ word authoritative guide

5. Format as MDX with project components
```

When you share a LinkedIn URL and ask for a blog post, Claude matches this to your custom subagent.

Custom subagents are powerful for:

- **Domain-specific audits**: Accessibility review, performance analysis
- **Content workflows**: Documentation generation, changelog creation
- **Code analysis**: Dependency updates, migration planning

Model Context Protocol (MCP) servers connect Claude to external tools and services. They extend what Claude can do beyond the local filesystem.

**Perfect for:**

- Project management systems
- Database administration
- External APIs
- CI/CD pipelines
- Documentation systems

**Not ideal for:**

- Local file operations (built-in tools handle this)
- Codebase context (use Rules)
- Workflows (use Skills or Subagents)

Here are some widely-used MCP servers you can add to Claude Code:

**GitHub MCP** - Repository management, PRs, issues:

```bash
claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

**Postgres MCP** - Direct database queries and schema inspection:

```bash
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres
```

**Puppeteer MCP** - Browser automation and web scraping:

```bash
claude mcp add puppeteer -- npx -y @anthropic/mcp-server-puppeteer
```

**Slack MCP** - Send messages and read channels:

```bash
claude mcp add slack -- npx -y @anthropic/mcp-server-slack
```

With MCPs configured, Claude gains new capabilities:

| MCP | What Claude Can Do |
| --- | --- |
| GitHub | Create PRs, manage issues, review code across repos |
| Postgres | Query databases, inspect schemas, run migrations |
| Puppeteer | Screenshot pages, fill forms, test UI flows |
| Slack | Post updates, read conversations, search messages |
| Linear | Create issues, update status, manage sprints |

MCPs are configured in your Claude Code settings (`~/.claude/settings.json`):

```json
{

  "mcpServers": {

    "github": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-github"],

      "env": {

        "GITHUB_TOKEN": "your-github-token"

      }

    },

    "postgres": {

      "command": "npx",

      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]

    }

  }

}
```

1. #### Is it a project-wide standard or quick reference?
2. #### Is it deep domain knowledge?
3. #### Is it a repeatable workflow you trigger?
4. #### Is it a complex autonomous task?
5. #### Does it need external tool access?

The real power comes from combining all five layers:

```
CLAUDE.md:  Next.js conventions, React patterns, accessibility standards
Rules:      design-system.md, tailwind-v4.md
Skills:     /landing-page-builder
Subagents:  Explore (built-in)
MCPs:       None typically needed
```
```
CLAUDE.md:  Next.js conventions, code quality, testing requirements
Rules:      database-supabase.md (path-filtered), security.md
Skills:     /backend-architecture
Subagents:  Plan (built-in), security-auditor (custom)
MCPs:       Postgres, GitHub
```
```
CLAUDE.md:  Code quality, accessibility (for generated pages)
Rules:      design-system.md (for consistent formatting)
Skills:     /legal-pages
Subagents:  linkedin-to-mdx-blog (custom)
MCPs:       None typically needed
```

Anti-patterns

**1\. Skipping CLAUDE.md entirely** Without a master config, you repeat project standards every session. Create a CLAUDE.md with framework conventions and an index of your skills/rules.

**2\. Putting everything in CLAUDE.md** CLAUDE.md should be a quick reference, not a 500-line document. Move deep domain knowledge to separate rule files.

**3\. Putting workflows in rules** Rules should be context, not instructions. "Always use DOMPurify" is a rule. "When generating legal pages, do X, Y, Z" is a skill.

**4\. Not using path-based filtering** Loading database rules when editing CSS wastes context. Use frontmatter paths to load rules only when relevant.

**5\. Using skills for autonomous tasks** If a task doesn't need user input during execution, it should probably be a subagent that Claude invokes automatically.

**6\. Skipping MCPs for external integrations** Trying to hack external access through bash commands is fragile. MCPs provide proper tool interfaces.

1. #### Audit your current setup
2. #### Create CLAUDE.md first
3. #### Add domain-specific rules
4. #### Add skills for common workflows
5. #### Let subagents emerge naturally
6. #### Add MCPs for integrations

**Internal layers** (Claude's configuration):

- **CLAUDE.md** — Master config with project-wide standards and a quick reference index
- **Rules** — Deep domain knowledge with optional path-based filtering
- **Skills** — User-triggered workflows for repeatable multi-step processes
- **Subagents** — Autonomous specialists for complex tasks Claude invokes when needed

**External layer** (bridges to the outside):

- **MCPs** — Connect Claude to external tools like GitHub, databases, and Slack

**Getting started:** Build your internal config first (CLAUDE.md → Rules → Skills → Subagents), then add MCPs when you need to reach outside your codebase.

The goal isn't to use all five layers everywhere. It's to use the right layer for the right job. A well-customized Claude Code setup feels like having a senior developer who already knows your codebase, follows your conventions, and can reach any tool they need.

---

This article was inspired by content originally written by Mario Ottmann. The long-form version was drafted with the assistance of Claude Code AI and subsequently reviewed and edited by the author for clarity and style.