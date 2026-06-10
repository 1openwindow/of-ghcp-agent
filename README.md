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
git clone https://github.com/1openwindow/of-ghcp-agent
cd of-ghcp-agent
npx skills add 1openwindow/open-foundry --agent github-copilot -y
copilot                                   # open the Copilot CLI here
```

In the session, say:

> Deploy this Copilot agent to Foundry.

Once it's running, invoke the agent from this repo:

```bash
azd ai agent invoke of-ghcp-agent '{"input": "Tell me a joke"}'
```

The reply comes back in a swashbuckling pirate tone.

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
