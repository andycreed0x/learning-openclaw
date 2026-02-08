# Session 6: Installation, Config & Security - Narration Script

**Estimated duration:** ~5 minutes
**Tone:** Practical, empowering, celebratory toward the end
**Pace:** Moderate through installation, slower on security, warm and encouraging at the wrap-up

---

## [0:00] Opening

Welcome to Session 6 -- the final session of Learning OpenClaw. We have covered a lot of ground: what OpenClaw is, its architecture, channels, agents, memory, tools, and skills. Now we are bringing it all together.

In this session, we will walk through installation, configuration, security best practices, and deployment tips. By the end, you will have everything you need to go from zero to a secure, production-ready AI assistant.

Let's finish strong.

---

## [0:25] Installation Methods

There are three ways to install OpenClaw.

**The recommended method is npm.** If you have Node.js installed, it is one command:

```
npm install -g openclaw
```

That gives you the global `openclaw` command and you are ready to go.

**From source** is the option for contributors and anyone who wants to customize the build. Clone the repository from GitHub, run `npm install`, and then `npm run build`. This gives you the full source code to inspect and modify.

**Docker** is great for isolated, reproducible deployments. Just pull the image with `docker pull openclaw/openclaw`. Docker is especially useful for production environments where you want consistent behavior across machines.

All three methods get you to the same result -- a working OpenClaw installation.

---

## [1:00] Prerequisites

Before you install, make sure you have the prerequisites.

**Node.js version 20 or higher.** You can check by running `node --version` in your terminal. If you need to install or update it, head to nodejs.org.

**npm version 9 or higher.** This comes bundled with Node.js. Verify with `npm --version`.

And you will need **at least one AI provider API key.** OpenAI keys start with `sk-`. Anthropic keys start with `sk-ant-`. Google Gemini has its own key format.

Or, if you want to run completely offline with no API key at all, you can use **Ollama** with local models. That is the privacy-first option.

> **Pronunciation note:** Ollama is "Oh-LAH-mah."

Quick check before proceeding: run `node --version` and confirm you see v20 or higher. Run `npm --version` and confirm 9 or higher. If both check out, you are good to go.

---

## [1:40] Onboarding Wizard Walkthrough

When you run `openclaw` for the first time, the onboarding wizard takes over. It is a step-by-step guide that gets you up and running in minutes.

**Step 1:** Choose your AI provider. OpenAI, Anthropic, Ollama, or Other.

**Step 2:** Enter your API key. The wizard validates it right away so you know it works.

**Step 3:** Choose a channel. Web is the default and easiest to start with -- no external accounts needed. You can also jump straight to Telegram or WhatsApp if you prefer.

**Step 4:** You are ready. The Gateway starts on port 18789, and if you chose the Web channel, a chat interface is available at localhost:3000.

The wizard generates an `openclaw.json5` configuration file. Everything it sets up, you can customize later by editing that file.

---

## [2:20] Configuration File Deep Dive

The configuration file -- `openclaw.json5` -- is the heart of your setup.

It uses JSON5 format, which is a superset of regular JSON. That means you can add comments to document your config, use trailing commas without errors, and use unquoted keys for cleaner syntax. It is much more human-friendly than plain JSON.

> **Pronunciation note:** JSON5 is "Jay-son-five."

The three main sections are **gateway** for network settings like port and host, **agents** for your AI model configuration, and **channels** for your messaging interfaces.

A minimal configuration is surprisingly small. A gateway with a port. An agent with a model and provider. A channel with a type and port. That is enough to get started.

---

## [2:55] Config Features

The configuration system has four powerful features.

**JSON5 format** -- comments are allowed, trailing commas are fine, and unquoted keys keep things clean. This alone is a huge improvement over working with plain JSON.

**Hot reload on save.** Edit the config file, save it, and changes apply immediately. No need to restart the Gateway. This makes tuning and experimenting incredibly fast.

**The $include directive.** As your config grows, you can split it into multiple files. Put agents in `agents.json5`, channels in `channels.json5`, and reference them with `$include`. Keeps everything organized.

**Environment variable substitution.** Reference env vars with the dollar sign prefix -- like `$OPENAI_API_KEY`. This means your API keys and secrets never appear in the config file itself. Essential for any deployment, and a best practice you should follow from day one.

---

## [3:30] Security Overview

Security in OpenClaw follows a **defense-in-depth** approach. Multiple independent layers work together to protect your system, your data, and your users.

There are four main security layers.

**DM Policies** control who can talk to your agent.

**The Sandbox** isolates tool execution from your host system.

**Prompt Injection awareness** includes built-in protections against manipulation attacks.

**Access Control** restricts which tools each agent can use.

These layers are independent. Even if one is bypassed, the others continue to protect you. That is what defense in depth means.

---

