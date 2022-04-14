---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: CopDEM virtual tiles
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Create spatially expanded virtual tile mosaics"
tags:
  - DEM
  - CopDEM
  - AWS
  - virtual tiles
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2022-04-04 T18:17:25.000Z'
modified: '2022-04-04 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is an instruction for creating virtually expanded tiles for the Copernicus DEM (CopDEM) 90 m projected to projection system _ease2n_ (EASE 2 grid North). The core process used is [_gdalbuildvrt_](https://gdal.org/programs/gdalbuildvrt.html) automatically iterated from within Karttur's GeoImagine Framework.

## Introduction

The tiles produced in the [previous](../demproj-COPDEM04-awsCopDEM90_tiling) post fit seamlessly wall-to-wall. Many DEM derived landscape properties rely on adjacent cells or even the full region representing all upstream cells. As part of Karttur's GeoImagine Framework you can create expanded virtual tiles by mosaicking the edges of the adjacent tiles. The basic approach is summarised in the post [Mosaics for avoiding edge effects](http://localhost:4000/globaldem/demproj-DemTileMosaics/).

## Prerequisits

You must have installed Karttur's GeoImagine Framework, and then imported and tiled the DEM.

## Tiling and reprojection

The Framework process <span class='process'>MosaicAdjacentTiles</span> expands all tiles of the defined composition and within the defined region. The expansion is accomplished by creating a virtual tile that centres on an original tile and then adds the edges of all 8 neighbours. By default the added edge has a width of 101 cells. The width, given as number of tiles, can be changed by the parameter _overlap_.



```

```

### Next step

With the virtually expanded tiles prepared, the next step loops over them and create full size tiles using [GRASS GIS]() and then performs a set of different GRASS script on each. The output result is then reduced in spatial extent to the original tile and exported from GRASS and back into the Framework system.
