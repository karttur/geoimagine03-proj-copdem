---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Process chain for Copernicus DEM
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Complete process chain for Copernicus DEM using Kartturs GeoImagine Framework"
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

Processing a global DEM all the way from downloading tiles to create coherent landscape indices and metrics, is a typical task for which Karttur's GeoImagine Framework was built. Once the DEM is imported and organized within the Framework, it is easy to test different algorithms and visualizations. If you have access to some ground data you can also apply the Framework for comparing the results of different algorithms and parameters against ground data, and thus select the most appropriate landscape model.

## Prerequisits

You must have installed Karttur's GeoImagine Framework. You also need a registered project that covers the geographic regions of the DEM you want to process. The latter can, however, be done as part of the project itself.

## Overview

This post summarises the processes chain for Copernicus DEM in Karttur's GeoImagine Framework. Each step in the process chain is explained in more detail in linked posts. The Copernius DEM in 90 m spatial resoultion is used as an example.

### A note on hydrological corrections

Before running the DEM processing you need to decide how you want to manage hydrological errors in your DEM. If there are no errors, then you do not need to worry, but usually there are at least both pits and flat area. Both of these problems primarily affects the extraction of river basins. Different algorithms handle these problems differently. The simplest solution is to fill up all pits completely, with or without tilting or "burn in" the flow across flat surfaces. This, however, causes the, normally a-priori known, relative DEM accuracy to be replaced by an unknown error that might, or might not, have spatial or contextual biases. If the DEM is going to be solely used for flow routing this is perhaps the best method.

If there are independent vector datasets on stream networks and channels available, you can also burn a drainage pattern into the DEM. The geometric accuracy must match that of the DEM, and the vector  network contiguity must be correct. For wide rivers and lakes, burning a channel also causes problems with a preferential flow line with subsequent errors when calculating e.g. flooding and evapotranspiration.

