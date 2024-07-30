CCGP T. rubescens Genome Note Plots
================

#### R Markdown

## BUSCO and RepeatMasker Plots

``` r
# Load necessary libraries
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggplot2)
library(dplyr)

# Create a data frame with the necessary columns, this one contains all non-rounded values
species_table <- data.frame(
  Species = c("A. amphitrite", "B. glandula", "B. nubilus", "C. hameri", 
              "L. anatifera", "P. pollicipes", "S. balanoides", 
              "S. cariosus", "T. rubescens"),
  Core_genes_detected_complete = c(951, 426, 465, 813, 925, 917, 569, 935, 937),
  Core_genes_detected_complete_partial = c(980, 801, 835, 901, 965, 950, 733, 969, 971),
  Missing_core_genes = c(33, 212, 178, 112, 48, 63, 280, 44, 42),
  Sequences = c(2643, 239901, 206513, 20340, 2276, 1255, 16596, 2552, 6513),
  Total_length_nt = c(807604242, 582475369, 562916451, 1336202063, 1044949769, 770104822, 
                      486009102, 1215039272, 2441681606),
  N50_sequence_length_nt = c(536785, 3341, 3958, 80858, 1099682, 47009503, 56726, 2640000, 106952858),
  L50_sequence_count_nt = c(424, 51041, 41470, 4662, 268, 8, 1896, 1896, 10),
  GC_content = c(49.76, 47.10, 47.05, 49.10, 46.86, 52.34, 47.84, 46.5, 46.13),
  genome_size_pg = c(0.74, 1.4, 1.4, 1.23, 1.46, 0.9, 1.4, 1.4, 2.6),
  total_interspersed_repeats = c(47.03, 46.00, 46.00, 46.00, 35.00, 30.59, 45.26, 45.26, 60.86),
  retroelements = c(6.31, 5.00, 5.00, 5.00, 4.00, 4.05, 3.30, 3.30, 4.65),
  DNA_transposons = c(2.27, 2.00, 2.00, 2.00, 2.00, 2.45, 1.50, 1.50, 3.03),
  rolling_circles = c(0.70, 0.60, 0.60, 0.60, 0.11, 0.11, 0.51, 0.51, 1.08)
)

write.csv(species_table, file = "/Users/bailey/Documents/research/CCGP/results/species_table.csv")

# Create a dataframe with the necessary columns
species_data <- data.frame(
  Species = c("A. amphitrite", "B. glandula", "B. nubilus", "C. hameri", 
              "L. anatifera", "P. pollicipes", "S. balanoides", 
              "S. cariosus", "T. rubescens"),
  Core_genes_detected_complete = c(93.88, 42.05, 45.90, 80.26, 91.31, 90.52, 56.17, 92.30, 92.50),
  Core_genes_detected_complete_partial = c(96.74, 79.07, 82.43, 88.94, 95.26, 93.78, 72.36, 95.7, 95.85),
  Core_genes_detected_partial = c(2.86, 37.02, 36.53, 8.68, 3.95, 3.26, 16.19, 3.40, 3.35),
  Missing_core_genes = c(3.26, 20.93, 17.57, 11.06, 4.74, 6.22, 27.64, 4.3, 4.15),
  Sequences_in_thousands = c(3, 240, 210, 20, 2, 1, 17, 3, 7),
  Total_length_Gb = c(0.81, 0.58, 0.56, 1.34, 1.04, 0.77, 0.49, 1.22, 2.44),
  N50_sequence_length_Mb = c(0.54, 0.01, 0.01, 0.08, 1.10, 47.01, 0.06, 2.64, 106.95),
  L50_sequence_count_in_thousands = c(0.4, 51, 41, 5, 0.3, 8, 2, 0.1, 0.01),
#  GC_content = c(49.76, 47.10, 47.05, 49.10, 46.86, 52.34, 47.84, 46.52, 46.13),
  genome_size_pg = c(0.74, 1.40, 1.40, 1.23, 1.46, 0.90, 1.40, 1.40, 2.60),
  retroelements = c(6.31, 5.00, 5.00, 5.00, 4.00, 4.05, 3.30, 3.30, 3.98),
  DNA_transposons = c(2.27, 2.00, 2.00, 2.00, 2.00, 2.45, 1.50, 1.50, 2.17),
  rolling_circles = c(0.70, 0.60, 0.60, 0.60, 0.11, 0.11, 0.51, 0.51, 0.19),
  unclassified_repeats = c(47.03, 46.00, 46.00, 46.00, 35.00, 30.59, 45.26, 45.26, 62.23),
  simple_repeats = c(47.03, 46.00, 46.00, 46.00, 35.00, 30.59, 45.26, 45.26, 2.37),
  low_complexity_repeats = c(47.03, 46.00, 46.00, 46.00, 35.00, 30.59, 45.26, 45.26, 0.05),
  not_TE = c(47.03, 46.00, 46.00, 46.00, 35.00, 30.59, 45.26, 45.26, 29.01)
)

# Print the data frame to ensure it's correct
print(species_data)
```

    ##         Species Core_genes_detected_complete
    ## 1 A. amphitrite                        93.88
    ## 2   B. glandula                        42.05
    ## 3    B. nubilus                        45.90
    ## 4     C. hameri                        80.26
    ## 5  L. anatifera                        91.31
    ## 6 P. pollicipes                        90.52
    ## 7 S. balanoides                        56.17
    ## 8   S. cariosus                        92.30
    ## 9  T. rubescens                        92.50
    ##   Core_genes_detected_complete_partial Core_genes_detected_partial
    ## 1                                96.74                        2.86
    ## 2                                79.07                       37.02
    ## 3                                82.43                       36.53
    ## 4                                88.94                        8.68
    ## 5                                95.26                        3.95
    ## 6                                93.78                        3.26
    ## 7                                72.36                       16.19
    ## 8                                95.70                        3.40
    ## 9                                95.85                        3.35
    ##   Missing_core_genes Sequences_in_thousands Total_length_Gb
    ## 1               3.26                      3            0.81
    ## 2              20.93                    240            0.58
    ## 3              17.57                    210            0.56
    ## 4              11.06                     20            1.34
    ## 5               4.74                      2            1.04
    ## 6               6.22                      1            0.77
    ## 7              27.64                     17            0.49
    ## 8               4.30                      3            1.22
    ## 9               4.15                      7            2.44
    ##   N50_sequence_length_Mb L50_sequence_count_in_thousands genome_size_pg
    ## 1                   0.54                            0.40           0.74
    ## 2                   0.01                           51.00           1.40
    ## 3                   0.01                           41.00           1.40
    ## 4                   0.08                            5.00           1.23
    ## 5                   1.10                            0.30           1.46
    ## 6                  47.01                            8.00           0.90
    ## 7                   0.06                            2.00           1.40
    ## 8                   2.64                            0.10           1.40
    ## 9                 106.95                            0.01           2.60
    ##   retroelements DNA_transposons rolling_circles unclassified_repeats
    ## 1          6.31            2.27            0.70                47.03
    ## 2          5.00            2.00            0.60                46.00
    ## 3          5.00            2.00            0.60                46.00
    ## 4          5.00            2.00            0.60                46.00
    ## 5          4.00            2.00            0.11                35.00
    ## 6          4.05            2.45            0.11                30.59
    ## 7          3.30            1.50            0.51                45.26
    ## 8          3.30            1.50            0.51                45.26
    ## 9          3.98            2.17            0.19                62.23
    ##   simple_repeats low_complexity_repeats not_TE
    ## 1          47.03                  47.03  47.03
    ## 2          46.00                  46.00  46.00
    ## 3          46.00                  46.00  46.00
    ## 4          46.00                  46.00  46.00
    ## 5          35.00                  35.00  35.00
    ## 6          30.59                  30.59  30.59
    ## 7          45.26                  45.26  45.26
    ## 8          45.26                  45.26  45.26
    ## 9           2.37                   0.05  29.01

