Eastern Pacific Coastline Map
================
Bailey Carlson

#### R Markdown

## Practice map

``` r
# Install necessary packages
library(raster)
```

    ## Loading required package: sp

``` r
library(sf)
```

    ## Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE

``` r
library(rnaturalearth)
library(rnaturalearthdata)
```

    ## 
    ## Attaching package: 'rnaturalearthdata'

    ## The following object is masked from 'package:rnaturalearth':
    ## 
    ##     countries110

``` r
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:raster':
    ## 
    ##     intersect, select, union

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Load the coastline data
world <- ne_countries(scale = "medium", returnclass = "sf")

# Filter for the Pacific coastline
pacific_coast <- world %>%
  filter(region_un == "Americas")

# Define some example points (replace with your actual data points)
points <- data.frame(
  lon = c(-111.50, -122.5, -124.24, -124.25),
  lat = c(24.5, 37.49, 40.2442, 42.5),
  name = c("Baja California Sur", "San Francisco", "Cape Mendocino", "Burnt Hill")
)

# Plot the map with ggplot2
CCGP_Trubescens_map <- ggplot(data = pacific_coast) +
  geom_sf(fill = "#9ACD32", color = "#6B8E23") +  # Color the land
  coord_sf(xlim = c(-140, -105), ylim = c(20, 60), expand = FALSE) +
#  geom_arrow_chain(points, mapping = aes(x =lon, y=lat), linewidth = 0.7, linetype = "dotted", linejoin = "round", force_arrow = TRUE) +
  theme_minimal() +
  theme(panel.background = element_rect(fill = "#00B2EE", color = NA), # Color the ocean
        text = element_text(size = 14, color = "black"),
        plot.background = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.border = element_blank(),
        axis.line = element_line(color = "black")) +
  geom_point(data = points, aes(x = lon, y = lat), color = c("#EA879CFF","#EFEFCFFF","#846D86FF","#4F3855FF"), size = 2) +  # Add points
  geom_text(data = points, aes(x = lon, y = lat, label = name), nudge_x = -4, nudge_y = 1.15, color = "black") +  # Add labels for points
  scale_x_continuous(breaks = c(-140, -130, -120, -110)) +
  labs(x = "Longitude",
       y = "Latitude")
CCGP_Trubescens_map
```

![](T.rubescens_genome_note_map_files/figure-gfm/Create%20map%20with%20R%20colors-1.png)<!-- -->

``` r
ggsave("/Users/bailey/Documents/research/CCGP/figures/CCGP_Trubescens_map.png", CCGP_Trubescens_map, width = 6, height = 4, units = "in")
```

## Real Map

``` r
library(raster)
library(sf)
library(ggplot2)

# Replace with the path to your GeoTIFF file
tif_file <- "/Users/bailey/Documents/research/CCGP/figures/NE2_HR_LC_SR_W_DR/NE2_HR_LC_SR_W_DR.tif"

# Load the GeoTIFF file as a RasterBrick
raster_data <- brick(tif_file)

# Subset the raster data to focus on a specific region if needed
# For example, here we crop to the extent of North America
# Adjust xlim and ylim according to your region of interest
raster_data_subset <- crop(raster_data, extent(c(-140, -105, 20, 60)))

# Convert the subsetted raster data into a data frame
raster_df <- as.data.frame(raster_data_subset, xy = TRUE)

# Create RGB values for each pixel in the raster
raster_df$rgb <- rgb(raster_df$NE2_HR_LC_SR_W_DR_1, 
                     raster_df$NE2_HR_LC_SR_W_DR_2, 
                     raster_df$NE2_HR_LC_SR_W_DR_3, 
                     maxColorValue = 255)

# Define data points
points <- data.frame(
  lon = c(-109.89564722, -122.51211389, -124.40950000, -124.39168611, -123.02002500),
  lat = c(22.87607778, 37.73574167, 40.44013056, 42.23288611, 37.99544444),
  name = c("Cabo San Lucas", "San Francisco", "Cape Mendocino", "Burnt Hill", "Point Reyes")
)

# Plot the raster data using ggplot2
CCGP_Trubescens_map <- ggplot(data = raster_df, aes(x = x, y = y)) +
  geom_tile(aes(fill = rgb)) +
  scale_fill_identity() +  # Use identity scale to directly map colors
  coord_sf(xlim = c(-129, -108.75), ylim = c(22, 51), expand = FALSE) +
  geom_point(data = points[1,], aes(x = lon, y = lat),
             color = "#FF6EB4",
             size = 4) +
  geom_text(data = points[1,], aes(x = lon, y = lat, label = name),
            nudge_x = -4.15, nudge_y = 0.12, color = "black", size = 4.5) +
  geom_point(data = points[2,], aes(x = lon, y = lat),
             color = "#FF6EB4",
             size = 4) +
  geom_text(data = points[2,], aes(x = lon, y = lat, label = name),
            nudge_x = 3.4, nudge_y = -0.7, color = "black", size = 4.5) +
  geom_point(data = points[3,], aes(x = lon, y = lat),
             color = "#FF6EB4",
             size = 4) +
  geom_text(data = points[3,], aes(x = lon, y = lat, label = name),
            nudge_x = 4.15, nudge_y = 0, color = "black", size = 4.5) +
  geom_point(data = points[4,], aes(x = lon, y = lat),
             color = "#FF6EB4",
             size = 4) +
  geom_text(data = points[4,], aes(x = lon, y = lat, label = name),
            nudge_x = 2.5, nudge_y = 0, color = "black", size = 4.5) +
  geom_point(data = points[5,], aes(x = lon, y = lat),
             color = "white",
             size = 12,
             shape = "*") +
  geom_text(data = points[5,], aes(x = lon, y = lat, label = name),
            nudge_x = 2.9, nudge_y = 0.8, color = "black", size = 4.5) +
  scale_x_continuous(breaks = c(-125, -120, -115, -110)) +
  labs(x = "Longitude",
       y = "Latitude") +
  theme_bw() +
  theme(text = element_text(size = 12, color = "black"),
        axis.title = element_text(size = 12, color = "black"),
        axis.text = element_text(size = 12, color = "black"),
        plot.background = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.border = element_blank(),
        axis.line = element_line(color = "black"),
        legend.position = "none")
CCGP_Trubescens_map
```

![](T.rubescens_genome_note_map_files/figure-gfm/Create%20map%20with%20real%20map-1.png)<!-- -->

``` r
# Save the plot
ggsave("/Users/bailey/Documents/research/CCGP/figures/CCGP_Trubescens_map.png", width = 8, height = 6, units = "in", dpi = 300, bg = "white")
```
