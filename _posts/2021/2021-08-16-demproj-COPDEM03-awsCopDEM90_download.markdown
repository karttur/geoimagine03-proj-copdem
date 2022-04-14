---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Download CopDEM 90 m from AWS
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Download Copernicus DEM 90 m version from Amazon Web Services (AWS)"
tags:
  - DEM
  - CopDEM
  - AWS
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-08-16 T18:17:25.000Z'
modified: '2022-01-10 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is a manual for how to download and check the Copernicus DEM (CopDEM) 90 m version freely available from Amazon Web Services (AWS).

## Introduction

The AWS version of CopDEM 90 m is downloaded outside Karttur's GeoImagine Framework - all 26470 (!) tiles are downloaded with a single terminal command. To create a mosaic of these tiles, and then import (organise) them into the Framework is covered in this post.

## Prerequisits

You must have installed Karttur's GeoImagine Framework.

## AWS download

Installing and running Amazon Web Services Command Line Interface 2 (AWSCLIV2) is covered in the post [Global DEMs: access and download](../demproj-global-dems/). Skipping the details here, all you have to do to download the CopDEM 90 m version from AWS is to open a <span class='app'>Terminal</span> window, change directory (<span class='terminalapp'>cd</span>) to where you want to save the downloaded CopDEM tiles, and run the command:

<span class='terminal'>$ aws s3 cp s3://copernicus-dem-90m/ . \-\-recursive \-\-no-sign-request</span>

## A note on accessing the tiles

As noted above, the AWS dataset for CopDEM 90 consists of 26470 tif tiles. Each tile is located under an individual directory with the same name as the tif file, but without the _.tif_ extension. The <span class='app'>Finder</span> app on my MacOS does not manage to display the content of the directory where I downloaded the AWS tile - there are simply too many. I can use other software with Graphical User Interface (GUI) to view the content, including [<span class='app'>MuCommander</span>](https://www.mucommander.com) (available for all major operating systems). But in the end I turned to using terminal commands.

The download command above also retrieves some additional files apart from the tiles:

- readme.html
- tileList.txt (a list of all the tiles)
- grid.zip - a zip file with a vector shape file of all the tiles in geographic coordinates

To handle the files using the <span class='app'>Terminal</span> command line, <span class='terminalapp'>cd</span> to the parent folder where you downloaded the tiles (if you followed the steps above you are already in that very same directory already). To view <span class='file'>readme.html</span>

<span class='terminal'>$ open readme.html</span>

The tilelist is an ordinary text file (in this case similar to a csv file as there is only one column). To control the app to use when opening the text file, add the _-a_ flag to the <span class='terminalapp'>open</span> command:

<span class='terminal'>$ open -a atom tileList.txt</span>

Unzip <span class='file'>grid.zip</span> with the command:

<span class='terminal'>$ open grid.zip</span>

This should explode the content of the zip file to a sub-directory calls <span class='file'>grid</span>. <span class='terminalapp'>cd</span> to the exploded data:

<span class='terminal'>$ cd grid</span>

and then open the shape file:

<span class='terminal'>$ open dem90mGrid.shp</span>


it turns out that the grid file actually do not contain the DEM tiles, it contains all the 1 degree tiles (260 * 180 = 64800) that cover the entire globe, including oceans.

<figure>
<img src="../../images/CopDEM90_aws-rawtiles_bboxes.png">
<figcaption>AWS supplied grid over tiles.</figcaption>
</figure>

## Process chain

The process chain following the AWS download is available at [Kartturs GitHub account](#) and also found under the <span class= 'button'>Hide/show</span> button below. The process chain is an ordinary text file linking to a series of <span class='file'>json</span> files. Empty rows and rows starting with a "#" are ignored. When called from karttur's GeoImagine Framework, the processes of each json file are sequentially run. In the process chain below you can click on each json file name to see the complete content. The following sections also explain in more detail the processes of each json command file.

<button id= "toggleprocesschain" onclick="hiddencode('processchain')">Hide/Show Process chain</button>

<div id="processchain" style="display:none">

{% capture text-capture %}
{% raw %}

{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Bounding Boxes for raw tiles

The process <span class='process'>BBoxTiledRawData</span> creates bounding boxes for each raw tile. For the AWS CopDEM data that is not requires as the download also comes with a polygon vector with all the grids (see section on _A note on accessing the tiles_ above). If, for some reason, you need to create vector polygons of the downloaded data, then you can use the process <span class='process'>BBoxTiledRawData</span>. Because the CopDEM AWS dataset contains so many tiles, the process takes quite a long time.

The process generates a single vector data source with all the included tiles, this time restricted to only the actual tiles that contain data.

<figure>
<img src="../../images/CopDEM90_aws-actualtiles_bboxes.png">
<figcaption>Bounding boxes for the AWS version of Copernicus DEM 90 m raw tiles.</figcaption>
</figure>

As clearly seen in the figure above, the raw tiles are of arbitrary sizes with geographical coverage restricted to land. The next steps in the process chain converts these more arbitrary tiles to regular grids fitted to the projection system where you want to use the DEM.

![xspectre-connect-admin](../../images/CopDEM90_gs-nw-_bboxes.png){: .pull-left}
A closer scrutiny of the north-western corner of the map reveals that the tiles do not cover the entire western hemisphere, but leave a gap west of -179 degrees longitude. The map to the left shows the part of Russia between -180 and -170 degrees longitude and how the most western parts are not covered by the tiles of the CopDEM 90 m version available from gisco-services.

### Mosaic raw tiles

[0125_MosaicAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-90m.json/)

Downloaded data that are delivered as arbitrary tiles need to be mosaicked together before importing to the framework. (SMAP, MODIS and Sentinel data do not need mosaicking of raw tiles as these datasets either are downloaded in predefined tiling systems or as complete global datasets). The mosaicking must be done as virtual (<span class='file'>.vrt</span>) dataset. To actually create a true global mosaic at e.g 90 m, as in this example, would take a long time and also take up unnecessary much disk space. The raw tile mosaic is only going to be used as an intermediate for creating the projection system predefined tiles that will later be used for all further analysis. The process <span class='process'>MosaicAncillary</span> does not import (organize in the terminology of the Framework) into the database, that is done in the next step.

<figure>
<img src="../../images/CopDEM90_rawtiles_mosaic.png">
<figcaption>Virtual mosaic of the Copernicus DEM 90 m raw tiles.</figcaption>
</figure>

### Organise

[0125_MosaicAncillary_CopDEM-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-30m.json/)

With the CopDEM 90 m data downloaded, unzipped and mosaicked, the data can be organised for (or imported to) the Framework. For the Copernicus DEM data, the process _OrganizeAncillary_ as defined in the json object below only imports the virtual mosaic linked to the original (downloaded and unzipped) tiles.

Once you have run the json command, you can check in the Framework database, and you should find that the _ancillary_ schema contains a _layer_ with the _compid_ _dem_copdem_.

### Next step

The next step is tile the imported CopDEM to fit the projection system that you want to use for working with the data. The processing chain starting with the tiling, is, however, the same regardless of whihc version of CopDEM you choose to download, and is covered in the [next](#) instruction post.
