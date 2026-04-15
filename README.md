# Stream Macroinvertebrate NMDS Ordination
  This code and lesson accompanies the Methods in Stream Ecology Textbook chapter on benthic invertebrates. It provides a step-by-step workflow   for visualizing and statistically testing differences in macroinvertebrate community composition using Nonmetric Multidimensional Scaling       (NMDS).

## 📊 What This Analysis Does
This repository walks through a complete NMDS ordination workflow:
1. Data Preparation

  Join benthic count data with sample metadata
  Filter by season (Spring and Fall)
  Build a wide-format community matrix

2. Ordination

  Apply log(x+1) and Wisconsin double standardization
  Calculate Bray-Curtis dissimilarity
  Run NMDS in 2D and evaluate stress

3. Visualization

  NMDS plots by stream gradient type (Riffle vs. Boatable)
  NMDS plots by season (Spring vs. Fall)
  NMDS plots by gradient × season interaction

4. Statistical Testing

  PERMANOVA to test for significant group differences
  BETADISPER to verify homogeneity of dispersion
  Indicator species analysis (IndVal.g)
  Trait annotation using the master taxa table

## 🚀 Quick Start
  Prerequisites
    Install required R packages:
      rinstall.packages(c("dplyr", "tidyr", "ggplot2", "vegan", "ggrepel", "indicspecies"))
  Running the Analysis

    Clone or download this repository
    Open Code/NMDS_Macroinvertebrate_Analysis.Rmd in RStudio
    Confirm data files are in the Data/Raw/ folder
    Run all chunks or knit the document

Expected Runtime

  Small datasets (< 50 sites): ~1–2 minutes
  Medium datasets (50–200 sites): ~2–5 minutes
  Large datasets (> 200 sites): ~5–10 minutes

## 📁 Repository Structure
.
├── Code/
│   └── NMDS_Macroinvertebrate_Analysis.Rmd    # Main annotated R Markdown tutorial
├── Data/
│   ├── Raw/
│   │   ├── stationBenthicsTESTSITE.csv        # Macroinvertebrate counts (long format)
│   │   ├── stationInfoBenSampsTESTSITE.csv    # Sample metadata (gradient, season)
│   │   └── masterTaxaGenus.csv                # Taxonomic reference and trait table
│   └── ProcessedOutput/                       # Generated results (created on run)
├── InstructionDocuments/
│   ├── Instructions-NMDSAnalysis.docx         # Step-by-step guide
│   ├── QuickReferenceGuide-NMDS.docx          # Copy-paste code and quick lookups
│   └── NMDSResultsInterpretation.docx         # How to read and report your results
├── outputs/                                   # Generated plots (created on run)
└── README.md

## 📖 Documentation
This repository includes three comprehensive instruction documents in the InstructionDocuments/ folder:

### 1. Instructions-NMDSAnalysis.docx

  What NMDS is and when to use it
  Step-by-step walkthrough of every code section
  How to choose between parametric and non-parametric ordination
  How to customize the analysis for your own data

2. QuickReferenceGuide-NMDS.docx
  For quick lookups:

    Copy-paste ready code blocks
    Stress value interpretation table
    PERMANOVA and BETADISPER interpretation guide
    Troubleshooting checklist
    Citation templates

3. NMDSResultsInterpretation.docx
  Learn how to interpret and report your results:

    What NMDS axes represent
    How to read ellipses and centroids
    How to interpret PERMANOVA R² and p-values
    How to report BETADISPER results alongside PERMANOVA
    Example results write-up for a publication

## 🔬 Methods Background

The Dataset
  This analysis uses the same benthic macroinvertebrate dataset as the Diversity Analysis module. The data includes:

    88 unique sampling stations
    141 unique samples (BenSampID)
    182 taxa (primarily family-level identifications)
    Two gradient types: Riffle and Boatable
    Two seasons: Spring and Fall

