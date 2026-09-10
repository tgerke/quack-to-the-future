# September 2026: simplify the chapters to the README arc

Working notes for the restructure of 2026-09-10, after the ducklake and
dplyneage docs were simplified. The earlier deck is archived separately and
stays in git history before this commit.

## What changed

The hook and setup slides are untouched. The three chapters now follow the
ducklake README and Getting Started arc: attach, load the raw layer, derive
silver and gold, see the history; then break it, read the change feed, read
an earlier version, restore; then draw the lineage from the two recipes and
ask it an impact question.

The deck sits at 21 counted slides, down from 28. Backup holds five, down
from seven: the stitched whole-lake diagram moved into the main line, and
the "how it works" slide moved out of it.

## Decisions

- **One table, three schemas:** mtcars only, as `bronze.vehicles`,
  `silver.vehicles`, and `gold.efficiency_by_cyl`, mirroring the README.
  The origins join went. It cost a second table and a join on the
  silver/gold slide and bought only a second source node in the diagram.
  Schemas make the folder listing on the DuckLake slide literally the
  medallion, and every lineage node names its layer.
- **Gold is a summary, not the README's flag column:** `group_by(cyl)` and
  `summarise()` give the diagram an aggregation edge with an expression
  label, and a three-row table that reads from the back of the room.
- **Commit confirmations are on the slides:** write chunks set
  `message: true`, so the audience sees "Committed snapshot N (author):
  message" before a later slide uses that number. The line exists only in
  ducklake's development version (0.7.0.9000), so rendering needs
  `pak::pak("tgerke/ducklake-r")`, not the CRAN release. A hidden
  `stopifnot()` after Biff's commit fails the render if the snapshot
  numbers printed as literals on later slides ever drift.
- **Biff stays, in three slides:** `rows_delete()` rather than
  `replace_table()`, because the change feed follows row operations and a
  rewrite starts a new table id. The delete keys are narrowed to `model` so
  dplyr does not print "Ignoring extra y columns" on the slide.
- **No install step:** `attach_ducklake()` downloads the extension on first
  use, so "three lines to a lakehouse" became two, `library()` and attach.
- **Literal snapshot numbers on two slides** (`version = 3`, `start = 5`):
  they are part of the story, because the confirmation printed them two
  slides earlier, and the lake is rebuilt on every render, so they are
  deterministic. The guard chunk above is what makes that safe.

## Loose ends

- The gold table has three rows and the lake's inlining row limit is 10,
  so DuckLake keeps those rows in the catalog until a flush. The
  `gold/efficiency_by_cyl/` folder exists on disk but holds no Parquet file
  yet. The listing on the DuckLake slide shows the general layout, not a
  literal `ls` of this lake.
- Close-slide maturity line: ducklake 0.7.0 is on CRAN, dplyneage 0.3.1 is
  on GitHub. The old Zenodo DOI mention came out. Re-add it after checking
  that the DOI points at a current release.