``` r
# Transform data frame for facet_wrap purposes
species_data_long <- species_data %>%
  pivot_longer(cols = -Species, names_to = "Attribute", values_to = "Value")

# Order details by how you want them to appear in plot and legend
species_data_long$Attribute <- factor(species_data_long$Attribute, levels = c("Total_length_Gb", "genome_size_pg", "Sequences_in_thousands", "N50_sequence_length_Mb", "Core_genes_detected_complete", "Core_genes_detected_complete_partial", "Core_genes_detected_partial", "Missing_core_genes", "L50_sequence_count_in_thousands", "retroelements", "DNA_transposons", "rolling_circles", "unclassified_repeats", "simple_repeats", "low_complexity_repeats", "not_TE"))

# Order species appropriately
species_data_long$Species <- factor(species_data_long$Species, levels = c("A. amphitrite", "B. glandula", "B. nubilus", "S. balanoides", "S. cariosus", "C. hameri", "T. rubescens", "L. anatifera", "P. pollicipes"))

# Create multiplot
BUSCO_TE_plots <- ggplot(species_data_long, aes(x = Species, y = Value, fill = Species)) +
  geom_bar(stat = "identity") +
  labs(x = "Species",
       y = "Value") +
  facet_wrap(.~Attribute, nrow = 3, strip.position = 'left', scales = "free_y", labeller = as_labeller(c(
    Total_length_Gb="Assembly Length (Gb)", 
    genome_size_pg="Genome Size (pg)", 
    Sequences_in_thousands="Total Sequences (thousands)", 
    N50_sequence_length_Mb="N50 Length (Mb)", 
    Core_genes_detected_complete="Complete Genes (%)", 
    Core_genes_detected_complete_partial="Complete+Partial Genes (%)", 
    Missing_core_genes="Missing Genes (%)", 
    L50_sequence_count_in_thousands="L50 Count (thousands)", 
    retroelements="Retroelements (%)", 
    DNA_transposons="DNA Transposons (%)", 
    rolling_circles="Rolling Circles (%)",
    unclassified_repeats="Unclassified Repeats (%)", 
    simple_repeats="Simple Repeats (%)", 
    low_complexity_repeats="Low Complexity Repeats (%)", 
    not_TE="Not Transposable Elements (%)"))) +
  ylab(NULL) +
  paletteer::scale_colour_paletteer_d("ggprism::autumn_leaves") +
  paletteer::scale_fill_paletteer_d("ggprism::autumn_leaves") +
  theme_bw() +
  theme(legend.position = "none",
        axis.title.x = element_blank(),
        axis.text.x = element_text(size = 14, face = "italic", angle = 90, hjust = 1, vjust = 0.45, color = "black"),
        axis.text.y = element_text(size = 14, color = "black"),
        text = element_text(size = 14, color = "black"),
        plot.background = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.border = element_blank(),
        axis.line = element_line(color = "black"),
        strip.background = element_blank(),
        strip.placement='outside',
        strip.text.y = element_text(size = 14, color = "black"))
BUSCO_TE_plots
```

