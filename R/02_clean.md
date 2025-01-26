---
title: "Project"
author: "Miguel Blanco, Xavi Chapatte, Luis Martín"
format: html
editor: visual
---

### Load libraries


```r
library(tidyverse)
library(readxl)
library(stringr)
library(cowplot)
```

### Load Data


```r
data <- read_tsv("../data/01_dat_load.tsv")
```

### Manage nulls


```r
#Create a function to replace different types of 'null values' to one common NA value on the dataset
NA_values_replace <- function(data, na_vector){
  data |> 
    mutate(across(where(is.character), ~ case_when 
          (. %in% na_vector ~ NA,
          TRUE ~ .
          )))
}

data <- NA_values_replace(data = data, 
                          na_vector = c("[null]", 
                                        "/", 
                                        "-"))
```

### Proportions of NA in each column:

Remove all the columns with more 70% of nulls


```r
#The function takes the data as only argument and return the proportion of NA values of each column in the data 
NA_prop <- function(data) {
  data |> 
    summarise(across(everything(), 
                     ~ mean(is.na(.)), 
                     .names = "{col}")) |> 
    pivot_longer(everything(), 
                 values_to = "na_proportion", 
                 names_to = "column")
}

data_NA_proportion <- NA_prop(data)

columns_to_keep <- data_NA_proportion |> 
  filter(na_proportion <= 0.7) |>  
  pull(column)                     

data <- data |> 
  select(all_of(columns_to_keep))
```

### Rename some columns


```r
data <- data |> 
  rename(
    "Basic_diseases" = "Basic diseases (1 hypertension, 2 diabetes, 3 cardiovascular diseases, 4 cerebrovascular diseases, 5 COPD, 6 immunodeficiency (such as AIDS, hormone, immunosuppressant use history), 7 malignant tumor, 8 other 9 chronic kidney disease",
    "Age range" = "Age ranges[years]",
    "Fever days" = "Fever days (D1-D7)[days]",
    "Affected lung lobes 1" = "Number of affected lung lobes 1",
    "Affected lung lobes 2" = "Number of affected lung lobes 2",
    "Imaging Improvement>25% (after 1 week)" = "Does the imaging improve by more than 25% after one week (1 yes 2 no)",
    "APACHE Score (ICU Admission)" = "APACHEII score (admission to ICU)",
    "Usage days" = "Usage days[days]",
    "Hormone usage days" = "Hormone usage days[days]",
    "Anticoagulant days" = "Anticoagulant days[days]",
    "Death" = "Death (1 Yes 2 No)",
    "Symptoms" = "Symptoms (1. Whole body, 2. Respiratory, 3. Nervous, 4. Cardiovascular, 5. Gastrointestinal, 6. Other)",
    "Antiviral Drug" = "Antiviral drugs (1 nema, 2 azides, 3 others)",
    "Administration method" = "Administration method (1 oral and 2 nasal feeding)",
    "Hormones" = "Hormones (1 methylprednisolone, 2 dexamethasone, 3 others)",
    "Hospitalization days" = "Hospitalization time[days]",
    "ICU hospitalization days" = "ICU hospitalization time[days]",
    "Prone position ventilation" = "Prone position ventilation (1 yes 0 no)",
    "Mechanical ventilation days" = "Mechanical ventilation time (days)",
    "D1_oxygen_therapy_mode"
 = "D1 oxygen therapy mode (1. Mask 2. High flow oxygen therapy 3. Non invasive 4. Invasive 5. ECMO)",
 "Antibiotics" = "Antibiotics (1 Yes 2 No)",
 "Immunotherapy" = "Immunotherapy (1 Tor 2 Bar)",
 "Medication days" = "Medication- D1[days]",
 "Negative conversion" = "Negative conversion(1, Yes; 0,No)",
 "Bleeding events" = "Bleeding events (0 none, 1 digestive tract, 2 respiratory tract, 3 skin, 4 others)"
 )
```

### Eliminate Spaces

Most of the columns of our data frame have names separated by spaces " ", than can bring problems when calling them after. To avoid that, we are going to replace all the spaces in the colnames with "\_"


```r
data <- data |> 
  #rename_with renames the colnames, use gsub to substitute " " with "_" in all 
  #the columns (.)
  rename_with(~ gsub(" ", "_", .))
```

### Eliminate comments written after numerical values and round large decimal values


```r
data <- data |>  
  mutate(`ICU_hospitalization_days` = as.numeric(gsub("[^0-9\\.]", "", `ICU_hospitalization_days`))) |> 
  mutate(`Hormone_dosage[mg]` = as.numeric(gsub("[^0-9\\.]", "", `Hormone_dosage[mg]`))) |> 
  mutate(`NRL[%]` = round(as.numeric(`NRL[%]`), 2)) |> 
  mutate(`Bleeding_events` = as.numeric(gsub("[^0-9,\\.]", "", `Bleeding_events`)))
```

```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `Bleeding_events = as.numeric(gsub("[^0-9,\\.]", "", Bleeding_events))`.
## Caused by warning:
## ! NAs introduced by coercion
```

Some column-specific modifications


```r
#remove and 'Unknown' value for an NA value in Usage_days column
data <- data |> 
  mutate(Usage_days = na_if(Usage_days, "unknown"))

#Modify the Hormone_usage_days column 
data <- data |> 
  #convert to NA if the value of column hormone is also NA
  mutate(Hormone_usage_days = if_else(is.na(Hormones), NA, Hormone_usage_days)) |> 
  #Change two dirty non-numerical values to numerical values
  mutate(
  Hormone_usage_days = if_else(
    Hormone_usage_days == '4 (40mg)+3 (20mg)+4 (10mg)',
    "11",  # Convertir a carácter
    Hormone_usage_days
  )) |> 
  mutate(
  Hormone_usage_days = if_else(
    Hormone_usage_days == '6+',
    "6",  # Convertir a carácter
    Hormone_usage_days
  )) |> 
  #Unify the annotation of text value that are express long treatment in different ways to one the same 'long treatment' values
    mutate(
    Hormone_usage_days = if_else(
      grepl("[a-zA-Z]", Hormone_usage_days), 
      "long treatment",                     
      Hormone_usage_days                    
    )
  )
```

Save Clean data


```r
# Save the data as a TSV file
clean_dir <- "../data"
tsv_file <- file.path(clean_dir, "02_dat_clean.tsv")

# Check if the directory exists, create it if not
if (!dir.exists(clean_dir)) {
  dir.create(clean_dir, recursive = TRUE) 
}
#write tsv data
write.table(data, file = tsv_file, sep = "\t", row.names = FALSE)
```
