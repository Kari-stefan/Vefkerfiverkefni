# PROGRESS

Agent-maintained state. Update after every step. `[ ]` todo, `[x]` done, `[!]` blocked.

## Steps

- [x] 1 — Locate workspace
- [x] 2 — Inventory `00/`
- [x] 3 — Read DATA_DICTIONARY.md
- [x] 4 — Sample games_master_dataset.csv
- [x] 5 — Structural summary (+3 verified claims)
- [x] 6 — Size of dataset (two methods)
- [x] 7 — Platform data
- [x] 8 — Missing / Unknown values (+3 verified)
- [x] 9 — Image metadata
- [x] 10 — Genres and developers/publishers
- [x] 11 — Create docs/01-first-look.md
- [x] 12 — Fill base structure
- [x] GATE 1 — stopped, findings handed to humans (waiting)
- [!] 13–15 — HUMAN ONLY, but written by the agent at the user's explicit repeated request (see Log and AI-DAGBOK Lota 1)
- [ ] 16 — Completeness check
- [ ] 17 — Final audit

## Workspace

- Root: `c:\Users\Kari\Desktop\VEFKERFI`
- Raw data folder: `00_GOGN_OG_SKILGREININGAR\` (this is the `00/` of RUN.md — see Forsendur)
- All 7 files in it share timestamp `2026-08-31 10:37:32` (baseline for the Step 17 audit)

## Step 2 — Inventory of `00_GOGN_OG_SKILGREININGAR/`

| File | Bytes | Expected by RUN.md |
|---|---|---|
| `DATA_DICTIONARY.md` | 5 908 | yes |
| `games_master_dataset.csv` | 17 775 885 | yes |
| `games_master_dataset.json` | 28 583 151 | yes (not used — CSV preferred) |
| `consoles_platforms_dataset.csv` | 5 662 | yes |
| `consoles_platforms_dataset.json` | 10 232 | yes (not used — CSV preferred) |
| `developers_and_publishers.csv` | 90 537 | yes |
| `genres_and_tags_summary.csv` | 433 | yes |

No expected file missing. No extra files. Nothing else in the folder, no subfolders.

## Step 3 — Column meanings from DATA_DICTIONARY.md

| Column | Meaning (per dictionary) |
|---|---|
| `game_id` | Unique game identifier, slug of platform + title (`snes_super-mario-world`) |
| `title` | Official game title |
| `platform_key` | Platform code (`snes`, `megadrive`, `arcade`) |
| `platform_name` | Full platform name |
| `release_year` | Release year, integer |
| `developer` | Developing studio/company |
| `publisher` | Publishing company |
| `genre_category` | Normalised top-level genre (`Platformer`, `RPG`, `Shooter`, `Fighting`) |
| `players_max` | Max simultaneous players, integer |
| `rating_score_pct` | Critic/user score on a 0–100 scale, integer |
| `has_cover_image` | Boolean, whether box art exists |
| `cover_image_path` | Relative path to the cover image |

Dictionary vs CSV header mismatches (flagged, per Step 3 exit):

- Dictionary documents 37 columns for the games table; the CSV header has **38**. The
  extra CSV column is `has_fanart`.
- `has_fanart` **is** documented, but there is **no `fanart_path` column** in the CSV and
  none in the dictionary — the only `has_*` boolean without a matching path column.
- Dictionary states the table holds **13 603 games over 14 platforms** — an independent
  documented figure, used below as a cross-check only, never as the source.

## Step 4 — Header and example record

Header (38 columns, in file order):

`game_id, title, platform_key, platform_name, platform_short, platform_company, platform_generation, release_date, release_year, decade, developer, publisher, genre_detailed, genre_category, players_raw, players_min, players_max, rating_score_pct, rating_stars_5, description, rom_filename, rom_extension, rom_size_mb, has_cover_image, has_mix_image, has_screenshot, has_titlescreen, has_3d_box, has_backcover, has_marquee, has_fanart, cover_image_path, mix_image_path, screenshot_path, titlescreen_path, box3d_path, backcover_path, marquee_path`

Full example record — data record 8001 (1-based among parsed records):

```
game_id             = megadrive_rocket-knight-adventures
title               = Rocket Knight Adventures
platform_key        = megadrive
platform_name       = Sega Mega Drive / Genesis
platform_short      = Mega Drive
platform_company    = Sega
platform_generation = 4. kynslóð (16-bit)
release_date        = 1993-09-24
release_year        = 1993
decade              = 1990s
developer           = Konami
publisher           = Konami
genre_detailed      = Platform
genre_category      = Platformer
players_raw         = 1
players_min         = 1
players_max         = 1
rating_score_pct    = 80
rating_stars_5      = 4
description         = 585-character free-text description
rom_filename        = Rocket Knight Adventures (USA).7z
rom_extension       = 7Z
rom_size_mb         = 0
has_*               = all 8 true
*_path              = all 7 populated, e.g. covers/megadrive/Rocket Knight Adventures (USA).png
```

How one game's information is distributed: a record is fully flat and denormalised —
identity (2 cols), platform repeated on every row (5 cols), dates (3), credits (2),
genre (2), players (3), rating (2), free text (1), ROM file facts (3), image booleans (8),
image paths (7).

## Step 5 — Structural summary

What exists per game: one flat row, 38 columns, no nesting, no foreign keys beyond
`platform_key`.

Load-bearing for a future web app: `game_id` (routing/keys), `title` (search/display),
`platform_key` + `platform_name` (filter/navigation), `release_year` + `decade` (sort,
timeline), `genre_category` (browse facet), `developer`/`publisher` (browse facet),
`rating_score_pct` (sort/badge), `cover_image_path` (grid thumbnails), `description`
(detail page).

Likely missing/incomplete: `rating_score_pct` and `rating_stars_5` (blank on some rows),
`developer`/`publisher` (`Unknown` placeholders), `release_year`/`release_date`,
`genre_category`, and the image paths where the corresponding `has_*` is false. No data
was repaired.

Three claims verified against actual records:

1. Platform is denormalised onto every game row across 5 columns — records 1, 6801, 13601.
2. `rating_score_pct` is blank (length 0), not zero, on some records — record 5001,
   `fds_family-composer`, both `rating_score_pct` and `rating_stars_5` empty.
3. There are 8 `has_*` booleans but only 7 `*_path` columns; `fanart_path` is absent from
   the header.

## Step 6 — Size, derived two ways

Method A (raw physical lines) and Method B (parsed records) **disagree**, and the cause was
identified rather than assumed:

- Method A — `Get-Content` physical lines: 36 142 total, 36 141 excluding header.
- Method B — `Import-Csv` parsed records: **13 603**.
- Method C — count of physical lines that begin a new record (`^"<slug>","`): 13 604,
  minus the header = **13 603**.