Another alternative is to only fill up minor errors and errors that can be logically identified. The latter category would then include depressed channels and negative elevations along shorelines - and this is how it is done in this manual. But then you must also apply a flow routing algorithm that can pass depressions on the fly. The GRASS GIS module [r.watershed](https://grass.osgeo.org/grass79/manuals/r.watershed.html) does just that. And [r.watershed](https://grass.osgeo.org/grass79/manuals/r.watershed.html) is used for the DEM modelling in this post. To fill up all depressions, you can instead use the GRASS GIS module [r.terraflow](https://grass.osgeo.org/grass79/manuals/r.terraflow.html), or any of the DEM filling algorithms of SAGA GIS. If you apply hydrological DEM corrections, you must also decide which DEM version (corrected or non-corrected) to use for extracting other indexes, like slope, curvature etc.

## Process chain

The process chain is available at [Kartturs GitHub account](#) and also found under the <span class= 'button'>Hide/show</span> button below. The process chain is an ordinary text file linking to a series of <span class='file'>json</span> files. Empty rows and rows starting with a "#" are ignored. When called from Karttur's GeoImagine Framework, the processes of each json file are sequentially run. In the process chain below you can click on each json file name to see the complete content. The following sections also explain in more detail the processes of each json command file.

<button id= "toggleprocesschain" onclick="hiddencode('processchain')">Hide/Show Process chain</button>

<div id="processchain" style="display:none">

{% capture text-capture %}
{% raw %}

\# Create scaling for fitting DEM data to byte range
[0001_CreatesScaling_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0001_CreatesScaling_DEM/)

\# Add raster palette for byte range DEM data
[0002_AddRasterPalette_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/)

\# Create legend for byte range DEM data
[0003_createlegends_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0003_createlegends_DEM/)

\# Export legend for DEM data
[0005_exportlegend_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0005_exportlegend_DEM/)

\# Search online products for CopDEM 90 m version
[0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/)

\# Download online products for CopDEM 90 m version
[0110_DownloadCopernicus_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0110_DownloadCopernicus_CopDEM-90m.json/)

\# Explode (UnZip) donwloaded CopDem 90m products
[0120_UnZipRawData_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0120_UnZipRawData_CopDEM-90m.json/)

\# Mosaic the CopDEM 90m data
[0125_MosaicAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-90m.json/)

\# Mosaic the raw CopDEM 30 m data
[0125_MosaicAncillary_CopDEM-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-30m.json/)

\# Import the CopDEM 30 m virtual data to the Framework
[0125_MosaicAncillary_CopDEM-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-30m.json/)

\# Import the CopDEM 90 m virtual data to the Framework
[0160_OrganizeAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0160_OrganizeAncillary_CopDEM-90m.json/)

\# Tile the virtual CopDEM 30 m mosaic to tif tiles
[0180_TileAncillaryRegion_CopDem-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0180_TileAncillaryRegion_CopDem-30m.json/)

\# Tile the virtual CopDEM 90 m mosaic to tif tiles
[0180_TileAncillaryRegion_CopDem-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0180_TileAncillaryRegion_CopDem-90m.json/)

\# Create CopDEM 30 m virtual overlap mosaics for each original tile
[0190_MosaicAdjacentTiles_CopDEM-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0190_MosaicAdjacentTiles_CopDEM-30m.json/)

\# Create CopDEM 90 m virtual overlap mosaics for each original tile
[0190_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0190_MosaicAdjacentTiles_CopDEM-90m.json/)

\# Fill single pits and peaks (adjacent to streams)
[0230_GrassDemFillDirTiles_CopDEM_90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0230_GrassDemFillDirTiles_CopDEM_90m.json/)

\# Create CopDEM 90 m virtual overlap mosaics for each filled/flattened tile
[0191_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0191_MosaicAdjacentTiles_CopDEM-90m.json/)

\# Fill larger pits in streams of CopDEM 90 m with r.hydrodem
[0240_GrassDemHydroDemTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0240_GrassDemHydroDemTiles_CopDEM-90m.json/)

\# Create virtual overlaps of the twice filled CopDEM 90m
[0192_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0192_MosaicAdjacentTiles_CopDEM-90m.json/)

\# Correct the CopDEM 90m shoreline
[0210_GrassOnetoManyTiles-correct-shoreline_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0210_GrassOnetoManyTiles-correct-shoreline_CopDEM-90m.json/)

\# HERE WE ARE
[0301_GdalDemTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0301_GdalDemTiles_CopDEM-90m.json/)


{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Layout

To produce symbolised and labeled maps using the Framework, you have to define some Layout features and parameters. All map layout is done using unsigned byte range maps with values ranging from 0 to 255, Values 251 to 255 are reserved for specific purposes and not allowed for representing map values. The Framework Layout processes include:

- [CreateScaling](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-CreateScaling/),
- [AddRasterPalette](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-AddRasterPalette/),
- [CreateLegend](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-CreateLegend/), and
- [ExportLegend](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-ExportLegend/).

#### Create Scaling

Framework process: [CreateScaling](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-CreateScaling/)

Json command file: [0001_CreatesScaling_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0001_CreatesScaling_DEM/)

The [_CreateScaling_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-CreateScaling/) process converts any input raster into an unsigned byte (0-255) range file. The byte file is then used for assigning a color ramp (defined in the process [_AddRasterPalette_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-AddRasterPalette/), see next section).

Scaling is thus not defined on the fly, instead each map composition must be associated with a predefined scaling. Once defined, this scaling can not be changed. You can, however, change the color ramping by setting another raster palette.

#### Add Raster Palette

Framework process: [AddRasterPalette](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/)

Json command file: [0002_AddRasterPalette_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/)

In contrast to scaling, a _palette_ can be freely set for any map composition (but always using the same scaling). The _palette_ to use for any map layout is defined when calling the process that generates the layout. Thus the _palette_ must exist beforehand, created by using the process [_AddRasterPalette_](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/).

#### Create Legend

Framework process: [CreateLegend](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-CreateLegend/)

Json command file: [0003_createlegends_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0003_createlegends_DEM/)

With the scaling set and a palette defined, you can define a legend - that is how the combined scaling and palette will be presented in graphical layouts.

#### Export Legend

Framework process: [ExportLegend](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-ExportLegend/)

Json command file: [0005_exportlegend_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0005_exportlegend_DEM/)

Once created you can export a legend. All legends are exported as png, csv and pdf. Exporting a legend requires that you have installed the application [<span class='app'>inkscape</span>](https://inkscape.org) as outlined in the post [Python virtual environment](https://karttur.github.io/geoimagine03-docs-main/prep/prep-conda-environ/). The exported legends are stored under a director <span class='file'>legends</span> on the destination disk/volume. The image represented the raster palette is under a subfolder <span class='file'>images</span> (saved as a png image) and the legends with text added are under a subfolder <span class='file'>layouts</span>, and saved as three different versions:

- svg
- png
- pdf

### Create a project

[0020_ManageDefRegProj_copdem-ease2n.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/)

Data specific processing requires that your user has a predefined project that includes a predefined _tract_, where the _tract_ **must** be linked to a default region. All spatial processing that you process from that user and with that project will then operate on system tiles linked to parent default region of your project _tract_. The whole processing is set up in thin manner to minimise double processing.

Unless you have done so before you must setup the project and tract to use for processing the Copernicus DEM data. The example is for processing the DEM for the entire basins draining into Arctic waters. Thus the example assign the default region _northlandease2n_ as the parent region. Both the user specific project and its region are called _karttur-northlandease2n_.

### Search, download and unzip

Search and download differ for almost all datasets. Details for searching and downloading Copernicus DEM (and other DEMs) is in the post [Digital Elevation Models (DEMs)](../demproj-global-DEMs/). Unzipping is only required if the retrieved data is in a compressed zip format.

#### Search

Framework process: [SearchCopernicusProducts](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-SearchCopernicusProducts/)

Json command file: [0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/)

The search for the Copernicus DEM 90 m product is done using the special process [_SearchCopernicusProducts_](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/). The search requires that you installed <span class='terminalapp'>wget</span>, see the post on [Python virtual environment](https://karttur.github.io/geoimagine03-docs-main/prep/prep-conda-environ/), that also covers installing <span class='terminalapp'>wget</span>.

The process [<span class='process'>SearchCopernicusProducts</span>](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/) results in an <span class='file'>html</span> file with url links to all the resources to download. For the process defined in [0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/), the result is stored in the file <span class='file'>copernicusDEM90.html</span>

<button id= "togglehtml" onclick="hiddencode('htmldiv')">Hide/Show copernicusDEM90.html (parts of)</button>
<div id="htmldiv" style="display:none">
{% capture text-capture %}
{% raw %}
```
<table>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y-10x-20.zip">10_DEM_y-10x-20.zip</a></td><td align="right">2020-11-03 14:23  </td><td align="right">185K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y-10x-40.zip">10_DEM_y-10x-40.zip</a></td><td align="right">2020-11-03 14:25  </td><td align="right">135M</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y-10x-50.zip">10_DEM_y-10x-50.zip</a></td><td align="right">2020-11-03 14:26  </td><td align="right">398M</td><td>&nbsp;</td></tr>

<tr><td valign="top">...</td><td>...</td><td align="right">...</td><td align="right">...</td><td>&nbsp;</td></tr>

<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y80x70.zip">10_DEM_y80x70.zip</a></td><td align="right">2020-11-03 16:00  </td><td align="right">302K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y80x80.zip">10_DEM_y80x80.zip</a></td><td align="right">2020-11-03 16:01  </td><td align="right">300K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="10_DEM_y80x90.zip">10_DEM_y80x90.zip</a></td><td align="right">2020-11-03 16:02  </td><td align="right">5.8M</td><td>&nbsp;</td></tr>

</table>
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

#### Download

Framework process: [DownloadCopernicus](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadCopernicus/)

Json command file: [0110_DownloadCopernicus_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0110_DownloadCopernicus_CopDEM-90m.json/)

Also downloading the Copernicus DEM 90 m version is defined with a customized process, [_DownloadCopernicus_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadCopernicus/). This process reads the html file above (<span class='file'>copernicusDEM90.html</span>), extracts the url tag (\<a\>), looks for a local copy of the DEM tile, and if not found (or _overwrite_ set to _true_) downloads the tile. The downloading is predefined to a path on the destination disk and can not be changed. If you want to store that data somewhere else you have to move them manually, but the system will then lose track of them. The original tiles are not registered in the database. It is thus recommended that you leave the original data in the predefined location (for the data used here the default path is _/DISKPATH/DOWNLOADS/CopernicusDem90_).

#### Unzip

Framework process: [UnZipRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/)

Json command file: [0120_UnZipRawData_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0120_UnZipRawData_CopDEM-90m.json/)

Downloaded data that is retrieved as zip files can be unzipped with the command [_UnZipRawData_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/). This is a generic process, and you need to give the full path to both the source (zip) and target (expanded) datasets. The disk (volume) paths are as usual given through the parameters _srcpath_ and _dstpath_, but additioally you must also give the directories on the disks for both _srcdir_ and _dstdir_ under _parameters_. If _srcdir_ is a file, the script assumes that it is a list of the files to unzip; if _srcdir_ is a directory the script will explode all zip files in that directory.

For the example here I created a file listing all the zip files to explode (<span class='file'>CopernicusDem90_rawtiles.csv</span>). In MacOS or Linux you create the csv list by piping the <span class='terminalapp'>ls</span> command to a file:

<span class='terminal'>$ ls *.zip > CopernicusDem90_rawtiles.csv</span>

<button id= "toggleziplist" onclick="hiddencode('ziplist')">Hide/Show CopernicusDem90_rawtiles.csv</button>
<div id="ziplist" style="display:none">
{% capture text-capture %}
{% raw %}
```
10_DEM_y-10x-100.zip
10_DEM_y-10x-140.zip
10_DEM_y-10x-150.zip
...
...
...
10_DEM_y80x80.zip
10_DEM_y80x90.zip
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Import

The retrieved data is not yet organized as a dataset in the Framework database. A tiled dataset, like CopDEM, must be prepared as a virtual full coverage dataset prior to importing (organization) and then retiling into one, or more, projection system(s) that are used for the actual spatial data processing in the Framework. For the CopDEM dataset, the sequence of processes thus becomes:

- identification of raw tile bounding boxes (optional),
- mosaic raw tiles,
- organize (import into Framework), and
- tiling to Framework projection system.

#### Bounding Boxes for raw tiles

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0122_BBoxTiledRawData_gs-CopDEM-90m.json/)

The process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) creates bounding boxes for each raw tile. In the example used here, each tile to include is listed in a csv file, created in a similar way as the list of zip files in the previous section. Running this process is optional, and intended for allowing a visual inspection of the raw data source.

<button id= "toggleziplist" onclick="hiddencode('tiflist')">Hide/Show CopDem90_tiles.csv</button>
<div id="tiflist" style="display:none">
{% capture text-capture %}
{% raw %}
```
10_DEM_y-10x-100.tif
10_DEM_y-10x-140.tif
10_DEM_y-10x-150.tif
...
...
...
10_DEM_y80x80.tif
10_DEM_y80x90.tif
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

The process generates a single vector data source with all the included tiles.

<figure>
<img src="../../images/CopDEM90_rawtiles_bboxes.png">
<figcaption>Bounding boxes for the Copernicus DEM 90 m raw tiles.</figcaption>
</figure>

As clearly seen in the figure above, the raw tiles are of arbitrary sizes with geographical coverage restricted to land. The next steps in the process chain converts these more arbitrary tiles to regular grids fitted to the projection system where you want to use the DEM.

#### Mosaic raw tiles

Framework process: [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)

Json command file: [0125_MosaicAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-90m.json/)

Downloaded data that are delivered as arbitrary tiles need to be mosaicked together before importing to the Framework. (SMAP, MODIS and Sentinel data do not need mosaicking of raw tiles as these datasets either are downloaded in predefined tiling systems or as complete global datasets). The mosaicking must be done as virtual (<span class='file'>.vrt</span>) dataset. To actually create a true global mosaic at e.g 90 m, as in this example, would take a long time and also take up unnecessary much disk space. The raw tile mosaic will represent the raw source data imported (organized) into the Framework database. The process [<span class='process'>MosaicAncillary</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/) does not register the imported (organized in the terminology of the Framework) dataset into the database, that is done in the next step.

<figure>
<img src="../../images/CopDEM90_rawtiles_mosaic.png">
<figcaption>Virtual mosaic of the Copernicus DEM 90 m raw tiles.</figcaption>
</figure>

#### Organize (import and register) ancillary dataset

Framework process: [OrganizeAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeAncillary//)

Json command file: [0160_OrganizeAncillary_CopDEM-90m](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0160_OrganizeAncillary_CopDEM-90m.json/)

With the Copernicus data downloaded, unzipped and mosaicked, the data can be organized for (or imported to) the Framework. For the Copernicus DEM data, the process [_OrganizeAncillary_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeAncillary//) as defined in the json object below only imports the virtual mosaic linked to the original (downloaded and unzipped) tiles.

Once you have run the json command, you can check in the Framework database, and you should find that the _ancillary_ schema contains a _layer_ with the _compid_ _dem_copdem_. Apart from being defined as an individual layer, the dataset is also registered as a product (table: _compprod_) and a composition (table: _compdef_):

```
SELECT * FROM ancillary.layer WHERE compid = 'dem_copdem';
SELECT * FROM ancillary.compprod WHERE compid = 'dem_copdem';
SELECT * FROM ancillary.compdef WHERE compid = 'dem_copdem';
```

#### Tiling to Framework projection system

Framework process: [TileAncillaryRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileAncillaryRegion/)

Json command file: [0180_TileAncillaryRegion_CopDem-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0180_TileAncillaryRegion_CopDem-90m.json/)

All basic processing in the Framework is done using predefined tiling systems. The process [<span class='process'>TileAncillaryRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileAncillaryRegion/) constructs tiles while also reprojecting the source (_ancillary_) data to the projection system defined under _userproject_: _system_. For the CopDEM data in this example, the data are reprojected and tiled for the _ease2n_ (EASE GRID 2.0 North) projection system.

### Expanded virtual mosaic tiles

Spatial processing applying seamless tiles causes edge effects for kernel and regional based processes. To overcome that the Framework can use virtually expanded tiles as input in some processes. The process for creating virtually expanded tiles is [MosaicAdjacentTiles](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAdjacentTiles/).

#### Mosaic adjacent tiles

Framework process: [MosaicAdjacentTiles](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAdjacentTiles/)

Json command file: [0190_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0190_MosaicAdjacentTiles_CopDEM-90m.json/)

The process [MosaicAdjacentTiles](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAdjacentTiles/) creates the expanded virtual tiles. The post [Mosaics for avoiding edge effects](../../globaldem/demproj-DemTileMosaics) summarises the concept and processing.

### Hydrological DEM corrections

Hydrological corrections of the Copernicus DEM is done in several steps:

- Filling of single pits and peaks adjacent to streams
- Adjacent tile mosaic the single cell corrected DEM
- Filling of stream associated pits
- Adjacent tile mosaic of the dual corrected DEM

#### Filling of single pits and peaks adjacent to streams

Framework process: [MosaicAdjacentTiles](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-GrassDemFillDirTiles/)

Json command file: [0230_GrassDemFillDirTiles_CopDEM_90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0230_GrassDemFillDirTiles_CopDEM_90m.json/)

The first hydrological correction is the filling up of all single cell pits and the flattening of all single cell peaks adjacent to streams. The Framework process to apply is [_GrassDemFillDirTiles_](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0230_GrassDemFillDirTiles_CopDEM_90m.json/), which is a wrapper for the GRASS GIS module [_r.fill.dir_](https://grass.osgeo.org/grass79/manuals/r.fill.dir.html).

#### Adjacent tile mosaic the single cell corrected DEM

[0191_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0191_MosaicAdjacentTiles_CopDEM-90m.json/)

Create expanded virtual tiles of the single cell corrected DEM tiles for use in the next step. See the post on [Mosaics for avoiding edge effects](../demproj-DemTileMosaics) for details.

#### Fill stream associated pits

[0240_GrassDemHydroDemTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0240_GrassDemHydroDemTiles_CopDEM-90m.json/)

The second hydrological correction is the filling up of larger pits in streams. The Framework process is _GrassDemHydroDemTiles_, which is a wrapper for the GRASS GIS addon module [_r.hydrodem_](https://grass.osgeo.org/grass78/manuals/addons/r.hydrodem.html).

#### Adjacent tile mosaic the dual corrected DEM

[0192_MosaicAdjacentTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0192_MosaicAdjacentTiles_CopDEM-90m.json/)

Create expanded virtual tiles of the single cell corrected DEM tiles for use in the next step. See the post on [Mosaics for avoiding edge effects](../demproj-DemTileMosaics) for details.

### River mouth and shoreline corrections

[0210_GrassOnetoManyTiles-correct-shoreline_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0210_GrassOnetoManyTiles-correct-shoreline_CopDEM-90m.json/)

The river mouth and shoreline correction processing fills up all negative pits adjacent to the sea. No special process is used, instead the generic Framework wrapper for GRASS GIS is used by defining a sequence of GRASS commands under the process _GrassOnetoManyTiles_.

#### GRASS script

The above process generates a script file that loops over all the tiles belonging to the defined region. An example is shown under the Hide/show button below, the commented lines (starting with "#") have been added for explanation.

<button id= "toggleGrassScript0" onclick="hiddencode('GrassScript0div')">Hide/Show generated GRASS script for correcting DEM shorelines</button>

<div id="GrassScript0div" style="display:none">

{% capture text-capture %}
{% raw %}
```
# Change mapset to the present tile
g.mapset -c mapset=x04y07
# Import the original tile (always done when using virtual mosaics)
r.in.gdal input=/Volumes/Ancillary/ease2n/ESA/tiles/dem/x04y07/0/dem_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif output=originalTile --overwrite
# Import the virtual mosaic DEM
r.in.gdal -e input=/Volumes/Ancillary/ease2n/ESA/tiles/dem/x04y07/0/dem_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m-full.vrt output=DEM --overwrite
# Set region to the mosaic (expanded) DEM
g.region raster=DEM --overwrite
# Create Landdem by setting all cells==0 to null()
r.mapcalc "landDEM = if((DEM==0),null(),DEM)" --overwrite
# Run statistics on landdem and capture the return with the variable "data"
data="$(r.stats -p input=landDEM null_value='null')"
# If the statistics incudes null, the (expanded) tile includes a coastline
if echo "$data" | grep -q "null"; then
  echo "Coastline tile"
  # extract all cells at sea level
  r.mapcalc "drain_terminal = if((DEM==0),1,null())" --overwrite
  # Clump all cells at sea level
  r.clump input=drain_terminal output=terminal_clumps --overwrite
  # Convert raster clumps to vectors
  r.to.vect input=terminal_clumps output=terminal_clumps type=area --overwrite
  # Add db record for clump vector area
  v.to.db map=terminal_clumps type=centroid option=area columns=area_km2 units=kilometers --overwrite
  # Export clumps to ESRI shapefle (for inspection if needed)
  v.out.ogr input=terminal_clumps type=area format=ESRI_Shapefile output=/Volumes/Ancillary/ease2n/ESA/tiles/terminal-polys/x04y07/0/terminal-polys_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.shp --overwrite
  # Rasterize clumps of cells at sea level if smaller than 5 km2, these areas are not regarded as the sea
  v.to.rast input=terminal_clumps type=area where="area_km2< 5.0" output=drain_terminal_small use=val value=0 --overwrite
  # Rasterize clumps of cells at sea level if larger than 5 km2, these areas represent the sea
  v.to.rast input=terminal_clumps type=area where="area_km2>= 5.0" output=drain_terminal_large use=val value=0 --overwrite
  # Construct a layer of all land
  r.mapcalc "land = if(( isnull(drain_terminal_large)), 1 ,null())" --overwrite
  # Buffer a distance of 1 diagonal cell from the sea surface cells
  r.buffer input=drain_terminal_large output=shorep1 distance=128.56 units=meter --overwrite
  # Get the initial shoreline
  r.mapcalc "shoreline_ini = if((shorep1>0 && land==1),1,null())" --overwrite
  # retrieve all areas below sea level
  r.mapcalc "negative_DEM = if((DEM<0),0,null())" --overwrite
  # Get shorelines under sea level
  r.mapcalc "negativeshoreline_DEM = if((DEM<0 && shoreline_ini >0),1,null())" --overwrite
  # From the shorelines under sea level find all sea connected cells under sea level
  r.cost -n input=negative_DEM output=sea_pits start_raster=negativeshoreline_DEM max_cost=0 --overwrite
  # Reclass pis in the sea surface to null (the sea itself)
  r.mapcalc "inland_comp_DEM = if(( isnull(sea_pits) ), if((isnull(land)), null(),DEM) , null())" --overwrite
  # Get the final ocean mask
  r.mapcalc "terminal_drain = if((isnull (inland_comp_DEM)),1,null())" --overwrite
  # Buffer a distance of 1 diagonal cell from the final sea surface mask
  r.buffer input=terminal_drain output=shorep1 distance=128.56 units=meter --overwrite
  # find the shoreline (again) from the final ocean mask
  r.mapcalc "shoreline = if((shorep1>0 && inland_comp_DEM>0),1,null())" --overwrite
else
  # No coastline, just get the inland_comp_DEM as the landDEM
  r.mapcalc "inland_comp_DEM = landDEM" --overwrite
fi
# Set region to core tile prior to exporting raster data (automatic)
g.region raster=originalTile
# Export final inland composition DEM
r.out.gdal input=inland_comp_DEM output=/Volumes/Ancillary/ease2n/ESA/tiles/landdem/x04y07/0/land-dem_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Kernel DEM derivates  

With Karttur's GeoImagine Framework, kernel (filter) indexes derived from DEM data can be calculated using GDAL, GRASS GIS, SAGA GIS or using internal processes.

#### GDAL DEM derivatives

[0301_GdalDemTiles_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0301_GdalDemTiles_CopDEM-90m.json/)

The Framework process _GdalDemTiles_ is a wrapper for running the Geographic Data Abstraction Library (GDAL) DEM [(gdaldem) program](https://gdal.org/programs/gdaldem.html).


#### Framework DEM derivatives (_NumpyDemTiles_)

```
0303_NumpyDemTiles_CopDEM-90m.json
```

The Framework process for internal DEM derivates is focusing of calculating [Terrain Position Index [TPI] landforms](http://www.jennessent.com/downloads/tpi-poster-tnc_18x22.pdf) and can thus only retrieve the derivates for slope and TPI plus the landform categogization. The TPI extraction and thus the landform categorisation is, however, more advanced (thetorically more correct) compared to most other solutions.

<button id= "togglenumpydem" onclick="hiddencode('numpydemdiv')">Hide/Show 0303_NumpyDemTiles_CopDEM-90m.json</button>

<div id="numpydemdiv" style="display:none">

{% capture text-capture %}
{% raw %}
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
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "NumpyDemTiles",
      "version": "1.3",
      "overwrite": false,
      "parameters": {
        "mode": "landformTPI",
        "mosaic": true,
        "slope": "np-3x3",
        "sloperadius": "1",
        "radiuscsv": "1,3",
        "standardize": true,
        "tpithreshold": 10,
        "palette": "landformTPIdouble"
      },
      "srcpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "copdem90": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "copdem90": {
            "layerid": "auto",
            "prefix": "auto",
            "suffix": "auto",
            "dataunit": "auto"
          }
        }
      ]
    },
    {
      "processid": "NumpyDemTiles",
      "version": "1.3",
      "overwrite": false,
      "parameters": {
        "mode": "TPI",
        "mosaic": true,
        "radiuscsv": "1,3"
      },
      "srcpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "copdem90": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "copdem90": {
            "layerid": "auto",
            "prefix": "auto",
            "suffix": "auto",
            "dataunit": "auto"
          }
        }
      ]
    },
    {
      "processid": "NumpyDemTiles",
      "version": "1.3",
      "overwrite": false,
      "parameters": {
        "mode": "slope",
        "mosaic": true,
        "radiuscsv": "1,2,3"
      },
      "srcpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "copdem90": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "copdem90": {
            "layerid": "auto",
            "prefix": "auto",
            "suffix": "auto",
            "dataunit": "auto"
          }
        }
      ]
    },
    {
      "processid": "NumpyDemTiles",
      "version": "1.3",
      "overwrite": false,
      "parameters": {
        "mode": "elev",
        "mosaic": true,
        "radiuscsv": "1,2,3"
      },
      "srcpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "karttur",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "copdem90": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "copdem90": {
            "layerid": "auto",
            "prefix": "auto",
            "suffix": "auto",
            "dataunit": "auto"
          }
        }
      ]
    }
  ]
}
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

