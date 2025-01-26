---
title: "04_describe"
format: html
editor: visual
---

### Load augmented data

::: {.cell}

:::

### Rename column

Rename the Death variable to Outcome and Survived/Deceased value for better visualization of the tables

::: {.cell}

:::

### Change from binary to No, Yes and the labels in the tables

::: {.cell}

:::

### Exclude NA values from tables

::: {.cell}

:::

### Age group, Hospitalization time and ICU hospitalization days by outcome

::: {.cell}

:::

### Diseases by outcome

::: {.cell}

:::

### Symptoms by outcome

::: {.cell}

:::

### Antiviral drug, CTSS, Charlson index score, fever days and onset vs outcome

::: {.cell}

:::

### Age group, Symptoms and Diseases by Antiviral Drug

::: {.cell}
::: {.cell-output-display}

```{=html}
<div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Azduvine<br><span class='stratn'>(N=19)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>None<br><span class='stratn'>(N=27)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Paxlovid<br><span class='stratn'>(N=98)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>NA<br><span class='stratn'>(N=14)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=158)</span></span></th>
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
<td>1 (5.3%)</td>
<td>3 (11.1%)</td>
<td>8 (8.2%)</td>
<td>1 (7.1%)</td>
<td>13 (8.2%)</td>
</tr>
<tr>
<td class='rowlabel'>>90</td>
<td>1 (5.3%)</td>
<td>4 (14.8%)</td>
<td>12 (12.2%)</td>
<td>3 (21.4%)</td>
<td>20 (12.7%)</td>
</tr>
<tr>
<td class='rowlabel'>60-70</td>
<td>6 (31.6%)</td>
<td>4 (14.8%)</td>
<td>14 (14.3%)</td>
<td>4 (28.6%)</td>
<td>28 (17.7%)</td>
</tr>
<tr>
<td class='rowlabel'>70-80</td>
<td>5 (26.3%)</td>
<td>3 (11.1%)</td>
<td>23 (23.5%)</td>
<td>2 (14.3%)</td>
<td>33 (20.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>80-90</td>
<td class='lastrow'>6 (31.6%)</td>
<td class='lastrow'>13 (48.1%)</td>
<td class='lastrow'>41 (41.8%)</td>
<td class='lastrow'>4 (28.6%)</td>
<td class='lastrow'>64 (40.5%)</td>
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
<td>6 (31.6%)</td>
<td>5 (18.5%)</td>
<td>36 (36.7%)</td>
<td>2 (14.3%)</td>
<td>49 (31.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>13 (68.4%)</td>
<td class='lastrow'>21 (77.8%)</td>
<td class='lastrow'>55 (56.1%)</td>
<td class='lastrow'>10 (71.4%)</td>
<td class='lastrow'>99 (62.7%)</td>
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
<td>12 (63.2%)</td>
<td>18 (66.7%)</td>
<td>60 (61.2%)</td>
<td>6 (42.9%)</td>
<td>96 (60.8%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>7 (36.8%)</td>
<td class='lastrow'>8 (29.6%)</td>
<td class='lastrow'>31 (31.6%)</td>
<td class='lastrow'>6 (42.9%)</td>
<td class='lastrow'>52 (32.9%)</td>
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
<td>13 (68.4%)</td>
<td>16 (59.3%)</td>
<td>52 (53.1%)</td>
<td>5 (35.7%)</td>
<td>86 (54.4%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>6 (31.6%)</td>
<td class='lastrow'>10 (37.0%)</td>
<td class='lastrow'>39 (39.8%)</td>
<td class='lastrow'>7 (50.0%)</td>
<td class='lastrow'>62 (39.2%)</td>
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
<td>15 (78.9%)</td>
<td>23 (85.2%)</td>
<td>83 (84.7%)</td>
<td>10 (71.4%)</td>
<td>131 (82.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>4 (21.1%)</td>
<td class='lastrow'>3 (11.1%)</td>
<td class='lastrow'>8 (8.2%)</td>
<td class='lastrow'>2 (14.3%)</td>
<td class='lastrow'>17 (10.8%)</td>
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
<td>13 (68.4%)</td>
<td>21 (77.8%)</td>
<td>74 (75.5%)</td>
<td>10 (71.4%)</td>
<td>118 (74.7%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>6 (31.6%)</td>
<td class='lastrow'>5 (18.5%)</td>
<td class='lastrow'>17 (17.3%)</td>
<td class='lastrow'>2 (14.3%)</td>
<td class='lastrow'>30 (19.0%)</td>
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
<td>17 (89.5%)</td>
<td>19 (70.4%)</td>
<td>82 (83.7%)</td>
<td>8 (57.1%)</td>
<td>126 (79.7%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>2 (10.5%)</td>
<td class='lastrow'>7 (25.9%)</td>
<td class='lastrow'>9 (9.2%)</td>
<td class='lastrow'>4 (28.6%)</td>
<td class='lastrow'>22 (13.9%)</td>
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
<td>16 (84.2%)</td>
<td>23 (85.2%)</td>
<td>80 (81.6%)</td>
<td>9 (64.3%)</td>
<td>128 (81.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>3 (15.8%)</td>
<td class='lastrow'>3 (11.1%)</td>
<td class='lastrow'>11 (11.2%)</td>
<td class='lastrow'>3 (21.4%)</td>
<td class='lastrow'>20 (12.7%)</td>
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
<td>16 (84.2%)</td>
<td>24 (88.9%)</td>
<td>84 (85.7%)</td>
<td>11 (78.6%)</td>
<td>135 (85.4%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>3 (15.8%)</td>
<td class='lastrow'>2 (7.4%)</td>
<td class='lastrow'>7 (7.1%)</td>
<td class='lastrow'>1 (7.1%)</td>
<td class='lastrow'>13 (8.2%)</td>
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
<td>15 (78.9%)</td>
<td>21 (77.8%)</td>
<td>65 (66.3%)</td>
<td>11 (78.6%)</td>
<td>112 (70.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>4 (21.1%)</td>
<td class='lastrow'>5 (18.5%)</td>
<td class='lastrow'>26 (26.5%)</td>
<td class='lastrow'>1 (7.1%)</td>
<td class='lastrow'>36 (22.8%)</td>
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
<td>1 (5.3%)</td>
<td>3 (11.1%)</td>
<td>10 (10.2%)</td>
<td>2 (14.3%)</td>
<td>16 (10.1%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>18 (94.7%)</td>
<td class='lastrow'>23 (85.2%)</td>
<td class='lastrow'>87 (88.8%)</td>
<td class='lastrow'>12 (85.7%)</td>
<td class='lastrow'>140 (88.6%)</td>
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
<td>2 (10.5%)</td>
<td>1 (3.7%)</td>
<td>10 (10.2%)</td>
<td>1 (7.1%)</td>
<td>14 (8.9%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>17 (89.5%)</td>
<td class='lastrow'>25 (92.6%)</td>
<td class='lastrow'>87 (88.8%)</td>
<td class='lastrow'>13 (92.9%)</td>
<td class='lastrow'>142 (89.9%)</td>
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
<td>18 (94.7%)</td>
<td>25 (92.6%)</td>
<td>88 (89.8%)</td>
<td>12 (85.7%)</td>
<td>143 (90.5%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>1 (5.3%)</td>
<td class='lastrow'>1 (3.7%)</td>
<td class='lastrow'>9 (9.2%)</td>
<td class='lastrow'>2 (14.3%)</td>
<td class='lastrow'>13 (8.2%)</td>
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
<td>19 (100%)</td>
<td>24 (88.9%)</td>
<td>95 (96.9%)</td>
<td>13 (92.9%)</td>
<td>151 (95.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>0 (0%)</td>
<td class='lastrow'>2 (7.4%)</td>
<td class='lastrow'>2 (2.0%)</td>
<td class='lastrow'>1 (7.1%)</td>
<td class='lastrow'>5 (3.2%)</td>
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
<td>17 (89.5%)</td>
<td>19 (70.4%)</td>
<td>77 (78.6%)</td>
<td>11 (78.6%)</td>
<td>124 (78.5%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>2 (10.5%)</td>
<td class='lastrow'>7 (25.9%)</td>
<td class='lastrow'>20 (20.4%)</td>
<td class='lastrow'>3 (21.4%)</td>
<td class='lastrow'>32 (20.3%)</td>
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
<td>18 (94.7%)</td>
<td>25 (92.6%)</td>
<td>86 (87.8%)</td>
<td>12 (85.7%)</td>
<td>141 (89.2%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Yes</td>
<td class='lastrow'>1 (5.3%)</td>
<td class='lastrow'>1 (3.7%)</td>
<td class='lastrow'>11 (11.2%)</td>
<td class='lastrow'>2 (14.3%)</td>
<td class='lastrow'>15 (9.5%)</td>
</tr>
</tbody>
</table>
</div>
```

:::
:::
