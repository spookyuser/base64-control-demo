# base64-control-demo

Any base64-encoded string included in a message to the Claude API causes that request to fail. This is reproducible across sonnet and opus 4.6 on all platforms (claude.ai, mobile, API)

## Repo structure

### [`python-experiments/`](./python-experiments)

Proof-of-concept scripts demonstrating how the base64 refusal behavior can be exploited across different system architectures:

- **01_queue_worker.py** — A poisoned support ticket crashes a queue processor. Without error handling the whole pipeline halts; with it, the attacker's message is silently skipped.
- **02_tool_choice_bypass.py** — Base64 content is normally refused, but gets processed when `tool_choice` forces the model to respond via a tool — same content, different outcome depending on API parameters.
- **04_batch_at_scale.py** — Simulates 8M+ message batch processing. A few poisoned messages blend into normal error rates and become invisible at scale.
- **05_platform_poisoning.py** — Multiple independent Claude agents read from a shared platform. One poisoned post breaks all agents simultaneously — no knowledge of who's reading is required.

### [`skills-md/`](./skills-md)

A Next.js web app that serves as an open database and submission platform for SKILL.md files (instruction files for AI coding agents). Submissions are automatically screened for security threats using Claude, checking for prompt injection, malicious commands, data exfiltration, social engineering, and deceptive patterns. Built with Next.js 16, React 19, Neon Postgres, Drizzle ORM, and shadcn/ui.

## Setup

```bash
git clone --recurse-submodules https://github.com/spookyuser/base64-control-demo.git
```

If you've already cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

### python-experiments

```bash
cd python-experiments
uv sync && ANTHROPIC_API_KEY=xx uv run 01_queue_worker.py
```

### skills-md

```bash
cd skills-md
pnpm install
pnpm dev
```