#### GRASS DEM derivatives (_GrassOnetoManyTiles_)

GRASS GIS has more options for retrieving DEM derivates than either GDAL or the internal Framework process. GRASS modules for retrieving DEM derivatives include: [r.slope.aspect](https://grass.osgeo.org/grass78/manuals/r.slope.aspect.html), [r.params.scale](https://grass.osgeo.org/grass78/manuals/r.param.scale.html), [r.tpi](https://grass.osgeo.org/grass78/manuals/addons/r.tpi.html), [r.tri](https://grass.osgeo.org/grass78/manuals/addons/r.tri.html) and [r.geomorphon](https://grass.osgeo.org/grass78/manuals/r.geomorphon).
Retrieving DEM derivates using GRASS GIS though the Framework is done through the standard GRASS wrapper _GrassOnetoManyTiles_.

<button id= "togglegrassdemderivatives" onclick="hiddencode('grassdemderivatives')">Hide/Show 0303_NumpyDemTiles_CopDEM-90m.json</button>

<div id="grassdemderivatives" style="display:none">

{% capture text-capture %}
{% raw %}

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
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "GrassOnetoManyTiles",
      "version": "1.3",
      "overwrite": false,
      "dryrun": false,
      "verbose": 1,
      "parameters": {
        "asscript": true,
        "mosaic": true,
        "subparameter": [
          {
            "r.in.gdal": {
              "flags": "e",
              "input": "srcFPN",
              "output": "srcFN"
            }
          },
          {
            "g.region": {
              "flags": "s",
              "raster": "srcFN"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "elev3x3",
              "method": "elev",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "elev3x3",
              "type": "Float32",
              "output": "elev3x3out"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "elev9x9",
              "method": "elev",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "elev9x9",
              "type": "Float32",
              "output": "elev9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "elev9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "slope3x3",
              "method": "slope",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "slope3x3",
              "type": "Float32",
              "output": "slope3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "slope3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "slope9x9",
              "method": "slope",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "slope9x9",
              "type": "Float32",
              "output": "slope9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "slope9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "profc3x3",
              "method": "profc",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "profc3x3",
              "type": "Float32",
              "output": "profc3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "profc3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "profc9x9",
              "method": "profc",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "profc9x9",
              "type": "Float32",
              "output": "profc9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "profc9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "crosc3x3",
              "method": "crosc",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "crosc3x3",
              "type": "Float32",
              "output": "crosc3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "crosc3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "crosc9x9",
              "method": "crosc",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "crosc9x9",
              "type": "Float32",
              "output": "crosc9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "crosc9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "planc3x3",
              "method": "planc",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "planc3x3",
              "type": "Float32",
              "output": "planc3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "planc3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "planc9x9",
              "method": "planc",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "planc9x9",
              "type": "Float32",
              "output": "planc9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "planc9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "longc3x3",
              "method": "longc",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "longc3x3",
              "type": "Float32",
              "output": "longc3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "longc3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "longc9x9",
              "method": "longc",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "longc9x9",
              "type": "Float32",
              "output": "longc9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "longc9x9"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "lf3x3",
              "method": "feature",
              "size": 3
            }
          },
          {
            "r.out.gdal": {
              "input": "lf3x3",
              "output": "lf3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "lf3x3"
            }
          },
          {
            "r.param.scale": {
              "input": "srcFN",
              "output": "lf9x9",
              "method": "feature",
              "size": 9
            }
          },
          {
            "r.out.gdal": {
              "input": "lf9x9",
              "output": "lf9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "lf9x9"
            }
          },
          {
            "r.geomorphon": {
              "elevation": "srcFN",
              "search": 3,
              "skip": 0,
              "flat": 1,
              "dist": 0,
              "forms": "geomorph3x3"
            }
          },
          {
            "r.out.gdal": {
              "input": "geomorph3x3",
              "output": "geomorph3x3out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "geomorph3x3"
            }
          },
          {
            "r.geomorphon": {
              "elevation": "srcFN",
              "search": 9,
              "skip": 0,
              "flat": 1,
              "dist": 0,
              "forms": "geomorph9x9"
            }
          },
          {
            "r.out.gdal": {
              "input": "geomorph9x9",
              "output": "geomorph9x9out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "geomorph9x9"
            }
          },
          {
            "r.geomorphon": {
              "elevation": "elev3x3",
              "search": 5,
              "skip": 0,
              "flat": 1,
              "dist": 0,
              "forms": "geomorph5x5"
            }
          },
          {
            "r.out.gdal": {
              "input": "geomorph5x5",
              "output": "geomorph5x5out"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "geomorph5x5"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "elev3x3"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "srcFN"
            }
          }
        ]
      },
      "srcpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "*": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "elev3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copy",
            "prefix": "copy",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "elev9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copy",
            "prefix": "copy",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "slope3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copslope",
            "prefix": "slope",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "slope9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copslope",
            "prefix": "slope",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "profc3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copprofc",
            "prefix": "profc",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "profc9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copprofc",
            "prefix": "profc",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "crosc3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copcrosc",
            "prefix": "crosc",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "crosc9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copcrosc",
            "prefix": "crosc",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "longc3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "coplongc",
            "prefix": "longc",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "longc9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "coplongc",
            "prefix": "longc",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "planc3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copplanc",
            "prefix": "planc",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "planc9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copplanc",
            "prefix": "planc",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "lf3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "coplandform-ps",
            "prefix": "landform-ps",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "lf9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "coplandform-ps",
            "prefix": "landform-ps",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "geomorph3x3out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copgeomorph",
            "prefix": "geomorph",
            "suffix": "v01-90m-grass-3x3"
          }
        },
        {
          "geomorph9x9out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copgeomorph",
            "prefix": "geomorph",
            "suffix": "v01-90m-grass-9x9"
          }
        },
        {
          "geomorph5x5out": {
            "source": "copy",
            "product": "copy",
            "content": "copy",
            "layerid": "copgeomorph",
            "prefix": "geomorph",
            "suffix": "v01-90m-grass-5x5-elev3x3"
          }
        }
      ]
    }
  ]
}
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Hillslope DEM derivates

