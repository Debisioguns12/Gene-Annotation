# Gene-Annotation
This code is used for Gene Annotation Using Gallo Package in R
setwd("/Users/ogunbawoadebisi/Downloads/meta_analyse")
# Load necessary libraries
library(GALLO)
library(readxl)
library(tidyverse)
library(openxlsx)



if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install("rtracklayer")
library(rtracklayer)

# Gene Annotation
genes.inp <- import_gff_gtf(db_file="Bos_taurus.ARS-UCD1.3.111.gtf", file_type="gtf")

## With interval
# Read Excel file containing data
DATA <- read_xlsx("Reproduction_MA.xlsx")
# Find genes around markers based on a marker file
genes <- find_genes_qtls_around_markers(db_file = genes.inp, # Gene annotation file
                                        marker_file = DATA, # File with regions
                                        method = "gene", # Method of finding genes
                                        marker = "snp", # Type of marker: SNP or haplotype
                                        interval = 500000) # Interval

# Display the first few rows of the resulting genes
head(genes)

# Write the genes data to an Excel file
write.xlsx(genes, "genes_ra.xlsx")