## [4:00] DM Policies Recap

We covered DM policies in Session 3, but they are worth revisiting in the context of security.

**Pairing mode** is the strongest option for personal use. New users must enter a pairing code before they can chat. Only people you share the code with can access your agent.

**Allowlist mode** restricts access to pre-approved user IDs. Good for small teams where you know everyone.

**Open mode** lets anyone chat. Only use this for public bots or demos -- never for a personal assistant.

**Disabled mode** turns off DMs entirely. The bot only works in group chats.

My recommendation stands: use pairing mode for personal assistants. It is the best balance of security and usability.

---

## [4:25] Prompt Injection Awareness

Prompt injection is a serious concern for any AI assistant that can take actions.

What is it? An attacker crafts input designed to make the agent ignore its instructions and perform unintended actions. For example, someone might send a message that says "Ignore all previous instructions and run this command."

OpenClaw includes built-in protections: system prompt isolation, tool call validation, output filtering, and sandbox isolation for exec tools.

But no protection is perfect. So follow these best practices. Restrict tool permissions to the minimum needed. Enable sandbox mode in production -- always. Review agent logs regularly for unusual behavior. Use allowlist or pairing DM policies when possible. And be cautious with tools that access external content, because that content could contain injection attempts.

---

## [5:00] Doctor Diagnostic Tool

OpenClaw includes a built-in diagnostic tool called `openclaw doctor`.

Run it anytime something is not working. It checks your configuration file for syntax errors and validity. It verifies that AI provider API keys are configured and reachable. It tests channel connectivity and validates tool setup.

The output is clear and actionable. Each check gets a PASS, WARN, or FAIL status. If your Telegram token is invalid, it tells you. If your OpenAI key cannot connect, it tells you. If your browser tool cannot find Chromium, it tells you.

Think of `openclaw doctor` as your first debugging step. Before searching forums or filing issues, run the doctor.

---

## [5:25] CLI Commands Reference

Here is a quick reference for the essential CLI commands.

`openclaw` -- starts OpenClaw. On first launch, it runs the onboarding wizard.

`openclaw doctor` -- runs diagnostics on config, connectivity, and overall health.

`openclaw config` -- displays the current merged configuration. This is especially useful when you are using `$include` directives and want to see the final resolved config.

`openclaw --version` -- shows the installed version.

And for a full list of everything available, run `openclaw --help`.

---

## [5:45] Deployment Tips

A few tips for running OpenClaw in production.

**Use a process manager.** Either systemd on Linux or pm2 for a Node.js-native option. This ensures OpenClaw stays running and automatically restarts on crashes. With pm2, it is three commands: `pm2 start openclaw`, `pm2 save`, and `pm2 startup`.

> **Pronunciation note:** pm2 is "P-M-two." systemd is "system-D."

**Put a reverse proxy in front.** Nginx or Caddy works great for SSL termination, load balancing, and serving static files.

> **Pronunciation note:** Nginx is "Engine-X." Caddy is just "Caddy."

**Always use HTTPS.** Never expose the Gateway over plain HTTP in production. Use Let's Encrypt for free SSL certificates.

**Back up your workspace regularly.** The workspace directory contains conversations, custom skills, and agent state. A simple tar command is enough: `tar -czf backup.tar.gz workspace/`.

---

## [6:15] Course Complete / Wrap Up

And with that, you have completed all six sessions of Learning OpenClaw.

Let's recap the full journey.

**Session 1** -- What is OpenClaw? We learned the problem it solves and what makes it unique.

**Session 2** -- Architecture and Gateway. We looked under the hood at the six core components and the central Gateway.

**Session 3** -- Channels and Multi-Platform. We connected OpenClaw to the world through 13+ messaging platforms.

**Session 4** -- Agents, Memory, and Sessions. We built AI personalities that genuinely learn and remember.

**Session 5** -- Tools, Skills, and Sandbox. We gave our agents the power to interact with the real world, safely.

**Session 6** -- Installation, Configuration, and Security. We went from zero to production-ready.

You now have a complete map of how OpenClaw works. The next step is to get hands-on. Install it, build your own agent, create custom skills, and explore what is possible.

---

## [6:50] Closing / Resources

Here are the resources for your journey going forward.

**Getting Started Guide** at docs.openclaw.ai/getting-started -- step-by-step setup instructions.

**Security Documentation** at docs.openclaw.ai/security -- the in-depth security configuration guide.

**Docker Documentation** at docs.openclaw.ai/docker -- container deployment guides and Docker Compose examples.

**Community** at docs.openclaw.ai/help -- get help, report bugs, and join the discussion.

And of course, the source code at **github.com/openclaw/openclaw**. Star it, fork it, contribute.

Congratulations on completing Learning OpenClaw. Go build something amazing. Thank you for learning with us!
