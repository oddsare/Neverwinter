# Neverwinter Campaign — Claude Session Memory

This file is read automatically at the start of every Claude Code session. It captures the full working context for the Neverwinter D&D campaign project so no session starts cold.

---

## STANDING INSTRUCTIONS — READ FIRST

1. **Ask before each edit.** Propose changes, get approval, then act.
2. **The user writes all character names and personal backstories.** Use `[NAME TBD]` placeholders. Never invent a name or personal history.
3. **Do NOT connect plot threads, NPCs, locations, or items together unless the user explicitly says to.** Record what is given. Do not add implications, hooks, or cross-references unless instructed.
4. **Research when asked. Record what the user gives. Nothing more.**
5. **If a file already exists for a subject, add to that existing file. Do not create a duplicate.**
6. **Never connect an existing file to another file or thread unless the user says to.**
7. **Commit and push after every change.**
8. **Do NOT open new pull requests.** PR #1 is live on `claude/review-optimize-agents-Sr9R6`.
9. **Working branch:** `claude/review-optimize-agents-Sr9R6` — all work goes here unless told otherwise.

---

## Project Structure

- **Repo:** `oddsare/Neverwinter`
- **Vault root:** `NeverWinter/` (Obsidian markdown vault)
- Files use YAML frontmatter and Obsidian wikilinks: `[[path/to/file|display text]]`
- Full file map: `NeverWinter/Design Notes/Campaign Handoff.md`

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

**Lord Renaer Neverember** — current ruler. LOCKED as GOOD. Genuinely reformist. Knows the Neverember Alagondar bloodline claim is fraudulent (canon). File: `NPCs/Renaer Neverember.md`

**Lord Dagult Neverember** — Renaer's father. Fraudulent Alagondar claimant. ALIVE and trapped in the Neverneath for 23 years, age ~96. File: `NPCs/Dagult Neverember.md`

**Claugiyliamatar / "Old Gnawbone"** — ancient GREEN dragon. Central villain. Runs a covert network. Alias: "Lady Maeve Vance." File: `NPCs/Claugiyliamatar - Old Gnawbone.md`

**Aldric Vance** *(placeholder name — user writes real name)* — Claugiyliamatar's foster son. Suspected true Alagondar heir. Age 28. Adopted at ~15. Mid-level wizard. File: `Quests/The Foster King.md`

**Indrina Lamsensettle** — proved Dagult's Alagondar claim fraudulent. Genealogy dossier is the linchpin artifact. File: `NPCs/Indrina Lamsensettle.md`

**Soren Ironwake** — Guard Captain, Renaer's most trusted lieutenant. File: `NPCs/Soren Ironwake.md`

**Ophala Cheldarstorn** — owner of the Moonstone Mask. Powerful mage, Many-Starred Cloak. Keeps Zundaerazylym's hoard hidden. 160+ years old. File: `NPCs/Ophala Cheldarstorn.md`

**Zundaerazylym / "The Laughing Wyrm"** — ancient STEEL dragon (good). Human alias: Amundra Nelaerdra, laundress at the Moonstone Mask. Leads the Soft Claws. File: `NPCs/Zundaerazylym - The Laughing Wyrm.md`

**Voaraghamanthar / Two Black Brothers** — twin ancient black dragons, Mere of Dead Men. File: `NPCs/Voaraghamanthar - Two Black Brothers.md`

**Galaeron** — head paladin of the Shard of the Moon. Full details TBD by user.

**Vorgansharax** — Claugiyliamatar's adult green dragon son. In the Cult of the Dragon. Hates his mother. File: to be created when user is ready.

---

## Faction Map

- **GOOD** = Renaer's Government + Sons of Alagondar
- **BAD** = Dagult-loyalist remnants + 1502 conspirator successors
- **UGLY** = Claug's Network

### All Faction Files (`NeverWinter/Factions/`)
- `Renaer's Government.md`
- `The Alagondar Bloodline.md`
- `Claug's Network.md`
- `House [TBD] - Fey Pact.md` — SHELL; name/archfey/deal TBD
- `Luskan.md`
- `The Soft Claws.md`
- `Arcane Brotherhood.md`
- `City Guard.md`
- `The Harpers.md`

---

## Active Plot Threads

- **The Foster King** — `Quests/The Foster King.md`
- **The Peace Anniversary** — `Quests/The Peace Anniversary.md`
- **The Shadowy Force** — `Quests/The Shadowy Force.md`
- **The Shard of the Moon** — `Locations/Neverwinter/Shard of the Moon.md`
- **Kurtulmak's Chosen** — `Quests/Kurtulmak's Chosen - Underground Ascension.md`

---

## Outstanding TBD List

1. **Aldric Vance** — real name + backstory (user writes)
2. **Shard of the Moon** — High Priestess name; Galaeron's full details
3. **Peace Anniversary** — delegation names; specific political move; Renaer's knowledge level
4. **House [TBD] - Fey Pact** — family name, archfey, deal terms
5. **White Resin Dealer** — name and backstory (user writes)
6. **Branch consolidation** — Zundaerazylym + Soft Claws files on `claude/check-neverwinter-project-EcqrQ` need porting to `claude/review-optimize-agents-Sr9R6`

---

## Session Log

- **2026-05-15:** Kurtulmak's Chosen locked
- **2026-05-14:** Claugiyliamatar research locked; motive decided
- **2026-05-21 (Session 1):** Foster King, Peace Anniversary, Shard of the Moon, 5 faction files, Campaign Handoff doc
- **2026-05-22 (Session 2):** Zundaerazylym + Soft Claws added; CLAUDE.md created; White Resin item + dealer NPC added; Vorgansharax identified

---

## Canon Corrections (errors to never repeat)

- Claugiyliamatar is a **green dragon**, NOT a hag
- The Moonstone Mask is **not** canonically Selûnite — the Shard of the Moon is the Selûnite stronghold
- Ophala Cheldarstorn is the **original founder** of the Moonstone Mask (~1363 DR), not a descendant
- The Neverember Alagondar bloodline claim is **canonically fraudulent**