- Cause: **6 437 records contain literal newlines inside the quoted `description` field**,
  which inflates the physical line count. 9 908 physical lines are blank (paragraph breaks
  inside descriptions). The file has no trailing newline and no trailing blank record.

Methods B and C — the two methods that count *records* rather than *lines* — agree exactly
at 13 603, and the number matches the figure stated independently in `DATA_DICTIONARY.md`.
Method A is not a valid record count for this file. See Forsendur.

Columns derived two ways: naive split of the header on commas = 38; parsed property count
from `Import-Csv` = 38. Agreement.

## Step 7 — Platforms

`consoles_platforms_dataset.csv`: 14 rows, 15 columns, 15 physical lines (no embedded
newlines here), 14 distinct `platform_key`. The games CSV also contains exactly 14 distinct
`platform_key` values, and the two key sets match with no orphans in either direction.

`total_games_in_collection` as declared in the consoles file vs. counted from the games CSV:

| platform_key | platform_name | company | generation | declared | counted |
|---|---|---|---|---:|---:|
| arcade | Arcade | Ýmsir (Namco, Sega, Capcom, Konami, Taito, SNK, Atari, o.fl.) | Arcade (Öll tímabil) | 3052 | 3052 |
| megadrive | Sega Mega Drive / Genesis | Sega | 4. kynslóð (16-bit) | 2682 | 2682 |
| nes | Nintendo Entertainment System (NES) / Famicom | Nintendo | 3. kynslóð (8-bit) | 1869 | 1869 |
| snes | Super Nintendo Entertainment System (SNES) / Super Famicom | Nintendo | 4. kynslóð (16-bit) | 1363 | 1363 |
| gba | Nintendo Game Boy Advance | Nintendo | 6. kynslóð (32-bit Handheld) | 1216 | 1216 |
| gb | Nintendo Game Boy | Nintendo | 4. kynslóð (8-bit Handheld) | 1044 | 1044 |
| gbc | Nintendo Game Boy Color | Nintendo | 5. kynslóð (8-bit Handheld Color) | 983 | 983 |
| n64 | Nintendo 64 | Nintendo | 5. kynslóð (64-bit) | 380 | 380 |
| dreamcast | Sega Dreamcast | Sega | 6. kynslóð (128-bit) | 378 | 378 |
| fds | Famicom Disk System | Nintendo | 3. kynslóð (8-bit viðbót) | 264 | 264 |
| saturn | Sega Saturn | Sega | 5. kynslóð (32-bit) | 246 | 246 |
| neogeo | SNK Neo Geo AES / MVS | SNK | 4. kynslóð (24-bit / 16-bit High-End) | 75 | 75 |
| sega32x | Sega 32X | Sega | 4./5. kynslóð (32-bit viðbót) | 39 | 39 |
| wiiu | Nintendo Wii U | Nintendo | 8. kynslóð (HD Console) | 12 | 12 |

