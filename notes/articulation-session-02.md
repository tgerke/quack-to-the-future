# Meadowlark session 2: the outline

Working notes for the second coaching session. Their ask: present the outline so
they can hear the structure. A hook, the setup, about three chapters of meat,
and a conclusion if one exists. It does.

## The big idea, restated

The talk argues for a norm, not a package. This room treats version control for
code as a given (git, renv, CI) and gives the data those pipelines produce none
of it. A lakehouse closes that gap: open files, a catalog, snapshots on every
write. DuckLake is the road I took and the one I demo; someone who walks out
and picks Iceberg instead still proves the talk worked.

The stem sentence carries over from session 1:

> Data pipelines need a paper trail: what changed, who changed it, why, and
> where every column came from.

## The outline

**Hook** (2 slides). "Hey, this number changed since the last version. What
happened?" And you have no idea. Then the laugh: the FINAL_v2 filename pileup,
Slack as the audit log, memory as the lineage system.

**Setup** (4 slides).

1. The fading Polaroid: every overwrite quietly erases history.
2. New beat: we solved this once, for code. Commits, checkouts, `git blame`.
   The data gets overwrite-and-hope. This names the double standard out loud.
3. The stem slide.
4. The lakehouse in one slide: Parquet + a catalog + transactional snapshots.
   Iceberg and Delta solved this years ago behind a JVM and a platform team;
   this room was priced out of the solution, not the problem. Ends on "here's
   the door."

**Chapter I: the time machine** (build it). DuckDB already lives in your R
session; DuckLake makes the entire lakehouse a folder of Parquet plus a catalog
you can query. Three lines to attach, a medallion pipeline in plain dplyr,
every write an authored commit. Button: your data has a git log now.

**Chapter II: time travel** (drive it). Query any table as of any snapshot.
Biff deletes 26 rows under the message "small tweak"; the change feed answers
"what happened?" with the actual rows, not a guess; restore rolls forward so
even the recovery is auditable; a pinned read-only snapshot is what a regulator
can read. The PCCTC cameo lands the stakes here.

**Chapter III: the flux capacitor** (trace it). SQL teams get column lineage
from dbt and SQLMesh; dplyr had nothing. {dplyneage} walks the lazy query tree
dbplyr already built: one pipe from an existing pipeline to a live DAG, plus
impact analysis before you change the past. The almanac lesson.

**Conclusion and call to action.** The two packages assembled against the stem,
then the big idea said plainly on its own slide: strip away the duck and the
recipe is an open format, a catalog, and authored writes. DuckLake is one road
there. We made version control a habit for code; data's turn. CTA stays
Monday-sized: take one pipeline you already own and put its tables in a lake
with authored commits. Thirty minutes, reversible.

**Backup, after the close.** The whole-lake stitched DAG, the interchange
formats (Mermaid, OpenLineage), and lineage x time travel. Cut from the main
line for focus, kept alive for Q&A.

## What changed since session 1, and why

- Before: a 4-slide "what the big-data world does" block, then Part I
  (ducklake, 10 slides) and Part II (dplyneage, 6). Part I told two stories at
  once, and the context block held a lineage slide that belonged with Part II.
- Now: chapters split by job (build, drive, trace) rather than by package. The
  lakehouse concept moved into setup, which also serves the tool-agnostic
  frame.
- Added the double-standard beat in setup and the pattern-not-the-package beat
  in the close. That pair is the big idea; the demos are the evidence for it.
- The cut list from session 1 executed as written: interchange formats and the
  whole-lake DAG moved to backup, and the "one more thing" kicker collapsed to
  one line on the summary slide.

## Chapter jobs in one line each, if they ask

- I: a versioned lake costs three lines, not a platform team.
- II: history turns "why did this change?" from archaeology into a query.
- III: lineage answers the last clause of the stem, where every column came
  from.
