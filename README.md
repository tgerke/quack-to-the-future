# Quack to the Future

Slides for my talk at posit::conf(2026) in Houston:

> **Quack to the Future: Building Traceable Data Lakehouses with DuckLake in R**
>
> Modern data pipelines need more than clean outputs: they need a paper trail. This talk introduces two R packages for building a lakehouse stack with DuckLake for transactional storage and dplyneage for column-level lineage tracking — a medallion architecture built with familiar dplyr workflows, where every table write is versioned with an author and commit message, and every column traces back to its sources.

## Packages featured

- [ducklake](https://github.com/tgerke/ducklake-r) — tidyverse-friendly interface to [DuckLake](https://ducklake.select)
- [dplyneage](https://github.com/tgerke/dplyneage) — column-level lineage for dplyr pipelines

## Rendering the slides

The code chunks execute for real at render time (the demo lake in `data/` is rebuilt on each render), so both packages need to be installed:

```r
pak::pak("tgerke/ducklake-r")
pak::pak("tgerke/dplyneage")
```

Then:

```bash
quarto render
```

Slides land in `index.html`. Speaker notes are on every content slide (press `S` in the browser).

## Credits

Slide design riffs on Back to the Future (Universal Pictures); movie stills appear under fair-use conference-talk conventions. Deck structure follows the arc taught by the Articulation speaker-coaching team.
