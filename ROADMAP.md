# Polyforge — Roadmap

Polyforge is a tool for creating video games efficiently using AI: a creator gives a brief, the tool breaks the game into small buildable steps, builds them in a real engine, checks each step with evidence, and checkpoints every accepted state.

The phases below are this repo's build queue. Total scope is well beyond 20 hours of work; each phase ends in something demonstrable.

## Phase 1 — Orchestration core

- [ ] Provider registry: register models (provider APIs, OpenAI-compatible endpoints, MCP clients, local models) with capability and cost profiles
- [ ] Role router: assign the roles (Director, Architect, Coder, Artist, Composer, Playtester, Verifier) to models by configuration, not code
- [ ] Artifact pipeline: roles hand off files and specs, not chat text
- [ ] Session checkpoints: every accepted state is committed; an interrupted session resumes from the last good state

## Phase 2 — Godot bridge loop

- [ ] Wire the Godot MCP bridge: create and edit project files, run the game headless
- [ ] Capture: screenshots and scripted playthroughs as the tool's eyes
- [ ] The check step: run result, test outcomes, and screenshot comparison gate every accepted step; a failed check sends the step back with the evidence attached

## Phase 3 — Experiment 1: 2D fighter (staged build)

Spec: [docs/experiment-1-fighter.md](docs/experiment-1-fighter.md)

- [ ] Stage 0–1 greybox: one fighter, movement, one attack, hit detection — playable and verified
- [ ] Remaining stages through the launch roster (4 fighters, 1 stage, local versus, CPU opponent), verifying at each stage

## Phase 4 — Skeleton library

Map: [docs/skeleton-library.md](docs/skeleton-library.md)

- [ ] A file format for genre skeletons: core loop, rules core, and the generic verbs a family supports
- [ ] First families written up as buildable skeletons: traversal, duel (frame-data fighting), physics-puzzle / merger
- [ ] A newly verified verb or system, once proven in a build, is added to its family's skeleton for reuse

## Phase 5 — Demo and second engine

- [ ] End-to-end demo: one-sentence brief → playable Godot micro-game, two different models in two roles
- [ ] Second engine bridge (Unity or Blender)
- [ ] Creator-facing packaging: progress view without exposing the internal plan