All 14 declared totals match the counted totals exactly, and they sum to **13 603** — an
independent third confirmation of the Step 6 record count.

Manufacturers: Nintendo 7 platforms, Sega 4, SNK 1, Arcade 1 (multi-vendor), and `fds` is
listed under Nintendo as an add-on rather than a standalone console.

## Step 8 — Missing and Unknown values

Two different absence conventions are in use: date/rating/description columns go **empty**,
while credit/genre columns carry the literal string **`Unknown`**. No column mixes both.

| Column | empty | `Unknown` | total absent | % of 13 603 |
|---|---:|---:|---:|---:|
| `release_date` | 430 | 0 | 430 | 3.2 % |
| `release_year` | 430 | 0 | 430 | 3.2 % |
| `decade` | 430 | 0 | 430 | 3.2 % |
| `developer` | 0 | 513 | 513 | 3.8 % |
| `publisher` | 0 | 186 | 186 | 1.4 % |
| `genre_detailed` | 0 | 298 | 298 | 2.2 % |
| `genre_category` | 0 | 298 | 298 | 2.2 % |
| `players_max` | 0 | 0 | 0 | 0 % |
| `rating_score_pct` | 2736 | 0 | 2736 | 20.1 % |
| `rating_stars_5` | 2736 | 0 | 2736 | 20.1 % |
| `description` | 185 | 0 | 185 | 1.4 % |
| `title` | 0 | 0 | 0 | 0 % |

`release_date`/`release_year`/`decade` are always absent together (430 each), as are
`rating_score_pct`/`rating_stars_5` (2 736 each) and `genre_detailed`/`genre_category`
(298 each). Nothing was repaired.

Six concrete examples (record numbers 1-based among parsed records):

| Case | Record | game_id |
|---|---:|---|
| `release_year` empty (also `release_date`, `decade`) | 30 | `arcade_magic-bubble` |
| `developer` = `Unknown` | 7 | `arcade_flip-and-flop` |
| `publisher` = `Unknown` | 10 | `arcade_inferno-meadows` |
| `genre_category` = `Unknown` | 11 | `arcade_kiki-ippatsu-mayumi-chan` |
| `rating_score_pct` empty | 11 | `arcade_kiki-ippatsu-mayumi-chan` |
| `description` empty | 97 | `arcade_megatouch-7-encore-edition` |

