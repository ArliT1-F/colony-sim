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

6. **In-game weighted roadmap panel**
   - 15 features total (top 5 implemented + 10 draft backlog)

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
1. Add lightweight telemetry/debug panel (current multipliers, active policy/weather breakdown).
2. Split systems into modules/files (`systems/*.js`) once feature scope grows further.
3. Add automated sim tests (headless tick simulation) for balance regressions.
4. Integrate the extra 5 newly proposed draft features into the roadmap UI if desired.

