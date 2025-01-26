---
title: "03_augment"
format: html
editor: visual
---

## Load clean data


```r
data <- read_tsv(file = "../data/02_dat_clean.tsv", show_col_types = FALSE)
```

### Change the ages ranges from 5-years to 10-years


```r
data <- data |> 
  mutate(Age_mean = Age_range)

data <- data |> 
    mutate(`Age_range` = case_when(
    str_detect(`Age_range`, "^[0-5][0-9]-") ~ "<60",
    str_detect(`Age_range`, "^6") ~ "60-70",
    str_detect(`Age_range`, "^7") ~ "70-80",
    str_detect(`Age_range`, "^8") ~ "80-90",
    str_detect(`Age_range`, "^9") ~ ">90",
    str_detect(`Age_range`, "^100") ~ ">90"
  )) |> 
  mutate(`Age_range` = fct_relevel(`Age_range`, "<60", "60-70", "70-80", "80-90", ">90"))
```

### Create function to change dirty columns with multiple values to clean binary columns


```r
process_dirty_cols <- function(data, column, rename_map) {
  column_sym <- ensym(column)
  
  data_processed <- data |> 
    mutate(
      !!column_sym := str_replace_all(!!column_sym, "([^a-zA-Z()]*).*", "\\1")
    ) |>  
    # create rows for each diferent value in the column separated by the sig ',' or '+'
    separate_rows({{ column }}, sep = "[,+]") |> 
    # make sure values are numerical, asigning 0 if not
    mutate({{ column }} := as.numeric({{ column }})) |> 
    # expand the rows to columns with binary indicators
    pivot_wider(
      names_from = {{ column }}, 
      names_prefix = "temp_prefix_", #temporal prefix to identify the new created columns
      values_from = {{ column }}, 
      values_fn = ~1, 
      values_fill = 0
    )
  
  # if 'temp_prefix_NA', write NA values on the all the columns for that row
  if ("temp_prefix_NA" %in% names(data_processed)) {
    data_processed <- data_processed |> 
      mutate(across(starts_with("temp_prefix_"), ~if_else(temp_prefix_NA == 1, NA, .))) |> 
      select(-any_of(c("temp_prefix_NA", "temp_prefix_0")))
  } else {
    # if 'temp_prefix_NA' doesn't exist, remove 'temp_prefix_0'
    data_processed <- data_processed |> 
      select(-any_of(c("temp_prefix_0")))
  }
  
  # Renname the columns with the rename map
  data_processed <- data_processed |> 
    rename_with(
      ~gsub(paste0("^", "temp_prefix_"), "", .),  # take out the temporal prefix 'temp_prefix'
      starts_with("temp_prefix_")
    ) |> 
    rename_with(~ case_when(
      . %in% names(rename_map) ~ rename_map[.],
      TRUE ~ .
    ), everything())
  
  return(data_processed)
}
```

### Change the dirty columns with the previous function


```r
#Basic_disease column
rename_map <- c(
  "1" = "Hypertension_disease",
  "2" = "Diabetes_disease",
  "3" = "CVD_disease",
  "4" = "Cerebrovascular_disease",
  "5" = "COPD_disease",
  "6" = "Immunodeficiency_disease",
  "7" = "Malignant_tumor_disease",
  "8" = "Other_disease",
  "9" = "Chronic_Kidney_disease"
)

data <- process_dirty_cols(data, Basic_diseases, rename_map)

#Symptoms column
rename_map <- c(
  "1" = "Whole_body_symptoms",
  "2" = "Respiratory_symptoms",
  "3" = "Nervous_symptoms",
  "4" = "Cardiovascular_symptoms",
  "5" = "Gastrointestinal_symptoms",
  "6" = "Other_symptoms"
)

data <- process_dirty_cols(data, Symptoms, rename_map)

#D1_oxygen_therapy_mode
rename_map <- c(
  "1" = "Mask_Oxygen_therapy",
  "2" = "NonInvasive_Oxygen_therapy",
  "3" = "HighFlow_Oxygen_therapy",
  "4" = "Invasive_Oxygen_therapy",
  "5" = "ECMO"
)

data <- process_dirty_cols(data, D1_oxygen_therapy_mode, rename_map)

#Bleeding_events
rename_map <- c(
  "0" = "Bleeding_event_none",
  "1" = "Digestive_track",
  "2" = "Bleeding_event_Respiratory_track",
  "3" = "Bleeding_event_Skin",
  "4" = "Bleeding_event_Other"
  )

data <- process_dirty_cols(data, Bleeding_events, rename_map)
```

### Change numeric values for words values


```r
data <- data |> 
  mutate(`Antiviral_Drug` = case_when(
    `Antiviral_Drug` == 0 ~ "None",
    `Antiviral_Drug` == 1 ~ "Paxlovid",
    `Antiviral_Drug` == 2 ~ "Azduvine",
    `Antiviral_Drug` == 3 ~ "Other",
    TRUE ~ NA_character_
  )) |> 
    mutate(`Administration_method` = case_when(
    `Administration_method` == 1 ~ "Oral feeding",
    `Administration_method` == 2 ~ "Nasal Feeding",
    TRUE ~ NA_character_
  )) |>
    mutate(`Hormones` = case_when(
    `Hormones` == 1 ~ "Methylprednisolone",
    `Hormones` == 2 ~ "Dexamethasone",
    `Hormones` == 3 ~ "Other",
    TRUE ~ NA_character_
  )) |>
  mutate(`Immunotherapy` = case_when(
    `Immunotherapy` == 1 ~ "Tocilizumab",
    `Immunotherapy` == 2 ~ "Baricitinib",
    `Immunotherapy` == 0 ~ "None",
    TRUE ~ NA_character_
  ))
```