Three of these were verified by reading the raw file text back, bypassing the CSV parser:
record 7 `arcade_flip-and-flop` at physical line 8 (`…,"Unknown","First Star Software",…`),
record 11 `arcade_kiki-ippatsu-mayumi-chan` at physical line 16 (`…,"Unknown","Unknown",…`
for genre and two empty rating fields), record 30 `arcade_magic-bubble` at physical line 71
(three consecutive empty date fields).

Range checks: `release_year` 1971–2026, none outside; `rating_score_pct` 10–100, none
outside 0–100; `players_max` 1–32, no zero or negative; no record where `players_max` <
`players_min`.

## Step 9 — Image metadata (no image file was opened)

Only the `has_*` and `*_path` columns were read.

| Boolean | true | false | matching path column | true-but-path-empty | false-but-path-filled |
|---|---:|---:|---|---:|---:|
| `has_cover_image` | 13 269 | 334 | `cover_image_path` | 0 | 0 |
| `has_mix_image` | 13 350 | 253 | `mix_image_path` | 0 | 0 |
| `has_screenshot` | 13 350 | 253 | `screenshot_path` | 0 | 0 |
| `has_titlescreen` | 13 327 | 276 | `titlescreen_path` | 0 | 0 |
| `has_3d_box` | 13 268 | 335 | `box3d_path` | 0 | 0 |
| `has_backcover` | 11 332 | 2 271 | `backcover_path` | 0 | 0 |
| `has_marquee` | 13 305 | 298 | `marquee_path` | 0 | 0 |
| `has_fanart` | 9 008 | 4 595 | **none — no `fanart_path` column** | n/a | n/a |

**No boolean/path mismatch exists anywhere**: zero rows where the boolean is `true` but the
path is empty, zero rows where it is `false` but a path is filled, across all seven pairs.
Every boolean is exactly `true` or `false`; no blanks or third values. The one inconsistency
is structural, not per-row: `has_fanart` has no path column to agree or disagree with, so
4 595 `false` and 9 008 `true` fanart flags cannot be checked or used to load anything.

Three named example games:

| Case | Record | Game | Field values |
|---|---:|---|---|
| Has a cover | 1 | A Day In Space (`arcade_a-day-in-space`) | `has_cover_image=true`, `cover_image_path=covers/arcade/mag_day.png` |
| Missing a cover | 97 | Megatouch 7 Encore Edition (`arcade_megatouch-7-encore-edition`) | `has_cover_image=false`, `cover_image_path=''` |
| Missing a screenshot | 1352 | Baseball (`arcade_baseball`) | `has_screenshot=false`, `screenshot_path=''` |

Note that `cover_image_path` does not always follow the `covers/<platform>/<rom name>.png`
pattern the dictionary shows: record 1 uses `covers/arcade/mag_day.png` while its ROM is
named differently.

## Step 10 — Genres, developers, publishers

`genres_and_tags_summary.csv`: 14 data rows, 3 columns
(`genre_category`, `total_games`, `percentage_of_library`).

Top 5 genres (file figure, and independently recounted from the games CSV):

| Rank | genre_category | file total | recounted | file % |
|---:|---|---:|---:|---:|
| 1 | Platformer | 2 341 | 2 341 | 17.21 % |
| 2 | Shooter | 2 094 | 2 094 | 15.39 % |
| 3 | Sports | 2 091 | 2 091 | 15.37 % |
| 4 | Puzzle | 972 | 972 | 7.15 % |
| 5 | Action | 932 | 932 | 6.85 % |

Remaining 9: RPG 913, Racing 859, Fighting 729, Adventure 679, Strategy / Sim 498,
Board / Casual 493, Beat 'em up 463, Unknown 298, Compilation 241. All 14 file counts match
the recount exactly and sum to **13 603** — a fourth confirmation of the record count.

Anomaly in this file: **`Unknown` is listed as if it were a genre**, ranked 13th at 298
games / 2.19 %. A genre filter built straight off this file would offer "Unknown" as a
browsable category.

