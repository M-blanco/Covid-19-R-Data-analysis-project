# group_05_project

This is an academic project for _22160 R for Bio Data Science_ course in Technical University of Denmark.

# Project Contributors

s243302, luismrtnn,

s243547, nataliafecedg,

s243360, Xavi-Ch,

s242872, danielgc99,

s243284, mblanco387

# Presentation Link

Click the following link to access the presentation:

[Presentation](https://raw.githack.com/rforbiodatascience24/group_05_project/main/doc/presentation.html#/title-slide)

# Data Retrieval

The quarto file R/01_load.qmd loads the dataset _Clinical data of COVID-19 infected patients_ used for this project. The dataset in _.xlsx_ format, as well as the _README.md_ file, information about the authors, the date of publications and information about the date original study can be found at https://doi.org/10.5061/dryad.m63xsj49r. 

# Our Project and Code

Original data were acquired from patients with severe/critical COVID-19 diagnosed between December 2022 and January 2023 at a medical center in China. This data contains information about the electronic medical records of patients, demographic characteristics mainly include age range, the basic diseases and initial symptions of patients, temperature, fever days, pulmonary CT condition and antiviral drugs among other variables (more explained on the original README.md file from the original data repository). This data is cleaned (_02_clean.qmd_) for unifying _NA_ values, removing columns with more than 70% _NAs_, modifying the column names and som other column-specific changes. Then, the data is augmented (_03_augment.qmd_) to change dirty columns to binary-values columns, to change (some) numeric values to more descriptive values, polish binary values and create new variables. A brief description of the dataset structure can be found on the _04_describe.qmd_.

Our aim in this project is, firstly, to analyze the data and try to find which of the different measured variables is more related to the outcome of the disease. This is done firstly by creating visualizations that either confirm or deny our initial hypothesis (_05_analysis_01.qmd_). Then, an statistically approach is followed, looking for the highest correlation between all our variables and the outcome (_05_analysis_02.qmd_). 
The author of the papers and th edata repository concllude that after doing a Cox-regression analysis, Paxlovid is associated with a significantly reduced risk of death and improved lung imaging findings. The last part of our analysis (_05_analysis_03.qmd_) is dedicated to see if we can get the same conclusion as the original paper regarding to the efficacy of paxlovid doing other type of analysis. 
