# Agent Handoff – Colony Sim Expansion

## Purpose
This file is a continuity handoff so another agent can resume work if chat context is lost.

## Current branch / status
- Branch: `cursor/sim-rpg-expansion-223d`
- Base: `main`
- Main feature PR already exists for expansion work.
- Project is single-file gameplay implementation in `index.html` (plus this handoff file).

## What is implemented (high-level)
The game was expanded from a basic incremental colony simulator into a deeper sim/RPG hybrid with:

1. **Faction Reputation & Diplomacy Actions**
   - 3 factions (`riverGuild`, `ironLegion`, `sunAcademy`)
   - Passive faction bonuses and active cooldown abilities with rep costs
   - Reputation gains via expeditions + passive per-tick drift

2. **Hero RPG Progression**
   - Hero XP, level-ups, scaling XP thresholds
   - Gear crafting and perk upgrades
   - Hero stats affect expeditions and economic/security systems

3. **Expedition Quest Board**
   - Multiple quests with duration, cost, difficulty, rewards, failure penalties
   - Active expedition progress UI and completion processing

4. **Policy/Law System**
   - Toggleable policies with tradeoffs (food, science, security, diplomacy, etc.)
   - Policy effects integrated into rates/security/happiness/reputation logic

5. **Season + Weather System**
   - Calendar with seasons and rotating weather profiles
   - Weather effects integrated into production, energy demand, and happiness

**District Zoning**
   - New district panel with setup costs, unlock gates, and district-capacity limits
   - District effects include per-tick upkeep plus adjacency/synergy bonuses that scale with buildings and complementary districts
   - District output affects economy, security/politics, and happiness calculations

**In-game weighted roadmap panel**
   - 15 features total (top 6 implemented + 9 draft backlog)

## Additional balancing pass (latest)
A follow-up balancing update was applied after initial implementation:

- **Policy slot cap**
  - Added `MAX_ACTIVE_POLICIES = 2`
  - Cannot activate more than 2 policies at once
  - `togglePolicy` now blocks activation beyond cap and logs feedback
  - `renderPolicies` shows active count and disables non-active policy buttons when at cap

- **Softer policy extremes**
  - Reduced several policy effect magnitudes (less runaway stacking)
  - Examples:
    - `industrialMandate` reduced raw output boost and science penalty
    - `scholarshipFund` reduced science boost and upkeep penalty
    - `greenInitiative`, `civilGuard`, `openDiplomacy` slightly toned down

- **Expedition success formula rebalance**
  - `getQuestSuccessChance` now uses:
    - Lower base success
    - Lower stat scaling
    - Difficulty weighted more strongly
  - Clamped to 10%–90% to avoid near-guaranteed outcomes

- **Quest economy adjustments**
  - Increased some mission costs and reduced some rewards/rep gains
  - Slightly increased mission durations on mid/late quests
  - Reduced punitive fail penalties on some early/mid quests

- **Faction progression smoothing**
  - Reduced passive per-tick faction rep gain in `gameTick`
  - Reduced baseline rep gain multiplier floor in `gainFactionReputation`
  - Slightly reduced some faction passive coefficients and/or increased action friction

## Core-mechanics rebalance (newest pass)
The original/base game loop (pre-RPG systems) was also rebalanced:

- **Event pacing and volatility**
  - `EVENT_BASE_CHANCE`: reduced from `0.04` -> `0.03`
  - `EVENT_COOLDOWN_TICKS`: increased from `70` -> `90`
  - Goal: fewer swingy interruptions and more stable planning windows.

- **Building economy pacing**
  - Cost scaling reduced in `getBuildingCost`: `1.18` -> `1.15`
  - Build-time growth reduced in `calculateBuildTimeTicks`: `+6%` per stack -> `+5%`
  - Goal: less punishing exponential friction in midgame infrastructure.

