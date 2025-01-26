---
title: "01_load"
format: html
editor: visual
---

## Load the data


```r
# URL to download the file from
url <- URLencode("https://datadryad.org/stash/downloads/file_stream/3153128")

# Destination directory and file
raw_dir <- "../_raw"
raw_file <- file.path(raw_dir, 
                      "raw_data.xlsx")

# Check if the directory exists, create it if not
if (!dir.exists(raw_dir)) {
  dir.create(raw_dir, 
             recursive = TRUE) 
}

# Use download.file to fetch the file
download.file(url, 
              raw_file, 
              method = "curl", 
              extra = "-L --user-agent 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'")
```

## Create TSV


```r
# Load required package
library(readxl)

# Read the Excel file
xlsx_file <- "../_raw/raw_data.xlsx" 

data_tsv <- read_excel(xlsx_file)

# Save the data as a TSV file
clean_dir <- "../data"
tsv_file <- file.path(clean_dir, 
                      "01_dat_load.tsv")

# Check if the directory exists, create it if not
if (!dir.exists(clean_dir)) {
  dir.create(clean_dir, 
             recursive = TRUE) 
}

# write tsv data
write.table(data_tsv, 
            file = tsv_file, 
            sep = "\t", 
            row.names = FALSE)
```
