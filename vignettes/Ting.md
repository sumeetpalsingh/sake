# Test case1: Ting
Yu-Jui Ho and Toby Aicher  
`r Sys.Date()`  

# Data Input 

There are three different ways you can upload files into `sake` pacakge. It can either be 

- Rawdata file 
- Preloaded data
- Saved run result

## Rawdata file

The gene count data file is one of the most common types of data format. Each row represents expression value from a gene, and each column represents transcriptome library derived from a single sample. The package requires the first row to be the header, which you can provide information about each sample. The first column is required to be names/ID for each gene/transcript. 

You can also specify the delimiter according to different file type.

- Tab (\\t) - Default setting for `.txt` or `.out` file
- Comma (,) - Usually used by `.csv` file
- Semicolon (;) - Not so common, but some use this type

Example data sets should look like this

Gene    |MEF-1    |MEF-10   |MEF-11   |MEF-12   |MEF-2
--------|---------|---------|---------|---------|-----
Gm15772 |1493.562 |1714.470 |1178.217 |1858.733 |1199.904
Dnajc3  |75.209   |67.320   |291.554  |49.924   |166.867
Mdn1    |29.288   |7.819    |82.620   |1.262    |0.214
Mfap1b  |4.796    |1.335    |4.308    |0.000    |0.748
Zglp1   |1.939    |78.381   |3.385    |0.541    |3.205
Gm12359 |1.225    |13.159   |1.846    |0.000    |0.320
Gm16039 |0.408    |2.861    |0.154    |0.360    |56.406
Gm11149 |0.204    |0.000    |0.000    |0.000    |0.000

## Preloaded data

There are several preloaded gene expression data from published single-cell studies that were already been preloaded with the package. They include studies relate to neuronal differentiation^[Treutlein *et al*, Dissecting direct reprogramming from fibroblast to neuron using single-cell RNA-seq, Nature, 2016] and circulating tumor cells in pancreatic cancer^[Ting *et al*, Single-Cell RNA Sequencing Identifies Extracellular Matrix Gene Expression by Pancreatic Circulating Tumor Cells, Cell Reports, 2014]. The user can sift through and test each module using these data.    

In this case, we will select data published from Ting *et al*. 

<img src="Figures/Ting/preload.png" width="800px" height="450px" />


A successfully loaded data will look like this  

<img src="Figures/Ting/loaded_file.png" width="800px" height="450px" />

***

# Quality Control

You can normalize or transform your raw data after succesfully uploading it. We provide two simple data metrics to help identify samples that deviate from the majority of samples, which may indicate potential low-quality samples that can be removed before downstream analysis.

## Normalization 

Normalization allows you to compare read counts between samples and detect differentially expressed genes by accounting for technical variability between samples and adjusting for their sequencing depth (library size). Two methods to normalize scRNA-seq data are provided.

- **Reads per millions (RPM) normalization**: normalize the gene count from each sample according to their total read count. 

- **DESeq-like normalization**: noramlize the gene count using a method introduced in the [DESeq](http://bioconductor.org/packages/release/bioc/html/DESeq.html) package.

- **Upper quartile normalization**: noramlize the gene count based on upper quartile value from each library

<img src="Figures/Normalization.png" width="700px" height="450px" />

## Transformation 

Transformation allows you to take into account the presence of extreme values in scRNA-seq data and to adjust for mean variance dependency (the observation that genes with higher expression often have larger variances across samples). Two methods to transform scRNA-Seq data are provided.

- **Variance stablizing transformation (VST)**: transform the gene count using a method intoduced in the DEseq package.

- **Log transformation**: transform the gene count by log2(count+1)

<img src="Figures/Transformation.png" width="700px" height="450px" />

## Filtering 
After data normalization or transformation, there are two simple data metrics to help identify samples that deviate from the majority of samples, which can be selected and removed before downstream analysis.

### Read distribution

Read distribution summerizes the total number of read counts for each sample. The read count distribution should be as uniform as possible, and the normalization method you choose will affect the result. Distinctively low read counts may indicate RNA degradation or low sequencing efficiency.

### Gene coverage

Gene coverage summarizes the total number of genes with at least one read in each sample. This number should be relatively stable across libraries. Low gene coverage in a sample can indicate poor quality of a single-cell library. However, the number of expressed genes may be altered based on the difference between cell types, experimental conditions or sequencing protocals, intrinsic heterogeneity among cell population, among other reasons. User should be aware of these factors and decide how to treat outlier samples for downstream analysis.

### 

An example QC plot displaying read distribution (x-axis) and gene coverage (y-axis) will be used for identifying potential problematic samples. 

<img src="Figures/QC_plot.png" width="700px" height="450px" />

Samples with relatively low total transcript counts and gene coverage rates usually represent degraded or poorly amplified libraries. These can be identified visually and removed from the sample set before proceeding with downstream analyses.
User will be able to use a selection box to highlight samples in the left bottom corner. Samples within the selection box will be shown on the top right table. User can then inspect each of them and click on the ones they want to remove from downstream analysis. Selected samples will be displayed on the bottom right, and user can hit `Submit` button when they are ready to discard these samples. 

In this case, we remove 3 samples `ESC_13, ESC_28`, and `ESC_3` for downstream analysis. 