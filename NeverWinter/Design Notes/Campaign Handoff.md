---
type: design-note
world: Neverwinter
campaign: Neverwinter
date: 2026-05-21
description: "Full handoff document for a fresh Claude session - working agreement, campaign canon, plot threads, file map, and TBD list"
tags: [design-note, handoff, onboarding, worldbuilding]
---

# Campaign Handoff — Detailed Notes for a New Session

> **READ THIS FIRST.** This file is the onboarding document for a fresh Claude session with no memory of prior conversations. It captures the working agreement, the campaign's locked canon, the active plot threads, the file map, and everything still to be decided.

---

## PART 1 — Working Agreement (how to behave)

### The project
- This is a **D&D 5e Forgotten Realms homebrew campaign** set in and around **Neverwinter**, in the year **1525 DR**.
- The notes are an **Obsidian markdown vault**. Root of the git repo is `/home/user/Neverwinter`; the vault content lives under `NeverWinter/`.
- Files use **YAML frontmatter** and **Obsidian wikilinks**: `[[path/to/file|display text]]`.

### User preferences — IMPORTANT
1. **ASK before making each change.** Do not make unilateral edits. Propose what you intend to change and get approval first. The user wants control over every edit.
2. **The user writes character names and backstories themselves.** Do NOT invent a character's real name or personal history. Use clear placeholders like `[NAME TBD]` and flag them prominently.
3. **Commit and push after changes.** A stop-hook checks for uncommitted changes — always commit and push before ending a turn.
4. **Do NOT create pull requests.** PR #1 already exists (see below).

### Git workflow
- **Repo:** `oddsare/Neverwinter`
- **Branch:** `claude/review-optimize-agents-Sr9R6` — develop and push here, always.
- **PR #1** already exists for this branch: https://github.com/oddsare/Neverwinter/pull/1
- Pushing more commits to the branch **updates PR #1 automatically** — do not open new PRs.
- Push with `git push -u origin claude/review-optimize-agents-Sr9R6`.

---

## PART 2 — Campaign Canon (locked facts — do not contradict)

### The timeline
- **1502 DR:** Lord Dagult Neverember vanishes. (He is secretly **alive and trapped in the [[Locations/Neverwinter/The Neverneath|Neverneath]]** — Renaer does not know.)
- **1502–1520 DR:** The **Dance of the Revolving Lords** — six failed rulers in ~18 years.
- **1520 DR:** **Lord Renaer Neverember** ascends as the **7th** ruler. His **first act of reign** is to sign **peace with Luskan**.
- **1525 DR:** Campaign present. The 5-year anniversary of the Luskan peace.

### The key figures
- **Lord Renaer Neverember** — current ruler of Neverwinter. **LOCKED as GOOD** — genuinely trying to fix his father's mess. Knows the Neverember Alagondar claim is fraudulent. Doesn't want to die for the throne but reopening the question threatens his hard-won stability. (`NPCs/Renaer Neverember.md`)
- **Lord Dagult Neverember** — Renaer's father. Fraudulent Alagondar claimant (canon). A clinging villain. Alive and trapped in the Neverneath for 23 years; 96 years old; still bitter. Recovering him is a campaign-defining choice. (`NPCs/Dagult Neverember.md`)
- **Claugiyliamatar / "Old Gnawbone"** — **green dragon** (NOT a hag — this was a corrected error). The campaign's central villain. Runs a covert network. Has spent 200+ years planning. (`NPCs/Claugiyliamatar - Old Gnawbone.md`)
- **Aldric Vance** *(placeholder name)* — Claug's foster son, a suspected true Alagondar heir. See Part 3.
- **Indrina Lamsensettle** — actress/aristocrat who proved Dagult's claim fraudulent; her genealogy dossier is the linchpin artifact. Recommended status: alive, in hiding, terrified. (`NPCs/Indrina Lamsensettle.md`)
- **Soren Ironwake** — Guard Captain, Renaer's most trusted lieutenant. (`NPCs/Soren Ironwake.md`)
- **Ophala Cheldarstorn** — enigmatic owner of the Moonstone Mask, information broker. (`NPCs/Ophala Cheldarstorn.md`)

### Setting facts
- **The Alagondar Dynasty** — the true former royal line of Neverwinter. Bloodline-locked vaults + the **Lost Crown of Neverwinter** respond only to true Alagondar blood. The Crown kills false claimants.
- **Luskan** — pirate City of Sails, north of Neverwinter. Ruled publicly by the **Emperor of the Seas** + 5 High Captains; **true power = the Arcane Brotherhood** (a wizard cabal).
- **Selûne vs Shar** — twin-sister goddesses, the oldest cosmic conflict ("the Sister War"). It expresses through their clergies in Neverwinter, not the goddesses directly.
- **The 3-Faction Rule** — a campaign design principle: every faction splits internally into good / bad / ugly sub-factions. Applied recursively. (`Lore/3 Faction Rule.md`)

