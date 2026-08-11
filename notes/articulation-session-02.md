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

**Hook** (1 slide, no clicks). The whole cold open on one slide, straight off
the title: the FINAL_v2 filename pileup beside the Polaroid. The folder gets
the laugh; about two seconds in, the photo fades on its own while the question
rises beneath it: "hey, this number changed since the last version, what
happened?" and you have absolutely no idea.

**Setup** (2 slides).

1. We solved this once, for code. Two columns: for code, versioning is a
   reflex (commits, checkout, `git blame`); for data, we do our best (dated
   folders, `_v2` suffixes), but no author, no diff, no way back. The
   discipline has been there for years, just not the tool. One click brings
   the whole close at once: there's more than one way to get that tool, and
   mine is git-grade history for your data, without leaving R. The stem
   sentence is spoken here rather than printed; the bullets already carry
   what, who, and why.
2. The lakehouse in one slide: open files (almost always Parquet) + a catalog
   + transactional writes, shown as one block; a single click brings the
   catch. Iceberg and Delta solved this years ago for Spark clusters with
   platform teams; this room was priced out of the solution, not the problem.
   Light aside: the session is called "From laptop to cluster," and this talk
   runs on the laptop. Ends on "here's the door."

**Chapter I: build the time machine.** DuckDB already lives in your R
session; DuckLake makes the entire lakehouse a folder of Parquet plus a catalog
you can query. Three lines to attach, a medallion pipeline in plain dplyr,
every write an authored commit. Button: your data has a git log now.

**Chapter II: drive the time machine.** Query any table as of any snapshot.
Biff deletes 26 rows under the message "small tweak"; the change feed answers
"what happened?" with the actual rows, not a guess. Then the concrete example
from session 1's feedback, one serious slide: the Duke chemo-predictor case,
where verifying the data took two MD Anderson biostatisticians about 1,500
hours by hand; with a paper trail, that archaeology is a query. Restore rolls
forward so even the recovery is auditable; a pinned read-only snapshot is what
a regulator can read. The PCCTC cameo lands the stakes here.

**Chapter III: trace the flux capacitor.** SQL teams get column lineage
from dbt and SQLMesh; dplyr had nothing. {dplyneage} walks the lazy query tree
dbplyr already built: one pipe from an existing pipeline to a live DAG, plus
impact analysis before you change the past. The almanac lesson.

**Conclusion and call to action** (1 slide). The whole close on one slide: the
two packages against the stem, the recipe said plainly (an open format, a
catalog, authored writes; DuckLake is one road there), and the button: we made
version control a habit for code, data's turn. Install lines sit quiet at the
bottom, and the Doc Brown sign-off is spoken over it. CTA stays Monday-sized:
take one pipeline you already own and put its tables in a lake with authored
commits. Thirty minutes, reversible.

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
- The opening compressed for time. The postcard now leads as the hook, with
  the changed-number question and the FINAL_v2 pileup folded onto that same
  slide. No clicks: the photo fades on its own about two seconds after the
  slide appears, and the question fades in beneath it. The "version the data"
  ask came off the slide; the stem and the close still say it.
- Setup tightened to two slides. The git contrast absorbed the stem slide:
  the printed stem came off (the bullets already say what, who, and why; the
  sentence is now spoken over the reveal), and the slide closes on one line,
  git-grade history for your data, without leaving R. "Overwrite and hope"
  softened to "We've had the discipline for years, just not the tool." The deck
  sits at 30 counted slides, down from 33.
- Each chapter divider collapsed to one line: build the time machine, drive
  the time machine, trace the flux capacitor. The job verb moved into the
  title, and the line beneath now summarizes the chapter in a sentence. The
  titles are sized to hold a single line.
- Every fragment click after the Part I divider is stripped so the deck flips
  fast on the call; clicks can come back selectively later. The two setup
  clicks (git contrast, lakehouse catch) stay.
- The close collapsed from four slides to one: the table, the recipe, data's
  turn, install lines. The roads image and OUTATIME plate live on in git
  history.
- Chapter II gained the concrete example from session 1's feedback: one
  serious slide on the Duke chemo-predictor case, right after the change-feed
  payoff. Figures verified (about 1,500 hours of forensic work, three
  terminated trials, ten retractions), and MD Anderson still serves the full
  data and code, so a companion replay is possible. Deck now sits at 28
  counted slides.
- The lakehouse slide dropped from five clicks to one. Its bullets reworded
  for accuracy (open files, almost always Parquet), the JVM line translated
  into Spark clusters and platform teams, and a quiet aside added: the
  session title promises "laptop to cluster," and this talk runs on the
  laptop.

## Chapter jobs in one line each, if they ask

- I: a versioned lake costs three lines, not a platform team.
- II: history turns "why did this change?" from archaeology into a query.
- III: lineage answers the last clause of the stem, where every column came
  from.
