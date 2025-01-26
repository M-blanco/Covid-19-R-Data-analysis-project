---
title: "04_describe"
format: html
editor: visual
---

### Load augmented data


```r
library(table1)
data <- read_tsv(file = "../data/03_data_augmented.tsv")
```

```
## Rows: 316 Columns: 125
## ── Column specification ───────────────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: "\t"
## chr (35): Age_range, Fever_days, Imaging_Improvement>25%_(after_1_week), IL-6[pg/mL], Oxygenation_index_D1[mmHg], Lactic_acid_D...
## dbl (90): Number, Age_mean, Age_over_65(1,YES;_2,_NO), Charlson_index[score], Disease_onset_D1_highest_body_temperature[℃], D3_...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

### Rename column

Rename the Death variable to Outcome and Survived/Deceased value for better visualization of the tables


```r
data <- data|>
  rename(
    Outcome = 'Death',
  ) |>
  mutate(
    Outcome = case_when(
      Outcome == 0 ~ 'Survived',
      Outcome == 1 ~'Deceased')
  )
```

### Change from binary to No, Yes and the labels in the tables


```r
tabledat <- data |>
  mutate(
      Hypertension_disease = factor(Hypertension_disease, labels = c("No", "Yes")),
    Diabetes_disease = factor(Diabetes_disease, labels = c("No", "Yes")),
    CVD_disease = factor(CVD_disease, labels = c("No", "Yes")),
    COPD_disease = factor(COPD_disease, labels = c("No", "Yes")),
    Cerebrovascular_disease = factor(Cerebrovascular_disease, labels = c("No", "Yes")),
    Chronic_Kidney_disease = factor(Chronic_Kidney_disease, labels = c("No", "Yes")),
    Malignant_tumor_disease = factor(Malignant_tumor_disease, labels = c("No", "Yes")),
    Immunodeficiency_disease = factor(Immunodeficiency_disease, labels = c("No", "Yes")),
    Other_disease = factor(Other_disease, labels = c("No", "Yes")),
    Whole_body_symptoms = factor(Whole_body_symptoms, labels = c("No", "Yes")),
    Respiratory_symptoms = factor(Respiratory_symptoms, labels = c("No", "Yes")),
    Cardiovascular_symptoms = factor(Cardiovascular_symptoms, labels = c("No", "Yes")),
    Other_symptoms = factor(Other_symptoms, labels = c("No", "Yes")),
    Nervous_symptoms = factor(Nervous_symptoms, labels = c("No", "Yes")),
    Gastrointestinal_symptoms = factor(Gastrointestinal_symptoms, labels = c("No", "Yes")),
    Mask_Oxygen_therapy = factor(Mask_Oxygen_therapy, labels = c("No", "Yes")),
    NonInvasive_Oxygen_therapy = factor(NonInvasive_Oxygen_therapy, labels = c("No", "Yes")),
    Invasive_Oxygen_therapy = factor(Invasive_Oxygen_therapy, labels = c("No", "Yes")),
    HighFlow_Oxygen_therapy = factor(HighFlow_Oxygen_therapy, labels = c("No", "Yes"))
  )

label(tabledat[["Hypertension_disease"]]) <- "Hypertension"
label(tabledat[["Diabetes_disease"]]) <- "Diabetes"
label(tabledat[["CVD_disease"]]) <- "Cardiovascular Disease (CVD)"
label(tabledat[["COPD_disease"]]) <- "Chronic Obstructive Pulmonary Disease (COPD)"
label(tabledat[["Cerebrovascular_disease"]]) <- "Cerebrovascular Disease"
label(tabledat[["Chronic_Kidney_disease"]]) <- "Chronic Kidney Disease"
label(tabledat[["Malignant_tumor_disease"]]) <- "Malignant Tumor"
label(tabledat[["Immunodeficiency_disease"]]) <- "Immunodeficiency"
label(tabledat[["Other_disease"]]) <- "Other Disease"
label(tabledat[["Whole_body_symptoms"]]) <- "Whole Body Symptoms"
label(tabledat[["Respiratory_symptoms"]]) <- "Respiratory Symptoms"
label(tabledat[["Cardiovascular_symptoms"]]) <- "Cardiovascular Symptoms"
label(tabledat[["Other_symptoms"]]) <- "Other Symptoms"
label(tabledat[["Nervous_symptoms"]]) <- "Nervous Symptoms"
label(tabledat[["Gastrointestinal_symptoms"]]) <- "Gastrointestinal Symptoms"
label(tabledat[["Age_range"]]) <- "Age Group (Years)"
label(tabledat[["Hospitalization_days"]]) <- "Hospitalization Time (Days)"
label(tabledat[["ICU_hospitalization_days"]]) <- "ICU Hospitalization Time (Days)"
label(tabledat[["Fever_days"]]) <- "Fever Duration (Days)"

