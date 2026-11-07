# Polyforge Experiment 1 — 2D Fighter (Street Fighter clone)

Status: experiment spec (2026-10-24).
This is a test case for the hidden-plan, staged-build approach — not a
commitment to ship this fighter as a product.

Two standing rules for the experiment:
- The system's internal build plan does NOT need to be surfaced to the end
  user. The plan lives under the hood; the user just sees progress.
- Games decompose into smaller pieces — arguably more easily than regular
  software. Breakdown + a verified check at every join is the method.

## Target (v1)

Local 2D fighter: two fighters, one screen with corners, best-of-3 rounds,
99-second timer. Launch scope: 4 fighters, 1 stage, local versus, CPU
opponent, training mode. No online play, no story mode. Those are expansion
packs after the core proves itself.

## Rules the whole build obeys

- Fixed 60 fps simulation, separate from rendering, so frame data means the
  same thing on every machine.
- Fighters are data, not hard-coded scripts: every move has startup / active /
  recovery frames, damage, range, pushback, stun, cancel rules.
- Hitboxes and hurtboxes are invisible boxes the code checks — sprites are
  clothing on top. Art never decides whether a hit lands.
- Every stage has a playable checkpoint and a written pass/fail test before
  moving on.

## The shared skeleton (built once, at Stage 0–1)

One fighter contract every character reuses:
- one body: anchor point, facing, pushbox, hurtbox, width/height
- one brain: state machine — idle, walk, crouch, jump, attack, hitstun,
  blockstun, knockdown, wake-up
- one clock: current frame counter for the move
- one input buffer
- one health/meter object
- one move table
- animation slots with fixed names: idle, walk, jump, lp, mp, hp, hit,
  block, ko, etc.

Specials are built from generic verbs the skeleton understands, never
one-off code hacks:
spawn_projectile, dash_forward, teleport_relative, command_grab,
armor_frames, invulnerable_frames, chain_input_window, knockdown, launch,
recoil_pushback.

A special move is then just a data entry: animation slot, startup/active/
recovery frames, hitbox, damage, and which verbs fire on which frames.

## Staged build plan

- Stage 0 — Skeleton: empty arena, fixed-timestep loop, keyboard + gamepad
  input, debug readout (frame, fighter state, fps). Pass: locked 60 fps,
  toggleable overlay.
- Stage 1 — Greybox movement: two plain capsules; walk, crouch, jump arc,
  fighters always face each other, walls act as corners. Pass: movement
  matches the numbers on paper, measured in frames.
- Stage 2 — Combat core: health, hitstun, blockstun, knockback, KO, round
  reset, timer. One attack only: a jab (4 startup / 3 active / 8 recovery,
  30 damage; health is 1000). Blocking: hold back; chip damage 10%. Pass:
  jab connects exactly on frame 5 and not before; whiff never registers;
  blocking converts to chip; trades hurt both; double KO and timeout
  resolve cleanly.
- Stage 3 — Move system: full state machine, cancels into specials only in
  defined windows. Per fighter: light/medium/heavy punch and kick standing,
  simplified crouching set, one throw (beats blocking), two specials with
  classic motion inputs and a 6-frame input buffer. Pass: real frames
  measured from the running game match the data sheet; motions succeed at
  human speed and fail when performed wrong; throw whiffs at range and
  beats a blocking dummy.
- Stage 4 — Animation and hitbox alignment: real sprites on the same
  anchors; hitbox debug view stays available forever. Pass: animation
  changes never change frame data; a hit that looks like it connects does,
  frame by frame in the overlay.
- Stage 5 — Opponent: local 2-player first, then training dummy
  (stand/crouch/block/jump/record-playback), then layered CPU with human
  reaction delays and mistake rates. Difficulty = reaction time and mistake
  rate, not damage multipliers. Pass: a new player beats Easy, a decent
  player loses to Hard, the AI never does anything human inputs couldn't.
- Stage 6 — Game shell: title, character select, versus, round intro,
  KO slow-mo beat, victory pose, rematch/change character, pause, training
  menu, input display, control remapping, settings saved to disk.
- Stage 7 — Presentation: one stage with parallax, hitsparks, small
  screen-shake on heavy hits only (settings toggle), event-tied SFX,
  announcer clips, music that adds a layer in the final round.
- Stage 8 — Balance and QA: full frame-data table reviewed; edge cases —
  projectile vs projectile, hitting a fighter on the landing frame, corner
  pushback with no room, pausing mid-round, both players remapping.
  Performance checked on a low-end machine.
- Stage 9 — Ship: export builds, icon, credits, store page with the
  frame-data sheet as a player-facing extra. Growth list afterwards, in
  order: fighters 3–4 are already in the launch roster plan below; then a
  second stage, arcade ladder, and only much later netcode.

## Team mapping (the Polyforge part)

Director holds the plan and checkpoint list. Coder builds one stage at a
time. Artist works a stage ahead on sprites. Verifier — a different model —
runs the pass tests, measures real frame data from the running game, and
can veto a stage. The human is called in only at feel checkpoints:
movement (Stage 1), the jab (Stage 2), the full duel (Stage 5).

## Launch roster — 4 fighters, 2 human players (local versus + CPU)

Health is 1000 unless noted. Frame data is startup / active / recovery,
damage in points.

### 1. KAITO — the all-rounder (tuning baseline; built first)
Medium health, medium speed, a tool for every situation.
- Standing: LP jab (4/3/8, 30) · MP straight (7/4/14, 60) · HP hook
  (11/4/20, 90) · LK knee (5/3/10, 40) · MK roundhouse (10/4/18, 80) ·
  HK sweep-kick (13/4/24, 100)
