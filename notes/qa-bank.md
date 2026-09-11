# Q&A bank

Material that used to sit in the speaker notes, moved here on 2026-09-11 so the notes
hold only the spoken script. Grouped by the slide it belongs with.

## The lakehouse, in one slide

- The file format is technically pluggable (Iceberg also takes ORC and Avro) but Parquet
  is the near-universal choice. Every flavor has a catalog in function if not in name:
  Delta keeps a transaction log beside the data, Iceberg uses metadata files plus a
  catalog service, DuckLake uses a plain SQL database.
- "Spark clusters" stands in for the JVM stack nobody in this room runs. DuckDB reaches
  for a cluster only when the data outgrows one machine.
- The Iceberg catalog tier alone: Apache Polaris ships production Helm values of 4 CPU
  and 8 GiB per pod at three replicas, plus its own Postgres, and the in-memory default is
  documented as unfit for production. The older Hive Metastore path is no lighter, with
  Cloudera calling the 256 MB default heap inadequate, HiveServer2 starting at 4 GB, and a
  16 GB cap per instance because garbage collection pauses get long. A Spark or Trino
  cluster sits on top of that.
- Read path: three layers of metadata files read in sequence at roughly 100 ms a request,
  so half a second passes before you know which Parquet files to open. DuckLake asks one
  SQL question and gets one answer in milliseconds. Writes go the same way, about one
  transaction per second for Iceberg and Delta against roughly 100 for DuckLake, and
  Postgres alone sustains thousands. Every Iceberg insert also lands three or four files,
  which is how small-file cleanup becomes someone's job.
- Whether one machine holds: the cheapest MacBook, 8 GB of RAM with DuckDB capped at 5,
  ran all of ClickBench cold in 59.7 seconds, up to 2.8 times faster than the cloud
  instances it was measured against, and hot it came within 13 percent of a 16-vCPU box
  with four times the memory. TPC-DS SF100 on that same laptop: 1.63 second median, 15.5
  minutes for the suite. A Galaxy S24 Ultra finished TPC-H SF100 over 30 GB in 235
  seconds while an AWS r6id.large took 570.8. A 2012 MacBook Pro cleared all 22 TPC-H
  queries against a 265 GB database.
- Demand side: the median scan in Redshift and Snowflake reads about 100 MB, the 99.9th
  percentile under 300 GB.

## Attach a lake

- `install_ducklake()` exists only to fetch the extension ahead of time, for containers
  and CI.
- Safety: `create = FALSE` makes a mistyped path an error instead of a new, empty lake.

## Then Biff gets write access

- Row-level operations (`rows_delete()`, `rows_update()`) are what the change feed
  traces; a full `replace_table()` rewrite is a drop-and-create, so its history starts
  over. That is why Biff's tweak is a `rows_delete()` rather than a filter followed by
  `replace_table()`.

## "What happened?" is now a query

- Why `collect()`: the feed is a lazy table, and printing one adds a "Source: SQL" header
  with the catalog's file path. `collect()` brings the rows into R and prints a plain
  tibble.

## Fixing the timeline

- To freeze a whole session at one snapshot, attach the lake with `snapshot_version`,
  and `read_only = TRUE` for a pinned lake a regulator can read:
  `attach_ducklake("q3_submission", lake_path = "data/lake", snapshot_version = 4, read_only = TRUE)`.
  `plot_snapshots()` draws the snapshot timeline.

## The recipe is the lineage

- How it knows: dbplyr already built the query tree; dplyneage walks it directly, in pure
  R, so attribution is exact rather than best-effort string matching. Joins,
  aggregations, windows, and unions resolve to true source columns. dtplyr and arrow
  pipelines get the same walk; raw SQL and duckplyr go through sqlglot (Python,
  provisioned on first use).
- Beyond R: `lineage_json()` to commit next to the code and diff in CI, `lineage_check(old, new)`
  to fail the PR that rewires a column, `lineage_openlineage()` for DataHub, Marquez, and
  OpenMetadata; `lineage_mermaid()` renders on GitHub and in Quarto, `lineage_graphml()`
  opens in Gephi, yEd, and igraph.
- Plain data frames: a local dplyr pipeline has no query tree to walk, so no. The
  one-line fix is `dbplyr::tbl_lazy(df, name = "df")`, and the identical pipeline becomes
  traceable; `extract_lineage()` errors with exactly that pointer.

## The whole paper trail, assembled

- Maturity: experimental lifecycle badges on both. ducklake 0.7.0 is on CRAN; dplyneage
  0.3.1 is on GitHub. Docs at tgerke.github.io/ducklake-r and tgerke.github.io/dplyneage.

## Lineage × time travel

- `get_ducklake_table_version()` builds raw SQL under the hood, so that particular
  extraction runs on the sqlglot engine.
- The "lineage that travels with the data" article on the dplyneage site covers
  `commit_extra_info` and `lineage_from_json()`.

## The Duke case (cut from the deck, kept for Q&A)

- In one breath: Duke, 2006, a genomic predictor promised to match each cancer patient
  to the chemotherapy most likely to work. Baggerly and Coombes at MD Anderson spent
  about 1,500 hours reconstructing it and found sensitive/resistant labels reversed and
  gene lists shifted by one row. Three trials ran on it before Duke stopped them; ten
  papers retracted (Baggerly & Coombes 2009, Annals of Applied Statistics). Versioning
  would not have caught the errors by itself; it makes the reconstruction cheap.
- Timeline: Potti and Nevins published the predictors starting 2006 in Nature Medicine;
  MD Anderson clinicians asked Baggerly and Coombes to vet them before adopting; the
  reconstruction became the 2009 Annals of Applied Statistics paper; Duke's three trials
  were suspended, restarted, then terminated in November 2010 as the scandal broke and
  Potti resigned; Retraction Watch counts ten retractions plus seven corrections, and the
  federal Office of Research Integrity found misconduct in 2015.
- Their full supplement (raw data, R code, reports) is still served by
  bioinformatics.mdanderson.org, which is what would make a replay of this case possible.
