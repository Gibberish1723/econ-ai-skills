# AEA Replication Package README Template

> **Usage:** Copy this template into `Replication/README.md` and replace all `[FILL IN]` placeholders.
> Delete this usage block and the template instructions (lines starting with `>`) before submission.
> Follow the [Social Science Data Editors' template README](https://social-science-data-editors.github.io/template_README/) for the authoritative specification.

---

# Data and Code for: [FILL IN: Paper Title]

> **Title format:** Use "Data and Code for: [Title]" if data is included, or "Code for: [Title]" if all data is restricted/external.

## Overview

[FILL IN: One paragraph summarizing the replication package. What does it contain? What does it reproduce? Example: "This package contains the code and data needed to reproduce all tables and figures in [Paper Title] by [Authors], published in [Journal]. The code is written in R and requires approximately 2 hours to run on a standard desktop computer."]

## Data Availability and Provenance Statements

> Provide a separate statement for each data source. Organize into subsections for data that IS vs IS NOT provided.

### Data Provided

> For each dataset included in the package:

**[FILL IN: Dataset Name]**

- **Source:** [FILL IN: Organization or author that produced the data]
- **URL/DOI:** [FILL IN: Persistent URL or DOI]
- **Date accessed:** [FILL IN: YYYY-MM-DD]
- **License/Terms:** [FILL IN: License under which data is distributed, or "publicly available"]
- **Description:** [FILL IN: Brief description of what the data contains]

[FILL IN: Statement of rights. Example: "The author(s) of the manuscript certify that they have legitimate access to and permission to use the data used in this manuscript."]

### Data Not Provided

> For each dataset NOT included (restricted, proprietary, too large):

**[FILL IN: Dataset Name]**

- **Source:** [FILL IN: Organization that administers the data]
- **URL/DOI:** [FILL IN: URL where access can be requested]
- **Access conditions:** [FILL IN: How to obtain access — application process, cost, timeline, eligibility requirements]
- **Preservation:** [FILL IN: How long the data will remain available. AEA requires minimum 5-year preservation]
- **Description:** [FILL IN: What the data contains and why it cannot be shared]

> For restricted-access data, include: contact person/office, application URL, expected wait time, any cost, and a codebook or data dictionary describing all variables used.

## Dataset List

> List ALL data files used by the code, whether provided or not.

| Data File | Source | Provided | Notes |
|-----------|--------|----------|-------|
| `data/raw/[FILL IN].csv` | [FILL IN: Source] | Yes | [FILL IN: Brief note] |
| `data/raw/[FILL IN].dta` | [FILL IN: Source] | No | [FILL IN: Access instructions above] |

## Computational Requirements

### Software Requirements

> List ALL software with exact version numbers.

- **R** [FILL IN: version, e.g., 4.3.2]
  - `tidyverse` ([FILL IN: version])
  - `fixest` ([FILL IN: version])
  - `[FILL IN: package]` ([FILL IN: version])
  - The file `code/config.R` will install all dependencies. A `renv.lock` file is provided for exact version replication.

> If using Stata:
- **Stata** [FILL IN: version, e.g., MP 18.0]
  - `reghdfe` ([FILL IN: version])
  - `[FILL IN: package]` ([FILL IN: version])
  - The file `code/config.do` will install all dependencies.

> If using Python:
- **Python** [FILL IN: version, e.g., 3.11.5]
  - See `requirements.txt` for package versions.

### Hardware Requirements

> Describe the machine used for replication.

- **Processor:** [FILL IN: e.g., Intel Core i7-12700, Apple M2, etc.]
- **Memory (RAM):** [FILL IN: e.g., 16 GB — note minimum required if less than total]
- **Storage:** [FILL IN: e.g., approximately 2 GB required for data and outputs]
- **OS:** [FILL IN: e.g., <platform>, macOS 14.2, Ubuntu 22.04]

### Runtime

> Report wall-clock time for a complete run from raw data to final outputs.

- **Total runtime:** [FILL IN: e.g., approximately 45 minutes]
- **Longest individual script:** [FILL IN: e.g., `03_analysis.R` takes approximately 20 minutes]
- **Machine used for timing:** [FILL IN: same as hardware above, or specify if different]

### Random Seed

- **Seed value:** [FILL IN: e.g., `set.seed(12345)` — declared in `code/config.R`]
- **Where used:** [FILL IN: e.g., bootstrap standard errors in `03_analysis.R`, simulation in `05_robustness.R`]

## Description of Programs/Code

> List programs in the order they should be run. The master script (`00_master.R`) runs all programs in sequence.

| Program | Description | Inputs | Outputs |
|---------|-------------|--------|---------|
| `code/config.R` | Configuration: installs packages, sets paths and seed | — | — |
| `code/00_master.R` | Master script: runs all programs below in order | All raw data | All tables and figures |
| `code/01_clean.R` | [FILL IN: e.g., Cleans raw CPS data, constructs analysis variables] | `data/raw/[FILL IN]` | `data/clean/[FILL IN].rds` |
| `code/02_merge.R` | [FILL IN: e.g., Merges cleaned datasets, applies sample restrictions] | `data/clean/[FILL IN].rds` | `data/clean/[FILL IN].rds` |
| `code/03_analysis.R` | [FILL IN: e.g., Main regression tables] | `data/clean/[FILL IN].rds` | `output/tables/[FILL IN].tex` |
| `code/04_figures.R` | [FILL IN: e.g., Event study and summary figures] | `data/clean/[FILL IN].rds` | `output/figures/[FILL IN].pdf` |
| `code/05_robustness.R` | [FILL IN: e.g., Robustness checks and sensitivity analysis] | `data/clean/[FILL IN].rds` | `output/tables/[FILL IN].tex` |

### Code License

[FILL IN: e.g., "Code is licensed under a Modified BSD License. See LICENSE.txt for details." or "Code is provided under a MIT License."]

> AEA default is Modified BSD. Include a `LICENSE.txt` file in the package root.

## Instructions to Replicators

> Step-by-step instructions assuming a clean machine with the required software installed.

1. **Download** the replication package and unzip to a local directory.
2. **Set working directory** to the root of the unzipped package.
3. **Install dependencies:**
   - R: Run `source("code/config.R")` — this installs all required packages.
   - Alternatively: `renv::restore()` to install exact package versions from `renv.lock`.
4. **Obtain restricted data** (if applicable):
   - [FILL IN: Instructions for obtaining data not included in the package]
   - Place files in `data/raw/` with the filenames listed in the Dataset List above.
5. **Run the master script:**
   ```
   Rscript code/00_master.R
   ```
   This reproduces all tables and figures. Expected runtime: [FILL IN].
6. **Verify outputs:** Tables are saved to `output/tables/`, figures to `output/figures/`. Compare against the paper.

> If using Stata: `stata-mp -b do code/00_master.do`
> If using Python: `python code/00_master.py`

## List of Tables and Programs

> Cross-reference every table and figure in the paper to the script that produces it.

| Table/Figure | Program | Output File | Notes |
|-------------|---------|-------------|-------|
| Table 1 | `code/03_analysis.R` | `output/tables/table1_summary.tex` | [FILL IN: e.g., Summary statistics] |
| Table 2 | `code/03_analysis.R` | `output/tables/table2_main.tex` | [FILL IN: e.g., Main regression results] |
| Figure 1 | `code/04_figures.R` | `output/figures/fig1_event_study.pdf` | [FILL IN: e.g., Event study plot] |
| Figure 2 | `code/04_figures.R` | `output/figures/fig2_heterogeneity.pdf` | [FILL IN: e.g., Heterogeneity analysis] |
| Table A1 | `code/05_robustness.R` | `output/tables/tableA1_robustness.tex` | [FILL IN: e.g., Appendix robustness] |

## References

> Cite all data sources here, separately from the paper's bibliography. Use DOI when available. Follow [AEA Sample References](https://www.aeaweb.org/journals/data/references) format.

[FILL IN: Example entries below — replace with actual citations]

- Bureau of Labor Statistics. 2023. "Current Population Survey, Annual Social and Economic Supplement." Washington, DC: U.S. Department of Labor. https://doi.org/10.xxxx/xxxxx.
- Flood, Sarah, Miriam King, Renae Rodgers, Steven Ruggles, J. Robert Warren, and Michael Westberry. 2023. "Integrated Public Use Microdata Series, Current Population Survey: Version 11.0 [dataset]." Minneapolis, MN: IPUMS. https://doi.org/10.18128/D030.V11.0.

> Analysis data files produced by your code (e.g., `data/clean/analysis_sample.rds`) do NOT require citations.

## Acknowledgements (Optional)

[FILL IN: e.g., "The authors thank the AEA Data Editor for guidance on replication package preparation." or "Computations were performed on the Emory HPCC cluster."]

## RCT Registration (If Applicable)

> Required for randomized controlled trials. Registration number must also appear in the title page footnote of the paper.

- **Registry:** [FILL IN: e.g., AEA RCT Registry]
- **Registration number:** [FILL IN: e.g., AEARCTR-0001234]
- **URL:** [FILL IN: e.g., https://www.socialscienceregistry.org/trials/1234]

## IRB/Ethics Approval (If Applicable)

> Required for research involving human subjects, surveys, or experiments. Must also appear in the title page footnote.

- **Institution:** [FILL IN: e.g., Emory University IRB]
- **Protocol number:** [FILL IN: e.g., IRB-2023-001234]
- **Status:** [FILL IN: e.g., Approved / Exempt]