label(tabledat[["Antiviral_Drug"]]) <- "Antiviral Drug"
label(tabledat[["CTSS_score_1"]]) <- "CTSS score 1"
label(tabledat[["CTSS_score_2"]]) <- "CTSS score 2"
label(tabledat[["Charlson_index[score]"]]) <- "Charlson index score"
label(tabledat[["Mask_Oxygen_therapy"]]) <- "Mask Oxygen Therapy"
label(tabledat[["NonInvasive_Oxygen_therapy"]]) <- "Non-Invasive Oxygen Therapy"
label(tabledat[["Invasive_Oxygen_therapy"]]) <- "Invasive Oxygen Therapy"
label(tabledat[["HighFlow_Oxygen_therapy"]]) <- "High Flow Oxygen Therapy"
```

### Exclude NA values from tables


```r
tabledat <- tabledat |> 
  mutate(
    Hypertension_disease = factor(Hypertension_disease, exclude = NULL),
    Diabetes_disease = factor(Diabetes_disease, exclude = NULL),
    CVD_disease = factor(CVD_disease, exclude = NULL),
    COPD_disease = factor(COPD_disease, exclude = NULL),
    Cerebrovascular_disease = factor(Cerebrovascular_disease, exclude = NULL),
    Malignant_tumor_disease = factor(Malignant_tumor_disease, exclude = NULL),
    Immunodeficiency_disease = factor(Immunodeficiency_disease, exclude = NULL),
    Other_disease = factor(Other_disease, exclude = NULL),
    Whole_body_symptoms = factor(Whole_body_symptoms, exclude = NULL),
    Respiratory_symptoms = factor(Respiratory_symptoms, exclude = NULL),
    Cardiovascular_symptoms = factor(Cardiovascular_symptoms, exclude = NULL),
    Other_symptoms = factor(Other_symptoms, exclude = NULL),
    Nervous_symptoms = factor(Nervous_symptoms, exclude = NULL),
    Gastrointestinal_symptoms = factor(Gastrointestinal_symptoms, exclude = NULL),
    Chronic_Kidney_disease = factor(Chronic_Kidney_disease, exclude = NULL),
    ICU_hospitalization_days = factor(ICU_hospitalization_days, exclude = NULL),
    ICU_hospitalization_days = as.numeric(ICU_hospitalization_days),
    Antiviral_Drug = factor(Antiviral_Drug, exclude = NULL),
    CTSS_score_1 = factor(CTSS_score_1, exclude = NULL),
    CTSS_score_1 = as.numeric(CTSS_score_1),
    CTSS_score_2 = factor(CTSS_score_2, exclude = NULL),
    CTSS_score_2 = as.numeric(CTSS_score_2),
    Fever_days = factor(Fever_days, exclude = NULL),
    Fever_days = as.numeric(Fever_days),
    `Charlson_index[score]` = factor(`Charlson_index[score]`, exclude = NULL),
    `Charlson_index[score]` = as.numeric(`Charlson_index[score]`),
    Mask_Oxygen_therapy = factor(Mask_Oxygen_therapy, exclude = NULL),
    NonInvasive_Oxygen_therapy = factor(NonInvasive_Oxygen_therapy, exclude = NULL),
    Invasive_Oxygen_therapy = factor(Invasive_Oxygen_therapy, exclude = NULL),
    HighFlow_Oxygen_therapy = factor(HighFlow_Oxygen_therapy, exclude = NULL)
  )
```

### Age group, Hospitalization time and ICU hospitalization days by outcome


```r
table1_describe_01 <- table1(
  x = ~ Age_range + Hospitalization_days + ICU_hospitalization_days | Outcome,
  data = tabledat,
  overall = FALSE
)

output_dir <- "../results"  # Define the relative path to the results folder
output_file <- file.path(output_dir, "table1_describe_01.html")  # Construct the full file path
writeLines(as.character(table1_describe_01), output_file)  # Save the table1 object as HTML
```

### Diseases by outcome


```r
table1_describe_02 <- table1(
  x = ~ Hypertension_disease + Diabetes_disease + CVD_disease + COPD_disease + Cerebrovascular_disease + Chronic_Kidney_disease + Malignant_tumor_disease + Immunodeficiency_disease + Other_disease | Outcome, 
  data = tabledat,
  overall = FALSE
)

output_dir <- "../results"  
output_file <- file.path(output_dir, "table1_describe_02.html")  
writeLines(as.character(table1_describe_02), output_file)  
```

### Symptoms by outcome


```r
table1_describe_03 <- table1(
  x = ~  Whole_body_symptoms + Respiratory_symptoms + Cardiovascular_symptoms + Other_symptoms + Nervous_symptoms + Gastrointestinal_symptoms| Outcome,
  data = tabledat,
  overall = FALSE
)

