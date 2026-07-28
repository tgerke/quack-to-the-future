# Articulation session 1: core message

Working notes for the first coaching session. The stated goal: identify the talk's
core message and why it matters to the audience. Mark this up during the call.

## The governing idea

Current one-liner, as drafted on the stem slide:

> Data pipelines need a paper trail: what changed, who changed it, why, and where
> every column came from. Everything else is how to get it without leaving R.

Alternate phrasings to stress-test with Blythe/Acacia. Each shifts the emphasis a
little:

1. **Paper trail** (current): "Your data deserves a paper trail, and you don't have
   to leave R to build one." Emphasis on the artifact. Concrete, auditable, mine.
2. **Git envy**: "We version our code religiously and our data not at all. That gap
   is now closable in an afternoon." Emphasis on the double standard everyone
   already accepts for code.
3. **Query, not archaeology**: "'Why did this number change?' should be a query,
   not an investigation." Emphasis on the felt pain. This is the phrasing the new
   change-feed slide now pays off directly.

My lean: keep 1 as the stem, use 3 as the opening hook (which is how the deck is
already built). 2 is a supporting beat, not the message; git is an analogy, and
analogies make weak stems.

## Why it matters to this audience

- posit::conf attendees are mostly R analysts and data scientists without a
  platform team. The lakehouse world solved versioning and lineage years ago, but
  behind a JVM, a cluster, and someone else's infrastructure budget. This audience
  was priced out of the solution, not the problem.
- Everyone in the room has lived `analysis_data_v2_FINAL_fixed.csv` and the Slack
  thread asking which file is current. The pain is universal; the fix has not been.
- For the regulated slice of the room (pharma, clinical research, finance), this
  is not hygiene, it's compliance. A pinned snapshot with authorship is the
  difference between "trust me" and pointing at the commit. My PCCTC work is the
  credibility anchor here: I need this for regulators, not for style points.
- Timing: DuckDB is already in their toolbox and dbplyr already speaks it. The
  missing piece was small enough for one person to build, which is itself part of
  the message: this stopped being an infrastructure problem.

## Think, feel, do

- **Think**: versioned, explainable data is no longer an enterprise feature. It's
  two packages and a folder.
- **Feel**: recognition (the laugh at the filenames slide), then relief (three
  lines to a lakehouse), then agency (I could do this to my current project).
- **Do**: version the next pipeline instead of the next `write_csv()`. Try
  ducklake and dplyneage, file issues.

## How the deck serves the message

Current arc, mapped to the Articulation structure we pre-aligned on:

1. Opening & idea statement: the changed-number story, the fading photo, the stem
   slide. All three exist to earn one sentence.
2. Context & implications: what the big-data world does, why the infrastructure
   tax kept this audience out, the lineage gap for dplyr.
3. Proposition with stories: Part I (ducklake: commits, time travel, Biff, the
   change feed, the clinical cameo), Part II (dplyneage: column lineage, impact
   analysis, interchange formats).
4. Call to action: try it, version everything, "make it a good one."

If pressed to cut for focus, the candidates are (in order): the interchange
formats slide (Mermaid/OpenLineage; it serves data engineers, the smallest slice
of the room), the whole-lake stitched diagram (the single-pipeline diagram already
lands the point), and the sorted/partitioned internals I already left out. The
Biff sequence and the clinical cameo stay; they carry the who/why and the stakes.

## Questions they'll probably ask, and my current answers

- **Who exactly is this for?** The R data scientist who owns their pipeline
  end-to-end and has no platform team behind them. Not the Spark shop.
- **What do you want them to do Monday morning?** Take one pipeline they already
  have and put its tables in a lake with authored commits. Thirty minutes,
  reversible, no migration project.
- **Why you?** I run data science for a clinical trials consortium where audit
  trails are non-negotiable, and I built both packages because I needed them.
- **Why now?** DuckLake hit 1.0, DuckDB is mainstream in R, and ducklake is under
  CRAN review. The pieces just became boring enough to trust.
- **What's the one sentence they repeat to a colleague?** "You can get a
  versioned, git-log-style data lake in R with three lines of code."
- **What does the audience lose if they ignore this?** Nothing today. Six months
  from now, the answer to "why did this change?" That's the almanac lesson.
