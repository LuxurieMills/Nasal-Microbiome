# Nasal-Microbiome
How Air Quality Affects the Nose’s Microbial Community
## 1. Project Phase Breakdown & Roadmap

'''
Phase 1: Setup & Acquisition (Weeks 1-3)
  ├── Task 1.1: Source Nasal Microbiome Datasets (NCBI SRA/ENA)
  └── Task 1.2: Query Air Quality APIs (EPA AQS/OpenAQ) for PM2.5, PM10, NO2

Phase 2: Data Preprocessing & Bioinformatics (Weeks 4-6)
  ├── Task 2.1: Quality filtering & denoising via QIIME 2 (DADA2)
  └── Task 2.2: Taxonomic classification against SILVA database

Phase 3: Statistical Modeling & Ecology in R (Weeks 7-9)
  ├── Task 3.1: Alpha & Beta diversity calculations (phyloseq/vegan)
  └── Task 3.2: Differential abundance testing for biomarker identification

Phase 4: Geospatial Integration & Reporting (Weeks 10-12)
  ├── Task 4.1: Spatial mapping of pollution contours & microbial abundance
  └── Task 4.2: Synthesis of final capstone report & presentation
```

### Phase 1: Setup & Data Acquisition (Weeks 1–3)
*   **Objective:** Secure high-quality public biological datasets and temporally matching environmental records.
*   **Microbiome Mining:** Scan NCBI Sequence Read Archive (SRA) and EMBL-EBI European Nucleotide Archive (ENA) for 16S rRNA or metagenomic sequencing datasets focusing on human anterior nares. Target cohorts with geographic distribution variations (e.g., urban vs. rural, or high-traffic vs. low-traffic zones).
*   **Environmental Mining:** Identify geographic metadata (lat/long coordinates, ZIP codes, or cities) and collection timestamps from the microbiome metadata. Query the US EPA Air Quality System (AQS) API or OpenAQ API to retrieve exact corresponding 24-hour averages for PM2.5 and PM10, alongside 1-hour maximums for NO2.
*   **Risk Management:** Public metadata is frequently incomplete. If exact locations are hidden for privacy, pivot to macro-level regional data (city-wide averages) as an acceptable proxy.

### Phase 2: Bioinformatics Pipeline & Taxonomic Profiling (Weeks 4–6)
*   **Objective:** Convert raw sequencing reads into structured, clean taxonomic abundance tables.
*   **Quality Control & Denoising:** Import raw FASTQ data into QIIME 2. Execute `qiime dada2 denoise-paired` (or single-end, depending on the dataset) to trim adapter sequences, filter low-quality bases, and collapse identical sequences into Amplicon Sequence Variants (ASVs).
*   **Taxonomic Assignment:** Train a Naive Bayes classifier or utilize a pre-verified classifier optimized for the specific hypervariable region used in the study (e.g., V4 or V3-V4). Reference the SILVA or Greengenes2 database to map ASVs to clear taxonomic lineages down to the genus or species level.
*   **Data Export:** Transform internal QIIME 2 artifacts (`.qza`, `.qzv`) into an open format, generating a unified BIOM or flat CSV file containing the final feature table and taxonomy strings.

### Phase 3: Statistical Ecology & Correlation Modeling in R (Weeks 7–9)
*   **Objective:** Model mathematical relationships between air pollution levels and nasal microbial structures.
*   **Data Aggregation:** Load taxonomic counts, sample metadata, and corresponding pollutant concentrations (PM2.5, PM10, NO2) into a unified `phyloseq` object within R.
*   **Alpha Diversity Evaluation:** Compute within-sample richness metrics (e.g., Shannon Index, Faith’s Phylogenetic Diversity, Observed Features) using the `vegan` package. Apply Spearman's rank correlation or multi-variable linear regressions to analyze whether rising pollution levels significantly reduce microbial richness or evenness.
*   **Beta Diversity Analysis:** Compute between-sample dissimilarities using Bray-Curtis and Weighted/Unweighted UniFrac distance matrixes. Visualize clustering patterns through Principal Coordinate Analysis (PCoA). Execute a PERMANOVA (`vegan::adonis2`) test to determine what percentage of variance in the nasal microbial matrix is statistically driven by individual or combined air pollutants.
*   **Biomarker Selection:** Apply differential abundance frameworks like ANCOM-BC or DESeq2. Identify specific microbial families or genera that are significantly enriched or depleted in high-pollution clusters, singling out potential opportunistic pathogens or protective commensals.

### Phase 4: Geospatial Mapping & Capstone Synthesis (Weeks 10–12)
*   **Objective:** Translate statistical tables into intuitive spatial graphics and compile the final report.
*   **Spatial Visualizations:** Leverage R spatial packages (`sf`, `ggplot2`) to plot geographic distribution points of the sampled population. Overlay continuous interpolation heatmaps representing PM2.5 or NO2 gradients to provide clean visual evidence of environmental pressure.
*   **Synthesis:** Finalize documentation detailing the workflow, analytical metrics, biological conclusions, and limitations of using secondary public data.

