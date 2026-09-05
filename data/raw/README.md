# Raw data — not included in this repository

This folder is intentionally empty in version control (see `.gitignore`). Two raw source
files are required to run the notebooks in full, and neither is redistributed here:

## 1. AHTPDB export (`dataset.xlsx`)

Used by `notebooks/04_ahtpdb_cleaning_and_leakage_audit.ipynb` and
`notebooks/05_learning_curves_and_external_validation.ipynb`.

AHTPDB (Kumar et al., *Nucleic Acids Research*, 2015) does not have a confirmed
explicit bulk-redistribution license on its own site as far as we have checked. Rather
than assume permission, we do not bundle a raw export of it here. What we *do* provide,
in `data/derived/ahtpdb_cleaned_harmonized.csv`, is our own cleaned/unit-harmonized
derivative (2,610 rows, IC50s converted to μM, descriptors attached) — this is
sufficient to reproduce every result in this repository without needing the raw file.

If you want to reproduce the cleaning step itself (Section 3 of notebook 04) from
scratch, obtain the AHTPDB export directly from the database's own site/authors and
place it here as `dataset.xlsx`.

## 2. Wang et al. (2020) Supporting Information (728-peptide table)

Used by `notebooks/01_wang2020_baseline_descriptors.ipynb`,
`02_wang2020_protbert_embeddings.ipynb`, and `03_wang2020_esm2_embeddings.ipynb`.

ACS's stated policy permits downloading and using this article's Supporting
Information for research purposes, but does not grant a public redistribution license
for the raw file. Download it yourself from the official ACS SI page for:

> Wang, Y.-T., Russo, D. P., Liu, C., Zhou, Q., Zhu, H., & Zhang, Y.-H. (2020).
> Predictive modeling of angiotensin I-converting enzyme (ACE) inhibitory peptides
> using various machine learning approaches. *Journal of Agricultural and Food
> Chemistry*. https://doi.org/10.1021/acs.jafc.0c04624

and place it here (check the notebook's `DATA_PATH`/`file_path` variable for the
expected filename before running).

## What's safe to redistribute (and already included)

- `data/derived/` — our own cleaned/derived tables. These are transformed research
  outputs, not raw republications of either source database.
- `data/external/jiao2025_tripeptide_table.csv` — the 150-tripeptide table from Jiao et
  al. (2025), *RSC Advances* (CC BY-NC 3.0), reproduced with attribution as permitted by
  its license for non-commercial academic reuse. See `DATA_LICENSING.md` for the full
  citation and license text.
