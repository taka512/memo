---
title: "Introducing Agent HQ: Any agent, any way you work"
source: "https://github.blog/news-insights/company-news/welcome-home-agents/"
author:
  - "[[Kyle Daigle]]"
published: 2025-10-29
created: 2026-01-05
description: "At Universe 2025, GitHub's next evolution introduces a single, unified workflow for developers to be able to orchestrate any agent, any time, anywhere."
tags:
  - "clippings"
image: "https://github.blog/wp-content/uploads/2025/10/UniverseBlogHeader_07.jpg"
---
The current AI landscape presents a challenge we’re all too familiar with: incredible power fragmented across different tools and interfaces. At GitHub, we’ve always worked to solve these kinds of systemic challenges—by making Git accessible, code review systematic with pull requests, and automating deployment with Actions.

With 180 million developers, [**GitHub is growing at its fastest rate ever**](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/) —a new developer joining every second. What’s more, 80% of new developers are using Copilot in their first week. AI isn’t just a tool anymore; it’s an integral part of the development experience. Our responsibility is to ensure this new era of collaboration is powerful, secure, and seamlessly integrated into the workflow you already trust.

At GitHub Universe, we’re announcing **Agent HQ**, GitHub’s vision for the next evolution of our platform. Agents shouldn’t be bolted on. They should work the way you already work. **That’s why we’re making agents native to the GitHub flow.**