- **Energy system tuning**
  - Demand baseline in `computeRates` slightly increased:
    - population factor `0.2` -> `0.22`
    - building factor `0.15` -> `0.17`
  - Goal: make generator/power investment matter earlier.

- **Food/survival tuning**
  - Base food consumption adjusted upward:
    - `foodConsumptionMult` starts at `0.25` -> `0.28`
    - per-colonist consumption factor `0.8` -> `0.78` (net slightly harder early game)
  - Starvation loss reduced:
    - `5%` pop loss -> `4%`
  - Goal: more meaningful food management without overly harsh death spirals.

- **Population growth pacing**
  - Migrant growth condition tightened:
    - requires `rates.food > 1.4` (was `> 1`)
    - requires food stock `> 45` (was `> 30`)
  - Base migrant chance reduced:
    - `0.1 + 0.02*(level-1)` -> `0.07 + 0.015*(level-1)`, capped at `0.3`
  - Birth cadence slowed:
    - `BABY_COOLDOWN_TICKS`: `50` -> `65`
  - Goal: smooth runaway population inflation.

- **Core research pacing**
  - Increased costs for key progression techs:
    - `improvedTools` 40 -> 55
    - `agriculture` 80 -> 95
    - `housing` 70 -> 85
    - `logistics` 90 -> 115
    - `energyTech` 120 -> 145
    - `forestry` 70 -> 90
    - `mining` 90 -> 110
    - `education` 110 -> 135
    - `festivalCulture` 120 -> 145
    - `medicine` 130 -> 160
    - `securityTheory` 150 -> 175
    - `civicPlanning` 160 -> 185
    - `smartGrid` 200 -> 240
    - `megaEngineering` 600 -> 700
  - Goal: preserve meaningful long-run progression and prevent rushing late unlocks.

- **Prestige gate tuning**
  - `canPrestigeNow` raised from roughly:
    - level `3+`, buildings `8+`, research `2+`
    - to level `4+`, buildings `12+`, research `4+`
  - `estimatePrestigePointsGained` adjusted:
    - old: `floor(pop/25 + buildings/10 + research/4)`
    - new: `floor(pop/28 + buildings/11 + research/4.5)`
  - Goal: make prestige timing more deliberate and reduce early spam loops.

## Key files
- `index.html` – all gameplay systems, rendering, save/load, roadmap panel
- `agent.md` – this handoff file

## Important functions and where to look
- Balance knobs:
  - `POLICY_DEFS`
  - `FACTION_DEFS`
  - `QUEST_DEFS`
  - `HERO_PERK_DEFS`, `HERO_GEAR_DEFS`
  - `MAX_ACTIVE_POLICIES`
- Mechanics:
  - `getPolicyEffects`
  - `getQuestSuccessChance`
  - `gainFactionReputation`
  - `computeRates`
  - `computeSecurityAndPolitics`
  - `gameTick`
- UI:
  - `renderPolicies`
  - `renderHero`
  - `renderFactions`
  - `renderExpeditions`
  - `renderRoadmap`
- Persistence:
  - `DEFAULT_STATE`
  - `loadGame`

## Save compatibility notes
`loadGame` includes migration/guard code for new systems:
- `activePolicies`
- `calendar`
- `hero` (with default fields)
- `factions`
- `temporaryBuffs`
- `activeExpedition`

If new state keys are added later, mirror this migration pattern.

## Known design choices
- Entire game remains in one HTML file for now (intentionally matches repo simplicity).
- Policy cap is hardcoded to 2 active policies for balance and strategic tradeoff.
- Expedition success intentionally avoids >90% or <10% to preserve uncertainty.

## Suggested next steps
Implement **Supply Chains** (the next roadmap feature) with intermediate goods and building consumption loops.
2. Add lightweight telemetry/debug panel (current multipliers, active policy/weather/district breakdown).
3. Split systems into modules/files (`systems/*.js`) once feature scope grows further.
4. Add automated sim tests (headless tick simulation) for balance regressions.