Hillslope indexes dereived from DEM data are calcualted using either GRASS GIS or SAGA GIS.

#### GRASS GIS hillslope DEM derivates

```
0311_GrassOnetoManyTiles_hillslope-derivates_copDEM-90m.json
```

Hillslope DEM indexes can be created by the general GRASS GIS wrapper _GrassOnetoManyTiles_.


<button id= "togglegrasshilslope" onclick="hiddencode('grasshillslope')">Hide/Show 0311_GrassOnetoManyTiles_hillslope-derivates_copDEM-90m.json</button>

<div id="grasshillslope" style="display:none">

{% capture text-capture %}
{% raw %}
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
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "GrassOnetoManyTiles",
      "version": "1.3",
      "overwrite": false,
      "dryrun": false,
      "verbose": 1,
      "parameters": {
        "asscript": true,
        "mosaic": true,
        "regionlayer": "DEM",
        "subparameter": [
          {
            "r.in.gdal": {
              "flags": "e",
              "input": "srcFPN",
              "output": "DEM"
            }
          },
          {
            "g.region": {
              "raster": "DEM"
            }
          },
          {
            "r.mapcalc": {
              "\"blocking ": " 0\"",
              "overwrite": true
            }
          },
          {
            "r.watershed": {
              "flags": "a",
              "elevation": "inland_comp_DEM",
              "max_slope_length": 750,
              "blocking": "blocking",
              "accumulation": "MFDupstream",
              "drainage": "MFDflowdir",
              "stream": "MFDstream",
              "tci": "MFDtci",
              "spi": "MFDspi",
              "length_slope": "MFDslopelength",
              "slope_steepness": "MFDslopesteepness",
              "threshold": 617,
              "memory": 4000,
              "overwrite": true
            }
          },
          {
            "r.watershed": {
              "flags": "as",
              "elevation": "inland_comp_DEM",
              "accumulation": "SFDupstream",
              "threshold": 617,
              "memory": 4000,
              "overwrite": true
            }
          },
          {
            "r.stream.extract": {
              "elevation": "inland_comp_DEM",
              "accumulation": "MFDupstream",
              "threshold": 247,
              "mexp": 1.2,
              "stream_length": 4,
              "stream_rast": "extractstream",
              "stream_vector": "extractstreamvect",
              "direction": "extractflowdir",
              "memory": "4000",
              "overwrite": true
            }
          },
          {
            "r.stream.distance": {
              "stream_rast": "extractstream",
              "direction": "extractflowdir",
              "elevation": "inland_comp_DEM",
              "distance": "streamproximity",
              "difference": "hydhead",
              "method": "downstream",
              "memory": "4000",
              "overwrite": true
            }
          },
          {
            "r.stream.distance": {
              "flags": "n",
              "stream_rast": "extractstream",
              "direction": "extractflowdir",
              "elevation": "inland_comp_DEM",
              "distance": "neardividehorizontal",
              "difference": "neardividevertical",
              "method": "upstream",
              "memory": "4000",
              "overwrite": true
            }
          },
          {
            "r.stream.distance": {
              "stream_rast": "extractstream",
              "direction": "extractflowdir",
              "elevation": "inland_comp_DEM",
              "distance": "fardividehorizontal",
              "difference": "fardividevertical",
              "method": "upstream",
              "memory": "4000",
              "overwrite": true
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "blocking"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDflowdir",
              "type": "Float32",
              "output": "MFD_flowdir"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDupstream",
              "type": "Float32",
              "output": "MFD_upstream"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "SFDupstream",
              "type": "Float32",
              "output": "SFD_upstream"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDtci",
              "type": "Float32",
              "output": "MFD_tci"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "MFDtci"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDspi",
              "type": "Float32",
              "output": "MFD_spi"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "MFDspi"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDslopelength",
              "type": "Float32",
              "output": "MFD_slope_length"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "MFDslopelength"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "MFDslopesteepness",
              "type": "Float32",
              "output": "MFD_slope_steepness"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "MFDslopesteepness"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "streamproximity",
              "type": "Float32",
              "output": "stream_proximity"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "streamproximity"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "hydhead",
              "type": "Float32",
              "output": "hydraulhead"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "hydhead"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "neardividehorizontal",
              "type": "Float32",
              "output": "near_divide_horizontal"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "neardividehorizontal"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "neardividevertical",
              "type": "Float32",
              "output": "near_divide_vertical"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "neardividevertical"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "fardividehorizontal",
              "type": "Float32",
              "output": "far_divide_horizontal"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "fardividehorizontal"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "fardividevertical",
              "type": "Float32",
              "output": "far_divide_vertical"
            }
          },
          {
            "g.remove": {
              "flags": "f",
              "type": "raster",
              "name": "fardividevertical"
            }
          },
          {
            "r.out.gdal": {
              "flags": "f",
              "input": "extractstream",
              "output": "stream_rast"
            }
          }
        ]
      },
      "srcpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "*": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "MFD_flowdir": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "mfd-flowdir",
            "prefix": "mfd-flowdir",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "MFD_upstream": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "mfd-updrain",
            "prefix": "mfd-updrain",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "SFD_upstream": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "sfd-updrain",
            "prefix": "sfd-updrain",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "MFD_tci": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "tci",
            "prefix": "tci",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "MFD_spi": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "spi",
            "prefix": "spi",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "MFD_slope_length": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "rusle-slopelength",
            "prefix": "rusle-slopelength",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "MFD_slope_steepness": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "rusle-slopesteepness",
            "prefix": "rusle-slopesteepness",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "stream_proximity": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "stream-dist",
            "prefix": "stream-dist",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "hydraulhead": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "hydraulhead",
            "prefix": "hydraulhead",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "near_divide_horizontal": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "near-divide-dist",
            "prefix": "near-divide-dist",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "far_divide_vertical": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "far-divide-head",
            "prefix": "far-divide-head",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "near_divide_vertical": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "near-divide-head",
            "prefix": "near-divide-head",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "far_divide_horizontal": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "far-divide-dist",
            "prefix": "far-divide-dist",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "stream_rast": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "stream-rast",
            "prefix": "stream-rast",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        },
        {
          "stream_vect": {
            "source": "copy",
            "product": "copy",
            "content": "terrain",
            "layerid": "stream-vect",
            "prefix": "stream-vect",
            "suffix": "v01-pfpf-hydrdem4+4-90m",
            "ext:": ".shp"
          }
        }
      ]
    }
  ]
}
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

