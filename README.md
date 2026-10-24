# Polyforge

**An open, LLM-agnostic game creation platform. Plug in any model — or several at once — each routed to the work it does best.**

One game. Every model at its strength.

---

## The problem

Today's AI game builders lock you into *their* models:

- **Rosebud, Star, Websim, Verse8** compose models internally, but the platform chooses them. You can't bring your own, swap a weak link, or use a local model.
- The models you'd want are good at *different things* — one writes the cleanest code, another has the best design taste, another generates the most consistent art, another is the cheapest tireless playtester. A single-model platform averages all of that away.
- One model left alone both **proposes and disposes** — it generates and judges its own work — and that is where quality collapses. Every experienced builder rediscovers this: you need a verifier that is not the author.

Meanwhile the parts for something better already exist in the open: MCP bridges let any capable LLM drive real engines (Godot, Unity, Unreal, Blender), model gateways route between providers, and multi-agent frameworks (ChatDev, MetaGPT) proved role-based teams work for general software. Nobody has assembled them into a game creation platform whose core design is *model pluralism*.

## The idea

Polyforge is the orchestration layer between **you** (the creative director), **a team of models**, and **real game engines**:

```
        You — direction, taste, final say
                    │
            ┌───────▼────────┐
            │   Polyforge    │  roles, routing, verification, memory
            └───┬───┬───┬────┘
        ┌───────▼┐ ┌▼──────┐ ┌▼─────────┐
        │ Model A│ │Model B│ │ Model C… │  any provider, any endpoint,
        │ (code) │ │(design)│ │ (art/QA) │  cloud or local
        └───────┬┘ └┬──────┘ └┬─────────┘
            ┌───▼───▼────────▼───┐
            │  Engine bridges    │  Godot · Unity · Unreal · Blender
            │  (MCP tool layer)  │  → a real, exportable game project
            └────────────────────┘
```

### Design principles

1. **Any LLM can plug in.** OpenAI-compatible endpoints, provider APIs, MCP clients (Claude Code, Cursor, Codex, Gemini CLI…), and local models (Ollama) are all first-class. A new model release is a config change, not a platform update.
2. **Many LLMs at once, routed by strength.** Each role — Director, Architect, Coder, Artist, Composer, Playtester, **Verifier** — can run on a different model, chosen per role and per budget. Routing is data, not code: benchmark a role, swap the model, keep the game.
3. **Propose ≠ dispose.** No model judges its own output. Generation and verification are separate roles, ideally separate models, and verification is grounded in evidence: the game compiles, the tests pass, the screenshot matches the brief, a playthrough reaches the goal.
4. **Real engines, real projects.** Output is a standard Godot/Unity/Unreal project you own and can open, edit, and ship — no platform lock-in, no rebuild-to-export.
5. **The human stays the decider.** The platform does the legwork and brings back checked options; taste, direction and the final call belong to the creator.

## Prior art — and the gap

| Project | What it proves | What it isn't |
| --- | --- | --- |
| **CodeScape** | An AI game creation engine can accept many providers + custom endpoints, with an MCP bridge for external agents | A single vendor's engine; model-plural *routing by role/strength* is not its stated design |
| **Verse8** | Multi-agent orchestration can turn prompts into playable games | Closed platform, their agents and models, crypto/IP layer attached |
| **Rosebud AI** | Composing several models (code, image, video) beats one model | *They* pick the models; users can't plug in or route their own |
| **Engine MCP servers** (godot-mcp, Unity-MCP, Blender MCP, UE 5.8 MCP) | Any MCP-capable LLM can drive a real editor end-to-end | Plumbing, not a platform: no roles, routing, taste, or verification |
| **ChatDev / MetaGPT** | Role-based agent teams (PM, architect, engineer, QA) build software | General software, OpenAI-centric, no engine bridge or asset pipeline |
| **UniGen (research)** | Multi-agent pipelines can generate playable 3D prototypes in Unity | A paper, not a platform |

**The gap Polyforge occupies:** open + any-LLM + *several* LLMs routed by strength + game-native creation, with verification built in.

## Architecture sketch

- **Model layer** — provider registry (API, OpenAI-compatible, MCP client, local), capability profiles per model (code, design, art direction, audio, long-context, cost), and a role router that assigns work accordingly.
- **Role layer** — Director (breaks a brief into a build plan), Architect (spec, acceptance criteria), Coder, Artist, Composer, Playtester (plays the build, files evidence), Verifier (judges evidence against criteria; can veto). Roles communicate through artifacts, not vibes.
- **Engine layer** — MCP tool bridges to Godot first (open, scriptable, headless-friendly), then Unity/Unreal/Blender. Screenshot + playthrough capture as the platform's eyes.
- **Loop** — brief → plan → build → run → verify against criteria → repair or escalate to the human → checkpoint (every accepted state is committed; nothing good is ever lost to a bad prompt).

## Roadmap (v0.x)

- [ ] Provider registry + role router (config-driven)
- [ ] Godot MCP bridge wired to Coder + Playtester + Verifier loop
- [ ] First end-to-end demo: one-sentence brief → playable Godot micro-game, two different models in two roles
- [ ] Screenshot-diff verification for art/level briefs
- [ ] Second engine bridge (Unity or Blender)

## Status

Concept repository — created 2026-10-24. Design discussion happens in Issues.

*Name is a working title; the repo is private while the concept settles.*
