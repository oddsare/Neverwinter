---
description: "Phase 2 migration pending - campaign content lives in NeverWinter/ awaiting /om-vault-upgrade"
tags:
  - brain
  - migration
  - pending
---

# Phase 2 Pending — Neverwinter Campaign Migration

## Status

⏳ **PHASE 2 NOT YET RUN.** Phase 1 (obsidian-mind infrastructure install) is complete in this repo. The Neverwinter D&D campaign content **still lives in its original location** at `NeverWinter/` (a subfolder of this repo root) and has NOT yet been migrated into mind's structure (`work/`, `brain/`, etc.).

## What's where

- **Mind infrastructure (Phase 1):** at the repo root — `.claude/`, `.codex/`, `.gemini/`, `bases/`, `brain/`, `org/`, `perf/`, `templates/`, `work/`, `CLAUDE.md`, `Home.md`, etc.
- **Campaign content (awaiting migration):** in `NeverWinter/` — `Quests/`, `NPCs/`, `Factions/`, `Locations/`, `Lore/`, `Items/`, `Design Notes/`, etc.

## Why this file exists

So that any AI agent (Claude, Codex, Gemini) starting work in this vault knows:

1. The campaign content in `NeverWinter/` is **real, in-use, and important** — do not delete or ignore it
2. The user explicitly chose to migrate it INTO mind's structure (Path B)
3. The migration is done via mind's own `/om-vault-upgrade` slash command — **only runnable from a Claude session attached to this vault locally (Obsidian + Claude Code + this vault opened as the active vault)**
4. Until Phase 2 runs, treat `NeverWinter/` as the canonical source of truth for the campaign

## Phase 2 — what the user needs to do

1. Pull this repo to their local machine
2. Open the repo **root** (not `NeverWinter/`) as an Obsidian vault
3. Enable Obsidian CLI in Obsidian Settings (requires Obsidian 1.12+)
4. Start Claude Code in the repo root
5. Run `/om-vault-upgrade` and follow the prompts to migrate `NeverWinter/` content into mind's folders

## Pointers to existing campaign docs

- [[NeverWinter/Design Notes/Campaign Handoff]] — comprehensive onboarding doc for the campaign
- [[NeverWinter/Design Notes/2026-05-21 Worldbuilding Session]] — most recent design session log
- [[NeverWinter/Campaign Home]] — campaign vault entry point

## Working agreement (user's locked preferences)

- **ASK before each edit.** Do not make unilateral changes.
- **The user writes character names and backstories.** Use placeholders, never invent.
- **Commit and push after changes.** A stop-hook enforces this.
- **Branch:** `claude/review-optimize-agents-Sr9R6` — PR #1 exists; new commits update it.
- **Do NOT create new PRs.**
