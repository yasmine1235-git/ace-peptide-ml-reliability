# Data Licensing and Provenance

This project uses three external data sources. They are not all redistributed in the repository; the policy for each source is based on its applicable publication or database terms.

| Source                                                                                  | Used for        |                                       Raw file in repo? | Derived data in repo? | Repository policy                                                                                                                                                                   |
| --------------------------------------------------------------------------------------- | --------------- | ------------------------------------------------------: | --------------------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Wang et al. (2020), *Journal of Agricultural and Food Chemistry* Supporting Information | Notebooks 01–03 |                                                  **No** |                **No** | The raw dataset is not redistributed. Users should obtain it independently from the original publication.                                                                           |
| AHTPDB (Kumar et al., 2015, *Nucleic Acids Research*)                                   | Notebooks 04–05 |                                                  **No** |                **No** | AHTPDB data are not redistributed. The repository provides the preprocessing and harmonization procedures needed to reproduce the analysis from independently obtained source data. |
| Jiao et al. (2025), *RSC Advances*, Table 1                                             | Notebook 05     | **Yes** — `data/external/jiao2025_tripeptide_table.csv` |                     — | Included under the applicable **CC BY-NC 3.0** license with attribution to the original publication.                                                                                |

## Required citations

### Wang et al. (2020)

If you use or reproduce analyses based on the Wang et al. dataset, cite:

> Wang, Y.-T., Russo, D. P., Liu, C., Zhou, Q., Zhu, H., & Zhang, Y.-H. (2020). Predictive Modeling of Angiotensin I-Converting Enzyme Inhibitory Peptides Using Various Machine Learning Approaches. *Journal of Agricultural and Food Chemistry*, 68(43), 12132–12140.
> https://doi.org/10.1021/acs.jafc.0c04624

The raw Wang dataset is not included in this repository. See `data/raw/README.md` for information on obtaining the source data.

### AHTPDB

If you use the AHTPDB-derived analyses or reproduce the preprocessing workflow, cite the original database publication:

> Kumar, R., Chaudhary, K., Sharma, M., Nagpal, G., Chauhan, J. S., Singh, S., Gautam, A., & Raghava, G. P. S. (2015). AHTPDB: A Comprehensive Platform for Analysis and Presentation of Antihypertensive Peptides. *Nucleic Acids Research*, 43(D1), D956–D962.
> https://doi.org/10.1093/nar/gku1141

The article was published online in 2014 and subsequently appeared in the 2015 volume/issue; **2015** is used here as the bibliographic publication year.

### Jiao et al. (2025)

If you use `data/external/jiao2025_tripeptide_table.csv`, cite:

> Jiao, F., Yang, J., Wang, F., Peng, S., & Zhou, B. (2025). Discovery of novel angiotensin-converting enzyme inhibitory peptides by in silico and in vitro studies. *RSC Advances*, 15, 39885–39897.
> https://doi.org/10.1039/D5RA06104K

The article is published under the **Creative Commons Attribution–Non-Commercial 3.0 Unported Licence (CC BY-NC 3.0)**. Attribution must be retained when the table data are reused.

## Data provenance and redistribution

This repository distinguishes between **code**, **third-party source data**, and **results derived from those data**.

* The Wang et al. raw dataset is not redistributed.
* AHTPDB data, including cleaned or harmonized derivatives, are not redistributed.
* The Jiao et al. (2025) tripeptide table is redistributed under its stated CC BY-NC 3.0 license.
* Analysis results, figures, and derived features generated from the external datasets are included as part of the research record where applicable.

Users reproducing the Wang or AHTPDB analyses should obtain the corresponding source data independently and follow the terms applicable to those sources.

## Code License

The original code in this repository, including notebooks and analysis scripts, is released under the **MIT License** (see `LICENSE`).

The MIT License applies **only to the original code in this repository**. It does not grant rights to third-party datasets, publications, or other external materials, which remain subject to their respective licenses and terms.