![](T.rubescens_genome_note_plots_files/figure-gfm/Create%20plots-1.png)<!-- -->

``` r
ggsave("/Users/bailey/Documents/research/CCGP/figures/BUSCO_TE_plots.png", BUSCO_TE_plots, width = 12, height = 9, units = "in")

# Genome size and length
genome <- species_data[,c(1,7,10)]

# Order species appropriately
genome$Species <- factor(genome$Species, levels = c("A. amphitrite", "B. glandula", "B. nubilus", "S. balanoides", "S. cariosus", "C. hameri", "T. rubescens", "L. anatifera", "P. pollicipes"))

# Create custom colors for each species
custom_colors <- c("#244E57FF", "#6DCFCFFF", "#2D9C9CFF","#659A32FF", "#99CE64FF", "#4F5F28FF","#A73933FF", "#894D9FFF", "#BD6DC2FF")

# Create genome plot
genome_plot <- ggplot(data = genome, 
                      aes(x = Total_length_Gb, y = genome_size_pg, color = Species, fill = Species)) + 
  geom_point(size = 7) + 
  geom_abline(intercept = 0, slope = 1, color = "orange", linetype = "dashed") +
  scale_color_manual(values = custom_colors) +
  scale_fill_manual(values = custom_colors) +
  theme(text = element_text(size = 14), 
        legend.position = "right", 
        legend.text = element_text(size = 14, face = "italic"),
        axis.text = element_text(size = 14, color = "black"),
        axis.line = element_line(color = "black"),
        panel.background = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.border = element_blank()) + 
  scale_x_continuous(breaks = c(0.5,1,1.5,2,2.5), limits = c(0,2.75)) +
  scale_y_continuous(breaks = c(0.5,1,1.5,2,2.5), limits = c(0,2.75)) +
  labs(x = "Total Sequence Length (Gb)", y = "Genome Size (pg)")
genome_plot
```

![](T.rubescens_genome_note_plots_files/figure-gfm/Create%20plots-2.png)<!-- -->

