# Multiplayer Design: Answer Stations + Team Charges + Power Play (Netcode for GameObjects)

Summary
- Goal: Let the team control when to answer questions and create meaningful combat windows to damage the boss, with clean server authority and minimal replication.
- Chosen package: 1) Stations + Charges + Power Play — scalable to co-op and later PvPvE.
- Netcode stack: Unity Netcode for GameObjects (server authoritative states, clients request actions).

Core Mechanics
1) Answer Stations (Majority Ready-Check)
- Players approach a Station and press Interact to mark “Ready.”
- When majority (configurable: e.g., ≥50% of connected players) are Ready, the server enters Answer Mode:
  - Show Question UI and start the timer
  - Boss enters question-phase behavior (e.g., patrol tied to question) if desired
- Leaving station or pressing interact again toggles readiness.
- Exiting Answer Mode:
  - On answer submission or timeout, server hides Question UI and stops the timer
  - Boss returns to normal combat
- Anti-grief:
  - Debounce station toggles (e.g., 0.5–1.0s)
  - Rate-limit requests
  - Majority rule to enter/exit
  - Optional lockout (short cooldown) after a miss to prevent spam

2) Team Charges (Shared Resource)
- Correct answers grant Team Charges (integer, max cap e.g., 3–5).
- Any player can spend a charge for:
  - Heavy Attack (e.g., +300–400% damage, short stagger)
  - Utility (optional): barrier pulse, brief heal, or slow field
- Server authoritative on adding/spending charges; clients request spend.
- HUD: show team charge count; feedback when spent (who spent, action type).

3) Team Power Play Window (Burst Moments)
- On correct answer, server starts a First-Hit or Timeout Power Play window (max 5.0s):
  - Select one player (weighted random excluding last winner; brief 0.5s claim option is allowed) and highlight them.
  - The selected player can move and attack; boss continues normal behavior (no freeze). Answer panel is hidden team-wide during the window to keep flow consistent.
  - The window ENDS immediately on the first successful damage instance dealt by the selected player to the boss, or when the 5.0s timer elapses — whichever happens first.
  - First-hit only buff: apply Power Play multipliers to that single hit only; after consumption or timeout, revert to normal rules.
  - Boss vulnerability applies only to that first hit (e.g., 1.75x damage to the boss on that hit).
  - Player damage buff applies only to that first hit (e.g., +35% for that hit). If you prefer a trailing feel, allow a minor +10% decay over 1.0s after consumption (optional).
  - Optional baton bonus: +0.15 if a different player is selected than last time (applied to the first hit, cap total multiplier at 2.0x).
- Team Charges synergy: the empowered hit may be a Heavy Attack if a charge is spent, but the server enforces at most one Heavy Attack per window.
- Hard cap and cooldown: enforce a 10s cooldown between Power Play windows to prevent chaining.


Optional: Time Bank
- Correct answers add seconds to a shared “Focus Bank.”
- Entering Answer Mode consumes banked time; exiting pauses.
- Allows tactical long sessions when team is ready, without stalling indefinitely.

Event and State Model (Server Authoritative)
Server-owned states (replicated via NetworkVariable/NetworkList):
- answerModeActive: NetworkVariable<bool>
- teamCharges: NetworkVariable<int>
- powerPlayActive: NetworkVariable<bool>
- powerPlayTimer: NetworkVariable<float> (server updates; clients display)
- stationReadyMask or stationReadyCount: NetworkVariable<int> or NetworkList<ulong> (player IDs ready)

New Events (integrate with existing EventBus):
- AnswerModeEntered / AnswerModeExited
- TeamChargeAdded / TeamChargeSpent
- PowerPlayStarted / PowerPlayEnded
- StationReadyChanged (for HUD prompt updates)
- NextQuestionRequested (for non-station flows if needed)

RPCs and Validation
Client → Server RPCs (rate-limited, validated):
- RequestToggleReadyAtStation(stationId, isReady)
- RequestSpendTeamCharge(actionType)
- RequestStartAnswerMode() and RequestExitAnswerMode() only accepted if majority reached
- Optional: RequestNextQuestion() for non-station single-player mode
Server → Clients:
- AnswerMode state changes
- Team charge count updates
- Power Play state & timer
- Station prompts (ready counts)

Integration Points
QuestionManager
- Decouple auto-next-question:
  - After AnswerSubmitted or Timeout: do NOT auto-start next question
  - Hide panel and stop timer; set pending state until station majority requests new session
- On AnswerModeEntered: start question, show panel, run timer
- On AnswerModeExited: hide panel, stop timer, settle results

BossCombat
- On PowerPlayStarted: set vulnerable/exposed flag, optionally pause question patrol for the window
- On PowerPlayEnded: clear vulnerability, resume normal behavior
- Ensure movement/attack logic plays nicely with exposure (no conflicting animation windows)

PlayerWeaponController
- Apply temporary damageMultiplier buff during Power Play (e.g., +50% for 3s)
- Heavy Attack path consumes a team charge; server validates and broadcasts spend
- Maintain compatibility with existing Hitbox/Hurtbox timing and PlayerQuestionGuard behavior

HUD/UI
- Answer Station Prompt: “Ready at Station — 2/3” (updates from StationReadyChanged)
- Team Charges: icon stack or counter (replicated int)
- Power Play: bar/timer with multiplier text; boss weak-point indicator
- Keep lightweight; prioritize clear feedback in co-op

Pacing & Balance (initial tuning)
- Station majority: ≥50% players (configurable)
- Team charges: max 3; one charge per correct answer
- Power Play window: 1.8s; vulnerability 2x; player buff +50% for 3s
- Baton multiplier: +0.25 ramp per different player correct (cap at 2.0x total)
- Debounce: 0.5s on station toggles; spend rate limit 0.2s; global cooldowns as needed

Authority & Anti-Grief
- Server authoritative on answer state, questions, charges, power play timers
- Validate answer submissions server-side
- Majority gating; lockouts after miss/timeouts; hard caps on window chaining
- Log suspicious toggling or spend patterns for tuning

Testing Plan
- Single-player simulation: ensure station logic still allows manual next-question and charges/power-play work locally
- Two+ players (local host + clients): verify:
  - Ready-check majority gates entry/exit
  - Charges replicate and spend correctly (no desync)
  - Power Play timer replicated and ends cleanly on all clients
  - Boss vulnerability and player buff apply consistently
- Edge cases: player disconnects mid-answer; host migration (if applicable); late-join replication of states

Roadmap
Phase 1 (Local + server authority):
- Implement Station prefab & controller (server-tracked readiness)
- Wire QuestionManager to station majority for AnswerMode enter/exit
- Add Team Charges + spend RPCs; add Power Play window
- Minimal HUD elements

Phase 2 (UX & balance):
- Polish prompts, charge visuals, power play bar, weak-point feedback
- Tune durations, caps, and debounces
- Add optional Time Bank if desired

Phase 3 (Netcode polishing):
- Robust validation, cooldowns, error feedback
- Stress test replication; performance checks
- Prepare for PvPvE variants (contested stations, per-team charges)

Adoption Notes
- Backward compatible with current single-player flow by using a simple keybind to mark ready (majority of 1).
- Station logic can be swapped for manual NextQuestion in non-multiplayer scenes.