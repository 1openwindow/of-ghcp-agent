# of-ghcp-agent

A minimal **GitHub Copilot on Foundry hosted agent** demo, stripped down from the
[github-copilot sample](https://github.com/microsoft-foundry/foundry-samples/tree/main/samples/python/hosted-agents/bring-your-own/invocations/github-copilot)
to **keep only its skills**.

Used to demo deploying a Bring-Your-Own GitHub Copilot agent to Foundry with
[`open-foundry`](https://github.com/1openwindow/open-foundry) (`ghcp-foundry-runtime`).

## What's here

```
.github/skills/
└── joke/
    └── SKILL.md   ← tells Copilot to respond like a pirate
```

The Copilot CLI auto-loads any subdirectory under `.github/skills/` that contains
a `SKILL.md`. This repo keeps only the `joke` skill — nothing else from the
original sample (no `main.py`, `Dockerfile`, `agent.yaml`, `requirements.txt`,
etc.).

## Deploy to Foundry with open-foundry

You bring this repo; the [`open-foundry`](https://github.com/1openwindow/open-foundry)
skill does the rest. Run it from the **GitHub Copilot CLI** — this sample's own
harness — so the deploy is exercised end-to-end:

```bash
cd of-ghcp-agent
gh skill install 1openwindow/open-foundry --allow-hidden-dirs # install (once; needs gh >= 2.90.0)
copilot                                   # open the Copilot CLI here
```

In the session, say:

> Deploy this Copilot agent to Foundry, then send it "Tell me a joke" and show the reply.

The skill scaffolds the standard files and deploys — no `Dockerfile`, `agent.yaml`,
or `requirements.txt` to write by hand. Two Copilot-specific points it will confirm
with you:

- **Runtime image** selects the harness — use a `ghcp-foundry-runtime` image (e.g.
  `ghcr.io/1openwindow/ghcp-foundry-runtime:latest`), not the pi image.
- **Model auth is BYOK (API key)** — Copilot doesn't support managed identity, so
  supply your model's API key + base URL when asked.

A pirate-tone reply (`Arrr`, `matey`, `ye`) confirms the `joke` skill loaded.
Reset afterwards with `git clean` (below).

## Testing repeatedly (keep the sample fresh)

This sample is **fresh BYO**. The files open-foundry generates on deploy
(`Dockerfile`, `.dockerignore`, `azure.yaml`, `agent.yaml`, `agent.manifest.yaml`)
plus `.azure/` are **git-ignored** — so deploying never dirties the tracked
sample and you cannot accidentally commit them.

Reset back to the pristine sample after a test run:

```bash
git clean -fdX             # remove all ignored files (including .azure/)
git clean -fdX -e .azure   # ...but keep your azd env for a faster re-deploy
```

Afterwards `git status` should report nothing to commit.

## Use it (once running)

Send the agent a message asking for a joke:

```json
{"input": "Tell me a joke"}
```

The reply comes back in a swashbuckling pirate tone.

## Adding more skills

Create a new folder under `.github/skills/` with a `SKILL.md`:

```
.github/skills/
└── my-skill/
    └── SKILL.md
```

```markdown
---
name: my-skill
description: What this skill does.
---

# My Skill

Instructions for Copilot when this skill is active.
```