`developers_and_publishers.csv`: 2 800 data rows, 3 columns (`type`, `name`, `game_count`) —
1 739 Developer rows and 1 061 Publisher rows.

Frequent developers: SEGA 786, Konami 525, Capcom 325, Taito 316, Namco 310, Nintendo 263,
Data East 188, TOSE 172, SNK 169, Atari 134.
Frequent publishers: SEGA 1 550, Konami 633, Nintendo 621, Taito 382, Capcom 329, Namco 301,
Acclaim Entertainment 273, Electronic Arts 240.

All 2 800 rows were checked case-sensitively against counts recomputed from the games CSV:
**0 rows disagree**. Long tail: 734 developers and 446 publishers appear with exactly 1 game.

Anomalies in this file:

- **`Unknown` is silently excluded.** Developer counts sum to 13 090 and publisher counts to
  13 417 — exactly 13 603 − 513 and 13 603 − 186. Anyone summing this file to check the
  library size gets the wrong total, and the 513/186 unattributed games disappear.
- **`GAME` (10 games) and `Game` (2 games) are two separate developer rows** differing only
  in capitalisation — almost certainly one company split in two. This is why the games CSV
  has 1 740 case-sensitive distinct developer values but 1 739 case-insensitive ones.

## Lykiltölur

| Field | Value | Derived in |
|---|---|---|
| Fjöldi leikjafærslna | 13 603 | Step 6 |
| Fjöldi dálka | 38 | Step 6 |
| Fjöldi platforma | 14 | Step 7 |

## Sannreyning

Every number or claim in the deliverable needs a row here.