Agent HQ transforms GitHub into an open ecosystem that unites every agent on a single platform. **Over the coming months, coding agents from Anthropic, OpenAI, Google, Cognition, xAI, and more will become available directly within [GitHub as part of your paid GitHub Copilot subscription](https://github.com/features/copilot/plans?utm_source=blog-day1-recap-copilot-cta&utm_medium=blog&utm_campaign=universe25)**.

![](https://www.youtube.com/watch?v=KniyIrpTDE8)

To bring this vision to life, we’re shipping a suite of new capabilities built on the primitives you trust. This starts with a **mission control**,a single command center to assign, steer, and track the work of multiple agents from anywhere. It extends to **VS Code** with new ways to plan and customize agent behavior. And it is backed by enterprise-grade functionality: a new generation of **agentic code review**,a dedicated **control plane** to govern AI access and agent behavior, and a **metrics dashboard** to understand the impact of AI on your work.

**We are also deeply committed to investing in our platform and strengthening the primitives you rely on every day**. This new world of development is powered by that foundational work, and we look forward to sharing more updates.

Let’s dive in.

## GitHub is your Agent HQ: An open ecosystem for all agents

The future is about giving you the power to orchestrate a fleet of specialized agents to perform complex tasks in parallel, not juggling a patchwork of disconnected tools or relying on a single agent. As the pioneer of asynchronous collaboration, we believe it’s our responsibility to make sure these next-generation async tools *just work*.

With **Agent HQ** what’s *not* changing is just as important as what *is.* You’re still working with the primitives you know—Git, pull requests, issues—and using your preferred compute, whether that’s GitHub Actions or self-hosted runners. You’re accessing agents through your existing paid Copilot subscription.

On top of that foundation, we’re opening the doors to a new world of capability.Over the coming months, **coding agents from Anthropic, OpenAI, Google, Cognition, and xAI will be available on GitHub** **as part of your paid GitHub Copilot subscription**.

Don’t want to wait? Starting this week, Copilot Pro+ users can begin working with **OpenAI Codex in** [**VS Code Insiders**](https://code.visualstudio.com/insiders/), the first of our partner agents to extend beyond its native surfaces and directly into the editor.

!['Our collaboration with GitHub has always pushed the frontier of how developers build software. The first Codex model helped power Copilot and inspired a new generation of AI-assisted coding. We share GitHub’s vision of meeting developers wherever they work, and we’re excited to bring Codex to millions more developers who use GitHub and VS Code, extending the power of Codex everywhere code gets written.' 
- Alexander Embiricos, Codex Product Lead, OpenAI

'We’re partnering with GitHub to bring Claude even closer to how teams build software. With Agent HQ, Claude can pick up issues, create branches, commit code, and respond to pull requests, working alongside your team like any other collaborator. This is how we think the future of development works: agents and developers building together, on the infrastructure you already trust.' 
- Mike Krieger, Chief Product Officer, Anthropic

'The best developer tools fit seamlessly into your workflow, helping you stay focused and move faster. With Agent HQ, Jules becomes a native assignee, streamlining manual steps and reducing friction in everyday development. This deeper integration with GitHub brings agents closer to where developers already work, making collaboration more natural and efficient.'
- Kathy Korevec, Director of Product at Google Labs](https://github.blog/wp-content/uploads/2025/10/Quotes_v5.jpg?w=2796)

'Our collaboration with GitHub has always pushed the frontier of how developers build software. The first Codex model helped power Copilot and inspired a new generation of AI-assisted coding. We share GitHub’s vision of meeting developers wherever they work, and we’re excited to bring Codex to millions more developers who use GitHub and VS Code, extending the power of Codex everywhere code gets written.' - Alexander Embiricos, Codex Product Lead, OpenAI 'We’re partnering with GitHub to bring Claude even closer to how teams build software. With Agent HQ, Claude can pick up issues, create branches, commit code, and respond to pull requests, working alongside your team like any other collaborator. This is how we think the future of development works: agents and developers building together, on the infrastructure you already trust.' - Mike Krieger, Chief Product Officer, Anthropic 'The best developer tools fit seamlessly into your workflow, helping you stay focused and move faster. With Agent HQ, Jules becomes a native assignee, streamlining manual steps and reducing friction in everyday development. This deeper integration with GitHub brings agents closer to where developers already work, making collaboration more natural and efficient.' - Kathy Korevec, Director of Product at Google Labs

## Mission control: Your command center, wherever you build

The power of Agent HQ comes from **[mission control](https://github.blog/changelog/2025-10-28-a-mission-control-to-assign-steer-and-track-copilot-coding-agent-tasks/?utm_source=blog-day1-recap-mission-control-cta&utm_medium=blog&utm_campaign=universe25)**, a unified command center that follows you wherever you work. It’s not a single destination; it’s a consistent interface across GitHub, VS Code, mobile, and the CLI that lets you direct, monitor, and manage every AI-driven task. With mission control, you can choose from a fleet of agents, assign them work in parallel, and track their progress from any device.

We’re also providing:

- New **branch controls** that give you granular oversight over when to run CI and other checks for agent-created code.
- **Identity features** to control which agent is building the task, managing access, and policies just like you would with any other developer on your team.
- **One-click merge conflict resolution**, improved file navigation, and better code commenting capabilities.
- **New integrations for Slack and Linear,** on top of our recently announced connections for Atlassian Jira, Microsoft Teams and Azure Boards, and Raycast.
![Logos for Slack, Linear, Microsoft Teams, VS Code, Azure Boards, Jira, and Raycast.](https://github.blog/wp-content/uploads/2025/10/BlogImage_LogoWall_02.jpg?w=2960)

Logos for Slack, Linear, Microsoft Teams, VS Code, Azure Boards, Jira, and Raycast.

<video height="1080" width="1878" controls="" src="https://github.blog/wp-content/uploads/2025/10/DotCom_MissionControl_v2.mp4"></video>

[**Try mission control today.**](https://github.blog/changelog/2025-10-28-a-mission-control-to-assign-steer-and-track-copilot-coding-agent-tasks/?utm_source=blog-day1-recap-mission-control-cta&utm_medium=blog&utm_campaign=universe25)

## New in VS Code: Plan, customize, and connect

Mission control is in VS Code, too, so you’ve got a single view of all your agents running in VS Code, in the Copilot CLI, or on GitHub.

Today’s **brand new release in VS Code** is all about working alongside agents on projects, and it’s not surprising that great results start with a great plan. Getting the context right before a project is critical, but that same context needs to carry through into the work. Copilot already adapts to the way your team works by learning from your files and your project’s culture, but sometimes you need more pointed context.

So today, we’re introducing **Plan Mode**, which works with Copilot, and asks you clarifying questions along the way, to help you to build a step-by-step approach for your task. Providing the context upfront improves what Copilot can do and helps you find gaps, missing decisions, or project deficiencies early in the process—before any code is written. Once you approve, your plan goes to Copilot to start implementing, whether that’s locally in VS Code or using an agent in the cloud.

<video height="1896" width="3024" controls="" src="https://github.blog/wp-content/uploads/2025/10/VScode_PlanMode_v2.mp4"></video>

For even finer control, you can now create custom agents in VS Code with **[AGENTS.md](https://code.visualstudio.com/docs/copilot/customization/custom-instructions#_use-an-agentsmd-file-experimental?utm_source=blog-day1-recap-agents-md-cta&utm_medium=blog&utm_campaign=universe25)** files, source-controlled documents that let you set clear rules and guardrails such as “prefer this logger” or “use table-driven tests for all handlers.” This shapes Copilot’s behavior without you re-prompting it every time.

Now you can rely on the new **[GitHub MCP Registry](https://code.visualstudio.com/docs/copilot/customization/mcp-servers?utm_source=blog-day1-recap-mcp-registry-in-vs-code-cta&utm_medium=blog&utm_campaign=universe25), available directly in VS Code**. VS Code is the *only* editor that supports the full MCP specification. Discover, install, and enable MCP servers like Stripe, Figma, Sentry, and others, with a single click. When your task calls for a specialist, create custom agents in GitHub Copilot with their own system prompt and tools to help you define the ways you want Copilot to work.

<video height="1080" width="1878" controls="" src="https://github.blog/wp-content/uploads/2025/10/DotCom_MissionControl_v2_aed2be.mp4"></video>

Agent HQ doesn’t just give you more power—it gives you confidence. Ensuring code quality, understanding AI’s influence on your workflow, and maintaining control over how AI interacts with your codebase and organization are essential for your team’s success, and we’re tackling these challenges head-on.

When it comes to code quality, the core problem is that “LGTM” doesn’t always mean “the code is healthy.” A review can pass, but can still degrade the codebase and quickly become long-term technical debt. With **[GitHub Code Quality](https://github.blog/changelog/2025-10-28-github-code-quality-in-public-preview/?utm_source=blog-day1-recap-code-quality-cta&utm_medium=blog&utm_campaign=universe25)**, in public preview today, you’ve got org-wide visibility, governance, and reporting to systematically improve code maintainability, reliability, and test coverage across every repository. Enabling it extends Copilot’s security checks to look at the maintainability and reliability impact of the code that’s been changed.

And we’ve **added a code review step** into the Copilot coding agent’s workflow, too, so Copilot gets an initial first-line review and addresses problems (before you even see the code).

![Screenshot of GitHub Code Quality, showing the results of Copilot's review.](https://github.blog/wp-content/uploads/2025/10/BlogImage_CodeQuality_01.jpg?w=3004)

Screenshot of GitHub Code Quality, showing the results of Copilot's review.

As an organization, you need to know how Copilot is being used. So today, we’re announcing the public preview of the **[Copilot metrics dashboard](https://github.blog/changelog/2025-10-28-copilot-usage-metrics-dashboard-and-api-in-public-preview/?utm_source=blog-day1-recap-copilot-metrics-dashboard-cta&utm_medium=blog&utm_campaign=universe25)**, showing Copilot’s impact and critical usage metrics across your entire organization.

For enterprise administrators who are managing AI access, including AI agents and MCP, we’re focused on providing consistent **AI controls for teams with the [control plane](https://github.blog/changelog/2025-10-28-enterprise-ai-controls-the-agent-control-plane-are-in-public-preview/?utm_source=blog-day1-recap-control-plane-cta&utm_medium=blog&utm_campaign=universe25)** — **your agent governance layer**. Set security policies, audit logging, and manage access all in one place. Enterprise admins can also control which agents are allowed, define access to models, and obtain metrics about the Copilot usage in your organization.

## For developers, by developers

We built Agent HQ because we’re developers, too. We know what it’s like when it feels like your tools are *fighting* you instead of helping you. When “AI-powered” ends up meaning more context-switching, more babysitting, more subscriptions, and more time explaining what you need to get the value you were promised.

That ends today.

Agent HQ isn’t about the hype of AI. It’s about the reality of shipping code. It’s about bringing order and governance to this new era without compromising choice. It’s about giving *you* the power to build faster, with more confidence, and on your terms.

Welcome home. Let’s build.

## Tags:

## Written by

## Related posts

[News & insights](https://github.blog/news-insights/)

### The future of AI-powered software optimization (and how it can help your team)

We envision the future of AI-enabled tooling to look like near-effortless engineering for sustainability. We call it Continuous Efficiency.

[News & insights](https://github.blog/news-insights/)

### Let’s talk about GitHub Actions

A look at how we rebuilt GitHub Actions’ core architecture and shipped long-requested upgrades to improve performance, workflow flexibility, reliability, and everyday developer experience.

[Company news](https://github.blog/news-insights/company-news/)

### GitHub Availability Report: November 2025

In November, we experienced three incidents that resulted in degraded performance across GitHub services.