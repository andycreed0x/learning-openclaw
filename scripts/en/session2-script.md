# Session 2: Architecture & Gateway - Narration Script

**Estimated duration:** ~5 minutes
**Tone:** Technical but approachable, building on Session 1
**Pace:** Slightly slower during architecture explanations, allow time for diagrams

---

## [0:00] Opening

Welcome back to Learning OpenClaw. This is Session 2, and today we are going under the hood. Now that you know *what* OpenClaw is, we are going to explore *how* it works -- the full architecture, the Gateway that sits at the center of everything, and how all the pieces connect together.

---

## [0:15] The 6 Core Components Overview

OpenClaw is built around six core components. Let's meet them.

First, the **Gateway**. This is the central orchestrator and WebSocket server. Everything connects to it.

Second, **Channels**. These are your messaging integrations -- WhatsApp, Telegram, Discord, and more.

Third, **Agents**. These are AI personalities, each with its own model and system prompt.

Fourth, **Nodes**. These are distributed hosts that run channels -- we will get into why this matters shortly.

Fifth, **Sessions**. These handle conversation memory and context.

And sixth, **Tools**. These are the capabilities your agents can use -- browser automation, code execution, scheduled tasks, and custom extensions.

These six components work together to create a flexible, distributed AI assistant platform.

---

## [1:00] Full Architecture Diagram Walkthrough

Now let's see how these pieces fit together in the full architecture diagram.

On one side, you have your channels: WhatsApp, Telegram, Discord, Web, and Slack. All of them connect to the Gateway via WebSocket on port 18789.

> **Pronunciation note:** Port 18789 -- just say "one-eight-seven-eight-nine."

Inside the Gateway, you have three key pieces: the WebSocket Server that accepts connections, the Router that decides where messages go, and the Session Manager that maintains conversation context.

Messages flow from channels into the WebSocket Server, through the Router, into the Session Manager, and then out to Agents. Agents can call Tools like the browser, code execution, or cron jobs.

The important thing to notice here is that the Gateway is the central hub. Everything flows through it.

---

## [1:50] Gateway Deep Dive

Let's zoom in on the Gateway because it is the brain of OpenClaw.

The Gateway runs a **WebSocket control plane** that listens on port 18789. Every channel, every node connects here. It is the single point of contact for the entire system.

The config is **hot-reloadable**. This means you can edit your `openclaw.json` file, save it, and the changes apply instantly. No restart required. That is a huge quality-of-life feature during setup and tuning.

Internally, the Gateway handles **authentication** -- making sure only authorized nodes and channels can connect. It manages the **Router**, which maps channels and users to the right agents. It runs the **Session Manager**, which tracks conversation state using SQLite. And it watches the **config file** for changes to trigger hot reloads.

> **Pronunciation note:** SQLite is typically pronounced "S-Q-L-ite" or "sequel-ite" -- both are common.

---

## [2:40] Request Flow Step by Step

Let me walk you through exactly what happens when a message arrives.

A user sends a message. The channel -- let's say Telegram -- receives it. The channel forwards it over WebSocket to the Gateway. The Gateway's Router looks at the message and determines which agent should handle it based on your configuration. The Session Manager loads or creates a session for that user-agent pair.

The agent processes the message using its configured AI model. If the agent decides it needs to take an action, it calls a tool -- maybe browse a website or run a calculation. The tool returns a result, the agent formulates its response, and that response travels back through the same path: agent to Gateway, Gateway to channel, channel to the user.

Every single message follows this exact path. The response always travels back the same way it came.

---

## [3:20] Tech Stack Overview

Let's talk about the technology under the hood.

The **runtime** is Node.js with TypeScript. This gives you a fast, async runtime with full type safety.

> **Pronunciation note:** Node.js is "Node-jay-ess." TypeScript is just "Type-Script."

The **protocol** is WebSocket for real-time, bidirectional communication. No polling, no delays.

**Configuration** uses JSON5 format -- that is JSON with comments, trailing commas, and unquoted keys. Much more pleasant to work with than plain JSON.

> **Pronunciation note:** JSON5 is "Jay-son-five."

The **AI integration** is multi-provider -- OpenAI, Anthropic, Gemini, Ollama, and anything OpenAI-compatible.

The **database** is SQLite for session storage. No external database server needed -- it is all self-contained.

And the **sandbox** uses isolated-vm for secure tool execution. This keeps your system safe even when agents run code.

> **Pronunciation note:** isolated-vm is "isolated-V-M" -- VM as in virtual machine.

---

## [3:55] Configuration with JSON5

All configuration lives in a single file: `openclaw.json` -- using JSON5 format.

Here is what a minimal config looks like. You have a `gateway` section with the port and a name. You have an `agents` section where you define your AI agents -- each with a model, a system prompt, and the tools it can use. And you have a `channels` section where you configure your messaging integrations.

There are three key config features to know about.

**Hot reload.** Just save the file and changes apply instantly. No restart needed.

**The $include directive.** You can split your config into multiple files for better organization. For example, put all your agents in `agents.json5` and reference it with `$include`.

> **Pronunciation note:** $include is "dollar-include."

**Environment variables.** Reference them with a dollar sign prefix -- like `$TELEGRAM_BOT_TOKEN`. This way, you never hardcode secrets in your config file.

---

## [4:30] Multi-Node Architecture

Now here is where the architecture gets really interesting. OpenClaw supports a **multi-node** setup.

You have one Gateway -- that is always the central brain. But you can have multiple Nodes running on different machines. A Node on your local machine. Another on a cloud VPS. A third on a home server. Each Node runs channels and connects back to the central Gateway via WebSocket.

---

## [4:45] Nodes Explained

Why would you want this? Because different channels have different requirements.

WhatsApp needs a persistent connection and might work better on a VPS with a stable IP. Telegram is more flexible and can run anywhere. Slack might run from your office network.

So you could have Node 1 at home running Telegram and the Web UI. Node 2 on a VPS running WhatsApp and Discord. Node 3 at the office running Slack.

The Gateway handles routing across all of them seamlessly. From the user's perspective, it is all one assistant.

The WebSocket protocol between Nodes and the Gateway is straightforward: a Node connects, authenticates with a token, and receives an acknowledgment. From there, messages flow both ways. The connection is persistent -- not polling -- so everything is real-time. The Gateway can even push config updates directly to Nodes.

---

## [5:20] Key Takeaways

Let's wrap up with the key takeaways from this session.

**The Gateway is the brain.** Everything connects to it. It routes messages, manages sessions, and orchestrates all components.

**Everything communicates via WebSocket.** Channels, Nodes, and the Gateway use persistent WebSocket connections on port 18789 for real-time performance.

**Config is JSON5 with hot reload.** One config file in a human-readable format, with changes that apply instantly on save. Use `$include` to split files and dollar-sign variables for secrets.

**Distributed by design.** The multi-node architecture lets you run channels on different machines while keeping one central Gateway.

These are the foundations you need to understand everything else in OpenClaw.

---

## [5:50] Closing / Resources

For more details, check out the architecture documentation at **docs.openclaw.ai** and the Gateway configuration reference. The source code on GitHub is also a great way to understand how the Gateway works under the hood.

Coming up next in Session 3, we dive into Channels and Nodes in detail -- how to connect OpenClaw to all your messaging platforms and deploy across multiple machines. See you there!