output_dir <- "../results"  
output_file <- file.path(output_dir, "table1_describe_03.html")  
writeLines(as.character(table1_describe_03), output_file)  
```

### Antiviral drug, CTSS, Charlson index score, fever days and onset vs outcome


```r
table1_describe_04 <- table1(
  x = ~  Antiviral_Drug + CTSS_score_1 + CTSS_score_2 + Fever_days + `Charlson_index[score]` + onset| Outcome, 
  data = tabledat,
  overall = FALSE
)

output_dir <- "../results"  
output_file <- file.path(output_dir, "table1_describe_04.html")  
writeLines(as.character(table1_describe_04), output_file)  
```

### Age group, Symptoms and Diseases by Antiviral Drug


```r
table1(
   x = ~ Age_range + Hypertension_disease + Diabetes_disease + CVD_disease + COPD_disease + Cerebrovascular_disease + Chronic_Kidney_disease + Malignant_tumor_disease + Immunodeficiency_disease + Other_disease + Whole_body_symptoms + Respiratory_symptoms + Cardiovascular_symptoms + Other_symptoms + Nervous_symptoms + Gastrointestinal_symptoms| Antiviral_Drug,
  data = tabledat
)
```

<!--html_preserve--><div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Azduvine<br><span class='stratn'>(N=38)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>None<br><span class='stratn'>(N=54)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Paxlovid<br><span class='stratn'>(N=196)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>NA<br><span class='stratn'>(N=28)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=316)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>Age Group (Years)</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'><60</td>
<td>2 (5.3%)</td>
<td>6 (11.1%)</td>
<td>16 (8.2%)</td>
<td>2 (7.1%)</td>
<td>26 (8.2%)</td>
</tr>
<tr>
<td class='rowlabel'>>90</td>
<td>2 (5.3%)</td>
<td>8 (14.8%)</td>
<td>24 (12.2%)</td>
<td>6 (21.4%)</td>
<td>40 (12.7%)</td>
</tr>
<tr>
<td class='rowlabel'>60-70</td>
<td>12 (31.6%)</td>
<td>8 (14.8%)</td>
<td>28 (14.3%)</td>
<td>8 (28.6%)</td>
<td>56 (17.7%)</td>
</tr>
<tr>
<td class='rowlabel'>70-80</td>
<td>10 (26.3%)</td>
<td>6 (11.1%)</td>
<td>46 (23.5%)</td>
<td>4 (14.3%)</td>
<td>66 (20.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>80-90</td>
<td class='lastrow'>12 (31.6%)</td>
<td class='lastrow'>26 (48.1%)</td>
<td class='lastrow'>82 (41.8%)</td>
<td class='lastrow'>8 (28.6%)</td>
<td class='lastrow'>128 (40.5%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Hypertension_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>12 (31.6%)</td>
<td>10 (18.5%)</td>
<td>72 (36.7%)</td>
<td>4 (14.3%)</td>
<td>98 (31.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>26 (68.4%)</td>
<td class='lastrow'>42 (77.8%)</td>
<td class='lastrow'>110 (56.1%)</td>
<td class='lastrow'>20 (71.4%)</td>
<td class='lastrow'>198 (62.7%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Diabetes_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>24 (63.2%)</td>
<td>36 (66.7%)</td>
<td>120 (61.2%)</td>
<td>12 (42.9%)</td>
<td>192 (60.8%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>14 (36.8%)</td>
<td class='lastrow'>16 (29.6%)</td>
<td class='lastrow'>62 (31.6%)</td>
<td class='lastrow'>12 (42.9%)</td>
<td class='lastrow'>104 (32.9%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>CVD_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>26 (68.4%)</td>
<td>32 (59.3%)</td>
<td>104 (53.1%)</td>
<td>10 (35.7%)</td>
<td>172 (54.4%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>12 (31.6%)</td>
<td class='lastrow'>20 (37.0%)</td>
<td class='lastrow'>78 (39.8%)</td>
<td class='lastrow'>14 (50.0%)</td>
<td class='lastrow'>124 (39.2%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>COPD_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>30 (78.9%)</td>
<td>46 (85.2%)</td>
<td>166 (84.7%)</td>
<td>20 (71.4%)</td>
<td>262 (82.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>8 (21.1%)</td>
<td class='lastrow'>6 (11.1%)</td>
<td class='lastrow'>16 (8.2%)</td>
<td class='lastrow'>4 (14.3%)</td>
<td class='lastrow'>34 (10.8%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Cerebrovascular_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>26 (68.4%)</td>
<td>42 (77.8%)</td>
<td>148 (75.5%)</td>
<td>20 (71.4%)</td>
<td>236 (74.7%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>12 (31.6%)</td>
<td class='lastrow'>10 (18.5%)</td>
<td class='lastrow'>34 (17.3%)</td>
<td class='lastrow'>4 (14.3%)</td>
<td class='lastrow'>60 (19.0%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Chronic_Kidney_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>34 (89.5%)</td>
<td>38 (70.4%)</td>
<td>164 (83.7%)</td>
<td>16 (57.1%)</td>
<td>252 (79.7%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>4 (10.5%)</td>
<td class='lastrow'>14 (25.9%)</td>
<td class='lastrow'>18 (9.2%)</td>
<td class='lastrow'>8 (28.6%)</td>
<td class='lastrow'>44 (13.9%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Malignant_tumor_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>32 (84.2%)</td>
<td>46 (85.2%)</td>
<td>160 (81.6%)</td>
<td>18 (64.3%)</td>
<td>256 (81.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>6 (15.8%)</td>
<td class='lastrow'>6 (11.1%)</td>
<td class='lastrow'>22 (11.2%)</td>
<td class='lastrow'>6 (21.4%)</td>
<td class='lastrow'>40 (12.7%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Immunodeficiency_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>32 (84.2%)</td>
<td>48 (88.9%)</td>
<td>168 (85.7%)</td>
<td>22 (78.6%)</td>
<td>270 (85.4%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>6 (15.8%)</td>
<td class='lastrow'>4 (7.4%)</td>
<td class='lastrow'>14 (7.1%)</td>
<td class='lastrow'>2 (7.1%)</td>
<td class='lastrow'>26 (8.2%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Other_disease</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>30 (78.9%)</td>
<td>42 (77.8%)</td>
<td>130 (66.3%)</td>
<td>22 (78.6%)</td>
<td>224 (70.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>8 (21.1%)</td>
<td class='lastrow'>10 (18.5%)</td>
<td class='lastrow'>52 (26.5%)</td>
<td class='lastrow'>2 (7.1%)</td>
<td class='lastrow'>72 (22.8%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Whole_body_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>2 (5.3%)</td>
<td>6 (11.1%)</td>
<td>20 (10.2%)</td>
<td>4 (14.3%)</td>
<td>32 (10.1%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>36 (94.7%)</td>
<td class='lastrow'>46 (85.2%)</td>
<td class='lastrow'>174 (88.8%)</td>
<td class='lastrow'>24 (85.7%)</td>
<td class='lastrow'>280 (88.6%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Respiratory_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>4 (10.5%)</td>
<td>2 (3.7%)</td>
<td>20 (10.2%)</td>
<td>2 (7.1%)</td>
<td>28 (8.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>34 (89.5%)</td>
<td class='lastrow'>50 (92.6%)</td>
<td class='lastrow'>174 (88.8%)</td>
<td class='lastrow'>26 (92.9%)</td>
<td class='lastrow'>284 (89.9%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Cardiovascular_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>36 (94.7%)</td>
<td>50 (92.6%)</td>
<td>176 (89.8%)</td>
<td>24 (85.7%)</td>
<td>286 (90.5%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>2 (5.3%)</td>
<td class='lastrow'>2 (3.7%)</td>
<td class='lastrow'>18 (9.2%)</td>
<td class='lastrow'>4 (14.3%)</td>
<td class='lastrow'>26 (8.2%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Other_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>38 (100%)</td>
<td>48 (88.9%)</td>
<td>190 (96.9%)</td>
<td>26 (92.9%)</td>
<td>302 (95.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>0 (0%)</td>
<td class='lastrow'>4 (7.4%)</td>
<td class='lastrow'>4 (2.0%)</td>
<td class='lastrow'>2 (7.1%)</td>
<td class='lastrow'>10 (3.2%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Nervous_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>34 (89.5%)</td>
<td>38 (70.4%)</td>
<td>154 (78.6%)</td>
<td>22 (78.6%)</td>
<td>248 (78.5%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>4 (10.5%)</td>
<td class='lastrow'>14 (25.9%)</td>
<td class='lastrow'>40 (20.4%)</td>
<td class='lastrow'>6 (21.4%)</td>
<td class='lastrow'>64 (20.3%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Gastrointestinal_symptoms</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>No</td>
<td>36 (94.7%)</td>
<td>50 (92.6%)</td>
<td>172 (87.8%)</td>
<td>24 (85.7%)</td>
<td>282 (89.2%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>2 (5.3%)</td>
<td class='lastrow'>2 (3.7%)</td>
<td class='lastrow'>22 (11.2%)</td>
<td class='lastrow'>4 (14.3%)</td>
<td class='lastrow'>30 (9.5%)</td>
</tr>
</tbody>
</table>
</div><!--/html_preserve-->
