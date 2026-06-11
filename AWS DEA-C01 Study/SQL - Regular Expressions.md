---
title: SQL — Regular Expressions
exam: AWS Certified Data Engineer – Associate (DEA-C01)
section: Data Engineering Fundamentals — SQL Review
type: topic-note
tags:
  - aws
  - dea-c01
  - sql
  - fundamentals
created: 2026-06-09
---

# SQL — Regular Expressions

> [!info] Core idea
> Regex = **pattern matching** — think a much more powerful `LIKE`. Used to match strings against a pattern (Postgres/Redshift style shown below).

## Match operators

| Operator | Meaning |
|---|---|
| `~` | matches the regex (case-**sensitive**) |
| `~*` | matches the regex (case-**insensitive**) |
| `!~` | does **not** match (case-sensitive) |
| `!~*` | does **not** match (case-insensitive) |

## Pattern building blocks ("regex 101")

| Symbol | Meaning | Example |
|---|---|---|
| `^` | start of string | `^boo` → matches `boo`, `book`, `boots` (starts with "boo") |
| `$` | end of string | `boo$` → matches `boo`, `taboo`, not `book` (ends with "boo") |
| `\|` | alternate / OR | `sit\|sat` → matches "sit" and "sat" |
| `[ ]` | range / character set | `[a-z]` → any lowercase letter |
| `{n}` | repeat n times | `[a-z]{4}` → any 4-letter lowercase word |

## Ranges (character sets) — `[ ]`
Match **one** character from the set inside the brackets.

| Pattern | Matches |
|---|---|
| `[a-z]` | any one lowercase letter |
| `[A-Z]` | any one uppercase letter |
| `[0-9]` | any one digit |
| `[a-zA-Z]` | any one letter (either case) |
| `[aeiou]` | any one vowel (list specific chars) |
| `[^0-9]` | any one char that is **NOT** a digit (`^` *inside* `[ ]` = negate) |

> Note: `^` *inside* brackets means "not"; `^` at the *start of the pattern* means "starts with" — two different jobs.

## Alternate characters (OR) — `|`
Matches **either** side. Group with `( )` to scope it.

| Pattern | Matches |
|---|---|
| `sit\|sat` | "sit" or "sat" |
| `^(fire\|ice)` | starts with "fire" or "ice" |
| `gr(a\|e)y` | "gray" or "grey" |

## Repeats (quantifiers) — `{ }`, `+`, `*`, `?`
Say **how many times** the previous item repeats.

| Pattern | Meaning | Example |
|---|---|---|
| `{n}` | exactly n times | `[a-z]{4}` → any 4-letter lowercase word |
| `{n,}` | n or more | `\d{3,}` → 3+ digits |
| `{n,m}` | between n and m | `\d{2,4}` → 2 to 4 digits |
| `+` | one or more | `\d+` → one or more digits |
| `*` | zero or more | `a*` → "", "a", "aaa"… |
| `?` | zero or one (optional) | `colou?r` → "color" or "colour" |

## Special metacharacters (shortcuts)

| Token | Matches |
|---|---|
| `\d` | any digit (same as `[0-9]`) |
| `\w` | any letter, digit, or underscore |
| `\s` | whitespace |
| `\t` | tab |
| `.` | any single character (wildcard) |

## Example
```sql
SELECT * FROM name WHERE name ~* '^(fire|ice)';
```
→ rows where `name` **starts with** "fire" or "ice", **case-insensitive**.

> [!tip] Exam focus
> Remember the four operators (`~`, `~*`, `!~`, `!~*`) and the anchors `^` (start) / `$` (end). Regex = "supercharged LIKE" for pattern matching/validation.

## Related
- [[SQL - Aggregation]] · [[SQL - Grouping]] · [[SQL - Aliases]] · [[Study Q&A Log]]