### Pulishing binary values (0-1, 1-2 and yes-no)


```r
data <- data |> 
  mutate(`Death` = case_when( 
    `Death` == 1 ~ "1",
    `Death` == 2 ~ "0",
    TRUE ~ NA_character_
  )) |>
  #mutate(`Death` = as.factor(`Death`, levels = c("1", "0"))) |> 
  mutate(`Prone_position_ventilation` = case_when(
    `Prone_position_ventilation` == 1 ~ "yes",
    `Prone_position_ventilation` == 0 ~ "no",
    TRUE ~ NA_character_
  )) |> 
  mutate(`Antibiotics` = case_when(
    `Antibiotics` == 1 ~ "yes",
    `Antibiotics` == 2 ~ "no",
    TRUE ~ NA_character_
  )) |> 
  mutate(`Imaging_Improvement>25%_(after_1_week)` = case_when(
    `Imaging_Improvement>25%_(after_1_week)` == 1 ~ "yes",
    `Imaging_Improvement>25%_(after_1_week)` == 2 ~ "no",
    TRUE ~ NA_character_
  )) |> 
  mutate(`Negative_conversion` = case_when(
    `Negative_conversion` == 1 ~ "yes",
    `Negative_conversion` == 0 ~ "no",
    TRUE ~ NA_character_
  ))
```

### Create Early/Late onset group based on D1 Fever


```r
data <- data |> 
  mutate(onset = case_when(
    `Disease_onset_D1_highest_body_temperature[℃]` > 37.5 ~ "Early",
    TRUE ~ "Late"
  ))
```

### Sum of Basic_Diseases


```r
data <- data |> 
  mutate(Comorbidities_sum = rowSums(select(data, Hypertension_disease : Other_disease)))
```

### Age mean column

Create a new column called 'Age_mean' with the mean values for every range of age group, so this numeric number can be used for later analysis


```r
data <- data |> 
  mutate(Age_mean = case_when(
    str_detect(Age_mean, "^20") ~ "22.5",
    str_detect(Age_mean, "^25") ~ "27.5",
    str_detect(Age_mean, "^30") ~ "32.5",
    str_detect(Age_mean, "^35") ~ "37.5",
    str_detect(Age_mean, "^40") ~ "42.5",
    str_detect(Age_mean, "^45") ~ "47.5",
    str_detect(Age_mean, "^50") ~ "52.5",
    str_detect(Age_mean, "^55") ~ "57.5",
    str_detect(Age_mean, "^60") ~ "62.5",
    str_detect(Age_mean, "^65") ~ "67.5",
    str_detect(Age_mean, "^70") ~ "72.5",
    str_detect(Age_mean, "^75") ~ "77.5",
    str_detect(Age_mean, "^80") ~ "82.5",
    str_detect(Age_mean, "^85") ~ "87.5",
    str_detect(Age_mean, "^90") ~ "92.5",
    str_detect(Age_mean, "^95") ~ "97.5",
    str_detect(Age_mean, "^100") ~ "102.5"
  )) |> 
  relocate(Age_mean, .after = Age_range)
```

### CT Progression and Lung Lobes Augmentation


```r
data <- data|>
  rename(
    CT_D1_days_1 = `CT1-D1[days]`,
    CT_D1_days_2 = `CT2-D1[days]`
  ) |>
  mutate(
    `Charlson_index[score]` = as.numeric(`Charlson_index[score]`),
    CTSS_score_1 = as.numeric(CTSS_score_1),
    CTSS_score_2 = as.numeric(CTSS_score_2),
    CT_D1_days_1 = as.numeric(CT_D1_days_1),
    CT_D1_days_2 = as.numeric(CT_D1_days_2),
    Affected_lung_lobes_1 = as.numeric(Affected_lung_lobes_1),
    Affected_lung_lobes_2 = as.numeric(Affected_lung_lobes_2),
  )
```

### Early Onset Temperature Augmentation


```r
data |>
  mutate(
       `Disease_onset_D1_highest_body_temperature[℃]` = as.numeric(`Disease_onset_D1_highest_body_temperature[℃]`))
```

### ICU ADMISSION (BINARY)


```r
data <- data |> 
  mutate(ICU_admission = case_when(
    ICU_hospitalization_days == 0 ~ "0",
    TRUE ~ "1"
  )) |> 
  relocate(ICU_admission, .after = ICU_hospitalization_days)
```

### Set death as a Categorical variable


```r
data <- data |> 
  mutate(Death = as.factor(Death))
```

### Creating variable Lymphocyte_Test_Day y Lymphocyte


```r
data <- data |>
  mutate(
    `D1_lymphocyte_copy` = `D1_lymphocyte_count[10^9/L]`,
    `D7_lymphocyte_copy` = `D7_lymphocyte_count[10^9/L]`) |>
  pivot_longer(cols = c(`D1_lymphocyte_copy`, `D7_lymphocyte_copy`), names_to = "Lymphocyte_Test_Day", values_to = "Lymphocytes") 
```

### Save files


```r
# Save the data as a TSV file
clean_dir <- "../data"
tsv_file <- file.path(clean_dir, "03_data_augmented.tsv")

# Check if the directory exists, create it if not
if (!dir.exists(clean_dir)) {
  dir.create(clean_dir, recursive = TRUE) 
}
#write tsv data
write.table(data, file = tsv_file, sep = "\t", row.names = FALSE)
```

### 
