# Neverwinter Campaign — Claude Session Memory

This file is read automatically at the start of every Claude Code session. It captures the full working context for the Neverwinter D&D campaign project so no session starts cold.

---

## Working Agreement

1. **Ask before each edit.** Propose changes, get approval, then act. Do not make unilateral edits.
2. **The user writes all character names and personal backstories.** Use `[NAME TBD]` placeholders and flag them clearly. Never invent a PC or NPC's real name or personal history.
3. **Commit and push after every change.** Always commit before ending a turn.
4. **Do NOT open new pull requests.** PR #1 is live on `claude/review-optimize-agents-Sr9R6`. Additional commits to that branch update it automatically.
5. **Working branch:** `claude/review-optimize-agents-Sr9R6` — all work goes here unless told otherwise.
6. Today's session (2026-05-21) also pushed Zundaerazylym + Soft Claws to `claude/check-neverwinter-project-EcqrQ` — those files need porting to the main working branch.

---

## Project Structure

- **Repo:** `oddsare/Neverwinter`
- **Vault root:** `NeverWinter/` (Obsidian markdown vault)
- Files use YAML frontmatter and Obsidian wikilinks: `[[path/to/file|display text]]`
- Full file map: `NeverWinter/Design Notes/Campaign Handoff.md` — read this for the complete directory tree

---

## Campaign Canon — Locked Facts

### Setting
- **Game:** D&D 5e Forgotten Realms homebrew
- **Location:** Neverwinter, Sword Coast North, Faerûn
- **Year:** 1525 DR (Dale Reckoning) — campaign start

### Timeline (locked)
- **1451 DR:** Cataclysm (Mount Hotenow) devastates Neverwinter
- **~1495 DR:** Emperor of the Seas begins reign over Luskan
- **1502 DR:** Lord Dagult Neverember vanishes — actually alive, trapped in the Neverneath, age ~96 in 1525 DR. Renaer does not know.
- **1502–1520 DR:** The Dance of the Revolving Lords — six failed rulers, 18 years of chaos
- **1520 DR:** Lord Renaer Neverember ascends as 7th ruler. His first act: signs peace with Luskan.
- **1525 DR:** Campaign present. 5-year anniversary of the Luskan peace.

### Key Locked NPCs

**Lord Renaer Neverember** — current ruler. LOCKED as GOOD. Genuinely reformist. Knows the Neverember Alagondar bloodline claim is fraudulent (canon). Wants to govern well enough that citizens don't care. Fragile — one bad week from losing it all. File: `NPCs/Renaer Neverember.md`

**Lord Dagult Neverember** — Renaer's father. Fraudulent Alagondar claimant. ALIVE and trapped in the Neverneath for 23 years, age ~96, still bitter. Recovering him is a campaign-defining choice. File: `NPCs/Dagult Neverember.md`

**Claugiyliamatar / "Old Gnawbone"** — ancient GREEN dragon (not a hag — corrected error). The campaign's central villain. 1,200+ years old. CR 22 (5e MM) / CR 28 (Wyrms of the North with Rogue 4/Druid 4). Obsessed with becoming a permanent human noblewoman while retaining dragon powers. Already operating a covert network inside Neverwinter under the alias **"Lady Maeve Vance"** (alter self + Ring of Chameleon Power). File: `NPCs/Claugiyliamatar - Old Gnawbone.md`

**Aldric Vance** *(placeholder name — user writes real name)* — Claugiyliamatar's foster son. Suspected true Alagondar heir. Age 28 in 1525 DR. Adopted at ~15 in 1512 DR. Mid-level wizard (5–7), NOT a master. Lives at the Vance estate on Neverwinter's outskirts as live-in steward. Believes Lady Maeve is his loving mother. Claug scries him daily; he doesn't know. Real name and backstory reserved for user. File: `Quests/The Foster King.md`

**Indrina Lamsensettle** — actress/aristocrat who proved Dagult's Alagondar claim fraudulent. Her genealogy dossier is the linchpin artifact. Recommended status: alive, in hiding, terrified. File: `NPCs/Indrina Lamsensettle.md`

**Soren Ironwake** — Guard Captain, Renaer's most trusted lieutenant. File: `NPCs/Soren Ironwake.md`

**Ophala Cheldarstorn** — owner of the Moonstone Mask. Canonically a powerful mage, member of the Many-Starred Cloak (Silverymoon). Keeps Zundaerazylym's hoard hidden. Knows the dragon's true identity. 160+ years old via magical longevity. File: `NPCs/Ophala Cheldarstorn.md`

**Zundaerazylym / "The Laughing Wyrm"** — ancient STEEL dragon (good). Neverwinter's secret guardian. Human alias: **Amundra Nelaerdra**, jolly laundress/seamstress at the Moonstone Mask. Leads the **Soft Claws** mercenary company. Closest ally is Ophala. Laughs during battle. Grand plan: manipulate evil dragons into orc territories; use alchemy to integrate good dragons with humanoid society. Claugiyliamatar is moving into her territory — this is the central dragon-vs-dragon tension. File: `NPCs/Zundaerazylym - The Laughing Wyrm.md`

**Voaraghamanthar / Two Black Brothers** — twin ancient black dragons, Mere of Dead Men. Possible Claug allies; Twin Crowns of Miramaran could shield Aldric from Harper diviners. File: `NPCs/Voaraghamanthar - Two Black Brothers.md`

**Galaeron** — head paladin of the Shard of the Moon. Full details TBD by user.

---

## Faction Map

### The 3-Faction Rule
Every faction splits internally into good / bad / ugly sub-factions. Lore file: `Lore/3 Faction Rule.md`