---

## PART 3 — The Foster King (central political plot)

File: `Quests/The Foster King.md`. The campaign's tier 1-3 political spine.

**Premise:** Claugiyliamatar suspects she has found a true Alagondar heir — a magically gifted orphan she adopted and raised. She poses as **"Lady Maeve Vance,"** a widowed noble (via Ring of Chameleon Power + *alter self*). The heir believes she is his loving mother. She genuinely loves him — and would still sacrifice him if the plan required it.

### Aldric Vance — LOCKED details (decided 2026-05-21)
- **Age 28** in 1525 DR.
- **Adopted at ~15** in 1512 DR (adopted as a teen, not a young child).
- **Mid-level wizard (5-7), NOT a master mage.**
- Lives at the **Vance estate on the outskirts of Neverwinter** as live-in steward.
- Actively **building a reputation in Neverwinter society** — salons, arcane circles, patrons.
- **Lady Maeve does NOT live at the estate.** She "travels between properties"; really she works from Claug's dragon lair.
- **Crystal-ball surveillance:** Claug scries Aldric daily. He doesn't know. Party can notice her "too-good timing" as a tier-2 reveal.
- **"Still at home at 28" tell:** framed as steward-of-estate, not stunted. The off-note is HOW he speaks of mother (unguarded childlike warmth).
- **Ritual timeline:** throne FIRST (mid-late campaign — bloodline qualifies him, not magic); Claug's permanent-human ritual LATER (post-campaign / late tier 3, after ~10-20 more years of magic growth).
- Tier ages: Tier 2 ~30-32, Tier 3 ~35-40.

### RESERVED FOR USER — do not invent
- **Aldric's real name** (placeholder "Aldric Vance" only).
- **Aldric's backstory** — how he was orphaned, memories of real parents, Claug's cover story for finding him at 15.

### The 3-Faction Map (corrected 2026-05-21)
- **GOOD** = Renaer's Government + the Sons of Alagondar (aligned on civic good, divided on tactics).
- **BAD** = Dagult-loyalist remnants + 1502 conspirator successors (suppress any real Alagondar to keep the Neverember lie alive, because a real heir proves they were all accomplices to fraud).
- **UGLY** = Claug's Network (has the heir; would proxy-rule the city).

---

## PART 4 — Active Plot Threads

### The Peace Anniversary
File: `Quests/The Peace Anniversary.md`. **Early tier-1 event.**
- 5-year anniversary of the 1520 Renaer–Luskan peace, landing 1525 DR.
- Luskan sends a delegation: the **Emperor of the Seas' son** *(name TBD)* + an **Arcane Brotherhood advisor** *(name TBD)*.
- Surface intent: ceremonial peace renewal. **Real intent: a political move against Renaer** — incite a riot / seize a public moment / walk away with lasting leverage. NOT assassination or magical sabotage.
- Bitter irony: Luskan very likely killed Dagult (the 1502 plot), yet Renaer made peace with them. Three readings of whether Renaer knows — left ambiguous.
- Pacing: party arrives and settles in Neverwinter first, then the anniversary lands.

### The Shadowy Force
File: `Quests/The Shadowy Force.md`. The campaign's surface mystery — disappearances on moonless nights (Shar's dark moon). Likely a Sharran cult operation.

### The Shard of the Moon
File: `Locations/Neverwinter/Shard of the Moon.md`. **New location (created 2026-05-21).**
- A **floating tower** in the **Tower District**, temple of **Selûne**.
- Formerly the Sharran **"Shard of Night"** — reclaimed and re-consecrated. The Shar→Selûne flip is the Sister War made physical inside the city.
- Gated entry (trusted visitors only). Mechanical perk: trusted visitors gain an extra healing surge on a long rest taken inside.
- **Leader:** High Priestess of Selûne — *[NAME TBD]*.
- **Head paladin:** **Galaeron** *(spelling may change)*.
- Note: the Moonstone Mask is NOT canonically Selûnite; the Shard is the real Selûnite stronghold.

