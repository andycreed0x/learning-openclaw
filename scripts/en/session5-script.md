# Session 5: Tools, Skills & Sandbox - Narration Script

**Estimated duration:** ~5 minutes
**Tone:** Energetic, practical, emphasizing power and safety in equal measure
**Pace:** Moderate, with emphasis on security considerations around the sandbox

---

## [0:00] Opening

Welcome to Session 5 of Learning OpenClaw. This is the session where your agents go from conversationalists to doers.

Up until now, we have talked about how messages flow, how agents are configured, and how memory works. But an agent that can only *talk* is limited. Today we are covering three topics that unlock the real power: Tools, Skills, and the Sandbox.

Tools let agents interact with the real world. Skills package reusable functionality. And the Sandbox keeps everything safe. Let's dive in.

---

## [0:30] What Are Tools?

So what are tools exactly?

**Without tools**, an agent can only generate text responses. It is limited to what the AI model already knows. It cannot check a website, it cannot run a calculation, it cannot look at a file. It is just a text generator.

**With tools**, everything changes. An agent can browse the web, read and write files, execute code, send messages, schedule tasks, and much more.

Tools allow agents to interact with the real world -- reading data, performing actions, and returning results back to the conversation. They are the hands and eyes of your AI agent.

---

## [1:00] Tool Categories Overview

OpenClaw organizes tools into six categories.

**Browser tools** let agents navigate the web. Visit URLs, take screenshots, click elements, extract content from pages.

**Canvas tools** handle visual content creation -- drawing and generating images.

**Node tools** perform system-level operations like reading and writing files, or making HTTP requests.

**Cron tools** handle scheduled tasks. Set up reminders, periodic checks, recurring actions.

> **Pronunciation note:** Cron is "KRON" -- it comes from the Greek word "chronos" meaning time. It is a Unix scheduling concept.

**Webhook tools** let external systems trigger the agent. You expose an HTTP endpoint, and when something hits it, the agent can respond.

**Exec tools** run shell commands and scripts. This is the most powerful -- and the most sensitive -- category.

Each category can be enabled or disabled independently for each agent. A general assistant might only get browser and cron tools, while a developer agent gets exec tools too.

---

## [1:50] Tool Execution Flow

When an agent receives a request, here is what happens behind the scenes.

First, the agent evaluates whether it needs a tool. If it can answer directly from its knowledge, it does -- no tool needed. But if it needs information it does not have, or it needs to take an action, it selects the appropriate tool.

Next, the system checks whether sandboxing is enabled. If sandbox mode is on, the tool runs inside an isolated environment. If not, it runs directly on the host. Either way, the tool does its work and returns a result.

The agent then processes the result. It might use the result directly in its response, or it might decide it needs to call another tool. Once it has everything it needs, it formulates the final response and sends it back to the user.

This entire cycle -- decide, execute, process, respond -- can happen multiple times within a single conversation turn.

---

## [2:30] Browser Tool Deep Dive

The Browser tool deserves special attention because it is one of the most powerful tools in OpenClaw.

It runs a real **headless Chromium browser** using Puppeteer. This is not a simple URL fetcher -- it is a full browser engine.

> **Pronunciation note:** Puppeteer is "PUP-uh-teer" -- like a puppeteer controlling a puppet. Chromium is "CROW-mee-um."

What can it do? Navigate to any URL. Take screenshots of full pages or specific elements. Click buttons and fill out forms. Extract content from the page. Wait for dynamic, JavaScript-rendered content to load. Even handle multiple tabs.

Here is a real-world example. A user asks: "Check the weather on weather.com." The agent thinks: I need the browser tool. It navigates to weather.com, extracts the forecast data, and returns it to the user as a clean text response: "Currently 72 degrees and sunny in New York City."

The browser tool handles JavaScript-heavy sites, single-page applications, and even pages that require authentication. It is a full browser in the hands of your AI agent.

---

## [3:15] Exec Tool

The Exec tool lets agents run shell commands. And yes, this is exactly as powerful -- and as dangerous -- as it sounds.