### Political Map (corrected 2026-05-21)
- **GOOD** = Renaer's Government + Sons of Alagondar (aligned on civic good, divided on tactics)
- **BAD** = Dagult-loyalist remnants + 1502 conspirator successors (suppress the real Alagondar bloodline to keep the Neverember fraud buried)
- **UGLY** = Claug's Network (has the heir, would proxy-rule Neverwinter through Aldric)

### All Faction Files (`NeverWinter/Factions/`)
- `Renaer's Government.md` — GOOD; his reformist administration
- `The Alagondar Bloodline.md` — dormant dynastic claim; Sons of Alagondar sub-faction
- `Claug's Network.md` — Claug's covert infrastructure
- `House [TBD] - Fey Pact.md` — SHELL; noble house with archfey pact (name/archfey/deal TBD)
- `Luskan.md` — Luskan as a political entity
- `The Soft Claws.md` — Zundaerazylym's secret mercenary network (added 2026-05-21)
- `Arcane Brotherhood.md` — Luskan's true power; tutoring Aldric on Claug's payroll
- `City Guard.md` — Renaer's institutional arm
- `The Harpers.md` — good-aligned intelligence network

---

## Active Plot Threads

### The Foster King (central political spine, Tier 1–3)
File: `Quests/The Foster King.md`
Claug has groomed Aldric as a proxy Alagondar heir. Plan: install him on the throne (mid-late campaign), then decades later have him craft her permanent-human ritual out of filial love. Renaer is the obstacle — but a GOOD one.

### The Peace Anniversary (Tier 1 event)
File: `Quests/The Peace Anniversary.md`
5-year anniversary of the 1520 Renaer–Luskan peace. Luskan sends: Emperor's son (name TBD) + Arcane Brotherhood advisor (name TBD). Surface intent: ceremonial renewal. Real intent: political leverage move against Renaer (not assassination). Party arrives and settles first; anniversary lands shortly after.

### The Shadowy Force (surface mystery)
File: `Quests/The Shadowy Force.md`
Disappearances on moonless nights — ships, cargo, caravans. Likely a Sharran cult operation. Shar's dark moon is the tell.

### The Shard of the Moon (location + Sister War thread)
File: `Locations/Neverwinter/Shard of the Moon.md`
Floating tower in the Tower District. Temple of Selûne. Formerly Shar's "Shard of Night" — reclaimed and re-consecrated. The Shar→Selûne flip is the Sister War made physical. Head paladin: Galaeron (details TBD). High Priestess: [NAME TBD]. Trusted visitors gain an extra healing surge on long rests inside.

### Kurtulmak's Chosen (parallel breathing-world thread)
File: `Quests/Kurtulmak's Chosen - Underground Ascension.md`
A divinely touched kobold is building an underground city and working toward transformation (draconic ascension / Kurtulmak avatar / new kind of being). Progresses on its own timeline whether or not players engage. LOCKED 2026-05-15.

### Zundaerazylym vs. Claugiyliamatar (dragon-vs-dragon, Tier 2–3)
Files: `NPCs/Zundaerazylym - The Laughing Wyrm.md`, `NPCs/Claugiyliamatar - Old Gnawbone.md`
An ancient good dragon (Zundaerazylym) has protected Neverwinter for 150+ years. An ancient evil dragon (Claugiyliamatar) is now infiltrating it. Same tier of power. Pulling in opposite directions. The Soft Claws are already running counter-ops against Claug's network.

---

## Outstanding TBD List

These are reserved for future sessions — do not invent without user:

1. **Aldric Vance** — real name + full personal backstory (user writes)
2. **Shard of the Moon** — High Priestess name; Galaeron's race, personality, backstory
3. **The Peace Anniversary** — delegation names (Emperor's son + Brotherhood advisor); the specific political move; which of three Renaer-knowledge readings is canon
4. **House [TBD] - Fey Pact** — family name; archfey patron; deal terms (gained / owed / forgotten / not-yet-called-in clause)
5. **Stone ancient dragon** — user asked about one; no file exists yet. Existing dragons: Claugiyliamatar (green), Voaraghamanthar + Two Black Brothers (black), Zundaerazylym (steel/good), Chartilifax (corrupted green, canon), Lorragauth (ancient black bones under the Dread Ring, canon)
6. **Branch consolidation** — Zundaerazylym + Soft Claws files are on `claude/check-neverwinter-project-EcqrQ`; need porting to `claude/review-optimize-agents-Sr9R6`

---

## Session Log

- **2026-05-15:** Kurtulmak's Chosen locked
- **2026-05-14:** Claugiyliamatar research locked; motive decided
- **2026-05-21 (Session 1):** Foster King plot, Peace Anniversary, Shard of the Moon, 5 faction files, Campaign Handoff doc, session log
- **2026-05-21 (Session 2):** Zundaerazylym + Soft Claws added (wrong branch — needs porting)

---

## Key Canon Corrections (errors to never repeat)

- Claugiyliamatar is a **green dragon**, NOT a hag
- The Moonstone Mask is **not** canonically Selûnite — the Shard of the Moon is the Selûnite stronghold
- Ophala Cheldarstorn is the **original founder** of the Moonstone Mask (~1363 DR), not a descendant — Liset Cheldar filled in temporarily post-1451
- The Neverember Alagondar bloodline claim is **canonically fraudulent** — Indrina proved it

---

## Related Files (read for full context)

- `NeverWinter/Design Notes/Campaign Handoff.md` — full 8-part onboarding doc
- `NeverWinter/Design Notes/2026-05-21 Worldbuilding Session.md` — session log
- `NeverWinter/TIMELINE.md` — full chronological history
- `NeverWinter/Campaign Home.md` — quick links hub
