# Colony Sim – Incremental + RPG Expansion

Colony Sim is a browser-based incremental management game that now blends:

- classic resource colony simulation,
- construction/research/prestige progression,
- RPG hero progression,
- faction diplomacy,
- expeditions,
- seasonal world simulation.

This project is currently a single-file game (`index.html`) plus documentation/handoff files.

---

## Current Gameplay Scope

### Core Simulation (original systems, rebalanced)
- Resource economy: Food, Wood, Stone, Metal, Science, Energy
- Job assignment system
- Building construction queue with scaling costs/timers
- Research tree with prereqs and economy modifiers
- Random positive/negative colony events
- Population growth, births, starvation, happiness
- Prestige loop with permanent upgrades
- Long-run victory condition

### New Systems (implemented)
1. **Faction Reputation & Diplomacy Actions**
2. **Hero RPG Progression (XP, level, perks, gear)**
3. **Expedition Quest Board**
4. **Policy / Law System**
5. **Season + Weather Simulation**

### UI Additions (implemented)
- Policy panel
- Hero command panel
- Faction diplomacy panel
- Expedition board + active expedition progress
- In-game roadmap panel (implemented priorities + draft backlog)

---

## Recent Balance Passes

### Expansion-system balance
- Policy slot cap added (`MAX_ACTIVE_POLICIES = 2`)
- Policy strengths/penalties tuned for tradeoffs
- Expedition success curve, costs, durations, rewards, and cooldowns tuned
- Passive reputation gain reduced to limit runaway diplomacy loops

### Core-mechanics balance (legacy systems)
- Event pressure reduced (`EVENT_BASE_CHANCE` down, cooldown up)
- Food economy tightened (slightly lower farmer output + higher baseline food consumption)
- Building progression smoothed (cost/build-time scaling adjusted)
- Energy demand curve reweighted toward population pressure
- Population growth chance reduced for slower snowball
- Prestige unlock requirements raised for deeper run commitment
- Prestige point formula adjusted for steadier meta progression
- Positive/negative random events rebalanced to reduce swinginess

---

## Controls / Play Notes

- Assign colonists in **Jobs**
- Build infrastructure in **Buildings**
- Unlock multipliers in **Research**
- Use **Policies** for strategic tradeoffs (max 2 active)
- Grow your hero and run **Expeditions**
- Build faction **Reputation** and trigger diplomacy actions
- Manage **Energy** and **Food** to avoid efficiency loss/starvation
- **Prestige** when threshold is met to gain long-term power

---

## Save / Load

- Save key is browser `localStorage`-based
- Includes migration guards for new state fields (hero, factions, calendar, policies, expedition, buffs)
- If balancing is changed heavily, older saves still attempt compatibility via default-field hydration

---

## Draft Feature Backlog (all current drafts)

The project currently has **16 draft features** (10 original draft backlog + 5 additional draft ideas + 1 balancing/ops draft) grouped below into **pairs of 2**.

### Pair plan with weights, update name, and version

> Pair weight = combined importance score for the two features.

| Pair | Features (2) | Pair Weight | Update Name | Version |
|---|---|---:|---|---|
| 1 | District Zoning + Supply Chains | 14 | **Steel & Streets** | `v0.9.0` |
| 2 | Character Traits for Colonists + Guilds & Internal Politics | 12 | **Faces of the City** | `v0.9.1` |
| 3 | Espionage Layer + Crisis Council | 11 | **Shadows & Sirens** | `v0.9.2` |
| 4 | City Projects + Great Wonders & National Legacies | 10 | **Monuments of Power** | `v0.9.3` |
| 5 | Naval Logistics + Dynamic Market & Trade Prices | 12 | **Tides of Commerce** | `v0.9.4` |
| 6 | Artifact Crafting + Ecology & Pollution | 8 | **Relics and Ruin** | `v0.9.5` |
| 7 | Lineage / Dynasty System + Religion / Culture / Ideology Blocs | 11 | **Blood and Belief** | `v0.9.6` |
| 8 | Infrastructure Wear & Maintenance + Telemetry & Balance Dashboard | 10 | **Stress Test** | `v0.9.7` |

### Draft feature weights (reference)
- District Zoning (7)
- Supply Chains (7)
- Character Traits for Colonists (6)
- Guilds & Internal Politics (6)
- Espionage Layer (6)
- City Projects (5)
- Naval Logistics (5)
- Crisis Council (5)
- Artifact Crafting (4)
- Ecology & Pollution (4)
- Dynamic Market & Trade Prices (7)
- Lineage / Dynasty System (6)
- Infrastructure Wear & Maintenance (6)
- Religion / Culture / Ideology Blocs (5)
- Great Wonders & National Legacies (5)
- Telemetry & Balance Dashboard (4)

---

## Development Notes

- Main runtime: plain browser JS in `index.html`
- Styling: Tailwind CDN
- No build step required for local play: open `index.html` in a browser

If this session context is lost, see **`agent.md`** for detailed handoff state.