- Crouching: LP (4/3/8, 30) · LK (5/3/9, 40) · HP uppercut (6/5/22, 90,
  standing anti-air) · HK sweep (11/3/26, 90, knocks down)
- Throw: shoulder toss, 140
- Specials: Spirit Bolt (QCF+P — fireball, 13f startup, 60, speed varies
  by button) · Rising Dragon Fist (F-D-DF+P — leaping uppercut, 4f startup,
  invincible frames 1–6, 100, huge recovery on whiff) · Hurricane Kick
  (QCB+K — spinning advance, 80, flies over low attacks)
- Super: Dragon Tempest (double QCF+P — giant multi-hit fireball, 250)

### 2. BRUNO "THE WALL" KOVAC — the grappler (built second)
Health 1150, slowest walk, longest throw range. Tests big bodies, armor,
grabs.
- Standing: LP chop (6/3/12, 40) · MP body blow (10/4/18, 80) · HP headbutt
  (14/5/24, 110) · LK stomp (7/3/14, 50) · MK knee (12/4/20, 90) ·
  HK bear swipe (16/5/28, 120)
- Crouching: LP (5/3/10, 40) · LK shin kick (6/3/12, 50) · HP rising
  headbutt (9/5/26, 100) · HK trip (13/4/30, 100, knocks down)
- Throw: backbreaker slam, 180
- Specials: Iron Grasp (half-circle+P at point blank — command grab, 5f
  startup, 200, 45f recovery on whiff) · Bull Charge (charge B, F+P —
  shoulder rush with armor, absorbs one hit, 90) · Cyclone Lariat (P×3 —
  spinning double-arm strike, hits both sides, beats projectiles, 100)
- Super: Final Verdict (full-circle+P at point blank — cinematic slam, 300)

### 3. SABLE — the rushdown (built third)
Health 900, fastest feet, no fireball; only fighter with a quick forward
step-dash. Tests chains, air moves, the dash.
- Standing: LP snap jab (3/2/6, 25 — fastest normal in the game) · MP elbow
  (6/3/12, 50) · HP palm strike (9/3/18, 70) · LK shin tap (4/2/8, 30) ·
  MK side kick (8/3/14, 70) · HK axe kick (12/4/20, 90, overhead)
- Crouching: LP (3/2/6, 25) · LK slide-step kick (5/3/10, 35, moves her
  forward) · HP launcher-lite (8/4/20, 80) · HK low sweep (10/3/22, 80,
  knocks down)
- Throw: flipping neck throw, 120
- Specials: Triple Fang (QCF+P, then P up to twice more — three-hit dashing
  chain, 40 per hit) · Heel Drop (in air, QCB+K — diving kick, 70) ·
  Phantom Step (QCB+P — fast dash through the opponent's attack, no hitbox)
- Super: Hundred Hands (double QCF+P — rushing barrage + launcher, 240)

### 4. VESPER — the zoner (built last)
Health 950, slow walk, staff normals reach ~twice as far as anyone else's.
Tests projectiles and extreme range after the hitbox system is proven.
- Standing: LP staff jab (6/3/12, 35, long reach) · MP thrust (10/4/16, 65,
  very long) · HP overhead staff slam (15/4/24, 95) · LK prod (6/3/12, 40)
  · MK sweeping arc (12/4/20, 85) · HK rising arc (14/5/26, 100, anti-air)
- Crouching: LP (5/3/10, 35) · LK low prod (6/3/12, 40) · HP upward thrust
  (10/4/22, 85) · HK low sweep (13/4/26, 90, knocks down, longest sweep)
- Throw: staff-vault toss, 130 — throws the opponent far away, back to her
  preferred range
- Specials: Grave Orb (QCF+P — slow floating projectile, 55; slow is the
  point, it walks beside her as a shield) · Ashen Step (QCB+P — reappears a
  set distance back, or forward with kick; pure spacing, long recovery if
  baited) · Reaper's Arc (charge D, U+K — leaping staff spin, 85, unsafe
  on block)
- Super: Covenant of Thorns (double QCF+K — ground eruption across most of
  the screen, 230)

### Why these four work as a set
Vesper keeps Bruno out with range, but Sable dashes through her spacing
and mauls her up close. Bruno needs only two touches, but Kaito's fireball
and uppercut keep him honest. Kaito answers everyone and is weak against
no one — which is why he is the tuning baseline. Every matchup teaches a
different skill: patience, spacing, pressure, defense.

Build order: Kaito first (defines the data template), Bruno second,
Sable third, Vesper last.

## Phone-first trial

The Stage 0–1 greybox is small enough to build on the phone itself:
- Tool: GDevelop's iOS app — a real phone-native editor with cloud sync,
  and GDevelop 5 is already on the dev Mac, so the same project opens
  on desktop later with no rebuild. (Its built-in AI assistant is
  GDevelop's own, credit-metered, no bring-your-own-model — the trial is
  about the skeleton, not the AI.)
- Build only: one arena, two capsules, walk/crouch/jump, facing rule,
  corner walls, and an on-screen frame/state readout.
- Pass test (same as Stage 1): walk speed, jump height and jump duration
  match the numbers on paper, measured in frames — not eyeballed.
- If the greybox feels right on the phone, the project graduates to the
  Mac and Stage 2 (the jab) is next. If it doesn't, nothing else was built
  on top of it — which is the whole point of the staged plan.
