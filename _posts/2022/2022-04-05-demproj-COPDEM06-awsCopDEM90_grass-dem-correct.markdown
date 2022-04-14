---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: CopDEM hydro correction
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Hydrological correction of Coperncisu DEM with GRASS GIS"
tags:
  - DEM
  - CopDEM
  - AWS
  - virtual tiles
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2022-04-05 T18:17:25.000Z'
modified: '2022-04-05 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is an instruction for hydrological correction of DEM data using [GRASS GIS](#) from inside Karttur's GeoImagine Framework.

## Introduction

Digital Terrain Models (DTMs) are widely used for routing water flow through the landscape. Ideally the hillside and its valley should show progressively lower elevations until the stream empties in a larger river and then the sea. This is, however, seldom the case and most DTMs will contain artificial smaller pits and larger depressions. Water levels are notoriously difficult to capture. In side looking Synthetic Aperture Radar (SAR) data (the most common source method fore deriving global DEMs), water surfaces are often depressed relative to other surfaces. Vegetated ground, on the other hand, is often elevated as the true terrain level is hidden below a dense canopy.

This manual covers how to use [GRASS GIS](https://grass.osgeo.org) and Karttur's GeoImagine Framework for hydrological correction of DTMs.

## Prerequisits

You must have installed Karttur's GeoImagine Framework, and then imported and tiled the DEM.

## Tiling and reprojection

The Framework process <span class='process'>MosaicAdjacentTiles</span> expands all tiles of the defined composition and within the defined region. The expansion is accomplished by creating a virtual tile that centres on an original tile and then adds the edges of all 8 neighbours. By default the added edge has a width of 101 cells. The width, given as number of tiles, can be changed by the parameter _overlap_.



```

```

### Next step

With the virtually expanded tiles prepared, the next step loops over them and create full size tiles using [GRASS GIS]() and then performs a set of different GRASS script on each. The output result is then reduced in spatial extent to the original tile and exported from GRASS and back into the Framework system.