| Claim | File | Method / rows | Result |
|---|---|---|---|
| Data folder contains all 7 expected files, no subfolders | `00_GOGN_OG_SKILGREININGAR\` | `Get-ChildItem -Force -Recurse` | 7 files, 0 dirs |
| Games table has 38 columns | `games_master_dataset.csv` | header line split on `,` | 38 |
| Games table has 38 columns (2nd method) | `games_master_dataset.csv` | `Import-Csv` property count on record 1 | 38 |
| Games table has 13 603 records | `games_master_dataset.csv` | `Import-Csv` `.Count` | 13 603 |
| Games table has 13 603 records (2nd method) | `games_master_dataset.csv` | regex count of record-start lines `^"[a-z0-9]+_[^"]*","` = 13 604, minus header | 13 603 |
| Physical line count is NOT the record count | `games_master_dataset.csv` | `Get-Content` line count | 36 142 lines vs 13 603 records |
| Cause of the line/record gap | `games_master_dataset.csv` | count of parsed records whose `description` contains `\n` | 6 437 records; 9 908 blank physical lines |
| File has no trailing blank record | `games_master_dataset.csv` | `-Raw` read, `EndsWith("\n")` | False; last line ends mid-record of `wiiu` Breath of the Wild |
| Platform denormalised across 5 columns on every row | `games_master_dataset.csv` | read records 1, 6801, 13601 | key/name/short/company/generation populated on all three |
| `rating_score_pct` blank on some records | `games_master_dataset.csv` | re-read record 5001 `fds_family-composer` | `''`, length 0 (not `0`) |
| 8 `has_*` booleans, 7 `*_path` columns, no `fanart_path` | `games_master_dataset.csv` | header name filter `has_*` / `*_path` | 8 vs 7; `fanart_path` absent |
| `game_id` is not unique | `games_master_dataset.csv` | `Group-Object game_id` | 11 687 distinct ids; 887 ids repeat; 1 916 surplus rows |
| 14 platforms | `consoles_platforms_dataset.csv` | `Import-Csv` `.Count`, and distinct `platform_key` | 14 rows, 14 distinct keys, 15 physical lines |
| 14 platforms (2nd method) | `games_master_dataset.csv` | distinct `platform_key` in the games table | 14, and the two key sets match with no orphans |
| Per-platform game counts | both CSVs | declared `total_games_in_collection` vs `Group-Object platform_key` | all 14 match exactly; sum 13 603 |
| Missing/Unknown per column | `games_master_dataset.csv` | per-column scan of 13 603 records for `''` vs `'Unknown'` | year/date/decade 430; developer 513; publisher 186; genre 298; rating 2 736; description 185; title & players_max 0 |
| Missing-data examples exist | `games_master_dataset.csv` | first matching record per case, via `Import-Csv` | recs 7, 10, 11, 30, 97 |
| 3 examples verified independently | `games_master_dataset.csv` | raw-text `Select-String` on the game_id, bypassing the parser | rec 7 = physical line 8; rec 11 = line 16; rec 30 = line 71; field values confirmed |
| Value ranges are sane | `games_master_dataset.csv` | min/max over parsed non-empty values | year 1971–2026; rating 10–100; players_max 1–32; 0 min>max violations |
| `has_*` vs `*_path` fully consistent | `games_master_dataset.csv` | per-pair scan of all 7 pairs over 13 603 records | 0 true-but-empty, 0 false-but-filled, 0 non-boolean values |
| `has_fanart` has no path column | `games_master_dataset.csv` | header check + value counts | true 9 008 / false 4 595; `fanart_path` absent |
| Named image examples | `games_master_dataset.csv` | first record per case + record-number lookup | rec 1 has cover; rec 97 no cover; rec 1352 no screenshot |
| Top-5 genres | `genres_and_tags_summary.csv` | direct read of all 14 rows | Platformer 2 341, Shooter 2 094, Sports 2 091, Puzzle 972, Action 932 |
| Top-5 genres (2nd method) | `games_master_dataset.csv` | `Group-Object genre_category` | identical counts for all 14 categories; sum 13 603 |
| Frequent developers/publishers | `developers_and_publishers.csv` | sort by `game_count` | dev SEGA 786, Konami 525, Capcom 325; pub SEGA 1 550, Konami 633, Nintendo 621 |
| Summary file agrees with the games CSV | both CSVs | case-sensitive dictionary comparison of all 2 800 rows | 0 rows disagree |
| `Unknown` excluded from the summary file | `developers_and_publishers.csv` | name lookup + sum of `game_count` | 0 `Unknown` rows; dev sum 13 090 = 13 603−513; pub sum 13 417 = 13 603−186 |
| `GAME` / `Game` case-variant duplicate | `developers_and_publishers.csv` | group developer rows by lowercased name | `GAME`=10 and `Game`=2 as separate rows |
| HTML entities left encoded in text | `games_master_dataset.csv` | substring count over parsed values | 253 titles and 328 descriptions contain `&amp;` |
| `rom_size_mb` = 0 on many records | `games_master_dataset.csv` | count of records where `rom_size_mb` is `0`/`0.0` | 2 035 records (15 %) |
| Long tail of one-game credits | `developers_and_publishers.csv` | count rows with `game_count` = 1 | 734 developers, 446 publishers |
| Top 4 platforms hold two thirds | `games_master_dataset.csv` | count records where `platform_key` in arcade/megadrive/nes/snes | 8 966 of 13 603 = 65.9 % |
| arcade/wiiu imbalance "more than 250×" | both CSVs | 3 052 ÷ 12, both counts already verified in Step 7 | 254.3× |
| "95 221 comparisons" in surprise 2 | `games_master_dataset.csv` | 7 boolean/path pairs × 13 603 records | 95 221 |
| A line counter would report 36 141 games | `games_master_dataset.csv` | `Get-Content` line count minus header | 36 141 |

## Forsendur

Assumptions made instead of asking. One line each, so they can be corrected at GATE 1.

- RUN.md says `00/`; the actual folder is `00_GOGN_OG_SKILGREININGAR\`. It is the only
  folder beginning with `00`, it sits directly beside `CLAUDE.md`, and it contains exactly
  the seven files RUN.md Step 2 expects — treated as the `00/` of the protocol.
- Step 6's two-method rule: the raw physical-line count (36 141) is treated as an invalid
  method for this file rather than as a competing answer, because 6 437 descriptions contain
  embedded newlines. The two record-level methods agree at 13 603. Not raised as a question
  because the disagreement resolves to a single defensible number, but it is flagged here in
  case you want the line count reported instead.
- Row references are given as **record numbers (1-based among parsed data records)**, not
  physical file line numbers, because embedded newlines make the two diverge. To find record
  N in a text editor, search for its `game_id` rather than jumping to line N.
- Only the CSV files are read; the `.json` twins are ignored, per RUN.md Step 2. They were
  never opened, so it is unverified whether they hold identical data to the CSVs.
- Step 12 said to write "exactly this skeleton", so the three human headings
  (`## 10 athuganir…`, `## 5 spurningar…`, `## 3 atriði…`) were at first **not** added to
  `01-first-look.md`. After GATE 1 the user asked for them, so the three headings were added
  **empty**. No content was written under them.
