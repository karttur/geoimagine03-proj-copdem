---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Process chain for CopDEM 90 (Gisco Service)
categories: copdemgs
datasource: dem
biophysical: elevation
excerpt: "Process chain for CopDEM 90 m version from Gisco-Service using Kartturs GeoImagine Framework"
tags:
  - process chain
  - Copernicus
  - DEM
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-08-11 T18:17:25.000Z'
modified: '2021-08-11 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

The Gisco-Services version of Copernicus Digital Elevation Model (DEM) (CopDEM) has errors along the date-line. This was discovered only after visualizing the results of the DEM correction. Further, in February 2022 the Forest and Building adjusted CopDEM (FABDEM) was published, and is at time of editing this (April 2022) the preferred version of CopDEM for hydrological modeling. The process chain in this post is redundant but kept for future reference.

For a more comprehensive instructions on the processing of Copernicus DEM, see the post on the processing of the CopDEM AWS version.

## Prerequisits

You must have installed Karttur's GeoImagine Framework. You also need a registered project that covers the geographic regions of the DEM you want to process. The latter can, however, be done as part of the project itself.

## Overview

This post summarises the processes chain for the CopDEM version available on Gisco-Services in Karttur's GeoImagine Framework.

## Process chain

The process chain is under the <span class= 'button'>Hide/show</span> button below. The process chain is an ordinary text file linking to a series of <span class='file'>json</span> files. Empty rows and rows starting with a "#" are ignored. When called from Karttur's GeoImagine Framework, the processes of each json file are sequentially run. In the process chain below you can click on each json file name to see the complete content.

<button id= "toggleprocesschain" onclick="hiddencode('processchain')">Hide/Show Process chain</button>

<div id="processchain" style="display:none">

{% capture text-capture %}
{% raw %}

#\# Search gisco-services online products for CopDEM 90 m version
[0100_SearchCopernicusProducts_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0100_SearchCopernicusProducts_gs-CopDEM-90m/)

\#\# Download gisco-services online products for CopDEM 90 m version
[0110_DownloadCopernicus_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0110_DownloadCopernicus_gs-CopDEM-90m/)

\#\# Explode (UnZip) downloaded gisco-services CopDem 90m products
[0120_UnZipRawData_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0120_UnZipRawData_gs-CopDEM-90m/)

\#\# Create polygons for all the downloaded gisco-services CopDEM tiles (optional)
[0122_BBoxTiledRawData_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0122_BBoxTiledRawData_gs-CopDEM-90m/)

\#\# Mosaic the gisco-services CopDEM 90m raw tiles
[0125_MosaicAncillary_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0125_MosaicAncillary_gs-CopDEM-90m/)

\#\# Import the CopDEM 90 m virtual data to the Framework
[0160_OrganizeAncillary_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0160_OrganizeAncillary_gs-CopDEM-90m/)

\#\# Tile the virtual CopDEM 90 m mosaic to tif tiles
[0180_TileAncillaryRegion_gs-CopDem-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0180_TileAncillaryRegion_gs-CopDem-90m/)

\#\# Create CopDEM 90 m virtual overlap mosaics for each original tile
[0190_MosaicAdjacentTiles_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0190_MosaicAdjacentTiles_gs-CopDEM-90m/)

\#\# Fill single pits and peaks (adjacent to streams)
[0230_GrassDemFillDirTiles_gs-CopDEM_90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0230_GrassDemFillDirTiles_gs-CopDEM_90m/)

\#\# Create CopDEM 90 m virtual overlap mosaics for each filled/flattened tile
[0191_MosaicAdjacentTiles_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0191_MosaicAdjacentTiles_gs-CopDEM-90m/)

\#\# Fill larger pits in streams of CopDEM 90 m with r.hydrodem
[0240_GrassDemHydroDemTiles_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0240_GrassDemHydroDemTiles_gs-CopDEM-90m/)

\#\# Create virtual overlaps of the twice filled CopDEM 90m
[0192_MosaicAdjacentTiles_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0192_MosaicAdjacentTiles_gs-CopDEM-90m/)

\#\# Correct the CopDEM 90m shoreline
[0210_GrassOnetoManyTiles-correct-shoreline_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/copdem90gs/projects-0210_GrassOnetoManyTiles-correct-shoreline_gs-CopDEM-90m/)

{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>
