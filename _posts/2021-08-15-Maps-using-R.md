---
layout: post
title: "Maps using R"
modified: 2021-08-15 10:10:36 -0500
category:
tags:   
  - GIS
  - blog
  - R
  - map
image:
  feature: texture-feature-20.jpg
  credit: Cerro Yupati. La Pedrera, Amazonas. Colombia. Diego J. Lizcano
  creditlink: https://www.flickr.com/photos/diegolizcano/14734613538
comments: true
share: true
read_time: true
---

# R as mapping tool. 


R has added more tools to make beautiful maps and has evolved from a simple GIS tool to a very capable software to make maps. New packages such as sf, mapview, and tmap are game changers, extending R capabilities to make excellent maps.


This is one example from the papper [Elevation as an occupancy determinant of the little red brocket deer (Mazama rufina) in the Central Andes of Colombia](https://doi.org/10.15446/caldasia.v43n2.85449). https://doi.org/10.15446/caldasia.v43n2.85449

![map](https://github.com/dlizcano/Mazama_rufina/raw/main/figs/README-unnamed-chunk-3-1.png) 


## How to make it?

The background is a terrain raster image from Open Street Map of a bounding box window. Later we plot the country and states limits using the tmap package. 


```r

cams_loc_QR <- read.csv("data/cams_location.csv") # read data
cams_loc_QR_sf <- st_as_sf(cams_loc_QR, coords = c("Longitud", "Latitud"), crs = "+proj=longlat +datum=WGS84 +no_defs") # make data sf

bb <- c(-75.60, 4.59, -75.39,  4.81) # bounding box
andes_osm1 <- read_osm(bb, zoom = NULL, type="stamen-terrain", mergeTiles = TRUE) # type can be also bing and osm 
collimit <- gadm_sf_loadCountries("COL", level=0, basefile="./")
deptos <- gadm_subset(colombia, regions=c("Risaralda", "Quindío"))

depto_window <- qtm(andes_osm1)  +  # on osm add..
  tm_shape(cams_loc_QR_sf) + # add points
  tm_dots(col = "red", size = 0.2,  
          shape = 16, title = "Sampling point", legend.show = TRUE,
          legend.is.portrait = TRUE,
          legend.z = NA) + 
  tm_layout(scale = .9) + # legend
  tm_legend(position = c("left", "bottom"), frame = TRUE,
            bg.color="white") + 
  tm_layout(frame=F) + tm_scale_bar() + tm_compass(position = c(.75, .82), color.light = "grey90") # aditionals

dep_map <-  tm_shape(deptos$sf) + tm_polygons() + # state box
  tm_shape(data_box) + tm_symbols(shape = 0, col = "red", size = 0.25)
col_map <- tm_shape(collimit$sf) + tm_polygons() + tm_shape(deptos$sf) + tm_polygons() # country box

##### print all
depto_window
print(dep_map, vp = viewport(0.73, 0.40, width = 0.25, height = 0.25))
print(col_map, vp = viewport(0.73, 0.65, width = 0.25, height = 0.25))

```