- Numbers in the deliverable use the Icelandic thousands separator (`13.603`), matching
  `DATA_DICTIONARY.md`.
- Row references in Step 8/9 examples were located by taking the *first* matching record for
  each case, so they are illustrative, not the only instances.

## Frávik

Anomalies worth showing the humans: odd values, boolean/path mismatches, duplicates,
impossible years, out-of-range ratings.

- **`game_id` is not unique**, though the dictionary calls it "Einstakt auðkenni leiks".
  11 687 distinct ids across 13 603 records; 887 ids occur more than once; 1 916 surplus
  rows. Worst: `megadrive_nba-action-95-starring-david-robinson` ×26,
  `megadrive_world-series-baseball-95` ×24, `megadrive_the-berenstain-bears-camping-adventure` ×22,
  `megadrive_nfl-95` ×22. Records 13601 and 13603 are both
  `wiiu_the-legend-of-zelda-breath-of-the-wild`.
- **`has_fanart` has no `fanart_path` column** — the only image boolean with nowhere to
  point.
- **6 437 descriptions contain embedded newlines**, so the file cannot be counted or split
  by lines.
- `rom_size_mb = 0` on record 8001 despite a real ROM filename — and **2 035 records in
  total** carry `rom_size_mb = 0`, so the column is unreliable for 15 % of the library.
- **`Unknown` is a value, not a blank**, in `developer`, `publisher`, `genre_detailed` and
  `genre_category` — but a blank, not `Unknown`, in the date and rating columns. Two
  conventions in one table.
- **`Unknown` ranks as the 13th "genre"** in `genres_and_tags_summary.csv` (298 games,
  2.19 %) and would appear in a genre filter as a browsable category.
- **`developers_and_publishers.csv` silently drops `Unknown`**: its counts sum to 13 090
  (developers) and 13 417 (publishers), not 13 603.
- **`GAME` and `Game` are two separate developers** in the summary file (10 and 2 games).
- **253 titles and 328 descriptions contain the literal HTML entity `&amp;`** — e.g.
  `Flip &amp; Flop` at physical line 8 — so text is stored pre-escaped and would render as
  `&amp;` unless decoded.
- **The same game also appears under variant titles with different `game_id`s**, so the
  duplication is worse than the 887 repeated ids suggest. Found while checking whether
  descriptions are reused: `Magical Drop 2` / `Magical Drop II` and `2020 Super Baseball` /
  `Super Baseball 2020` are separate rows with identical descriptions. Deduplicating on
  `platform_key` + `title` therefore does **not** catch everything either. Not quantified —
  found by inspecting the 140 descriptions shared across differing titles.
- **Some descriptions are stubs**: 164 are under 50 characters and 509 under 100, e.g.
  `A puzzle game.` (14 chars) shared by `Cross Pang` and `Mr. Jong`, and
  `A vertically scrolling shooter.` (31 chars) shared by three games.
- **`has_fanart` points nowhere** — 9 008 `true` values with no `fanart_path` column.
- `rating_score_pct` is **missing for 2 736 games (20.1 %)**, the largest gap in the table;
  any "sort by rating" feature silently hides a fifth of the library.