### Other threads referenced
- **Kurtulmak's Chosen** — an underground kobold-ascension plot (`Quests/Kurtulmak's Chosen...`).
- **Voaraghamanthar / Two Black Brothers** — black dragons, possible Claug allies (Twin Crowns of Miramaran could shield Aldric from diviners).

---

## PART 5 — Faction Files (created 2026-05-21)

All in `Factions/`:
- **`Luskan.md`** — Luskan as a political entity (Emperor, 5 High Captains, Brotherhood true power).
- **`Renaer's Government.md`** — **GOOD** faction; Renaer's reformist administration.
- **`Claug's Network.md`** — Claug's covert infrastructure (Aldric, Lady Maeve persona, hidden lieutenants, contracted Sharran ritualists + Brotherhood mages).
- **`The Alagondar Bloodline.md`** — the bloodline as a dormant restoration faction; Sons of Alagondar as a sub-faction.
- **`House [TBD] - Fey Pact.md`** — **SHELL** for a new Neverwinter noble house with an archfey pact (see Part 6).
- Pre-existing: `Arcane Brotherhood.md`, `City Guard.md`, `The Harpers.md`.

---

## PART 6 — Outstanding TBD List (for future sessions)

1. **Aldric Vance:** real name + full backstory (user writes these).
2. **Shard of the Moon:** High Priestess name; Galaeron's full details (race, personality, backstory).
3. **The Peace Anniversary:** delegation names (the son + the Brotherhood advisor); the specific political move; which of the three Renaer-knowledge readings is canon.
4. **House [TBD] - Fey Pact:** family name; which archfey patron (Queen of Air and Darkness / Titania / Oberon / Hyrsam / Prince of Frost / other); the deal terms (what gained, what owed, what forgotten, the not-yet-called-in clause).
5. **Possible new NPC:** a "stone ancient dragon" was asked about — no notes exist yet. The user may want to create one. Existing dragons: Claugiyliamatar (green), Voaraghamanthar + Two Black Brothers (black), Chartilifax (corrupted green, canon), Lorragauth (ancient black dragon bones under the Dread Ring, canon).

---

## PART 7 — File Map (quick reference)

```
NeverWinter/
  Campaign Home.md, Campaign Hook.md, TIMELINE.md
  Design Notes/
    Campaign Handoff.md            <- THIS FILE
    2026-05-21 Worldbuilding Session.md
  Quests/
    The Foster King.md             <- central political plot
    The Peace Anniversary.md       <- Luskan delegation, tier-1 event
    The Shadowy Force.md           <- surface mystery (disappearances)
  Factions/
    Luskan.md, Renaer's Government.md, Claug's Network.md,
    The Alagondar Bloodline.md, House [TBD] - Fey Pact.md
    Arcane Brotherhood.md, City Guard.md, The Harpers.md
  NPCs/
    Claugiyliamatar - Old Gnawbone.md, Renaer Neverember.md,
    Dagult Neverember.md, Indrina Lamsensettle.md,
    Soren Ironwake.md, Ophala Cheldarstorn.md,
    Voaraghamanthar - Two Black Brothers.md,
    Selune and Shar - Sister Goddesses.md
  Locations/
    Luskan.md
    Neverwinter/
      Neverwinter.md, The Moonstone Mask.md, The Neverneath.md,
      Shard of the Moon.md         <- new Selunite tower
      Districts/ (Tower, Protectors Enclave, Bluelake, Docks,
                  Chasm, Neverdeath)
  Lore/
    The Alagondar Dynasty.md, The Lost Crown and Hidden Alagondar Vaults.md,
    3 Faction Rule.md, Current Era.md, and more
```

---

## PART 8 — Plot Architecture (how the threads connect)

- **Claug's grand plan:** put Aldric (a true Alagondar) on Neverwinter's throne, then have him perform her permanent-human ritual decades later. She rules the city by proxy.
- **Renaer is the obstacle** — but he is GOOD, which makes the conflict tragic rather than simple. He might even step aside for a true heir, if it served the city.
- **Luskan / Arcane Brotherhood** want Neverwinter weak and divided. The Peace Anniversary delegation is their tier-1 move. The Brotherhood also secretly tutors Aldric on Claug's payroll — so the delegation may double as a check-in on her asset.
- **Shar's cult** (the Shadowy Force) is the surface mystery; the Sister War is the cosmic backdrop. The Shard of the Moon is a physical Shar-vs-Selûne battleground in the city.
- **The Fey-Pact house** is a planned parallel to the Foster King: another noble family with a hidden supernatural patron (fey instead of dragon).
- **Dagult, alive in the Neverneath,** is a buried bomb — he knows the 1502 Luskan conspirators, where Indrina's dossier is, and the identities of rival Alagondar heirs.

---

## Related

- [[Design Notes/2026-05-21 Worldbuilding Session]]
- [[Quests/The Foster King]]
- [[Quests/The Peace Anniversary]]
- [[Quests/The Shadowy Force]]
- [[Locations/Neverwinter/Shard of the Moon]]
- [[Factions/Luskan]]
- [[Factions/Renaer's Government]]
- [[Factions/Claug's Network]]
- [[Factions/The Alagondar Bloodline]]
- [[Factions/House [TBD] - Fey Pact]]
