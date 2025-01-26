---
title: "05_analysis_03"
author: "Miguel Blanco"
format: html
editor: visual
---

## Statistical analysis of Paxlovid effect on Lymphocytes


```r
data <- read_tsv(file = "../data/03_data_augmented.tsv", show_col_types = FALSE)
```

### 1. Check normality


```r
data |>
  pull(`D7_lymphocyte_count[10^9/L]`) |> 
  shapiro.test()
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  pull(data, `D7_lymphocyte_count[10^9/L]`)
## W = 0.27212, p-value < 2.2e-16
```

Visualization of distribution


```r
Lymp_dist <- ggplot(data, aes(x = `D7_lymphocyte_count[10^9/L]`)) +
  geom_histogram(binwidth = 0.5, fill = "skyblue", color = "black") +
  labs(title = "Distribution of Day 7 Lymphocyte Levels [10^9/L]", x = "Lymphocyte Levels", y = "Count") +
  xlim(0,4)+
  theme_minimal()+
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold")
  )

ggsave("../results/05_Lymp_distribution.png", plot = Lymp_dist, 
       width = 10, height = 6, dpi = 300)
```

```
## Warning: Removed 24 rows containing non-finite outside the scale range (`stat_bin()`).
```

```
## Warning: Removed 2 rows containing missing values or values outside the scale range (`geom_bar()`).
```

### 2. Wilcox-test and summary stats

Summary statistics


```r
lymph_stats <- data |> 
  group_by(`Antiviral_drugs_(1_nema_0_no)`) |> 
  summarise(
    median_lymphocytes_D7 = median(`D7_lymphocyte_count[10^9/L]`, na.rm = TRUE),
    mean_lymphocytes_D7 = mean(`D7_lymphocyte_count[10^9/L]`, na.rm = TRUE),
    count = n(),
    median_lymphocytes_D1 = median(`D1_lymphocyte_count[10^9/L]`, na.rm = TRUE),
    mean_lymphocytes_D1 = mean(`D1_lymphocyte_count[10^9/L]`, na.rm = TRUE),
    count = n()
  )
```

Wilcox test


```r
wilcox_res <- wilcox.test(
  `D7_lymphocyte_count[10^9/L]` ~ `Antiviral_drugs_(1_nema_0_no)`,
  data = data
)

print(wilcox_res)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  D7_lymphocyte_count[10^9/L] by Antiviral_drugs_(1_nema_0_no)
## W = 13406, p-value = 8.082e-06
## alternative hypothesis: true location shift is not equal to 0
```


```r
p1 <- ggplot(data = data,
       mapping = aes(x = `Antiviral_drugs_(1_nema_0_no)`,
                     y = `D1_lymphocyte_count[10^9/L]`)) +
  geom_boxplot()+
  ylim(0,2)+
  theme_minimal()

p2 <- ggplot(data = data |> filter(`D7_lymphocyte_count[10^9/L]` < 15),
       mapping = aes(x = `Antiviral_drugs_(1_nema_0_no)`,
                     y = `D7_lymphocyte_count[10^9/L]`)) +
  ylim(0,2)+
  geom_boxplot()+
  theme_minimal()

p1+p2
```

```
## Warning: Continuous x aesthetic
## ℹ did you forget `aes(group = ...)`?
```

```
## Warning: Removed 8 rows containing non-finite outside the scale range (`stat_boxplot()`).
```

```
## Warning: Continuous x aesthetic
## ℹ did you forget `aes(group = ...)`?
```

```
## Warning: Removed 10 rows containing non-finite outside the scale range (`stat_boxplot()`).
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)


```r
data <- data |>  
  mutate(`Antiviral_drugs_(1_nema_0_no)` = factor(
    `Antiviral_drugs_(1_nema_0_no)`, 
    levels = c(0,1),
    labels = c("No", "Yes"))) |>
  mutate(Death = factor(Death, levels = c(0, 1), labels = c("No", "Yes")))
```


```r
  p1 <- ggplot(data |> filter(Lymphocyte_Test_Day == "D7_lymphocyte_copy"), aes(x = Lymphocyte_Test_Day, y = Lymphocytes, color = `Antiviral_drugs_(1_nema_0_no)`)) +
  geom_boxplot( alpha = 0.8) + 
  scale_color_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069")
  ) +
  labs(
    title = "Lymphocyte Counts on Day 7",
    x = "Lymphocyte_Test_Day",
    y = "Lymphocyte Count [10^9/L]",
    color = "Paxlovid Consumption",
  ) +
  #scale_y_continuous(limits = c(0, 3)) + 
  scale_x_discrete(labels = "D7_LC") +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 14),
    axis.text = element_text(size = 10),
    axis.title = element_text(size = 12),
    legend.position = "top" 
)