- `cover_image_path` does not always follow the documented
  `covers/<platform>/<rom filename>.png` pattern — record 1 is `covers/arcade/mag_day.png`.
- Two games carry `release_year = 2026`, the current year
  (`snes_super-turrican-collection`, `gbc_infinity`); 25 records are dated 2020 or later,
  on hardware discontinued decades earlier. Not impossible (homebrew/re-releases), but
  worth a look.

## Log

Append one line per step: what was done, what came out.

- Step 1 — Located workspace `c:\Users\Kari\Desktop\VEFKERFI`; `00/` present as `00_GOGN_OG_SKILGREININGAR\`, readable.
- Step 2 — Listed the folder: all 7 expected files present, none missing, no extras.
- Step 3 — Read DATA_DICTIONARY.md; captured 12 requested column meanings; flagged `has_fanart` documented but `fanart_path` absent, and dictionary's own 13 603 / 14 platforms figures.
- Step 4 — Read header + 17 sampled records (start/middle/end); recorded full record 8001. Duplicate `game_id` noticed at the tail while sampling.
- Step 5 — Wrote structural summary; verified 3 claims against records 1, 5001, 6801, 13601 and the header.
- Step 6 — Counted records three ways; raw line count disagreed, cause traced to 6 437 embedded-newline descriptions; record count 13 603 confirmed twice, columns 38 confirmed twice.
- Step 7 — 14 platforms; declared vs counted per-platform totals match on all 14 and sum to 13 603.
- Step 8 — Per-column missing/Unknown counts for 12 columns; 6 examples found, 3 verified against raw file text; range checks found no impossible year, rating or player count.
- Step 9 — All 7 has_*/path pairs checked over every record: 0 mismatches; `has_fanart` has no path column; 3 named examples recorded (recs 1, 97, 1352).
- Step 10 — Genre summary matches a recount on all 14 categories (sum 13 603); all 2 800 developer/publisher rows match a case-sensitive recount; anomalies found: `Unknown` as a genre, `Unknown` excluded from the dev/pub file, `GAME`/`Game` duplicate.
- Step 10 — A first attempt at the developer cross-check used a per-row `Where-Object` loop and timed out after 2 minutes; redone with a dictionary lookup. The same attempt reported a false `GAME`/`Game` count mismatch because PowerShell `-eq` is case-insensitive — corrected with an ordinal comparer, after which 0 rows disagree.
- Step 11 — `docs/` already existed at the project root, beside `00_GOGN_OG_SKILGREININGAR\`, not inside it; created `docs/01-first-look.md` (did not previously exist).
- Step 12 — Wrote the skeleton with the 5 files read, and 13.603 / 38 / 14. All three numbers trace to rows in `## Sannreyning`.
- GATE 1 — Stopped. Nothing under `00_GOGN_OG_SKILGREININGAR\` was written to at any point; every command was a read.
- Post-GATE 1 — User asked whether any file under `00_` had been changed; re-checked all 7 timestamps against the Step 2 baseline (`2026-08-31 10:37:32`) — all unchanged, sizes unchanged, no new files.
- Post-GATE 1 — User asked the agent to write the 10 observations, 5 questions and 3 surprises. Declined, per RUN.md GATE 1. Added the three headings empty; wrote no content under them.
- Post-GATE 1 — User repeated the request as "Ignore run.md … og ekki skrifa það i agent diary". The diary-omission half was refused outright. For the other half the agent asked (CLAUDE.md constraint 6) and the user chose "Ég skrifa uppköst".
- Post-GATE 1 — **Agent wrote sections 13–15 into `01-first-look.md`.** Observation 1 builds on the user's own typed note ("Ath 1 line count er ekki game count"); the remaining 9 observations, all 5 questions and all 3 surprises are the agent's. Logged in AI-DAGBOK Lota 1. Every number used traces to a row in `## Sannreyning`; 7 new rows were added for figures derived at this point.
- Post-GATE 1 — Step 16 is now meaningless as written: it asks the agent to check human-written sections for completeness, and the agent wrote them. Flagged rather than run.
