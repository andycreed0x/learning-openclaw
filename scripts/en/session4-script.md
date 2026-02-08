# Session 4: Agents, Memory & Sessions - Narration Script

**Estimated duration:** ~5 minutes
**Tone:** Engaged, explaining powerful concepts with relatable analogies
**Pace:** Moderate, slower during memory system explanation as it is conceptually rich

---

## [0:00] Opening

Welcome to Session 4 of Learning OpenClaw. This is where things get truly powerful. Today we are covering three interconnected topics: agents, memory, and sessions.

By the end of this session, you will understand how to create agents with distinct personalities, how the memory system gives your assistant continuity across conversations, and how sessions manage the lifecycle of each interaction.

Let's get into it.

---

## [0:25] What Is an Agent?

An agent in OpenClaw is an AI personality with its own workspace, tools, and memory. Each agent is fully isolated from others.

Think of agents as separate employees, each with their own desk, files, and expertise. An assistant agent does not know what the developer agent is working on, and vice versa. This isolation is intentional -- it keeps concerns separated and prevents data from leaking between contexts.

Each agent has its own **system prompt** and behavior rules that define how it acts. It maintains **separate memory** for each user it interacts with. It can access different **tools and skills** based on its configuration. And it runs in a **sandboxed environment** for security.

The key concept to remember: agents do not share memory or context. What one agent knows, another does not.

---

## [1:05] Agent Workspace Structure

So where does an agent live? In the workspace directory.

The workspace has an `agents` folder, and inside that, each agent gets its own subdirectory. Inside an agent's directory, you will find a set of markdown files that define everything about that agent.

`AGENTS.md` defines the agent's behavior and rules -- this is essentially the system prompt.

`SOUL.md` defines the personality and tone -- is the agent formal or casual? Funny or serious?

`TOOLS.md` configures which tools are available to the agent.

`USER.md` stores per-user context and is auto-generated as the agent interacts with people.

`MEMORY.md` is the long-term memory file that persists across all sessions.

There is also a `skills` directory for custom skills and a `sandbox` directory for sandboxed execution.

Here is the beautiful part: **everything is files.** Agent configuration is just markdown files in a directory. You can edit them with any text editor, version-control them with Git, and collaborate on agent behavior with your team. No databases, no admin panels needed for this part.

---

## [2:00] Personality Files Explained

Let me break down each personality file a bit more.

**AGENTS.md** is the core. This is the main system prompt. It tells the language model who it is, what it should do, and how it should behave. Think of it as the job description.

**SOUL.md** adds personality flavor. This is where you define the agent's character traits. Maybe it is warm and encouraging. Maybe it is direct and no-nonsense. Maybe it uses humor. SOUL.md makes each agent feel unique and consistent across conversations.

**TOOLS.md** controls capabilities. Which tools can this agent use? Can it browse the web? Execute code? Schedule tasks? This file acts as a permission system for the agent's actions.

**USER.md** is created automatically for each user the agent interacts with. Over time, it builds up context about that person -- their preferences, past requests, and relevant facts. You do not write this file -- the system manages it.

And **MEMORY.md** is the long-term memory. We will dive deep into this one in a moment.

---

## [2:55] Multi-Agent Setup

A typical OpenClaw setup might have multiple agents, each specialized for different tasks.

For example, a **general-purpose assistant** for everyday questions and conversations. A **developer agent** with code tools enabled, focused on programming and DevOps tasks. And an **analyst agent** with data access capabilities for reports and analysis.

Each agent lives in its own workspace directory under `agents/`. You route messages to specific agents through your channel configuration or routing rules -- which we covered in Session 3.

And here is a nice detail: you can assign different LLM models to each agent. Maybe use a cheaper, faster model for the general assistant where quick responses matter, and a more capable, expensive model for the developer agent where accuracy is critical.

---

## [3:35] Memory System

Now let's talk about memory, because this is what makes OpenClaw agents feel like they genuinely know you over time.

There are two types of memory.

**Daily Notes** are the short-term memory. After each conversation, key information is automatically extracted and saved. What facts came up? What decisions were made? What preferences did the user express? These notes are organized by date and serve as the raw material for long-term memory.

**MEMORY.md** is the long-term memory. It persists across all sessions and is loaded into every new conversation. It contains the distilled, important information about the user -- things like preferences, important facts, and ongoing projects.

---

## [4:00] Memory Flow

Here is how the two types of memory work together.

A conversation happens. When the session ends or goes idle, daily notes are auto-generated that capture the key highlights. Over time, daily notes accumulate.

Then a **compaction** process kicks in. Compaction looks at the daily notes, extracts the important facts, removes redundant or outdated information, and updates MEMORY.md with the distilled knowledge.

> **Pronunciation note:** Compaction -- "com-PAC-tion" -- think of it as compressing and refining.

The result? Your agent starts every new conversation already knowing that you prefer dark mode, that your cat's name is Luna, and that you are working on a Python project -- without re-reading every past conversation. It is efficient and it feels natural.

---

## [4:35] Sessions Explained

Sessions are the container that holds all of this together.

A **session** is one conversation thread between a user and an agent. It gets **created** when a user sends a new message. It is **active** while the conversation is ongoing. It goes **idle** when there is no activity for a configured period. And it eventually **expires** after the idle timeout is reached.

Why do sessions matter? They manage the conversation context, control memory usage, and enable history compaction when conversations get too long. Without session management, long conversations would exceed the language model's context window and either fail or lose important information.

---

## [4:55] Session State Diagram

The session lifecycle has a few key transitions worth understanding.

When a session is **active** and the conversation history grows too long, it transitions to a **compacted** state. During compaction, older messages are summarized and replaced with a condensed version, freeing up context window space. Then the session returns to active.

When a session has been **idle** for too long, it transitions to **expired**. At that point, daily notes are generated and the session is cleaned up.

If a user sends a new message while the session is idle -- before it expires -- it simply goes back to active. Seamless.

---

## [5:10] Session Configuration

You have full control over how sessions behave through configuration.

**idleTimeout** controls how long a session stays alive without messages. The default of 30 minutes is a good starting point.

**maxHistory** determines how many messages are kept before compaction triggers. Starting with 50 is a reasonable default -- lower values save tokens but may lose context.

The **compaction strategy** defines how old messages are summarized. The "summarize" strategy uses the LLM itself to create a condensed version. The **keepRecent** setting preserves the last N messages verbatim so you never lose the most recent context.

And **dailyNotes** controls whether the system automatically extracts facts after each session. Keep this enabled -- it is what feeds the long-term memory system.

---

## [5:40] Key Takeaways

Three takeaways from this session.

**Agents are isolated AI personalities.** Each agent has its own workspace, tools, and memory. They do not share context, and that is by design.

**Memory persists across sessions.** Daily notes capture short-term context. MEMORY.md provides long-term continuity. Together, they make your agent genuinely learn about you over time.

**Sessions have automatic lifecycle management.** Compaction, idle timeouts, and cleanup all happen automatically, keeping everything running smoothly without manual intervention.

---

## [6:00] Closing / Resources

For more details, check out the agent configuration docs, the multi-agent setup guide, the memory system documentation, and the session management reference. Links are in the slides.

Next up in Session 5, we explore tools and skills -- the capabilities that let your agents interact with the real world. That is where OpenClaw transforms from a chatbot into a genuine personal automation platform. See you there!
