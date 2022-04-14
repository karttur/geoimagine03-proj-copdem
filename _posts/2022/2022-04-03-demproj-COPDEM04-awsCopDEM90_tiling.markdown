---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Tile CopDEM 90 m from AWS
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Tile Copernicus DEM 90 m version from Amazon Web Services (AWS) while also reprojecting to EASE Grid North"
tags:
  - DEM
  - CopDEM
  - AWS
  - tiling
  - gdalwarp
  - EASE grid north
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2022-04-03 T18:17:25.000Z'
modified: '2022-04-03 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is an instruction for tiling the Copernicus DEM (CopDEM) 90 m version to predefined projected tiling system. The core process used is [_gdalwarp_](https://gdal.org/programs/gdalwarp.html) automatically iterated from within Karttur's GeoImagine Framework to produce DEM tiles for the predefined projection system _ease2n_ (EASE 2 grid North).

## Introduction

The [previous]() covers how to download and organise the AWS version of CopDEM 90 m. If you followed the instructions you should now have a virtual layer representing CopDEM at 90 m spatial resolution for the whole Earth. The virtual mosaic is composed of 26470 individual tiles. In this post you will use the Framework to reproject the CopDEM into the predefined tiles of the [projection system _ease2n_]().

## Prerequisits

You must have installed Karttur's GeoImagine Framework. You must also have [defined and created the Framework projection system for _ease2n_ (EPSG:6931, EASE GRID 2 North)](#). If you did not yet create the projection system, its default tiling system and a user with access to the projection system, you need to follow the instructions in the blog [blog](#) and do that before continuing.

## Tiling and reprojection

The Framework process <span class='process'>TileAncillaryRegion</span> expects a regional dataset belonging to the system _ancillary_ as input. _ancillary_ data is almost always in geographic coordinates (EPSG:4326) whereas all other systems are projected. The user (_userid_), project (_projectid_) and tract (_tractid_) must be defined using the projection system (_system_) also defined in json tag _userproject_:

```
{
  "userproject": {
    "userid": "karttur",
    "projectid": "karttur-northlandease2n",
    "tractid": "karttur-northlandease2n",
    "siteid": "*",
    "plotid": "*",
    "system": "ease2n"
  },
```
The tiles in the projection system (_ease2n_) covering the tract (_karttur-northlandease2n_) are already defined in the postgres database. To check it out, start the database manager (e.g. <span class=''>TablePlus</span>) and execute the following query:

```
SELECT * FROM regions.tracts WHERE tractid = 'karttur-northlandease2n';
```

The _parentid_ in the example above is _northlandease2n_. The parent of any tract is always a default region. You can now ask the database which tiles in the projection system _ease2n_ that are covered by the default region _northlandease2n_:

```
SELECT * FROM ease2n.regions WHERE regionid='northlandease2n';
```

This should return 104 tiles.

When you run the json command file <span class='file'>0180\_TileAncillaryRegion\_aws-CopDem-90m.json</span>, the Framework will automatically find the 104 tiles linked to the tract _karttur-northlandease2n_. For each tile the corners are used for defining a [_gdalwarp_](https://gdal.org/programs/gdalwarp.html) operation. By default the process generates a shell script (parameters _asscript_ is by default set to _true_). For the example above, the first lines of the shell script looks like this:

```
xyz
```

Setting the parameter _asscript_ to _false_ will cause the process to execute the [_gdalwarp_](https://gdal.org/programs/gdalwarp.html) operations on the fly instead.

To loop over and generate all 104 tiles covering the land mass of the northern hemisphere at 90 m spatial resolution takes time.

### Next step

Many DEM related processes are done using neighbourhoods or regions (e.g. basins). If such processes are done using wall to wall fitted data, edge effects will cause errors. To overcome that Karttur's GeoImagine Framework can use spatially [expanded tiles by using virtual](../../globaldem/demproj-DemTileMosaics/). The next post covers the processing for creating virtual tiles for the 104 CopDEM tiles created in this post.
