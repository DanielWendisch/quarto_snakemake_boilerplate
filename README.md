# quarto_snakemake_boilerplate

Minimal template for a reproducible single-cell RNA-seq analysis: **Snakemake** runs parameterised **Quarto** notebooks, and each notebook is rendered to a standalone HTML report. Use it as a starting point for new projects.

![rulegraph](rulegraph.png)

## What it does

The example workflow takes a Seurat object and, for each `size` in `sizes` (see the top of `Snakefile`):

1. `1_first.qmd` downsamples the object to `size` cells per identity and writes `intermediate_data/small_seurat_{size}_obj.rds`
2. `2_second.qmd` reads `user_input.xlsx` (analysis settings edited by hand), filters the object accordingly and writes `intermediate_data/filtered_small_seurat_{size}_obj.rds`
3. Reports are written to `output/reports/` (set in `_quarto.yml`)

Notebooks receive `size` through Quarto parameters (`quarto render -P downsample_size=...`), and keep working when run interactively with their default parameter values.

## Run

```bash
# input expected at ../../raw_data/mock_data/Monocytes_{size}.Rds  (edit the Snakefile to point elsewhere)
snakemake --cores 4
```

R packages are pinned in `renv.lock` (`renv::restore()`), Python helpers in `requirements.txt`.

## Adapting it

- Change `sizes` and the `rule downsample` input path in `Snakefile`
- Replace the notebooks with your own steps, keeping the `-P` parameter pattern
- Add settings that biologists should be able to edit to `user_input.xlsx`
