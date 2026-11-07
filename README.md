# Polyforge

**A tool for creating video games efficiently using AI.** You bring the idea and the final say; Polyforge breaks the game into small, buildable steps, builds them in a real engine, checks every step with evidence, and hands you back a playable project you own.

One game, built step by step — and no step is accepted until it checks out.

---

## Why

Creating a game with AI help today usually means one assistant does everything: it plans, codes, makes the art calls, and then grades its own work. That is where quality collapses — a builder that judges itself lets its own mistakes through, and a single model averages away the fact that different models are good at different parts of making a game.

The pieces for something better already exist in the open: bridges that let an AI drive real engines (Godot, Unity, Unreal, Blender), gateways that connect many model providers, and team-style workflows that proved role-based building works for software. Polyforge assembles them into one practical tool whose only job is shipping games.

## How it works

```
        You — the idea, the taste, the final say
                    │
            ┌───────▼────────┐
            │   Polyforge    │  breaks the game into steps,
            │                │  routes each step, checks results
            └───┬───┬───┬────┘
        ┌───────▼┐ ┌▼──────┐ ┌▼─────────┐
        │ Model A│ │Model B│ │ Model C… │  the right model for each
        │ (code) │ │(design)│ │ (art/QA) │  job — cloud or local
        └───────┬┘ └┬──────┘ └┬─────────┘
            ┌───▼───▼────────▼───┐
            │  Real engines      │  Godot first, then Unity /
            │  Godot · Unity ·   │  Unreal / Blender — output is a
            │  Unreal · Blender  │  standard project you own
            └────────────────────┘
```

The working loop: **brief → plan → build → run → check against the plan → fix or ask you → checkpoint.** Every accepted state is saved, so a bad step never costs you a good build.

### Design principles

1. **Built for games, start to finish.** Games break down into smaller pieces unusually well — a core loop, scenes, systems, content. Polyforge's staged method (a playable skeleton first, then verified layers) is built around that, with a check at every join.
2. **The right model for each job.** Coding, design, art direction, music and playtesting are different strengths. Polyforge routes each role — Director, Architect, Coder, Artist, Composer, Playtester, Verifier — to the model that does it best, and a new model release is a config change, not a rebuild.
3. **The builder never grades its own homework.** Building and checking are separate steps, ideally separate models. A step passes on evidence — the game runs, the tests pass, the screenshot matches the brief, a playthrough reaches the goal — not on the builder's say-so.
4. **Real engines, real projects.** The output is a standard Godot/Unity/Unreal project you can open, edit, and ship. No lock-in, no rebuild-to-export.
5. **You stay the decider.** The tool does the legwork and brings back checked options; taste, direction and the final call belong to the creator. The plan stays under the hood — you see progress, not plumbing.

## What's in this repo

- **The Polyforge core** — provider registry and role router (config-driven), the artifact pipeline roles hand work through, and session checkpoints.
- **The Godot bridge** — the first engine connection: build, run headless, capture screenshots and playthroughs.
- **The skeleton library** — [docs/skeleton-library.md](docs/skeleton-library.md): the finite set of game skeletons (core loop + rules core) that genre after genre is built from.
- **Experiment 1** — [docs/experiment-1-fighter.md](docs/experiment-1-fighter.md): a staged 2D fighter build used to prove the method end to end.
- **The plan** — [ROADMAP.md](ROADMAP.md).

## Prior art — and the gap

| Project | What it proves | What it isn't |
| --- | --- | --- |
| **CodeScape** | An AI game creation engine can accept many providers, with a bridge for external agents | A single vendor's engine; routing different models to different roles by strength is not its design |
| **Verse8** | Orchestrated agents can turn prompts into playable games | Closed platform — their agents, their models |
| **Rosebud AI** | Composing several models (code, image, video) beats one model | *They* pick the models; creators can't route their own |
| **Engine MCP servers** (godot-mcp, Unity-MCP, Blender MCP, UE 5.8 MCP) | An AI can drive a real editor end-to-end | Plumbing, not a product: no roles, routing, or checking |
| **ChatDev / MetaGPT** | Role-based teams (PM, architect, engineer, QA) build software | General software, no engine bridge or game pipeline |

**The gap Polyforge occupies:** an open tool, built only for games, that puts several models to work at their strengths and checks every step before it counts.

## Architecture sketch

- **Model layer** — provider registry (provider APIs, OpenAI-compatible endpoints, MCP clients, local models), capability and cost profiles per model, and the role router.
- **Role layer** — Director (breaks a brief into a build plan), Architect (spec and acceptance criteria), Coder, Artist, Composer, Playtester (plays the build, files evidence), Verifier (judges evidence against the criteria; can send a step back). Roles communicate through artifacts, not chat.
- **Engine layer** — bridges to Godot first (open, scriptable, headless-friendly), then Unity/Unreal/Blender. Screenshots and playthrough capture are the tool's eyes.
- **Loop** — brief → plan → build → run → check → repair or escalate to you → checkpoint.

## Status

In active design and early build (started 2026-10-24). MIT licensed. Design discussion happens in Issues; the build queue is in [ROADMAP.md](ROADMAP.md).

*Polyforge is a working title.*
