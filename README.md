# Unsupervised Psychological Measurement via Semantic Projections

This repository contains the official implementation, pre-calculated results, and figures for the research work:
**"A theory-driven and unsupervised framework for measuring psychological constructs through semantic projections in LLM embedding space"** 
*Developed by Maria Luongo, Davide Marocco, and Nicola Milano at the Natural and Artificial Cognition Laboratory (Department of Humanistic Studies, University of Naples Federico II).*

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Methodology](#methodology)
3. [Repository Structure](#repository-structure)
4. [File & Results Guide](#file--results-guide)
   - [Calculated Excel Files](#calculated-excel-files)
   - [Figures & Tables](#figures--tables)
5. [Execution Guide (Google Colab)](#execution-guide-google-colab)
6. [Git & GitHub Setup](#git--github-setup)

---

## Project Overview

Traditional psychological assessments rely heavily on structured self-report scales (such as the PHQ-9, CES-D, GAD-7, and PSWQ). This project introduces a **theory-driven, unsupervised framework** to measure psychological states (specifically **depression, anxiety, and worry**) directly from natural language responses.

By utilizing high-dimensional sentence embeddings (via **SentenceBERT**) and projecting these representations onto custom **semantic axes** derived from clinical scale items and lexical anchors, we capture psychological constructs continuously without training a supervised classifier. The results show strong alignment with clinical ground-truths across various text lengths and formats (such as selected words, short phrases, and full written texts).

---

## Methodology

1. **Embedding Generation**: Natural language responses (e.g., words, phrases, or full text written by participants) are embedded using the `all-roberta-large-v1` SentenceBERT model.
2. **Semantic Axis Construction**: Centroids of positive anchors (representing calmness or positive states) and negative anchors (representing depressive or anxious states) are calculated in the embedding space. The vector difference between the positive and negative centroids defines the direction of the **Semantic Axis**.
3. **Projection**: Participant text embeddings are projected onto the semantic axis using vector dot products, generating an unsupervised score.
4. **Validation & Reliability**:
   - Correlation with clinical self-report scores (PHQ, CES-D, GAD, PSWQ).
   - Split-half reliability analysis across response formats.
   - Distributional similarity profiling via **Wasserstein Distance ($WD_z$)**.
   - Performance benchmark comparisons with lexicon-based sentiment analysis (**VADER**).

---

## Repository Structure

The workspace is organized into a clean repository layout:

```
Semantic_Axis_Projection/
├── README.md                           # This documentation file
├── .gitignore                          # Excludes system, temp, and private files (e.g., Article.docx)
├── dataset.xlsx                        # The raw input dataset (267 observations)
├── semantic_axis_projections.py        # Complete analysis script, optimized for Google Colab
├── calculated_excel/                   # Main pre-calculated Excel sheets (results)
│   ├── SemanticAxes.xlsx               # Semantic axis projections, reliability, and scoring
│   └── Sentiment_Analysis_Benchmark.xlsx # VADER Sentiment Analysis benchmark results
└── tables_and_figures/                 # Main output figures (TIFF format) and pivot tables
    ├── Semantic_Tables_1_2_3.xlsx      # Pivot Tables 1, 2, and 3 from the paper
    ├── FIGURE 1.tiff                   # Semantic axis construction and projection demo
    ├── FIGURE 2.tiff                   # Sensitivity analysis of correlations across formats
    ├── FIGURE 3.tiff                   # Distributional similarity (Wasserstein Distance) - ALL
    ├── FIGURE 3_T1.tiff                # Distributional similarity - Baseline/Time 1 (T1)
    ├── FIGURE 3_T2.tiff                # Distributional similarity - Follow-up/Time 2 (T2)
    ├── FIGURE 4.tiff                   # Convergent/discriminant validity correlation matrix - ALL
    ├── FIGURE 4_T1.tiff                # Convergent/discriminant validity matrix - T1
    ├── FIGURE 4_T2.tiff                # Convergent/discriminant validity matrix - T2
    └── FIGURE 5.tiff                   # Comparison of projection vs. VADER sentiment performance (Δ)
```

---

## File & Results Guide

### Calculated Excel Files

Located inside [calculated_excel/](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/calculated_excel):
- **[SemanticAxes.xlsx](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/calculated_excel/SemanticAxes.xlsx)**: This sheet holds all unsupervised semantic projection scores across all axes (`DEP_WORDS`, `DEP_CESD`, `DEP_ZUNG`, `WOR_WORDS`, `WOR_ZUNG`, `WOR_STAI`) and formats. It contains the following sheets:
  - `Semantic_CrossSectional`: Pearson correlation, partial correlation, confidence intervals, and disattenuation parameters.
  - `Heatmap_Semantic`: Summarized correlations for scale/axis visualization.
  - `Dati_Scoring_Semantic`: The actual individual projection scores computed for each participant.
  - `Data_ALL` / `Data_T1` / `Data_T2`: Split data subsets.
  - `Reliability_Scale`: Predefined reliability parameters for clinical instruments.
  - `Reliability_Proiezioni`: Computed split-half reliability values for each projection format.
- **[Sentiment_Analysis_Benchmark.xlsx](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/calculated_excel/Sentiment_Analysis_Benchmark.xlsx)**: Sentiment baseline results calculated using VADER. Useful for verifying how embedding-based semantic projection compares to traditional sentiment-intensity metrics.

### Figures & Tables

Located inside [tables_and_figures/](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/tables_and_figures):
- **[Semantic_Tables_1_2_3.xlsx](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/tables_and_figures/Semantic_Tables_1_2_3.xlsx)**: Contains the 3 main tables reported in the paper:
  - `Table1`: Associations between depression-related semantic projections and self-reports (CESD and PHQ) across response formats (from selected words to free text).
  - `Table2`: Associations between worry/anxiety semantic projections and self-reports (GAD and PSWQ) across response formats.
  - `Table3`: Split-half reliability estimates of projection scores across response formats.
- **FIGURE 1.tiff**: Visualizes the geometric layout of positive/negative anchors and participant responses projected orthogonally onto a 2D PCA representation of the embedding space.
- **FIGURE 2.tiff**: Displays the sensitivity analysis, comparing raw correlations, partially disattenuated correlations, and fully disattenuated correlations for both depression and worry.
- **FIGURE 3.tiff (and subsets T1/T2)**: Evaluates the distributional similarity between the projected scores and corresponding clinical questionnaire scores using Wasserstein Distance ($WD_z$).
- **FIGURE 4.tiff (and subsets T1/T2)**: Correlation matrices illustrating convergent and discriminant validity across depression-oriented and worry/anxiety-oriented axes.
- **FIGURE 5.tiff**: Plots $\Delta$ (the difference in partially disattenuated correlation coefficients between embedding-based projections and VADER sentiment). Projections show significant advantages in free-text (`Write text`) responses.

---

## Execution Guide (Google Colab)

The script [semantic_axis_projections.py](file:///c:/Users/maria/Desktop/Semantic_Axis_Projection/semantic_axis_projections.py) is tailored to execute sequentially in **Google Colab**.

### How to Run:
1. Open [Google Colab](https://colab.research.google.com/).
2. Create a new notebook, click on **File** -> **Upload notebook**, and select `semantic_axis_projections.py`.
3. Run the notebook cell by cell.
4. **Step 1** will automatically install `sentence-transformers` via pip.
5. **Step 2** will prompt you with a file upload widget. Click **Choose Files** and upload `dataset.xlsx`.
6. The script will automatically calculate:
   - Semantic projections and reliability estimates.
   - VADER sentiment benchmarking.
   - Wasserstein Distance inference and statistics.
   - Recreate and save all Figures (1–5) and Tables (1–3).
7. At the end of each section, the files will be downloaded automatically to your local machine via browser triggers.

---

## Git & GitHub Setup

To track changes and upload this repository to GitHub, follow these commands in your local terminal:

1. **Initialize Git**:
   ```bash
   git init
   ```
2. **Stage files for commit** (this will stage all code, README, configurations, and results, while ignoring `Article.docx` and temp files as configured in `.gitignore`):
   ```bash
   git add .
   ```
3. **Commit files**:
   ```bash
   git commit -m "Initial commit: Organized repository structure with Colab script, results, and figures"
   ```
4. **Link to GitHub and Push**:
   Create a repository on GitHub (e.g., named `Semantic-Axis-Projection`) and execute:
   ```bash
   git branch -M main
   git remote add origin https://github.com/YOUR_GITHUB_USERNAME/Semantic-Axis-Projection.git
   git push -u origin main
   ```
