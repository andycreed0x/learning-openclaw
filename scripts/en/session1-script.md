# Session 1: What is OpenClaw? - Narration Script

**Estimated duration:** ~4 minutes
**Tone:** Welcoming, enthusiastic, accessible
**Pace:** Moderate - give listeners time to absorb new concepts

---

## [0:00] Opening / Welcome

Hey there, and welcome to Learning OpenClaw! This is Session 1 of 6, and today we are answering the big question: What is OpenClaw?

By the end of this session, you will understand the problem OpenClaw solves, how it works at a high level, and why it is different from every other AI assistant out there.

Let's jump in.

---

## [0:20] The Problem with Current AI Assistants

So let's start with the problem.

Right now, if you want to use an AI assistant, you are tied to a single vendor. You use ChatGPT through OpenAI's app. You use Claude through Anthropic's app. You use Gemini through Google's app.

Each one is locked in its own silo. Your conversations, your data, your context -- all trapped on someone else's platform. You cannot choose which model to use per conversation. You cannot reach your assistant from WhatsApp *and* Telegram *and* Discord at the same time. And you definitely do not own your data.

Think about it: different apps, different contexts, no control. That is the world we live in today.

---

## [0:55] Introducing OpenClaw as the Solution

And that is exactly where OpenClaw comes in.

> **Pronunciation note:** OpenClaw is pronounced "Open-Claw" -- like the claw of an animal.

OpenClaw is a personal, self-hosted AI assistant platform. Let me break down what that means.

**Local-first.** It runs on your hardware. Your data stays yours. Period.

**Multi-channel.** You can connect it to WhatsApp, Telegram, Discord, Slack, a web interface -- all at once. Same assistant, every platform.

**Model-agnostic.** Use OpenAI, Anthropic, Google Gemini, or even run models locally with Ollama for complete privacy. You are never locked into a single AI provider.

**Open source.** Licensed under AGPLv3. You can inspect the code, modify it, and contribute back to the project.

---

## [1:40] High-Level Architecture Overview

Now let's look at how OpenClaw is structured at a high level. It has four core layers.

First, **Users** interact through **Channels** -- that is your WhatsApp, Telegram, or web chat.

Those channels connect to the **Gateway**, which is the central orchestrator -- think of it as the brain of the system.

The Gateway routes messages to **AI Models** -- whatever provider you have configured.

And those AI models can invoke **Tools** to actually do things in the real world -- browse the web, execute code, schedule tasks, and more.

So the flow is: User, to Channel, to Gateway, to AI, to Tools. Simple and clean.

---

## [2:15] Key Differentiators Comparison

Let's put this side by side with the big players.

Self-hosted? OpenClaw: yes. ChatGPT, Claude, Gemini: no.

Multi-channel support? OpenClaw: yes. Everyone else: no.

Model choice? OpenClaw works with any model. The others lock you into their own.

Open source? OpenClaw is AGPLv3. The others are proprietary.

Tool execution? OpenClaw is fully extensible. The others have limited, controlled tool access.

Data ownership? With OpenClaw, one hundred percent of your data is yours. With the others, it lives on their servers.

No other major assistant offers all of these together. That is what makes OpenClaw unique.

---

## [2:55] How It Works -- Simplified Flow

Let me walk you through the lifecycle of a single message.

A user sends a message through a channel -- say, WhatsApp. The channel picks it up and forwards it to the Gateway over WebSocket. The Gateway looks at the message, figures out which agent should handle it, and routes it there.

> **Pronunciation note:** WebSocket is pronounced "Web-Socket" -- it is a real-time communication protocol.

The agent processes the message using an AI model. If the agent needs to take action -- like checking a website or running a calculation -- it calls a tool. The tool does its work, returns the result, and the agent formulates a response.

That response flows back through the same path: agent to Gateway, Gateway to channel, channel to the user. The whole thing happens in seconds.

---

## [3:35] Supported AI Models

One of the best things about OpenClaw is model flexibility. You are truly not locked in.

You can use **OpenAI** models like GPT-4o, GPT-4, or GPT-3.5.

**Anthropic's Claude** -- Opus, Sonnet, or Haiku.

**Google Gemini** -- Gemini Pro or Gemini Flash.

> **Pronunciation note:** Ollama is pronounced "Oh-lah-mah."

**Ollama** for running models locally -- Llama, Mistral, Phi -- completely offline, no API key needed.

**OpenRouter** gives you access to over 100 models through a single API.

And if you have any **OpenAI-compatible API** server -- like LM Studio or vLLM -- OpenClaw works with it out of the box.

You can even use different models for different agents. Maybe a cheaper model for quick questions and a more capable one for complex tasks.

---

## [4:10] What's Coming in the Next Sessions

So that is the big picture. Over the next five sessions, we are going deep into each part of OpenClaw.

**Session 2** covers the architecture and Gateway -- the core components, the WebSocket protocol, and configuration.

**Session 3** is about channels and distributed nodes -- how you set up multi-channel support and deploy across machines.

**Session 4** dives into agents and sessions -- AI personalities, the memory system, and context management.

**Session 5** explores tools and extensibility -- built-in tools, custom tools, and sandboxing for security.

**Session 6** wraps it all up with deployment and operational best practices -- everything you need to run OpenClaw in production.

---

## [4:40] Closing / Resources

Before we go, here are the resources you should bookmark.

The official documentation lives at **docs.openclaw.ai** -- that is your primary reference for everything.

The source code is on GitHub at **github.com/openclaw/openclaw**. Star it, fork it, dig into the code.

And for community support, head to **docs.openclaw.ai/help** -- that is where you can get help, report bugs, and join discussions.

Thanks for joining me for Session 1. I will see you in Session 2, where we look under the hood at the architecture and Gateway. See you there!