``` r
ggsave("/Users/bailey/Documents/research/CCGP/figures/genome_plot.png", genome_plot, width = 6, height = 4, units = "in")

# Genes recovered and not recovered from each genome
genes_long <- species_data[,c(1:2,4:5)] %>%
  pivot_longer(cols = -Species, names_to = "Attribute", values_to = "Value")

# Order genes to determine order in barplot
genes_long$Attribute <- factor(genes_long$Attribute, levels = c("Missing_core_genes", "Core_genes_detected_partial", "Core_genes_detected_complete"))

# Order species appropriately
genes_long$Species <- factor(genes_long$Species, levels = c("A. amphitrite", "B. glandula", "B. nubilus", "S. balanoides", "S. cariosus", "C. hameri", "T. rubescens", "L. anatifera", "P. pollicipes"))

# Create custom legend labels
custom_labels <- c(
  "Core_genes_detected_complete" = "Complete",
  "Core_genes_detected_partial" = "Partial",
  "Missing_core_genes" = "Missing"
)

# Create gene plot
genes_plot <- ggplot(data = genes_long, mapping = aes(x = Species, y = Value, fill = Attribute)) + 
  geom_bar(stat = "identity", position = "stack") +
  scale_fill_manual(values = c("Core_genes_detected_complete" = "#6C568CFF", 
                               "Core_genes_detected_partial" = "#BFCDD9FF", 
                               "Missing_core_genes" = "#607345FF"),
  labels = custom_labels) +  # Change the legend labels
  theme_bw() +
  theme(text = element_text(size = 14), 
    legend.title = element_text(size = 14, angle = 90),
    legend.text = element_text(size = 14, angle = 90),
    axis.text.x = element_blank(),
    axis.text.y = element_text(size = 14, color = "black", angle = 90, hjust = 1, vjust = 0.4),
    axis.line = element_line(color = "black"),
    plot.background = element_blank(),
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    panel.border = element_blank()) + 
  labs(x = "", y = "", fill = "Genes Detected (%)")
genes_plot
```

![](T.rubescens_genome_note_plots_files/figure-gfm/Create%20plots-3.png)<!-- -->

``` r
ggsave("/Users/bailey/Documents/research/CCGP/figures/genes_plot.png", plot = genes_plot, width = 4, height = 6, units = "in")

# Transposable elements recovered and not recovered from each genome
TE_long <- species_data[,c(1:2,4:5)] %>%
  pivot_longer(cols = -Species, names_to = "Attribute", values_to = "Value")

# Order TE to determine order in barplot
TE_long$Attribute <- factor(TE_long$Attribute, levels = c("retroelements", "DNA_transposons", "rolling_circles", "unclassified_repeats", "simple_repeats", "low_complexity_repeats", "not_TE"))

# Order species appropriately
TE_long$Species <- factor(TE_long$Species, levels = c("A. amphitrite", "B. glandula", "B. nubilus", "S. balanoides", "S. cariosus", "C. hameri", "T. rubescens", "L. anatifera", "P. pollicipes"))

# Create custom legend labels
custom_labels <- c(
  "retroelements" = "Retro elements",
  "DNA_transposons" = "DNA Transposons",
  "rolling_circles" = "Rolling Circles",
  "unclassified_repeats" = "Unclassified Repeats",
  "simple_repeats" = "Simple Repeats",
  "low_complexity_repeats" = "Low Complexity Repeats",
  "not_TE" = "Not Transposable Elements"
)

# Create TE plot
# TE_plot <- ggplot(data = TE_long, mapping = aes(x = Species, y = Value, fill = Attribute)) + 
#   geom_bar(stat = "identity", position = "stack") +
#   scale_fill_manual(values = c("retroelements" = "#803233FF",
#                                "DNA_transposons" = "#4393C3FF",
#                                "rolling_circles" = "#303638FF" , 
#                                "unclassified_repeats" = "#ADD0B5FF", 
#                                "simple_repeats" = "#484357FF", 
#                                "low_complexity_repeats" = "#8FA3ABFF",
#                                "not_TE" = "#DC876CFF"),
#   labels = custom_labels) +  # Change the legend labels
#   theme_bw() +
#   theme(text = element_text(size = 14), 
#     legend.title = element_text(size = 14, angle = 90),
#     legend.text = element_text(size = 14, angle = 90),
#     axis.text.x = element_blank(),
#     axis.text.y = element_text(size = 14, color = "black", angle = 90, hjust = 1, vjust = 0.4),
#     axis.line = element_line(color = "black"),
#     plot.background = element_blank(),
#     panel.grid.major = element_blank(),
#     panel.grid.minor = element_blank(),
#     panel.border = element_blank()) + 
#   labs(x = "", y = "", fill = "Transposable Elements")
# TE_plot
# ggsave("/Users/bailey/Documents/research/CCGP/figures/TE_plot.png", plot = TE_plot, width = 4, height = 6, units = "in")
```

## Scaffold Plot