Why NMDS?
  NMDS (Nonmetric Multidimensional Scaling) is the most widely used ordination method in stream ecology because:

    It makes no assumptions about multivariate normality
    It works with Bray-Curtis dissimilarity, which is ideal for community data
    It handles the many zeros typical in species matrices
    Stress values provide a transparent measure of how well the 2D plot represents the data

Why Bray-Curtis?
  Bray-Curtis dissimilarity is standard for abundance-based community data:

    Ranges from 0 (identical communities) to 1 (no shared taxa)
    Ignores joint absences (two sites lacking the same taxon are not made more similar)
    Sensitive to dominant taxa when combined with abundance data

Why log(x+1) + Wisconsin standardization?
  Raw abundance data are right-skewed and often dominated by a few taxa (e.g., Chironomidae). Two sequential transformations correct this:

    log(x+1): Compresses the range of dominant taxa while preserving zeros
    Wisconsin double standardization: Scales by species maximum then by site total, giving rare taxa more balanced weight

## 🎯 Learning Objectives
After completing this module, you will be able to:

  - Build a community matrix from long-format macroinvertebrate count data
  - Apply appropriate data transformations for abundance-based ordination
  - Run and interpret an NMDS ordination using Bray-Curtis dissimilarity
  - Evaluate ordination fit using stress values and Shepard plots
  - Produce publication-ready NMDS plots with ellipses and centroids
  - Test for significant group differences using PERMANOVA
  - Verify PERMANOVA assumptions using BETADISPER
  - Identify indicator taxa using IndVal analysis
  - Annotate results with functional traits (FFG, tolerance values)

## 🐛 Troubleshooting

Error: "object 'comm_scaled' not found"
Cause: Steps were not run in order
Solution: Run all chunks from the top, or use Session → Restart R and Run All Chunks

NMDS stress > 0.20
Cause: Community data are too complex to represent in 2 dimensions
Solution: Try k = 3 in metaMDS() and interpret a 3D solution

PERMANOVA is significant but BETADISPER is also significant
Cause: Groups differ in both centroid position AND within-group spread
Solution: Report both results and interpret PERMANOVA with caution — see NMDSResultsInterpretation.docx

Warning: "group with fewer than 2 observations" in stat_ellipse
Cause: One group has too few samples to draw a confidence ellipse
Solution: Check group sample sizes with nmds_scores %>% count(Gradient, Season)

No indicator taxa found
Cause: IndVal threshold may need adjusting, or communities are not well differentiated
Solution: Try summary(indval_gradient, alpha = 0.10) to relax the significance threshold

See QuickReferenceGuide-NMDS.docx for a full troubleshooting checklist.

## 📚 Citation
If you use this code in your research, please cite:
  Methods in Stream Ecology Textbook:
  [Citation to be added]
  Key methods references:

    McCune, B., & Grace, J. B. (2002). Analysis of Ecological Communities. MjM Software.
    Bray, J. R., & Curtis, J. T. (1957). An ordination of the upland forest communities of southern Wisconsin. Ecological Monographs, 27(4),         325–349.
    Oksanen, J., et al. (2022). vegan: Community Ecology Package. https://CRAN.R-project.org/package=vegan
    De Caceres, M., & Legendre, P. (2009). Associations between species and groups of sites. Ecology, 90(12), 3566–3574.

## 🤝 Contributing
This is an educational resource. If you find errors or have suggestions for improvement, please:

  Open an issue describing the problem or suggestion
  For code fixes, submit a pull request
  For documentation improvements, submit suggested edits

## 📧 Contact
For questions about this module:

  Content questions: Refer to the Methods in Stream Ecology textbook
  Technical issues: Check the troubleshooting section in QuickReferenceGuide-NMDS.docx
  Bug reports: Open an issue in this repository

## 📜 License
  This educational material is provided for use with the Methods in Stream Ecology textbook. Please use responsibly and cite appropriately.

## Happy analyzing! 🐛📊🌊
