# Polyforge — Skeleton Family Coverage Map

Drafted 2026-10-24 from a full audit of classic game genres. Status: working draft — the coverage target for the skeleton library (ROADMAP Phase 4).

## The core idea

The classic genre list is finite at any given moment (finite like the list
of known species: complete for today, grows slowly — roughly one real new
family every few years, e.g. battle royale after PUBG, survivors-like after
Vampire Survivors). Genres are not the unit a platform has to build.
**Skeleton families** are: one state machine + core loop + physics/rules
core, reused across many genre labels. Genre labels on top of a skeleton
are mostly different verbs, data, and tuning.

So Polyforge's coverage target is not "~40 genres." It is the much smaller
set of skeleton families below. A genuinely new family is not backlog —
it is an invention, and gets added when it appears.

## The families

| # | Skeleton family | Core machine | Genre labels it covers |
|---|---|---|---|
| 1 | Traversal / character action | Run, jump, collide, camera follows one character | Platformer (2D/3D/precision), beat 'em up, hack-and-slash, endless runner |
| 2 | Shooter | Aim, fire projectiles, hit resolution, enemy waves/AI | FPS, third-person, twin-stick, rail/light-gun, shmup, bullet hell, hero shooter, extraction shooter |
| 3 | Duel / frame-data fighting | Two bodies, move tables (startup/active/recovery), hitbox vs hurtbox, rounds/timer | 2D/3D fighters, arena/platform fighters, boxing/MMA/wrestling games. Spec already exists: Polyforge Experiment 1 (2D fighter) |
| 4 | Vehicle physics | One vehicle, throttle/steering physics, track or open course, lap/position timing | Arcade/sim/kart/futuristic racing, vehicular combat, flight/space/driving sims |
| 5 | Physics-puzzle / merger | Drop or launch objects into a container, contact physics, merge/score rules, hazard line | Physics puzzles, bubble shooter, Suika-style mergers (Falling itself), brick-breaker (aim + bounce variant) |
| 6 | Tile / falling-block puzzle | Grid, falling or placed pieces, match/clear rules, rising difficulty | Falling-block (Tetris family), match-3, tile-matching |
| 7 | Logic-deduction puzzle | Static grid, rule set, solver-checkable unique solution | Sudoku family, Picross, Light Up, Black Box, Masyu, Hashiwokakero, Mid-Loop (Tenraku's core five live here), Sokoban/block-pushing (adds movement) |
| 8 | Strategy / tactics | Units on a map (grid or continuous), orders, fog/economy, win by position or elimination | RTS, turn-based strategy, 4X, tactics, grand strategy, tower defense (fixed attackers' path variant), MOBA, auto battler, wargames |
| 9 | Management / simulation-of-people | Simulated agents + budgets/schedules; player sets policy, system plays it out | Life sim, city builder, tycoon, sports management, colony sim (RimWorld family), farming sim, god game |
| 10 | Factory / automation | The player builds a production line; the machine plays the game | Factory/automation (Factorio, shapez), programming games (player writes the machine in code) |
| 11 | RPG / growth | Character stats, inventory, encounters, XP/level curve, quest state | JRPG, Western RPG, action RPG, tactical RPG (overlaps #8), dungeon crawler, roguelike/roguelite (run structure on top), monster tamer, virtual pet (care loop, minimal combat), MMORPG (networked) |
| 12 | Narrative / adventure | Nodes of text/scene, choices or object interaction advance state | Point-and-click, text adventure/interactive fiction, visual novels, dating sims, interactive movie/FMV, walking simulators |
| 13 | Card / board systems | Deck/hand state, turn order, a rule engine for a fixed game | CCG/deck-builders, traditional cards, solitaire, board/dice adaptations, casino games, trivia |
| 14 | Survival / sandbox crafting | Open world, resource gathering, crafting tree, needs/threat pressure | Sandbox builders, survival-crafting, open-world variants of #1/#11 |

## Borrowers, not owners (from the genre audit)

- **Sports games** borrow: sim sports = vehicle-like physics + team AI
  (family #4 physics with #8 orders), management = #9, arcade sports = #1,
  motion/party sports = minigame collections (#15 below).
- **Survivors-like / bullet heaven** = #2 inverted (the player is the
  bullet hell); no new skeleton needed.
- **Social deduction** = multiplayer voting/lying layer over a chat-like
  state machine — the one arguable gap in this map; flagging it rather
  than forcing it into a family.
- **Party / minigame collections** = a shell that hosts small versions of
  other families; rhythm/music = timing-input variant of #1/#6.

## What this means for the build order

1. Each family = one shared skeleton built once (like the fighter
   contract in Experiment 1: one body, one brain/state machine, one
   clock, data-driven content on top).
2. Genre coverage then becomes data: move tables, tile sets, rule
   parameters, level content — the part LLM role-routing is good at
   generating and verifying.
3. Verification differs per family and should be defined with the
   skeleton (fighter: frame-data tests; logic puzzles: solver proves a
   unique solution; merger: tier-ladder reachability), matching
   Polyforge's propose ≠ dispose rule.

## Open questions

- Is social deduction a 15th family, or a multiplayer layer that sits
  across families?
- Build order: fighter is Experiment 1 — is the merger family (#5,
  Falling's own skeleton) the natural Experiment 2, since a working
  instance already exists?
- Does this map go into the private polyforge repo as a design doc, or
  stay a local draft while the concept settles?
