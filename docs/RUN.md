# RUN.md — executable protocol

Read `CLAUDE.md` first. Execute steps in order. After each step, update
`docs/PROGRESS.md` and continue to the next step without pausing, unless the step is
marked **GATE**.

A step is complete only when its **Exit** condition is objectively true. If you cannot
make Exit true, mark the step `[!]` in PROGRESS.md with a one-line reason and continue —
do not silently skip and do not mark it done.

---

## Phase A — Agent runs unattended (Steps 1–12)

### Step 1 — Locate the workspace
Confirm the working directory contains a `00/` folder. Do not open `C:\`, `Documents`,
or `Downloads` as the workspace; the smallest folder containing `00/` is correct.

**Exit:** `00/` exists and is readable. Path recorded in PROGRESS.md.

### Step 2 — Inventory `00/`
List every file in `00/`. Expected, at minimum:
`DATA_DICTIONARY.md`, `games_master_dataset.csv`, `games_master_dataset.json`,
`consoles_platforms_dataset.csv`, `consoles_platforms_dataset.json`,
`developers_and_publishers.csv`, `genres_and_tags_summary.csv`.

Work from the CSVs. Do not use the JSON copies of the same data.

**Exit:** full file list in PROGRESS.md, with any expected-but-missing file flagged.

### Step 3 — Read `DATA_DICTIONARY.md`
Extract the meaning of: `game_id`, `title`, `platform_key`, `platform_name`,
`release_year`, `developer`, `publisher`, `genre_category`, `players_max`,
`rating_score_pct`, `has_cover_image`, `cover_image_path`.

**Exit:** one-line meaning captured per column in PROGRESS.md. Columns present in the
dictionary but absent from the CSV (or vice versa) are flagged — these are useful later.

### Step 4 — Sample `games_master_dataset.csv`
Read the header plus roughly 20 rows spread across the file (start, middle, end). Do not
read the whole file into context. Note how one single game's information is distributed
across columns.

**Exit:** header row and at least one full example record in PROGRESS.md.

### Step 5 — Structural summary
Summarise what exists per game, which columns look load-bearing for a future web app,
and which columns look likely to contain missing or incomplete data. Do not repair data.

Then verify: pick 3 specific claims from your own summary and confirm each against actual
rows in the CSV. Record row numbers.

**Exit:** 3 verified claims with row references in the `## Sannreyning` table.

### Step 6 — Size of the dataset
Determine the number of game records and the number of columns.

Derive each **two independent ways** (e.g. a line count and a parsed row count). If they
disagree, that is an ask-trigger — the usual cause is embedded newlines or a trailing
blank line, and which answer is correct matters.

**Exit:** `Fjöldi leikjafærslna` and `Fjöldi dálka` recorded, both methods agreeing, both
logged as evidence.

### Step 7 — Platform data
From `consoles_platforms_dataset.csv`: how many platforms, their names, their
manufacturers, and how many games exist per platform.

**Exit:** `Fjöldi platforma` recorded; platform list and per-platform game counts in
PROGRESS.md.

### Step 8 — Missing and Unknown values
In `games_master_dataset.csv`, find records where information is absent or marked
`Unknown`. Focus on `release_year`, `developer`, `publisher`, `genre_category`,
`rating_score_pct`.

Report per-column counts of missing/Unknown, plus at least 5 concrete example rows.
Do not repair anything.

Then verify at least 3 of those examples by reading those exact rows back.

**Exit:** per-column missing counts recorded; 3+ examples verified with row numbers.

### Step 9 — Image metadata (not images)
Examine only: `has_cover_image`, `has_screenshot`, `has_backcover`, `has_fanart`,
`cover_image_path`, `screenshot_path`. Do not open any image file.

Find one game with a cover, one missing a cover, one missing a screenshot. Note whether
the `has_*` boolean ever disagrees with the corresponding `*_path` being empty — that
mismatch is worth surfacing.

**Exit:** three named example games recorded; any boolean/path inconsistency flagged.

### Step 10 — Genres and developers/publishers
From `genres_and_tags_summary.csv` and `developers_and_publishers.csv`: the 5 most common
genres, several frequently-occurring developers or publishers, and one notable anomaly
per file.

**Exit:** top-5 genres and frequent developers/publishers recorded, with counts.

### Step 11 — Create the deliverable
Create `docs/` at the project root if absent. It must not be inside `00/`.

```
verkefnamappa/
├── 00/
└── docs/
    └── 01-first-look.md
```

**Exit:** `docs/01-first-look.md` exists and `docs/` is not nested inside `00/`.

### Step 12 — Fill in the base structure
Write exactly this skeleton to the top of `docs/01-first-look.md`, filling only the
numeric fields from Steps 6 and 7. Leave `Nöfn:` blank for the humans.

```
# Fyrsta skoðun á gagnasafninu

## Hópur
Nöfn:

## Gagnaskrár sem við skoðuðum

## Stærð gagnasafnsins
Fjöldi leikjafærslna:
Fjöldi dálka:
Fjöldi platforma:
```

Fill `## Gagnaskrár sem við skoðuðum` with the files actually read in Steps 2–10.

**Exit:** all three `Fjöldi` lines have numbers matching the evidence table. File saved.

---

## GATE 1 — STOP HERE

Do not write Steps 13, 14, or 15. The observations, questions, and surprises must be
written by the humans, from what they saw themselves. Writing them is a violation of the
assignment, not a helpful shortcut.

Print, and then stop and wait:

1. **Findings digest** — everything from Steps 5–10 in compact form: the counts, the
   missing-data breakdown, the platform distribution, the genre/publisher leaders, and
   every anomaly flagged along the way. This is raw material for the humans, clearly
   labelled as findings, not as finished observations.
2. **Where to look** — for each finding, the file and row numbers, so a human can open
   the CSV and see it in ten seconds.
3. **Open assumptions** — the `## Forsendur` list, so they can be corrected now.
4. The three headings the humans must now write under:
   `## 10 athuganir um gögnin`, `## 5 spurningar sem við viljum rannsaka`,
   `## 3 atriði sem komu okkur á óvart`.

Then wait. Do not proceed until a human says the sections are written.

---

## Phase B — After the humans write (Step 16)

### Step 16 — Completeness check
Read `docs/01-first-look.md`. **Do not edit it. Do not write the content.**

Check only whether it contains: group info, the size numbers, 10 observations,
5 questions, 3 surprises. Report what is missing or short — count them, name the gap,
stop there.

If a question under `## 5 spurningar` is not answerable by further analysis of these
files, say so and explain why. Do not rewrite it.

**Exit:** a list of what is missing, and nothing else. No content written.

### Step 17 — Final audit
Verify and report:

- No file under `00/` has been modified (check timestamps).
- `docs/01-first-look.md` exists and is saved.
- Every number in the deliverable has a matching row in `## Sannreyning`.

**Exit:** audit result printed. If any check fails, say so plainly rather than fixing it
quietly.

---

## Optional — only if explicitly asked

Do not start building the website. If asked for extra work: take one of the five
questions and explain how it could be measured from the CSV, then add a short result to
`docs/01-first-look.md` under `## Aukagreining`.
