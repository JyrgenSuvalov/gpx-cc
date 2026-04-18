# Building with AI: Claude Code + OpenSpec Workshop

A hands-on workshop for non-technical folks who want to build real software with AI-assisted development. By the end of this session, you'll have gone from zero to a working project — planned, built, reviewed, and pushed to GitHub — using a structured, persona-driven workflow.

---

## What we're building today

We won't just be chatting with an AI. We'll be running a structured development process where different AI "personas" handle different jobs — an analyst to figure out what to build, an architect to plan the technical approach, a developer to write the code, and a QA reviewer to catch problems. OpenSpec keeps everything organized with specs and tasks so the AI doesn't lose track of what it's doing.

---

## Prerequisites

- A Mac (not really, WSL on Windows is fine as well, but some instructions might differ)
- A way to pay for Claude Code usage. Any of these works:
  - A Claude Pro, Max, Team, or Enterprise subscription (sign in on first run)
  - An Anthropic API key with billing enabled (`export ANTHROPIC_API_KEY=...`)
  - A Bedrock or Vertex AI setup, if that's how your org provisions Claude
  - Access through a gateway/proxy your company provides (LiteLLM, internal router, etc.)

Most of this stuff will work with any agentic coding assistant — including tools like the GitHub Copilot CLI, Cursor's CLI, Aider, or others. The instructions are tailored for Claude Code, but the persona + OpenSpec workflow transfers; you'll mostly be adapting the `claude` invocations and slash commands to your tool's equivalents.

---

## Part 1: Setting Up Your Environment

We need to install a handful of tools before we can get started. Don't worry — each one takes about a minute, and we'll walk through them together.

### 1.1 Homebrew

**What it is:** A package manager for macOS. Think of it as an app store for developer tools — instead of downloading installers from websites, you type one command and it handles everything.

**Why:** It's what developers use to install and manage tools like Node.js, Git, and Ghostty. It keeps everything up to date and makes it easy to uninstall or switch versions later. It's also something your agents will probably default to.

**Install:** Open Terminal (press `Cmd + Space`, type "Terminal", hit Enter) and paste:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen instructions. When it finishes, it may ask you to run a couple of commands to add Homebrew to your path — do that too.

→ https://brew.sh/

### 1.2 Oh My Zsh

**What it is:** A configuration framework for your terminal shell. It makes the terminal friendlier with better autocompletion, syntax highlighting, and a nicer prompt that shows you which folder you're in. Not strictly required, but makes the terminal much less intimidating.

**Install:**

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

→ https://ohmyz.sh/#install

### 1.3 Ghostty

**What it is:** A modern, fast terminal emulator. The default macOS Terminal app works fine, but Ghostty is snappier, looks better, and handles things like scrollback and font rendering more cleanly. It's open-source and built for performance.

**Install:**

```bash
brew install --cask ghostty
```

After installing, open Ghostty and use it instead of Terminal for the rest of the workshop.

→ https://ghostty.org/docs/install/binary#macos

### 1.4 Node.js (via nvm, with pnpm)

