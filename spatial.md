---
title: "spatial"
format: html
---

# Types of technologies

### Sequencing-based vs. Imaging -based
- Lower vs. higher resolution
- Lower vs. higher sensitvity
- Full transcription vs. targeted plexy

### Differences

- Refer to review by [Cheng et al. 2023](https://www.sciencedirect.com/science/article/pii/S1673852723000759?via%3Dihub)
- How transcripts are spatially barcoded vary across technologies

# Data

- Sequencing-based
  + Same counts matrix (gene x spot) from single-cell but you have spatial coordinates of spot (usually the centroid)

# Processing

## Sequencing-based

- Reads have spot barcode instead of cell barcode
- SpaceRnager for converting
  + Sequence data to spot x gene expression matrix
  + Image file (H&E, .TIFF) to spot coordinates
  
- Alignment of spot to images (**understand more**)
  + Image used for alignment is taken before sequencing - with Visium same tissue for image and sequencing
  + Check if you need to manually change alignment
  + Bubbles or any artifact are often interpreted as tissue so might need to manually remove spot barcodes
  + Fiducial as reference, spots in image should align with spot in machine used for sequencing

- Filtered barcodes from SpaceRanger
  + Automated tissue detection based on image alignment
  + Use loupe to explore alignment betweeen image and sequencing-based, can even see which barcodes are outside tissue

### QC

- Spots
  + Not great < 500 transcrips per spot (this is for old Visium using poly-A which captures all transcripts)
  + Mark problematics spots - manual with loupe then get get barcodes for those problematics spots (can either remove or mark in spot metadata)

### Spatial clustering analysis

1. Cluster spots using transcriptome data
2. Cluster spots using transcriptome data on spatially variable genes
3. Cluster spots by modelling transcriptome data and spatial information

### Cell type deconvolution from spots

- Various methods - machine learning, statistical models, regression, mapping
- Reference-based methods vs. no reference based methods (infer transcriptional groups)
- CARD is a good starting in R, comparable to cell2loc but faster - gives cell type probability per spot
  + But CARD is reference-based

### Tools

- R ecosystem
  + In R, Seurat is the most mature, Bioconductor packages coming up at the moment tools are reimplementing functions (10 Sep 2024)
  + [Giotto](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02286-2)
 
### Questions

- Metric for diversity of image features/signature in a spot
