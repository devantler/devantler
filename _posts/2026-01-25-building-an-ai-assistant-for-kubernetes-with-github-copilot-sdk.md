---
layout: post
title: "Building an AI Assistant for Kubernetes with GitHub Copilot SDK"
description: How I built KSail's interactive AI chat assistant using Go, GitHub Copilot SDK, and Bubbletea TUI framework — with help from Claude Opus 4.5 and VS Code agents.
image: "assets/images/ksail-copilot-chat.png"
---

What if managing Kubernetes clusters was as simple as having a conversation?

KSail v5 introduces `ksail chat` — an AI-powered assistant that lets you create, manage, and troubleshoot Kubernetes clusters through natural language. No more memorizing flags or reading man pages; just describe what you want.

This post covers the technical journey of building this feature using the [GitHub Copilot SDK for Go](https://github.com/github/copilot-sdk-go), [Bubbletea](https://github.com/charmbracelet/bubbletea) TUI framework, and [glamour](https://github.com/charmbracelet/glamour) for markdown rendering.

![KSail Copilot Chat in action](/assets/images/ksail-copilot-chat.png)

- [See It in Action](#see-it-in-action)
- [Motivation](#motivation)
- [The Tech Stack](#the-tech-stack)
  - [GitHub Copilot SDK for Go](#github-copilot-sdk-for-go)
  - [Bubbletea TUI Framework](#bubbletea-tui-framework)
  - [Glamour for Markdown Rendering](#glamour-for-markdown-rendering)
- [Architecture Overview](#architecture-overview)
  - [Auto-Generated Tools from Cobra Commands](#auto-generated-tools-from-cobra-commands)
  - [Embedded Documentation Context](#embedded-documentation-context)
  - [Security Boundaries for File Operations](#security-boundaries-for-file-operations)
  - [User Experience Polish](#user-experience-polish)
- [Building with AI Assistance](#building-with-ai-assistance)
  - [The Image Snapshot Workflow](#the-image-snapshot-workflow)
  - [Iterating on TUI Design](#iterating-on-tui-design)
- [Technical Decisions and Trade-offs](#technical-decisions-and-trade-offs)
  - [Per-Turn Subscription Pattern](#per-turn-subscription-pattern)
  - [Context Cancellation Challenges](#context-cancellation-challenges)
  - [Tool Result Ordering](#tool-result-ordering)
- [What's Next](#whats-next)

## See It in Action

Before diving into the implementation, let's see what we're building. Here's `ksail chat` creating a production cluster on Hetzner Cloud from a single natural language request:

**1. Start the conversation** — ask to create a cluster on Hetzner

![Starting the chat with a Hetzner cluster request](/assets/images/ksail-copilot-chat-hetzner-start.png)

**2. Task complete** — the assistant finishes provisioning the cluster

![Cluster creation finished in chat](/assets/images/ksail-copilot-chat-hetzner-finished.png)

**3. Verification** — the cluster appears in Hetzner Cloud

![Verification in Hetzner Cloud dashboard](/assets/images/ksail-copilot-chat-hetzner-verificiation.png)

The entire workflow happens in the terminal — no context switching, no remembering flags.

## Motivation

KSail already makes Kubernetes development easier by bundling common tools (kubectl, helm, kind, k3d, flux, argocd) into a single Go binary. But users still need to remember commands, flags, and workflows. What if KSail could just understand what you want to do?

The goal was to create an AI assistant that:

1. **Understands KSail's capabilities** — knows about clusters, workloads, and GitOps
2. **Can execute real commands** — not just explain what *you* should do
3. **Works in the terminal** — no web UI, no context switching
4. **Respects security boundaries** — can't access files outside your project

## The Tech Stack

Building a conversational AI interface in a terminal requires three key pieces: an AI backend, a TUI framework, and a way to render rich output. Here's what I chose and why.

### GitHub Copilot SDK for Go

The [Copilot SDK for Go](https://github.com/github/copilot-sdk-go) provides the AI backbone. At time of writing, I used v0.1.18. It handles:

- **Streaming responses** — tokens arrive as they're generated
- **Tool calling** — the model can invoke functions you define
- **Conversation history** — multi-turn dialogues with context

The SDK abstracts away the complexity of the Copilot API, letting me focus on building the experience.

### Bubbletea TUI Framework

[Bubbletea](https://github.com/charmbracelet/bubbletea) is an Elm-inspired framework for building terminal apps in Go. It uses a functional model where:

- **Model** holds the application state
- **Update** handles messages and returns a new model
- **View** renders the model to a string

This architecture makes state management predictable — every state transition is explicit. See the [chat TUI model](https://pkg.go.dev/github.com/devantler-tech/ksail/v5/pkg/cli/ui/chat#Model) for the full implementation.

### Glamour for Markdown Rendering

AI responses often include markdown formatting. [glamour](https://github.com/charmbracelet/glamour) renders it beautifully in the terminal — code blocks get syntax highlighting, headers become bold, and lists render cleanly.

## Architecture Overview

With the tech stack in place, the interesting challenge was: how do we make the AI useful? An LLM that just talks is nice, but we need it to actually *do* things.

### Auto-Generated Tools from Cobra Commands

KSail has 92+ CLI commands. Manually defining tools for the Copilot SDK would be tedious and drift-prone. So I built a [generator](https://pkg.go.dev/github.com/devantler-tech/ksail/v5/pkg/svc/chat/generator) that auto-discovers them from KSail's existing Cobra command tree, converts flags to JSON Schema, and creates tool handlers automatically.

**The lesson learned**: auto-discovering all 92 commands broke things. LLMs have a practical limit on how many tools they can reason about effectively — this is a known issue called *tool overload*. When presented with too many options, the model's ability to select the right tool degrades significantly. Some commands (like internal debug utilities) also shouldn't be exposed at all. The solution was to keep auto-discovery but add an exclusion filter — now only ~25 curated commands become tools. Best of both worlds: no manual sync, but a focused toolset the model can actually use well.

### Embedded Documentation Context

The assistant needs to understand KSail's concepts. I use `go:embed` to bundle 100+ documentation files at compile time, giving the model deep knowledge of KSail's architecture, configuration options, and best practices — without requiring network requests. See [`BuildSystemContext`](https://pkg.go.dev/github.com/devantler-tech/ksail/v5/pkg/svc/chat#BuildSystemContext) for details.

### Security Boundaries for File Operations

The assistant can read and write files, but only within the current working directory. The `securePath` function in the [tools package](https://pkg.go.dev/github.com/devantler-tech/ksail/v5/pkg/svc/chat) validates all paths by resolving symlinks and verifying the target stays within the workspace — preventing symlink escape attacks where a malicious symlink could point outside the workspace.
### User Experience Polish

Beyond the core AI integration, the TUI includes thoughtful UX details:

- **Prompt history** — press ↑/↓ to recall previous messages
- **Real-time tool streaming** — see command output as it runs, not just when complete
- **Collapsible tool output** — Tab toggles individual tools, Ctrl+T toggles all
- **Mouse scrolling** — wheel scrolls the conversation viewport
- **Adaptive rendering** — markdown re-renders when terminal width changes
## Building with AI Assistance

Here's the meta twist: I built an AI assistant *using* AI assistance.

VS Code's agent mode with Claude Opus 4.5 was invaluable throughout this project. But TUI development presents a unique challenge — the AI can't "see" what renders in your terminal. How do you debug visual issues with a text-based assistant?

### The Image Snapshot Workflow

My solution: image snapshots. The workflow:

1. **Run the TUI** with test inputs
2. **Screenshot** the terminal output
3. **Share the image** with Claude via VS Code's attachment feature
4. **Describe what's wrong** — "The viewport is overlapping the input field"
5. **Apply the suggested fix** and iterate

This visual feedback loop was essential for:

- Debugging layout issues
- Fine-tuning the viewport scroll behavior
- Aligning the collapsible tool output sections

### Iterating on TUI Design

The TUI went through several iterations:

**Version 1**: Simple streaming text, no viewport — text would scroll off screen

**Version 2**: Added viewport with scroll handling — but input field position was wrong

**Version 3**: Proper layout with input at bottom, content scrolling above — working!

**Version 4**: Added collapsible tool invocations with Tab toggle — polished experience

Each iteration was a conversation with Claude, showing screenshots and describing what felt wrong.

## Technical Decisions and Trade-offs

Every project has its thorny edge cases. Here are the three that taught me the most.

### Per-Turn Subscription Pattern

Bubbletea uses subscriptions for async events (like streaming tokens). A naive approach subscribes once at initialization, but this causes issues when starting new turns — old subscriptions conflict with new ones. The solution was to subscribe fresh for each AI turn, creating a new channel and subscription tied to that specific conversation turn. See the [Model.Update](https://pkg.go.dev/github.com/devantler-tech/ksail/v5/pkg/cli/ui/chat#Model.Update) method for the implementation.

### Context Cancellation Challenges

The Copilot SDK doesn't support mid-stream cancellation. If you cancel the context, the SDK panics rather than gracefully stopping. My workaround: use a context that's never cancelled, and track "stop requested" via a separate flag. This isn't ideal, but it's documented in the code for future improvement when the SDK adds support.

### Tool Result Ordering

Tools can execute in any order, but results should appear in the order the model requested them. The SDK's channel-based output didn't guarantee this, so I implemented a mutex-protected FIFO buffer to ensure consistent ordering in the UI.

## What's Next

The chat assistant is functional and I use it daily, but there's plenty of room to grow:

- **More tools** — support for `kubectl` commands, log viewing, port forwarding
- **Conversation persistence** — save and resume chat sessions
- **Better context** — understand the user's specific cluster state
- **Streaming cancellation** — when the SDK supports it

If you want to try it:

```bash
# Install KSail v5+
brew install devantler-tech/tap/ksail

# Start a chat session (requires GitHub Copilot access)
ksail chat

# Or use non-TUI mode
ksail chat --tui=false "list my clusters"
```

The full implementation is in the [KSail repository](https://github.com/devantler-tech/ksail) — the TUI lives in `pkg/cli/ui/chat/` and the tools generator in `pkg/svc/chat/generator/`. Feedback and contributions welcome!

---

*This post was written with AI assistance (Claude Opus 4.5 via VS Code), following my outline and intent.*