**What it is:** Node.js is a JavaScript runtime — this is the most popular runtime for building full-stack applications in webdev - for both frontend and backend. It's arguably not the greatest and you might choose something else, but it's a sane default option to get started. We'll install it through **nvm** (Node Version Manager), which lets you switch between Node versions easily, and we'll use **pnpm** as our package manager (it's faster and more disk-efficient than npm).

**Install:**

1. Go to https://nodejs.org/en/download
2. Select: **LTS (v24)** → **macOS** → **nvm** → **pnpm**
3. It will show you the commands to run. Copy and paste them into your terminal one by one.
4. After installation, verify it worked:

```bash
node --version    # should show v24.x.x
pnpm --version    # should show a version number
```

### 1.5 Git

**What it is:** Version control for your code. It tracks every change you make so you can undo mistakes and share your work. Git comes pre-installed on macOS.

**Verify:**

```bash
git --version
```

If it prompts you to install Xcode Command Line Tools, say yes and wait for it to finish.

> **💡 Tip:** If you don't have a GitHub account yet, go create one at https://github.com/ — we'll need it later to push your project online.

### 1.6 Claude Code

**What it is:** Anthropic's command-line AI coding tool. It can read and write files, run commands, and work directly in your project. This is the tool we'll be driving the whole workflow through.

**Install:**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Verify:**

```bash
claude --version
```

**Authenticate** using whichever path matches your setup:

- **Subscription (Pro/Max/Team/Enterprise):** just run `claude` and sign in through the browser prompt.
- **Anthropic API key:** `export ANTHROPIC_API_KEY=sk-ant-...` (add it to your `~/.zshrc` to persist).
- **Bedrock / Vertex:** set the provider env vars your org uses (e.g. `CLAUDE_CODE_USE_BEDROCK=1` with AWS creds, or `CLAUDE_CODE_USE_VERTEX=1` with GCP creds). Check your internal docs for the exact values.
- **Gateway/proxy:** point `ANTHROPIC_BASE_URL` at your gateway and use the key it expects.

If you'd rather drive this whole workflow with a different agentic CLI (GitHub Copilot CLI, Cursor CLI, Aider, etc.), install that instead — just translate the `claude ...` commands and `/opsx:*` slash commands to your tool's equivalents as you go.

---

## Part 2: Preparing Your Tools

Now we'll set up the two key tools that make our AI workflow structured and well-informed.

### 2.1 Context7 MCP

**What it is:** Context7 is an MCP (Model Context Protocol) server that gives your AI coding assistant access to up-to-date library documentation. Instead of the AI guessing how a library works based on its (potentially outdated) training data, Context7 fetches the current official docs in real time. This means fewer broken code suggestions and less debugging.

**Setup:**

1. **Create an account** at https://context7.com/ (free tier is fine)
2. **Run the setup command** (we use `pnpx` since we installed pnpm):

```bash
pnpx ctx7 setup
```

This will walk you through an interactive setup. When it asks which agent to configure for, select **Claude Code**. It will set up the MCP server connection automatically.

### 2.2 OpenSpec

**What it is:** A lightweight spec-driven development framework. When you're building software with AI, the biggest problem isn't the coding — it's the AI losing track of what it's supposed to be building. OpenSpec solves this by creating structured specifications (specs), design documents, and task lists that the AI reads and follows. Think of it as a project manager's clipboard that the AI actually respects.

The workflow goes: **propose** (plan what to build) → **apply** (build it) → **archive** (record what was done). Each step produces artifacts — specs, designs, tasks — that keep the AI grounded.

**Install:**

```bash
pnpm install -g @fission-ai/openspec@latest
```

**Verify:**

```bash
openspec --version
```

---

## Part 3: The Persona-Driven Workflow

Here's the core idea of our approach: instead of using one generic AI session for everything, we create **specialized personas** with different system prompts. Each persona has a specific job and a specific mindset. This produces dramatically better results than asking one AI session to do everything.

### 3.1 Create Your System Prompts

Open the **Claude Desktop app** (not Claude Code — the regular chat app). We'll use it to draft our system prompts since it's easier to iterate on text in a conversational interface.

#### The Analyst (ANALYST.md)

Ask Claude to help you write a system prompt for a **product analyst** persona. This persona should:

- Interview you about the problem you want to solve
- Ask about who the users are and what they need
- Help you define the scope — what's in, what's out
- Push back on vague requirements and ask clarifying questions
- Produce a clear problem statement and list of requirements

Save the result as `ANALYST.md` in a folder you'll use for the workshop (e.g., `~/workshop/`).

#### The Architect (ARCHITECT.md)

Ask Claude to help you write a system prompt for a **software architect** persona. This persona should:

- Take the requirements from the analyst phase as input
- Recommend a tech stack with clear reasoning
- Discuss trade-offs between different approaches
- Create a high-level technical plan
- Consider what's realistic for AI-assisted development
- Produce a clear architecture document and implementation plan

Save as `ARCHITECT.md` in the same folder.

### 3.2 Discovery Phase (in Claude Code)

Now we switch to Claude Code in the terminal, which we installed back in §1.6.

#### Interview with the Analyst

```bash
cd ~/workshop/
claude --system-prompt-file ANALYST.md
```

This starts Claude Code with your analyst persona loaded. Have a conversation about what you want to build. The analyst will interview you, push back on vague ideas, and help you crystallize your requirements.

When you're happy with the output, ask it to save the requirements to a file (e.g., `REQUIREMENTS.md`).

> **💡 Tip:** Be honest about what you don't know. The analyst persona is designed to draw out requirements, not judge you. The better the input here, the better everything downstream.

#### Interview with the Architect

Start a **new session** (exit the previous one with `/exit` or `Ctrl+C`):

```bash
claude --system-prompt-file ARCHITECT.md
```

Share the requirements from the previous phase and let the architect recommend a tech stack and create a plan. Ask it to save its output as `ARCHITECTURE.md` and `IMPLEMENTATION_PLAN.md`.

### 3.3 Create Dev and QA Prompts

Go back to the **Claude Desktop app**. Now that you know what tech stack you're using, create two more system prompts:

#### The Developer (DEV.md)

A system prompt for a **senior developer** persona, tailored to the specific stack your architect chose. It should:

- Follow the architecture plan and specs closely
- Write clean, well-documented code
- Use OpenSpec commands (`/opsx:propose`, `/opsx:apply`, `/opsx:archive`) to structure its work
- Commit frequently with clear messages
- Ask before making architectural decisions that weren't in the plan

#### The QA Reviewer (QA.md)

A system prompt for a **QA engineer** persona. It should:

- Review code changes critically but constructively
- Check for bugs, edge cases, and missing error handling
- Verify the implementation matches the specs
- Write its findings to a review document
- Suggest specific fixes, not just flag problems

Save both to your workshop folder.

---

## Part 4: Building the Project

This is where it all comes together.

### 4.1 Initialize the Project

Open Ghostty and navigate to your workshop folder (create it if you haven't already):

```bash
mkdir -p ~/workshop/
cd ~/workshop/
mkdir my-project && cd my-project
```

**Initialize OpenSpec:**

```bash
openspec init
```

This creates the `openspec/` folder structure and configures your AI tool integration. When it asks which tools to configure, select **Claude Code**.

Context7 was already wired up globally in §2.1, so no per-project MCP setup is needed here.

**Initialize Git:**

```bash
git init
```

### 4.2 Development Session

Start Claude Code with your developer persona:

```bash
claude --system-prompt-file ~/workshop/DEV.md
```

#### Useful Claude Code commands to know

Before diving in, familiarize yourself with these built-in commands:

| Command       | What it does                                                                         |
| ------------- | ------------------------------------------------------------------------------------ |
| `/usage`      | Shows how much of your session limit you've used                                     |
| `/context`    | Shows how many tokens are in the current context window                              |
| `/model`      | Shows or switches the AI model                                                       |
| `/statusline` | Configures a persistent status bar — set it up to show usage percent and token count |
| `/compact`    | Summarizes the conversation to free up context space                                 |
| `/exit`       | Ends the session                                                                     |

**Set up the status line first** — it helps you keep an eye on how much capacity you have left:

```
/statusline
```

Ask it to show usage percentage of the 5-hour session limit and the token count from `/context`.

#### Start building with OpenSpec

Now use the OpenSpec workflow to structure development:

**Propose a change:**

Tell the developer persona what to build (referencing your requirements and architecture docs), then run:

```
/opsx:propose
```

This creates a proposal with specs, a design document, and a task list.

**Review the proposal.** Read through the generated artifacts in `openspec/changes/`. Do the specs make sense? Are the tasks reasonable? Edit them if needed.

**Apply the change:**

```
/opsx:apply
```

The developer persona will now implement the tasks one by one, writing actual code.

> **💡 Tip:** Keep an eye on `/context` (or the statusline). Try to stay under 200k tokens — if you're getting close, ask the agent to summarize its work into an MD file and start a new session. This avoids context rot.

### 4.3 QA Review

Once the developer has finished a round of implementation, start a **new session** with the QA persona:

```bash
claude --system-prompt-file ~/workshop/QA.md
```

Ask it to review the recent changes. The QA persona will read through the code, check it against the specs, and produce a review document:

```
Review the recent changes in this project. Check the code against the specs in openspec/. Write your findings to PHASE1-REVIEW.md.
```

### 4.4 Fix and Finalize

Start a **new developer session**:

```bash
claude --system-prompt-file ~/workshop/DEV.md
```

Ask the developer to:

1. **Read the review** — `Read PHASE1-REVIEW.md and implement the suggested fixes`
2. **Archive the completed work** — the agent will run `/opsx:archive`, which merges your delta specs into the main specs and moves the change to the archive
3. **Commit everything or ask the DEV to do so**

```
git add -A && git commit -m "Phase 1 complete"
```

### 4.5 Push to GitHub

Ask the developer persona to help you set up a GitHub repository:

```
Help me create a new repository on GitHub and push this project. Set up SSH keys if I don't have them yet.
```

The agent will walk you through:

- Generating SSH keys (if needed)
- Adding the public key to your GitHub account
- Creating a repo on GitHub
- Adding the remote and pushing

---

## Part 5: Level Up — Subagents

So far we've been manually switching between personas by starting new Claude Code sessions. This works, but it's clunky. **Subagents** let you define these personas as part of your project so Claude can delegate to them automatically.

### 5.1 Create Subagent Files

Create a `.claude/agents/` directory in your project:

```bash
mkdir -p .claude/agents
```

Then create a markdown file for each persona. For example, `.claude/agents/qa-reviewer.md`:

```markdown
---
name: qa-reviewer
description: Reviews code changes for bugs, edge cases, and spec compliance. Use after implementing features or before commits.
tools: Read, Grep, Bash, Glob
model: sonnet
---

You are a QA engineer. Review the code changes against the project specs...
(paste your QA.md content here)
```

Create an equivalent `.claude/agents/developer.md` using your `DEV.md` content, and do the same for any other personas (planner, analyst, etc.). The `description` field is important — Claude uses it to decide when to automatically delegate work to that subagent.

### 5.2 Use Them

Now when you're in a main Claude Code session, you can ask it to delegate tasks to the subagents by name. Have the `developer` subagent do the openspec propose and apply work, then have the `qa-reviewer` subagent review the changes — all without leaving the session. The main session stays focused on the overall workflow, while the subagents handle specialized tasks in their own context windows.

- _"Have the developer subagent use /opsx:propose to create a plan for the next phase"_
- _"Have the qa-reviewer check the latest changes"_
- _"Use the qa-reviewer agent to review the auth module"_

Claude will spawn the subagent in its own context window, do the work, and return a summary — keeping your main session clean.

> **💡 Tip:** Subagents are great for keeping your main context window lean. Each subagent gets its own context, does its work, and only returns the results. This means your main session doesn't get cluttered with hundreds of lines of review output. It might however be useful to ask them to log some stuff for you to review. You can set up skills for this.

### 5.3 Continue the Workflow

With subagents set up, you can now run the full OpenSpec cycle more fluidly:

1. **Propose** the next feature with `/opsx:propose`
2. **Apply** it with `/opsx:apply`
3. Delegate a **QA review** to the subagent
4. **Fix** issues based on the review
5. **Archive** with `/opsx:archive`
6. **Commit and push**

Repeat until you run out of tokens or time!

---

## Quick Reference

### OpenSpec Commands (in Claude Code)

| Command                   | What it does                                                 |
| ------------------------- | ------------------------------------------------------------ |
| `/opsx:propose <feature>` | Creates a full change proposal with specs, design, and tasks |
| `/opsx:apply`             | Implements the proposed changes                              |
| `/opsx:archive`           | Archives completed work and merges specs                     |
| `/opsx:explore`           | Investigate or research before committing to a change        |

### OpenSpec CLI (in terminal)

| Command                   | What it does                                       |
| ------------------------- | -------------------------------------------------- |
| `openspec init`           | Initialize OpenSpec in a project                   |
| `openspec update`         | Regenerate AI config files after upgrading         |
| `openspec list`           | List active changes                                |
| `openspec view`           | Interactive dashboard for specs and changes        |
| `openspec config profile` | Switch between core and expanded workflow profiles |

### Claude Code Commands

| Command       | What it does                           |
| ------------- | -------------------------------------- |
| `/usage`      | Session usage stats                    |
| `/context`    | Context window token count             |
| `/model`      | View or switch model                   |
| `/statusline` | Configure persistent status bar        |
| `/compact`    | Summarize conversation to free context |
| `/agents`     | View and manage subagents              |
| `/exit`       | End session                            |

---

## Troubleshooting

**"command not found: openspec"** — Your shell might not see globally installed pnpm packages. Try closing and reopening your terminal window, or run `pnpm setup` and follow its instructions.

**"command not found: claude"** — Make sure Claude Code is installed. Follow the installation instructions and restart your terminal.

**Context window filling up** — Have the agent summarize its work in a file, then start a new session and load that file as context.

**Claude seems confused about what to build** — Check that your OpenSpec specs are up to date (`openspec list`). The AI reads these files for context. If they're stale or missing, it's flying blind.

**SSH key issues with GitHub** — Ask the Claude Code developer persona to help. It can generate keys, show you where to paste them on GitHub, and test the connection.

**Double teaming** - sometimes it is better to ask the AI to tell you what commands to run in a separate terminal. Especially if sudo permissions are needed. You can ask the AI to write out the exact commands, then copy-paste them into your terminal.

---

## Resources

- **OpenSpec docs:** https://github.com/Fission-AI/OpenSpec
- **Context7:** https://context7.com/
- **Claude Code docs:** https://code.claude.com/
- **Ghostty:** https://ghostty.org/
- **Oh My Zsh:** https://ohmyz.sh/

---

_Happy building! Remember — the AI is a tool, not a replacement for thinking. Read the specs it generates, push back when something doesn't make sense, and stay engaged with the process. The best results come when you meet it halfway._

---

## Caveat: No dedicated design stage or designer persona

This workflow currently jumps from **Analyst → Architect → Developer → QA** with no dedicated **design** stage in between. For anything with a user interface, that's a gap: visual design, UX flows, component hierarchy, accessibility, and interaction patterns all end up as implicit decisions made by the developer persona mid-build. The result is usually "whatever the LLM defaulted to" rather than something intentional — which is fine for a throwaway prototype, but weak for anything you'd actually ship.

**Ways to close the gap:**

- **Add a Designer persona (`DESIGNER.md`)** that runs between Architect and Developer. Feed it the requirements and architecture; have it produce a `DESIGN.md` covering user flows, key screens, component inventory, states (empty/loading/error), and a basic design system (colors, typography, spacing). The developer persona then implements against that document instead of improvising.
- **Split design into UX and visual sub-stages.** One pass for flows, information architecture, and screen states; a second for look, feel, and design tokens. Bundling them is a big part of why generic AI output feels generic.
- **Tool-augment the designer.** Prose-only design docs are weak anchors. Pair the persona with something like v0, Figma Make, or a screenshot-to-code flow — or at minimum have it produce ASCII wireframes and Mermaid user-flow diagrams so the developer has something concrete to build against.
- **Promote the designer to a subagent** (see Part 5) once the flow feels right, so you can delegate design passes without starting a fresh session each time.

**Tradeoff to be aware of:** more personas means more session-switching and more context overhead, which already strains the 5-hour limit. A single Designer persona producing one `DESIGN.md` is probably the right starting point; go more granular only if you find the output is still too vague.