It can run any shell command, but you control exactly which commands are allowed through a whitelist. You configure a list of allowed commands -- things like `ls`, `cat`, `grep`, `curl`, `node`, `python3`. Anything not on the list is blocked.

You set a timeout to prevent runaway processes -- 30 seconds is a reasonable default.

And crucially, you can enable sandbox isolation so even the allowed commands run in a restricted environment.

**Security tip:** In production, always restrict allowed commands to the absolute minimum your agent needs. Start small and add commands only as needed. Do not give your agent access to commands it does not require.

---

## [3:55] What Are Skills?

Now let's talk about Skills. If tools are individual capabilities, skills are complete packages.

Think of skills as **apps for your agent**. A skill bundles together a system prompt that defines behavior, tool configurations, and optional trigger phrases -- all into a single, reusable package.

For example, a "web-search" skill might include a prompt like "You can search the web using the browser tool. Always cite your sources." It specifies that the browser tool is required. And it defines trigger phrases like "search for," "look up," or "find online."

Skills are **composable**. An agent can use multiple skills simultaneously. You can combine a web-search skill with a summarization skill and a translation skill, creating complex workflows from simple building blocks.

---

## [4:25] Skills Sources

Where do skills come from? Three places.

**Bundled skills** ship with OpenClaw out of the box. These cover common use cases like summarization, translation, and web search. They work immediately after installation.

**Workspace skills** are custom skills you create in your workspace's `/skills` directory. These are specific to your setup and your needs.

> **Pronunciation note:** ClawHub is "Claw-Hub" -- like GitHub but for OpenClaw skills.

**ClawHub** is the community marketplace. You can browse, install, and share skills with other OpenClaw users. Found a great skill someone built for managing calendars? Install it from ClawHub in seconds.

You can mix and match skills from all three sources. Your agent might use bundled skills for basics, your own custom skills for specialized workflows, and a few ClawHub skills for capabilities you did not want to build yourself.

---

## [4:55] Sandbox Modes

Now the safety side of this equation: the Sandbox.

There are three sandbox modes.

**Off** -- no sandbox at all. Tools run directly on the host with full access. This is fine for development and trusted environments, but never use it in production.

**Non-main** -- the main agent runs without sandbox, but sub-agents are sandboxed. This is a middle ground: you trust your primary agent but isolate any secondary ones.

**All** -- everything is sandboxed. All tool execution is isolated regardless of which agent triggers it. This is the recommended setting for production, multi-user environments, or any situation where you might process untrusted input.

Here is the security reality: with sandbox set to "off," a successful prompt injection attack could lead to arbitrary code execution on your system. That is a worst-case scenario. Always use "all" in production.

---

## [5:25] Sandbox Architecture

The sandbox uses a technology called **isolated-vm** to create a completely separate execution environment.

Code running inside the sandbox cannot access the main process memory. It cannot read arbitrary files from your system. It cannot make unauthorized network calls. It cannot access environment variables. And it cannot spawn child processes.

The sandbox enforces **memory limits** so a rogue tool cannot consume all your RAM. It enforces **CPU time limits** so nothing runs forever. And it restricts **filesystem access** to only what has been explicitly allowed.

Results are passed back to the agent through a controlled channel. The agent gets the output it needs, but the sandboxed code never gets access to anything it should not have.

---

## [5:55] Key Takeaways

Three things to remember from this session.

**Tools give agents real-world capabilities.** From browsing the web to executing code, tools transform conversational agents into powerful assistants that can actually take action.

**Skills are reusable packages.** They bundle prompts, tools, and triggers into shareable, composable apps for your agent. Use the bundled ones, create your own, or install from ClawHub.

**The Sandbox protects your system.** Always enable sandboxing in production. It isolates tool execution, preventing unauthorized access to your host system. This is non-negotiable for any deployment that handles untrusted input.

---

## [6:15] Closing / Resources

For more details, check out the tools documentation at **docs.openclaw.ai/tools**, the skills documentation at **docs.openclaw.ai/skills**, and the ClawHub marketplace at **clawhub.openclaw.ai**.

Next up is Session 6 -- our final session. We are bringing everything together with installation, configuration, and security best practices. By the end, you will be ready to deploy your own personal AI assistant. See you there!
