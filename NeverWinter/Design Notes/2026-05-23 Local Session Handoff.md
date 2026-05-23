---
type: design-note
world: Neverwinter
campaign: Neverwinter
date: 2026-05-23
description: "Handoff doc for a fresh LOCAL Claude Code session - explains state after cloud session installed obsidian-mind"
tags: [design-note, handoff, session-transition, local]
---

# Handoff to Local Claude — 2026-05-23

> **READ THIS FIRST.** You are a fresh Claude Code session running **locally on the user's Windows machine** (NOT a cloud container). The previous session was a CLOUD session that just installed obsidian-mind infrastructure into this repo. Your job is to run Phase 2 of the install and then continue working on the campaign.

---

## PART 1 — Your situation

- You are running **Claude Code CLI on Windows**, started by the user from their repo folder
- The user's name is **Gilbert** (their email/PowerShell prompt: `gilbertbrodie@gmail.com`, `C:\Users\Gilbe`)
- The repo is `oddsare/Neverwinter`, working branch `claude/review-optimize-agents-Sr9R6`
- A pull request (**PR #1**) is already open for this branch — pushing commits updates it, do NOT open new PRs
- Obsidian app is installed locally (v1.12.7), Node v24, Git 2.54 — all green for Phase 2
- The user has Obsidian **open** on the repo root folder as the active vault

---

## PART 2 — What the previous (cloud) session just did

**Phase 1 of obsidian-mind install — complete and pushed.** The repo root now contains:

- Mind infrastructure: `.claude/` (agents, commands, skills, hooks), `.codex/`, `.gemini/`, `.shardmind/`, `.mcp.json`
- Mind vault folders: `brain/`, `work/`, `org/`, `perf/`, `bases/`, `templates/`, `reference/`, `thinking/`, `scripts/`
- Mind root files: `Home.md`, `AGENTS.md`, `GEMINI.md`, `vault-manifest.json`, etc.
- The user's `CLAUDE.md` at root is **preserved as-is** (it has the campaign rules). Mind's CLAUDE.md content was renamed to `reference/Mind-Agent-Instructions.md` so it didn't overwrite the user's.
- The user's `.gitignore` was extended with mind's entries.

**Campaign content is UNTOUCHED.** It still lives in `NeverWinter/` (subfolder of the repo root). It has NOT yet been migrated into mind's `work/`/`brain/` structure.

---

## PART 3 — Your immediate job (Phase 2)

The whole reason you (local Claude) exist is to **run mind's `/om-vault-upgrade` slash command** to migrate the `NeverWinter/` campaign content into mind's folder structure.

### Steps

1. **Confirm you can see mind's commands.** Type `/help` or look at `.claude/commands/` — you should see `om-vault-upgrade` and 17 other `om-*` commands.
2. **Confirm Obsidian is open** with the repo root as the active vault (ask the user to confirm).
3. **Run `/om-vault-upgrade`** and follow the interactive prompts. It will:
   - Read every note in `NeverWinter/`
   - Semantically classify each (campaign canon → `brain/`? quest content → `work/`? NPCs → `org/`? etc.)
   - Move/transform notes into the right mind folders
   - Update frontmatter to match mind's schemas
4. **Review with the user** before committing. The migration touches ~150 files; surface what's about to change before pushing.
5. **Commit + push.** Standard commit message; do NOT open a new PR — PR #1 will update automatically.

### If `/om-vault-upgrade` doesn't appear

The command file lives at `.claude/commands/om-vault-upgrade.md`. If Claude Code doesn't auto-load it, the user may need to restart Claude in the repo folder. Verify Claude Code is reading the repo's `.claude/` settings.

---

## PART 4 — User's locked rules (from `CLAUDE.md`)

The user is **strict** about these. Memorize them before doing anything:

1. **Ask before each edit.** Propose, get approval, then act.
2. **The user writes all character names and personal backstories.** Use `[NAME TBD]` placeholders. Never invent.
3. **Do NOT connect plot threads, NPCs, locations, or items together unless the user explicitly says to.** Record what is given. Do not add implications, hooks, or cross-references unless instructed.
4. **Research when asked. Record what the user gives. Nothing more.**
5. **If a file already exists for a subject, add to that existing file.** Do not create a duplicate.
6. **Never connect an existing file to another file or thread unless the user says to.**
7. **Commit and push after every change.** A stop-hook enforces this.
8. **Do NOT open new pull requests.** PR #1 is live.
9. **Working branch:** `claude/review-optimize-agents-Sr9R6`.

The full canon (timeline, NPCs, factions, plot threads, faction map, outstanding TBDs) is in `CLAUDE.md` at the repo root — it auto-loads at the start of every session.

---

## PART 5 — Context you (local Claude) need that may not be obvious

### "Stone dragon" question — answered
The user asked earlier if there were notes on a "stone ancient dragon." The previous (cloud) session searched and reported "none found" — but **that was wrong**. The answer is **Zundaerazylym / "The Laughing Wyrm"** — an ancient **STEEL** dragon (good alignment), human alias **Amundra Nelaerdra** (laundress at the Moonstone Mask), leader of the **Soft Claws** faction. The cloud session misheard "steel" as "stone."

### Branch consolidation pending
The Zundaerazylym NPC file and the **Soft Claws** faction file currently live on a **different branch**: `claude/check-neverwinter-project-EcqrQ`. They need to be ported over to `claude/review-optimize-agents-Sr9R6` (this branch). The user has flagged this as TBD. Do NOT do this without asking first — it may require merge resolution.

### Cross-link violations from earlier in this session — possibly need walk-back
Before the previous (cloud) Claude read the user's `CLAUDE.md` rules about not connecting threads, it violated those rules repeatedly in this session. Files affected:
- `NeverWinter/Quests/The Foster King.md` (3-faction map update with implications)
- `NeverWinter/Quests/The Peace Anniversary.md` (cross-plot hooks throughout)
- `NeverWinter/Locations/Neverwinter/Shard of the Moon.md` (Sister War / Sharran reclamation hooks)
- `NeverWinter/Factions/*.md` (Quest Hooks + Enemies sections that connect threads)
- `NeverWinter/Design Notes/Campaign Handoff.md` ("Plot Architecture" section)

The user was asked whether to walk these back; they **dismissed the question without answering**. Treat this as **pending user decision** — surface it at an appropriate moment, do NOT walk back unilaterally.

### Other open TBDs
From `CLAUDE.md`:
- Aldric Vance — real name + backstory (user writes)
- Shard of the Moon — High Priestess name; Galaeron's full details
- Peace Anniversary — delegation names; political move details; Renaer's knowledge level
- House [TBD] - Fey Pact — family name, archfey, deal terms
- White Resin Dealer — name and backstory (user writes)
- Vorgansharax NPC file — to be created when user is ready

---

## PART 6 — File map after Phase 1

```
<repo-root>/
  CLAUDE.md                          <- user's campaign rules + memory (auto-loaded)
  Home.md                            <- mind's vault entry (will be the dashboard once Phase 2 runs)
  AGENTS.md, GEMINI.md               <- mind's agent docs
  README.md                          <- mind's repo README
  vault-manifest.json                <- mind's template metadata
  .gitignore                         <- extended with mind's entries
  .claude/                           <- mind's agents, commands, skills, hooks
    commands/om-vault-upgrade.md     <- THE COMMAND YOU NEED TO RUN
    agents/, skills/, scripts/
  .codex/, .gemini/, .shardmind/     <- mind agent configs
  bases/, brain/, org/, perf/        <- mind vault folders (mostly empty, will fill via Phase 2)
  templates/, work/, reference/, thinking/, scripts/
  reference/Mind-Agent-Instructions.md  <- mind's CLAUDE.md content (preserved here)
  brain/Phase 2 Pending - Neverwinter Migration.md  <- phase-2 tracker
  brain/Gotchas.md, Key Decisions.md, North Star.md, etc.  <- mind's brain stubs
  NeverWinter/                       <- THE CAMPAIGN (awaiting Phase 2 migration)
    Quests/ (Foster King, Peace Anniversary, Shadowy Force, Kurtulmak's Chosen)
    NPCs/ (Renaer, Dagult, Claug, Indrina, Soren, Ophala, etc.)
    Factions/ (Renaer's Government, Alagondar Bloodline, Claug's Network,
               Luskan, House [TBD] - Fey Pact, Arcane Brotherhood, City Guard, Harpers)
    Locations/Neverwinter/ (Districts, Moonstone Mask, Shard of the Moon, Neverneath, etc.)
    Lore/ (Alagondar Dynasty, Lost Crown, 3 Faction Rule, Current Era, etc.)
    Design Notes/ (Campaign Handoff, 2026-05-21 Session, THIS FILE)
    Items/, Maps/, _Templates/
```

---

## PART 7 — Quick start checklist for you (local Claude)

1. ☐ Confirm you're running locally on Windows (not cloud container)
2. ☐ Confirm Obsidian is open with the repo root as the active vault
3. ☐ Read `CLAUDE.md` at root — internalize the rules
4. ☐ Read this handoff (you're already here)
5. ☐ Read `brain/Phase 2 Pending - Neverwinter Migration.md`
6. ☐ Verify `.claude/commands/om-vault-upgrade.md` exists
7. ☐ Greet the user, summarize what you know, propose running `/om-vault-upgrade`
8. ☐ Get user approval before running
9. ☐ Run `/om-vault-upgrade`, walk through prompts WITH the user
10. ☐ Review changes before committing
11. ☐ Commit + push

---

## Related

- [[CLAUDE]] — campaign rules + memory (root)
- [[Design Notes/Campaign Handoff]] — full campaign onboarding doc (NeverWinter/Design Notes/)
- [[Design Notes/2026-05-21 Worldbuilding Session]] — prior session log
- [[brain/Phase 2 Pending - Neverwinter Migration]] — phase-2 tracker
- [[reference/Mind-Agent-Instructions]] — mind's bundled CLAUDE.md content
