# RNA-binding LCROME

A research workflow for characterizing **low-complexity regions (LCRs) in human RNA-binding proteins (RBPs)** and exploring how their sequence properties and domain context relate to RNA-target classes.

The project integrates RBP dataset curation, six LCR detection configurations, layered annotation, proteome-background comparisons, and unsupervised protein clustering. Prepared datasets, analysis tables, figures, and interpretation notes are included.

## Workflow

1. **Assemble the RBP set:** combine the Gerstberger census with Pfam 38.2 candidates from Ensembl release 116, RBPDB, and human MODOMICS entries; map and annotate proteins through UniProt.
2. **Detect and consolidate LCRs:** run AlcoR, CAST, fLPS strict, LCRFinder, SEG strict, and SEG intermediate. Merge redundant intervals within each protein and method, preserving provenance and keeping methods separate.
3. **Annotate three layers:** RNA-target superclass, position relative to retained Pfam domains, and rule-based physicochemical signatures such as RG/RGG repeats, SR/RS repeats, charge enrichment, and aromatic content.
4. **Compare LCR phenotypes:** evaluate differences among RNA-target classes and against a reconciled human proteome background using effect sizes and multiple-testing correction.
5. **Cluster proteins:** explore global phenotypes, including non-carriers, and detailed LCR-carrier phenotypes using PAM/k-medoids, hierarchical clustering, and HDBSCAN. RNA classes are used for interpretation, not as clustering features.

The committed global clustering covers **1,924 proteins**. The separate background analysis contains **20,427 proteins**, including **1,669 RBP-labeled proteins** after accession/gene reconciliation; these are distinct analysis populations.

## Repository layout

| Path | Contents |
| --- | --- |
| [`datasets/`](datasets/) | Source datasets, identifier mapping, RBP assembly, and background construction |
| [`rbp_lcrs/`](rbp_lcrs/) | Detector wrappers, raw/merged calls, method comparisons, and Pfam overlaps |
| [`rbp_superclasses/`](rbp_superclasses/) | RNA-target classification with confidence and evidence |
| [`lcr_analyses/`](lcr_analyses/) | Domain-position and physicochemical annotation, statistics, clustering, and visualizations |
| [`project_outcome_report.md`](project_outcome_report.md) | Consolidated statistical outputs and selected PAM clustering results |

## Getting started

To explore existing results, start with the [outcome report](project_outcome_report.md), [internal analysis guide](lcr_analyses/quantitative_analysis/lcr_quantitative_analysis_guide.md), and [background analysis guide](lcr_analyses/background_analysis/lcr_background_analysis_guide.md).

To rerun the quantitative analysis from the included annotation workbooks, clone the repository and install its core analysis dependencies in an activated Python environment:

```bash
git clone https://github.com/jalilbzn/rna-binding-lcrome.git
cd rna-binding-lcrome
python -m pip install numpy pandas scipy statsmodels matplotlib seaborn openpyxl
python lcr_analyses/quantitative_analysis/lcr_quantitative_analysis.py --outdir rerun_results/quantitative
```

Run scripts from the repository root. The example writes per-method Excel tables and figures to a separate output directory.

Other stages require additional packages: `scikit-learn`, `kmedoids`, and `hdbscan` for clustering; `requests`, `biopython`, and `xlrd` for relevant data-processing scripts; and `plotly` with an image-export backend for domain-position plots.

Fresh detection requires external installations: AlcoR, HMMER/Pfam resources, [self-hosted PlaToLoCo](rbp_lcrs/platoloco/RUN_PlaToLoCo.txt), and [LCRFinder with MATLAB or Octave](rbp_lcrs/lcrfinder/RUN_LCRFinder.txt). The LCRFinder guide documents required Octave patches.

## Interpretation and reproducibility

This is a collection of research scripts and recorded results, with no pinned environment or single end-to-end runner. Some defaults and historical notes contain local Windows paths or older filenames; check inputs, sheet names, and output paths before rerunning a stage. Despite legacy `ensembl` filenames, the adopted background is based on the reviewed UniProt human reference proteome plus reconciled additions; see the [construction notes](datasets/rbps_census/ensembl_v116/ensembl_background_construction.md).

Physicochemical signatures and clusters support exploratory hypotheses. They do not establish RNA binding or molecular function experimentally. Protein-level analyses underpin biological interpretation; LCR-level results and records marked `descriptive_only` require the qualifications in the analysis guides.

See the [physicochemical annotation specification](lcr_analyses/pc_properties/lcr_physicochemical_property_analysis.md) and [clustering workflow](lcr_analyses/clustering/lcr_clustering_phase_workflow.md) for detailed definitions and references.
