# AFE-SE-Corr
R scripts for analysis of correlation between AFE and SE in GTEx samples

## Overview
This R script orchestrates a multi-sample analysis linking Alternative First Exon (AFE) usage to Skipped Exon (SE) splicing events, computing per-event correlations, stratifying by genomic distance, and exporting BED files for downstream genomic analyses or visualization.

## Directory Structure & Input Files
<pre markdown>
project/
├─ samples/
│  ├─ SRR000001/
│  │  ├─ hit/SRR000001.AFEPSI
│  │  └─ rmats/SE.MATS.JC.txt
│  └─ SRR000002/ …
├─ scripts/
│  └─ SE_AFE_Corr.R
└─ outputs/
</pre>
## Pipeline Steps
List sample folders matching the pattern (e.g. SRR*).

#### Concatenate AFE & SE

makeConcatHIT(): merges per-sample AFEPSI into one table.

makeConcatrMATS(): merges per-sample rMATS SE PSI.

#### Merge Overlapping Regions

mergeProm.par(): collapse overlapping AFE promoter regions.

mergeSE.par(): collapse overlapping SE exons.

#### Build SE–AFE Pair Data

makeDFperSE.par(): for each SE region, gather SE PSI and overlapping AFE PSI.

#### Correlation Computation

calcRho*() functions compute Spearman or Kendall correlations.

#### Classification & Annotation

se.classifyDF.par(): annotate each pair with distance, direction, and bin.

#### Promoter Adjustment

adjustAFEtoProm.par(): expand promoters ±300 bp/±100 bp around TSS.

#### Quantile Stratification & Export

quantExt_toBED() & SEquantExt_toBED(): split into sextiles, plot distributions, write BEDs.

#### Final Object

Returns a list of all outputs, saved as PipeOut.RDS.

## Outputs & File Formats
Intermediate RDS in <output_dir>:
concatAFE.RDS, concatSE.RDS, mergeAFE.RDS, mergeSE.RDS,
dfPerSE.RDS, rho.RDS, classifyRho.RDS,
sextileRho.RDS, TSSdis.RDS, p20kb.RDS

Final BEDs:
AFE stratified: afe.s1.bed … afe.s6.bed
SE stratified: se.s1.bed … se.s6.bed
s3 and s4 are combined to reflect experimental profile of quintiles

PipeOut.RDS: named list of all main objects for quick load.
