---
type: design-note
world: Neverwinter
campaign: Neverwinter
date: 2026-05-21
description: "Worldbuilding session log - Aldric locks, Shard of the Moon, Peace Anniversary, faction restructure"
tags: [design-note, session-log, worldbuilding]
---

# Design Session — 2026-05-21

> Record of worldbuilding decisions and file changes made this session. All work pushed to branch `claude/review-optimize-agents-Sr9R6` (PR #1).

## 1. Aldric Vance — Locked Details

Updated `Quests/The Foster King.md`:

- **Age 28** in 1525 DR (was placeholder ~18).
- **Adopted at ~15** in 1512 DR by Claug (as "Lady Maeve"). Adopted as a teen, not a small child.
- **Mid-level wizard (5-7), NOT a master mage.** Master tier is years away.
- Lives at the **Vance estate on the outskirts of Neverwinter** as live-in steward.
- Actively **building a reputation in Neverwinter society** — salons, arcane circles, courting patrons.
- **Lady Maeve does NOT live at the estate.** She "travels between her properties." Really she works from Claug's dragon lair.
- **Crystal-ball surveillance:** Claug scries Aldric daily. He doesn't know — to him, mother just has uncanny instincts. The party can notice her "too good" timing as a tier-2 reveal beat.
- **"Still at home at 28" character tell:** framed as steward-of-estate, not stunted. The off-note is HOW he talks about mother (unguarded childlike warmth), not where he lives.
- **Ritual timeline:** throne FIRST (mid-late campaign — bloodline qualifies him, not magic), permanent-human ritual LATER (post-campaign / late tier 3, after ~10-20 more years of magic growth).
- Tier ages updated: Tier 2 ~30-32, Tier 3 ~35-40.

### Reserved for user (do NOT invent)
- **Aldric's real name** — placeholder "Aldric Vance" only.
- **Aldric's backstory** — how he was orphaned, his memories of real parents, Claug's cover story for finding him at 15.

## 2. Claug Correction

- Claug (Claugiyliamatar / "Old Gnawbone") is a **green dragon**, not a hag. Fixed an erroneous "hag lair" → "dragon lair" in the Foster King file.

## 3. The Shard of the Moon — New Location

Created `Locations/Neverwinter/Shard of the Moon.md`:

- **Floating tower** in the **Tower District** of Neverwinter.
- Temple of **Selûne**. Formerly the Sharran **"Shard of Night"** — reclaimed and re-consecrated. The Shar→Selûne flip is the Sister War in microcosm, inside the city walls.
- **Gated entry:** priests admit trusted visitors only.
- **Mechanical benefit:** trusted visitors who take a long rest gain an extra healing surge until their next long rest.
- **Leader:** High Priestess of Selûne — *[NAME TBD by user]* (separate person from the paladin).
- **Head paladin:** **Galaeron** *(spelling TBD)*.
- Cross-linked from Tower District file and the Selûne/Shar god-file.
- Clarified: the **Moonstone Mask is NOT canonically Selûnite**; the Shard is the real Selûnite stronghold.

## 4. The Peace Anniversary — New Quest

Created `Quests/The Peace Anniversary.md`:

- **5-year anniversary** of the 1520 DR Renaer–Luskan peace, landing **1525 DR** (campaign opening year).
- **Timeline locked:** Dagult vanished ~1502 DR → Dance of the Revolving Lords (1502-1520) → Renaer ascended 1520 DR. The Luskan peace was Renaer's **first act of reign** — "fix what his dad started."
- Bitter irony: Luskan very likely killed Dagult (the 1502 plot), and Renaer made peace with them anyway.
- **Luskan delegation:** the Emperor of the Seas' **son** *(name TBD)* + an **Arcane Brotherhood advisor** *(name TBD)*.
- **Surface intent:** ceremonial peace renewal. **Real intent:** a **political move** against Renaer — incite a riot / seize a public moment / walk away with lasting leverage. NOT assassination, NOT magical sabotage.
- **Timing:** early-tier event — party arrives and settles in Neverwinter first, then the anniversary lands as the first major plot beat.
- Three readings of whether Renaer knows Luskan killed his father — left ambiguous, to be collapsed by party investigation.

## 5. Faction Restructure — Five New Faction Files

- `Factions/Luskan.md` — Luskan as a political entity (Emperor, 5 High Captains, Brotherhood true power).
- `Factions/Renaer's Government.md` — **GOOD** faction; Renaer's reformist administration (Renaer + Soren + City Guard + reformist nobles + civil servants).
- `Factions/Claug's Network.md` — Claug's covert infrastructure (Aldric, Lady Maeve persona, hidden lieutenants, contracted Sharran ritualists + Brotherhood mages).
- `Factions/The Alagondar Bloodline.md` — the bloodline as a dormant restoration faction (Aldric the known candidate, unknown rivals possible, Sons of Alagondar as a sub-faction).
- `Factions/House [TBD] - Fey Pact.md` — **SHELL** for a new Neverwinter noble house with an archfey pact.

### Foster King 3-Faction Map — Fixed
Renaer is now **GOOD** (was wrongly marked Bad in the old map). New alignment:
- **Good** = Renaer's Government + Sons of Alagondar (aligned)
- **Bad** = Dagult-loyalist remnants + 1502 conspirator successors (suppress any real Alagondar to keep the Neverember lie alive)
- **Ugly** = Claug's Network (has the heir, would proxy-rule the city)

### Reserved for user (Fey-Pact house)
- Family **name**, the **archfey** patron (Queen of Air and Darkness / Titania / Oberon / Hyrsam / Prince of Frost / other), and the **deal terms** (what gained, what owed, what forgotten, the not-yet-called-in clause).

## 6. Stone Ancient Dragon — Checked, Does Not Exist

- Searched the vault: **no notes on a "stone ancient dragon."**
- Existing dragons: Claugiyliamatar (green), Voaraghamanthar + Two Black Brothers (black), Chartilifax (corrupted green, canon), Lorragauth (ancient black dragon bones under the Dread Ring, canon).
- Open: user may want to create a new stone/earth ancient dragon NPC later.

## Outstanding TBD List (for future sessions)

- Aldric's real name + full backstory
- Shard of the Moon — High Priestess name; Galaeron's full details (race, personality, backstory)
- Peace Anniversary — delegation names (son + Brotherhood advisor); specific political move; which Renaer-knowledge reading is canon
- Fey-Pact house — name, archfey, deal terms
- Possible new stone/ancient dragon NPC

## Related

- [[Quests/The Foster King]]
- [[Quests/The Peace Anniversary]]
- [[Locations/Neverwinter/Shard of the Moon]]
- [[Factions/Luskan]]
- [[Factions/Renaer's Government]]
- [[Factions/Claug's Network]]
- [[Factions/The Alagondar Bloodline]]
- [[Factions/House [TBD] - Fey Pact]]
