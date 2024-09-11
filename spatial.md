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

### Sequencing-based

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

##### QC

- Spots
  + Not great < 500 transcrips per spot (this is for old Visium using poly-A which captures all transcripts)
  + Mark problematics spots - manual with loupe then get get barcodes for those problematics spots (can either remove or mark in spot metadata)

##### Spatial clustering analysis

1. Cluster spots using transcriptome data
2. Cluster spots using transcriptome data on spatially variable genes
3. Cluster spots by modelling transcriptome data and spatial information

##### Cell type deconvolution from spots

- Various methods - machine learning, statistical models, regression, mapping
- Reference-based methods vs. no reference based methods (infer transcriptional groups)
- CARD is a good starting in R, comparable to cell2loc but faster - gives cell type probability per spot
  + But CARD is reference-based
  
##### Neighbourhood and cellular niche analysis

- Using `Seurat::TopNeighbors()` to get n nearest neighbours

##### Tools

- R ecosystem
  + In R, Seurat is the most mature, Bioconductor packages coming up at the moment tools are reimplementing functions (10 Sep 2024)
  + [Giotto](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02286-2)
 
# Questions

- Metric for diversity of image features/signature in a spot

### `In situ`-based

##### Transcript detection

- Codeword - on and signal per image
- Custom designing probes to detect certain parts of transcript

##### Differences between platform

- [Review of technologies, Wang et al. 2023](https://www.sciencedirect.com/science/article/pii/S0888754323001155#f0005)
- Probe design
- Presence of amplification step
  + Merscope - no amplification so there's a tissue clearing step to better see signal
  + Xenium
    - Rolling amplification step to amplify signal, so no need for tissue clearing
    - Amplification might remove gradient expression of genes compared to merscope
  + CosMx - has amplification step
  
##### Cell segmentation

- Better to have cell boundary staining for cell segmentation along with DAPI nuclei staining
- If cell boundary staining not present, nuclei expansion is done but can be problematic. Watch out for biases such that cells segmented to be large can occur in areas where there are not many neighbouring cells around (low cell density areas)
- Xenium Ranger which processes images to get to cell x gene matrix is usually run already but you may want to rerun if you need to adjust parameters like nuclei boundary parameter
- For areas where cell segmentation is bad, can use molecule/transcript coordinates (cell segmentation independent) to see if there are patterns like colocalisation of certain transcripts indicating cell boundaries

- Alternative segmentation - transcript-based segmentation

##### Using scRNAseq reference

- Reanalyse atlas using only the features available with Xenium data to see if Xenium features can separate the cell types you expect, if not maybe change combination of features to separate some cell types

##### Neighbourhood analysis

- Detect changes of particular cell type depending on the type of their neighbours