#### GRASS script

The above process generates a script file that loops over all the tiles belonging to the defined region. An example is shown under the Hide/show button below, the commented lines (starting with "#") have been added for explanation.

<button id= "toggleGrassScript1" onclick="hiddencode('GrassScript1div')">Hide/Show generated GRASS script for correcting DEM shorelines</button>

<div id="GrassScript1div" style="display:none">

{% capture text-capture %}
{% raw %}
```
# Change mapset to the present tile
g.mapset -c mapset=x04y07
# Import the original tile (always done when using virtual mosaics)
r.in.gdal input=/Volumes/Ancillary/ease2n/ESA/tiles/dem/x04y07/0/dem_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif output=originalTile --overwrite
# Set region to the mosaic (expanded) DEM
g.region raster=DEM --overwrite
# Creating an "empty" raster for hydrological blocking (no blocking)
r.mapcalc "blocking = 0" --overwrite
# Run r.watershed using Multiple Flow Direction and outputs for all hillslope data required
r.watershed -a elevation=inland_comp_DEM max_slope_length=750 blocking=blocking accumulation=MFDupstream drainage=MFDflowdir stream=MFDstream tci=MFDtci spi=MFDspi length_slope=MFDslopelength slope_steepness=MFDslopesteepness threshold=617 memory=4000 --overwrite
# Run r.watershed using Single Flow Direction
r.watershed -as elevation=inland_comp_DEM accumulation=SFDupstream threshold=617 memory=4000 --overwrite
# Extract streams as both raster and vectors from the MFD watershed analysis
r.stream.extract elevation=inland_comp_DEM accumulation=MFDupstream threshold=247 mexp=1.2 stream_length=4 stream_rast=extractstream stream_vector=extractstreamvect direction=extractflowdir memory=4000 --overwrite
# Analyse the distance and elevation difference for all cells to the nearest stream
r.stream.distance stream_rast=extractstream direction=extractflowdir elevation=inland_comp_DEM distance=streamproximity difference=hydhead method=downstream memory=4000 --overwrite
# Analyse the distance and elevation difference for all cells to the nearest waterhsed divide
r.stream.distance -n stream_rast=extractstream direction=extractflowdir elevation=inland_comp_DEM distance=neardividehorizontal difference=neardividevertical method=upstream memory=4000 --overwrite
# Analyse the distance and elevation difference for all cells to the farthest waterhsed divide
r.stream.distance stream_rast=extractstream direction=extractflowdir elevation=inland_comp_DEM distance=fardividehorizontal difference=fardividevertical method=upstream memory=4000 --overwrite
# Export all hillslope related indexes crated above
# For each exort ("r.out.gdal") the region is automaticaly changed to the core tile region and then reset.
# Once exported, most of the GRASS internal source files are removed (deleted)
g.region raster=originalTile
r.out.gdal -f input=MFDflowdir type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/mfd-flowdir_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.region raster=originalTile
r.out.gdal -f input=MFDupstream type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/mfd-updrain_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.region raster=originalTile
r.out.gdal -f input=SFDupstream type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/sfd-updrain_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.region raster=originalTile
r.out.gdal -f input=MFDtci type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/tci_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=MFDtci --overwrite
g.region raster=originalTile
r.out.gdal -f input=MFDspi type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/spi_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=MFDspi --overwrite
g.region raster=originalTile
r.out.gdal -f input=MFDslopelength type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/rusle-slopelength_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=MFDslopelength --overwrite
g.region raster=originalTile
r.out.gdal -f input=MFDslopesteepness type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/rusle-slopesteepness_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=MFDslopesteepness --overwrite
g.region raster=originalTile
r.out.gdal -f input=streamproximity type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/stream-dist_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=streamproximity --overwrite
g.region raster=originalTile
r.out.gdal -f input=hydhead type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/hydraulhead_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=hydhead --overwrite
g.region raster=originalTile
r.out.gdal -f input=neardividehorizontal type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/near-divide-dist_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=neardividehorizontal --overwrite
g.region raster=originalTile
r.out.gdal -f input=neardividevertical type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/near-divide-head_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=neardividevertical --overwrite
g.region raster=originalTile
r.out.gdal -f input=fardividehorizontal type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/far-divide-dist_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=fardividehorizontal --overwrite
g.region raster=originalTile
r.out.gdal -f input=fardividevertical type=Float32 output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/far-divide-head_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.remove -f type=raster name=fardividevertical --overwrite
g.region raster=originalTile
r.out.gdal -f input=extractstream output=/Volumes/Ancillary/ease2n/ESA/tiles/terrain/x04y07/0/stream-rast_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif --overwrite
g.region raster=DEM
g.mapset -c mapset=x04y08
r.in.gdal input=/Volumes/Ancillary/ease2n/ESA/tiles/dem/x04y08/0/dem_copdem_x04y08_0_v01-pfpf-hydrdem4+4-90m.tif output=originalTile --overwrite
g.region raster=DEM --overwrite
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### DEM basin extraction    

```
0321_GrassOnetoManyTilesCopDEM-basin-extract-stage2_copDEM.json
```

One (additional) problem with extrating river basins is the definition of outlets points, a problem unrelated to the problem of hydrological flow routing. This particualr problem is, however, of relevance only if you intend to use the basin dat for hydrolocail modeling. If you only intend to use the basin map for extracting spatial statistics, you do not need an un-ambiguosly identifed outlet point. This part is for creating a distinct, single cell, outlet point for basins. It maske use of GRASS GIS and the Framework wrapper _GrassOnetoManyTiles_.



<button id= "toggleGrassOutletdiv" onclick="hiddencode('GrassOutletdiv')">Hide/Show 0321_GrassOnetoManyTilesCopDEM-basin-extract-stage2_copDEM.json</button>

<div id="GrassOutletdiv" style="display:none">

{% capture text-capture %}
{% raw %}
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
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "GrassOnetoManyTiles",
      "version": "1.3",

      "overwrite": true,
      "dryrun": false,
      "verbose": 1,
      "parameters": {
        "regionlayer": "DEM",
        "asscript": true,
        "mosaic": true,
        "subparameter": [
          {
            "g.region": {
              "raster": "originalTile"
            }
          },
          {
            "null": {
              "data": "\"$(r.stats -p input=landDEM null_value='null')\""
            }
          },
          {
            "null": {
              "null": "if echo \"$data\" | grep -q \"null\"; then"
            }
          },
          {
            "null": {
              "null": "echo \"Coastline tile\""
            }
          },
          {
            "g.region": {
              "raster": "DEM"
            }
          },
          {
            "r.watershed": {
              "flags": "a",
              "elevation": "inland_comp_DEM",
              "accumulation": "MFDupstream",
              "drainage": "MFDflowdir",
              "threshold": 617,
              "memory": 4000,
              "overwrite": true
            }
          },
          {
            "r.watershed": {
              "flags": "as",
              "elevation": "inland_comp_DEM",
              "accumulation": "SFDupstream",
              "threshold": 617,
              "memory": 4000,
              "overwrite": true
            }
          },
          {
            "g.region": {
              "raster": "originalTile"
            }
          },
          {
            "r.mapcalc": {
              "\"basin_SFD_outlets ": " if((shoreline > 0), if ((SFDupstream >= 617),SFDupstream,null() ) , null())\"",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"basin_MFD_outlets ": " if((shoreline > 0), if ((MFDupstream >= 617),MFDupstream,null() ) , null())\"",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"basin_shoreline_outlets ": " if ((isnull(basin_SFD_outlets)), basin_MFD_outlets, basin_SFD_outlets)\"",
              "overwrite": true
            }
          },
          {
            "r.to.vect": {
              "input": "basin_shoreline_outlets",
              "output": "basin_outlet_pt",
              "type": "point",
              "overwrite": true
            }
          },
          {
            "g.region": {
              "raster": "DEM"
            }
          },
          {
            "r.mapcalc": {
              "\"lowlevel_DEM ": " if((DEM <= 1), 0, null())\"",
              "overwrite": true
            }
          },
          {
            "r.cost": {
              "input": "lowlevel_DEM",
              "output": "lowlevel_outlet_costgrow",
              "start_points": "basin_outlet_pt",
              "max_cost": 0,
              "mem": 4000,
              "overwrite": true
            }
          },
          {
            "r.clump": {
              "flags": "d",
              "input": "lowlevel_outlet_costgrow",
              "output": "lowlevel_outlet_clumps",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"shoreline_DEM ": " if((shoreline > 0 && DEM <= 0), 0, null())\"",
              "overwrite": true
            }
          },
          {
            "r.cost": {
              "input": "shoreline_DEM",
              "output": "shoreline_outlet_costgrow",
              "start_points": "basin_outlet_pt",
              "max_cost": 0,
              "mem": 6000,
              "overwrite": true
            }
          },
          {
            "r.clump": {
              "flags": "d",
              "input": "shoreline_outlet_costgrow",
              "output": "shoreline_outlet_clumps",
              "overwrite": true
            }
          },
          {
            "r.to.vect": {
              "input": "shoreline_outlet_clumps",
              "output": "basin_all_outlets_pt",
              "type": "point",
              "overwrite": true
            }
          },
          {
            "v.db.addcolumn": {
              "map": "basin_all_outlets_pt",
              "columns": "\"upstream DOUBLE PRECISION, mfdup DOUBLE PRECISION, sfdup DOUBLE PRECISION, elevation DOUBLE PRECISION, mouth_id INT, basin_id INT\"",
              "overwrite": true
            }
          },
          {
            "v.what.rast": {
              "map": "basin_all_outlets_pt",
              "raster": "SFDupstream",
              "column": "sfdup",
              "overwrite": true
            }
          },
          {
            "v.what.rast": {
              "map": "basin_all_outlets_pt",
              "raster": "MFDupstream",
              "column": "mfdup",
              "overwrite": true
            }
          },
          {
            "v.what.rast": {
              "map": "basin_all_outlets_pt",
              "raster": "DEM",
              "column": "elevation",
              "overwrite": true
            }
          },
          {
            "v.what.rast": {
              "map": "basin_all_outlets_pt",
              "raster": "shoreline_outlet_clumps",
              "column": "mouth_id",
              "overwrite": true
            }
          },
          {
            "v.what.rast": {
              "map": "basin_all_outlets_pt",
              "raster": "lowlevel_outlet_clumps",
              "column": "basin_id",
              "overwrite": true
            }
          },
          {
            "db.execute": {
              "sql": "\"UPDATE basin_all_outlets_pt SET upstream=mfdup\"",
              "overwrite": true
            }
          },
          {
            "db.execute": {
              "sql": "\"UPDATE basin_all_outlets_pt SET upstream=sfdup WHERE sfdup>mfdup\"",
              "overwrite": true
            }
          },
          {
            "v.to.db": {
              "map": "basin_all_outlets_pt",
              "option": "coor",
              "columns": "xcoord,ycoord",
              "overwrite": true
            }
          },
          {
            "v.out.ogr": {
              "type": "point",
              "input": "basin_all_outlets_pt",
              "format": "ESRI_Shapefile",
              "output": "allOutletsfpn_s2",
              "overwrite": true
            }
          },
          {
            "r.buffer": {
              "input": "shoreline_outlet_clumps",
              "output": "mouth_buffer",
              "distances": 128.56,
              "units": "meter",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"thickwall ": " if((drain_terminal == 1 && mouth_buffer > 1), 1, null())\"",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"drain_terminal_remain ": " if((drain_terminal == 1),  if ((isnull(thickwall)),1, null() ),null() )\"",
              "overwrite": true
            }
          },
          {
            "r.buffer": {
              "input": "drain_terminal_remain",
              "output": "mouthshoreline",
              "distances": 128.56,
              "units": "meter",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"shorewall ": " if((mouthshoreline > 0 && thickwall == 1 && isnull(inland_comp_DEM)), 9999, null())\"",
              "overwrite": true
            }
          },
          {
            "r.to.vect": {
              "input": "shorewall",
              "output": "shorewall_pt",
              "type": "point",
              "overwrite": true
            }
          },
          {
            "v.out.ogr": {
              "type": "point",
              "input": "shorewall_pt",
              "format": "ESRI_Shapefile",
              "output": "shorewall_pt_out",
              "overwrite": true
            }
          },
          {
            "r.mapcalc": {
              "\"fillholeDEM ": " if ((thickwall==1 && isnull(shorewall)),9999, null() )\"",
              "overwrite": true
            }
          },


          {
            "null": {
              "null": "else"
            }
          },
          {
            "null": {
              "null": "echo \"Inland tile\""
            }
          },

          {
            "null": {
              "null": "fi"
            }
          }
        ]
      },
      "srcpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "dstpath": {
        "volume": "Ancillary",
        "hdr": "tif"
      },
      "srccomp": [
        {
          "*": {
            "source": "ESA",
            "product": "copdem",
            "content": "dem",
            "layerid": "copdem",
            "prefix": "dem",
            "suffix": "v01-pfpf-hydrdem4+4-90m"
          }
        }
      ],
      "dstcopy": [
        {
          "shorewall_pt_out": {
            "source": "copy",
            "product": "copy",
            "content": "basin-mouths-prel",
            "layerid": "shorewall-pt",
            "prefix": "shorewall-pt",
            "suffix": "v01-pfpf-hydrdem4+4-90m",
            "ext": ".shp"
          }
        },
        {
          "allOutletsfpn_s2": {
            "source": "copy",
            "product": "copy",
            "content": "basin-mouths-prel",
            "layerid": "outlets-pt",
            "prefix": "outlets-pt",
            "suffix": "v01-pfpf-hydrdem4+4-90m",
            "ext": ".shp"
          }
        }
      ]
    }
  ]
}
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

#### GRASS script

The above process generates a script file that loops over all the tiles belonging to the defined region. An example is shown under the Hide/show button below, the commented lines (starting with "#") have been added for explanation.

<button id= "toggleGrassScript2" onclick="hiddencode('GrassScript2div')">Hide/Show generated GRASS script for correcting DEM shorelines</button>

<div id="GrassScript2div" style="display:none">

{% capture text-capture %}
{% raw %}
```
# Change mapset to the present tile
g.mapset -c mapset=x04y07
# Import the original tile (always done when using virtual mosaics)
r.in.gdal input=/Volumes/Ancillary/ease2n/ESA/tiles/dem/x04y07/0/dem_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.tif output=originalTile --overwrite
# Set region to the original (smaller) tile before checking for basin outlets
g.region raster=originalTile --overwrite
# Check if there is any coastline in this tile
data="$(r.stats -p input=landDEM null_value='null')"
# If the tile include a coasline
if echo "$data" | grep -q "null"; then
  echo "Coastline tile"
  # Set region to the mosaic (expanded) DEM
  g.region raster=DEM --overwrite
  # run r.watershed Multiple Flow Directions (MFD) (if this is already done, this line can be commented out)
  r.watershed -a elevation=inland_comp_DEM accumulation=MFDupstream drainage=MFDflowdir threshold=617 memory=4000 --overwrite
  # run r.watershed Single Flow Directions (SFD) (if this is already done, this line can be commented out)
  r.watershed -as elevation=inland_comp_DEM accumulation=SFDupstream threshold=617 memory=4000 --overwrite
  # Set region to the original (smaller)
  g.region raster=originalTile --overwrite
  # Extract shorelines with SFD basins larger than a threshold
  r.mapcalc "basin_SFD_outlets = if((shoreline > 0), if ((SFDupstream >= 617),SFDupstream,null() ) , null())" --overwrite
  # Extract shorelines with SFD basins larger than a threshold
  r.mapcalc "basin_MFD_outlets = if((shoreline > 0), if ((MFDupstream >= 617),MFDupstream,null() ) , null())" --overwrite
  # Combnine SFD and MFD shoreline outlets
  r.mapcalc "basin_shoreline_outlets = if ((isnull(basin_SFD_outlets)), basin_MFD_outlets, basin_SFD_outlets)" --overwrite
  # Vectorize the outlet points
  r.to.vect input=basin_shoreline_outlets output=basin_outlet_pt type=point --overwrite
  # Set region to the mosaic (expanded) DEM
  g.region raster=DEM --overwrite
  # Use mapcalc to find all low level cells
  r.mapcalc "lowlevel_DEM = if((DEM <= 1), 0, null())" --overwrite
  # Costgrow from identifed output cells over lowlevel areas to find cells that belong to the same basin but are separated
  r.cost input=lowlevel_DEM output=lowlevel_outlet_costgrow start_points=basin_outlet_pt max_cost=0 mem=4000 --overwrite
  # Clump the costgrow analysis to create uniqure ids for each basin
  r.clump -d input=lowlevel_outlet_costgrow output=lowlevel_outlet_clumps --overwrite
  # Create a DEM overt just the shoreline
  r.mapcalc "shoreline_DEM = if((shoreline > 0 && DEM <= 0), 0, null())" --overwrite
  # Costgrow along the soreline to find all adjacent outlet cell belonging to the same basin
  r.cost input=shoreline_DEM output=shoreline_outlet_costgrow start_points=basin_outlet_pt max_cost=0 mem=6000 --overwrite
  # # Clump the costgrow analysis to create uniqure ids for each outlet mouth
  r.clump -d input=shoreline_outlet_costgrow output=shoreline_outlet_clumps --overwrite
  # vectorize the outlet moouths as points
  r.to.vect input=shoreline_outlet_clumps output=basin_all_outlets_pt type=point --overwrite
  # add columns to the point vector for outlet mouths
  v.db.addcolumn map=basin_all_outlets_pt columns="upstream DOUBLE PRECISION, mfdup DOUBLE PRECISION, sfdup DOUBLE PRECISION, elevation DOUBLE PRECISION, mouth_id INT, basin_id INT" --overwrite
  # Extract upstream SFD area to the point vector for outlet mouths
  v.what.rast map=basin_all_outlets_pt raster=SFDupstream column=sfdup --overwrite
  # Extract upstream MFD area to the point vector for outlet mouths
  v.what.rast map=basin_all_outlets_pt raster=MFDupstream column=mfdup --overwrite
  # Extract elevation to the point vector for outlet mouths
  v.what.rast map=basin_all_outlets_pt raster=DEM column=elevation --overwrite
  # Extract mouth id to point vector of all basin outlets
  v.what.rast map=basin_all_outlets_pt raster=shoreline_outlet_clumps column=mouth_id --overwrite
  # Extract bsin id to point vector of all basin outlets
  v.what.rast map=basin_all_outlets_pt raster=lowlevel_outlet_clumps column=basin_id --overwrite
  # update the point vector of all outlets
  db.execute sql="UPDATE basin_all_outlets_pt SET upstream=mfdup" --overwrite
  # update the point vector of all outlets
  db.execute sql="UPDATE basin_all_outlets_pt SET upstream=sfdup WHERE sfdup>mfdup" --overwrite
  # Extract cell coordinates for point vector of basin points
  v.to.db map=basin_all_outlets_pt option=coor columns=xcoord,ycoord --overwrite
  # Export point vector as ESRI shape file
  v.out.ogr type=point input=basin_all_outlets_pt format=ESRI_Shapefile output=/Volumes/Ancillary/ease2n/ESA/tiles/basin-mouths-prel/x04y07/0/outlets-pt_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.shp --overwrite
  # Buffer to get all pixels 1 diagonal cell within the shoreline
  r.buffer input=shoreline_outlet_clumps output=mouth_buffer distances=128.56 units=meter --overwrite
  # Get all cells 1 diagonal from the shoreline
  r.mapcalc "thickwall = if((drain_terminal == 1 && mouth_buffer > 1), 1, null())" --overwrite
  # get the part of the terminal drainige (e.g. the sea) lay adjacent to land
  r.mapcalc "drain_terminal_remain = if((drain_terminal == 1),  if ((isnull(thickwall)),1, null() ),null() )" --overwrite
  # buffer from the waterline to set anohter shoreline
  r.buffer input=drain_terminal_remain output=mouthshoreline distances=128.56 units=meter --overwrite
  # Combine the shoreline and waterline data to create a wall at 9999 m.a.s.l just outside the shoreline
  r.mapcalc "shorewall = if((mouthshoreline > 0 && thickwall == 1 && isnull(inland_comp_DEM)), 9999, null())" --overwrite
  # vectorize the shorewall
  r.to.vect input=shorewall output=shorewall_pt type=point --overwrite
  # export the shorewall as a point vector (ESRI shape file)
  v.out.ogr type=point input=shorewall_pt format=ESRI_Shapefile output=/Volumes/Ancillary/ease2n/ESA/tiles/basin-mouths-prel/x04y07/0/shorewall-pt_copdem_x04y07_0_v01-pfpf-hydrdem4+4-90m.shp --overwrite
  # Find any disrepancies in the initial (thicker or diagonal) wall and the final (thinner) wall, the "fillholeDEM" will be used in a later stage.
  r.mapcalc "fillholeDEM = if ((thickwall==1 && isnull(shorewall)),9999, null() )" --overwrite
else
  # No shoreline to work with in this tile
  echo "Inland tile"
fi
```
{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>