``` r
scaffold_table <- data.frame(
  scaffold_number = c(1:30),
  scaffold_length = c(163566339, 155561112, 145385139, 131879518, 128416569, 127676036, 121229900, 116984712, 115326894, 108735405, 107618596,106534768, 105467455, 102402446, 95591984, 70499036, 7682696, 6775285, 5568435, 4291792, 4204583, 3705150, 3488183, 3406666, 3149461, 2970072, 2926998, 2808956, 2804073, 2793342),
  scaffold_length_rounded = c(163.6, 155.6, 145.4, 131.9, 128.4, 127.7, 121.2, 117.0, 115.3, 108.7, 107.6, 106.5, 105.5, 102.4, 95.6, 70.5, 7.7, 6.8, 5.6, 4.3, 4.2, 3.7, 3.5, 3.4, 3.1, 3.0, 2.9, 2.8, 2.8, 2.8)
)

scaffold_plot <- ggplot(data = scaffold_table, mapping = aes(x = scaffold_number, y = scaffold_length_rounded)) + 
  geom_bar(stat = 'identity', alpha = 1, position = position_dodge(width = 1000), color = "#FF1493", fill = "#FF1493") +
  theme_bw() +
  theme(text = element_text(size = 22), legend.text = element_text(size = 22),
    legend.position = "none",
    axis.text = element_text(size = 22, color = "black"),
    axis.line = element_line(color = "black"),
    plot.background = element_blank(),
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    panel.border = element_blank()) + 
  labs(x="Scaffold Number", y="Scaffold Length (Mb)", fill = "", colour = "") +
  scale_x_continuous(breaks = c(1:30), limits = c(0.5,30.5))
scaffold_plot
```

![](T.rubescens_genome_note_plots_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
ggsave("/Users/bailey/Documents/research/CCGP/figures/scaffold_plot.png", plot = scaffold_plot, width = 14, height = 14, units = "in")
```

## BUSCO score results

``` r
Amphibalanus_amphitrite
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    951 (93.88%)
                Complete + Partial:    980 (96.74%)
        # of missing core genes:    33 (3.26%)
        # of sequences:    2643
        Total length (nt):    807604242
        N50 sequence length (nt):    536785
        L50 sequence count:    424
        GC-content (%):    49.76

Balanus_glandula
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    426 (42.05%)
                Complete + Partial:    801 (79.07%)
        # of missing core genes:    212 (20.93%)
        # of sequences:    239901
        Total length (nt):    582475369
        N50 sequence length (nt):    3341
        L50 sequence count:    51041
        GC-content (%):    47.10
        
Balanus_nubilus
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    465 (45.90%)
                Complete + Partial:    835 (82.43%)
        # of missing core genes:    178 (17.57%)
        # of sequences:    206513
        Total length (nt):    562916451
        N50 sequence length (nt):    3958
        L50 sequence count:    41470
        GC-content (%):    47.05

Chirona_hameri
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    813 (80.26%)
                Complete + Partial:    901 (88.94%)
        # of missing core genes:    112 (11.06%)
        # of sequences:    20340
        Total length (nt):    1336202063
        N50 sequence length (nt):    80858
        L50 sequence count:    4662
        GC-content (%):    49.10
        
Lepas_anatifera 
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    925 (91.31%)
                Complete + Partial:    965 (95.26%)
        # of missing core genes:    48 (4.74%)
        # of sequences:    2276
        Total length (nt):    1044949769
        N50 sequence length (nt):    1099682
        L50 sequence count:    268
        GC-content (%):    46.86
        
Pollicipes_pollicipes
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    917 (90.52%)
                Complete + Partial:    950 (93.78%)
        # of missing core genes:    63 (6.22%)
        # of sequences:    1255
        Total length (nt):    770104822
        N50 sequence length (nt):    47009503
        L50 sequence count:    8
        GC-content (%):    52.34
        
Semibalanus_balanoides
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    569 (56.17%)
                Complete + Partial:    733 (72.36%)
        # of missing core genes:    280 (27.64%)
        # of sequences:    16596
        Total length (nt):    486009102
        N50 sequence length (nt):    56726
        L50 sequence count:    1896
        GC-content (%):    47.84
        
Semibalanus_cariosus
        Total # of core genes queried:    1013
        # of core genes detected
                    Complete:    935 (92.30%)
                Complete + Partial:    969 (95.7%)
        # of missing core genes:    44 (4.3%)
        # of sequences:    2552
        Total length (nt):    1215039272
        N50 sequence length (nt):    2644330
        L50 sequence count:    106
        GC-content (%):    46.5
        

Tetraclita_rubescens
        Total # of core genes queried:    1013
        # of core genes detected
                Complete:    937 (92.50%)
                Complete + Partial:    971 (95.85%)
        # of missing core genes:    42 (4.15%)
        # of sequences:    6513
        Total length (nt):    2441681606
        N50 sequence length (nt):    106952858
        L50 sequence count:    10
        GC-content (%):    46.13
```