p2 <- ggplot(data |> filter(Lymphocyte_Test_Day == "D7_lymphocyte_copy"), aes(x = Lymphocyte_Test_Day, y = Lymphocytes, color = `Antiviral_drugs_(1_nema_0_no)`)) +
  geom_boxplot(outlier.shape = NA, alpha = 0.8) + 
  scale_color_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069")
  ) +
  labs(
    title = "Lymphocyte Counts on Day 7",
    x = "Lymphocyte_Test_Day",
    y = "Lymphocyte Count [10^9/L]",
    color = "Paxlovid Consumption",
  ) +
  scale_y_continuous(limits = c(0, 3)) + 
  scale_x_discrete(labels = "D7_LC") +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 14),
    axis.text = element_text(size = 10),
    axis.title = element_text(size = 12),
    legend.position = "top"
  ) 

Lymp_wilcox <- (p1 + p2)
ggsave("../results/05_Lymp_wilcox.png", plot = Lymp_wilcox, 
       width = 10, height = 6, dpi = 300)
```

```
## Warning: Removed 10 rows containing non-finite outside the scale range (`stat_boxplot()`).
```

```
## Warning: Removed 13 rows containing non-finite outside the scale range (`stat_boxplot()`).
```

Muerte con limfocitos graphs


```r
wilcox_res_death <- wilcox.test(
  `Lymphocytes` ~ `Death`,
  data = data
)

print(wilcox_res)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  D7_lymphocyte_count[10^9/L] by Antiviral_drugs_(1_nema_0_no)
## W = 13406, p-value = 8.082e-06
## alternative hypothesis: true location shift is not equal to 0
```


```r
p1 <- ggplot(data, aes(x = Lymphocyte_Test_Day, y = Lymphocytes, color = Death)) +
  geom_boxplot(aes(fill = Death), alpha = 0.8) + # Boxplots
  scale_fill_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069"), # Fill colors for Death
    labels = c("Yes" = "Deceased", "No" = "Survived") # Update legend labels
  ) +
  scale_color_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069"), # Color mapping for Death
    labels = c("Yes" = "Deceased", "No" = "Survived") # Update legend labels
  ) +
  labs(
    title = NULL, # Elimina el título individual
    x = "Lymphocyte_Test_Day",
    y = "Lymphocyte Count [10^9/L]",
    color = "Outcome",
    fill = "Outcome"
  ) +
  scale_x_discrete(labels = c("D1_LC", "D7_LC")) +
  theme_minimal() +
  theme(
    axis.text = element_text(size = 10),
    axis.title = element_text(size = 12),
    legend.position = "none", # La leyenda será combinada globalmente
    strip.text = element_blank(), # Elimina los textos de las facetas ("Yes" y "No")
    plot.margin = margin(t = 10, b = 20, l = 10, r = 10) # Ajusta el margen
  ) +
  facet_wrap(~Death, scales = "free") # Facet by Death status

# Gráfico 2 (p2)
p2 <- ggplot(data, aes(x = Lymphocyte_Test_Day, y = Lymphocytes, color = Death)) +
  geom_boxplot(aes(fill = Death), outlier.shape = NA, alpha = 0.8) + # Boxplots
  scale_fill_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069"), # Fill colors for Death
    labels = c("Yes" = "Deceased", "No" = "Survived") # Update legend labels
  ) +
  scale_color_manual(
    values = c("Yes" = "#5995ED", "No" = "#DB3069"), # Color mapping for Death
    labels = c("Yes" = "Deceased", "No" = "Survived") # Update legend labels
  ) +
  labs(
    title = NULL, # Elimina el título individual
    x = "Lymphocyte_Test_Day",
    y = "Lymphocyte Count [10^9/L]",
    color = "Outcome",
    fill = "Outcome"
  ) +
  scale_y_continuous(limits = c(0, 3)) + # Ajusta los límites del eje Y
  scale_x_discrete(labels = c("D1_LC", "D7_LC")) +
  theme_minimal() +
  theme(
    axis.text = element_text(size = 10),
    axis.title = element_text(size = 12),
    legend.position = "none", # La leyenda será combinada globalmente
    strip.text = element_blank(), # Elimina los textos de las facetas ("Yes" y "No")
    plot.margin = margin(t = 20, b = 10, l = 10, r = 10) # Ajusta el margen
  ) +
  facet_wrap(~Death, scales = "free") # Facet by Death status

# Combinar los gráficos con un título compartido y leyenda debajo del título
Lymp_death <- (p1 + p2) +
  plot_layout(guides = "collect") + # Combina las leyendas
  plot_annotation(
    title = "Lymphocyte Counts on Day 1 and Day 7", # Título compartido
    theme = theme(
      plot.title = element_text(
        hjust = 0.5, 
        size = 16, 
        margin = margin(b = 5) # Reduce el margen inferior del título
      )
    )
  ) &
  theme(
    legend.position = "top", # Ubica la leyenda arriba
    legend.justification = "center", # Centra la leyenda
    legend.margin = margin(t = 0, b = 5) # Ajusta el margen superior e inferior de la leyenda
  )

Lymp_death <- (p1 + p2)
ggsave("../results/05_Lymp_wilcox.png", plot = Lymp_death, 
       width = 10, height = 6, dpi = 300)
```

```
## Warning: Removed 10 rows containing non-finite outside the scale range (`stat_boxplot()`).
```

```
## Warning: Removed 14 rows containing non-finite outside the scale range (`stat_boxplot()`).
```
