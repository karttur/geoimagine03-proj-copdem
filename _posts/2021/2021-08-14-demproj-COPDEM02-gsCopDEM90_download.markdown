---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Download CopDEM 90 m from gisco-services
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Search and download Copernicus DEM 90 m version from gisco-services using Kartturs GeoImagine Framework"
tags:
  - DEM
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-08-14 T18:17:25.000Z'
modified: '2022-01-10 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is a manual for how to use Karttur's GeoImagine Framework for searching, downloading and checking the Copernicus DEM (CopDEM) 90 m version freely available at [gisco-services](https://gisco-services.ec.europa.eu). Note that at time of writing this manual (January 2022), the gisco-services version of CopDEM 90 m has a data gap between -180 and -179 degrees latitudes. Thus, if you need a truly global version of the CopDEM 90 m, you should use the processing chain in the post using the [AWS version of the CopDEM 90 m](#).

## Introduction

Searching, downloading, unzipping and checking tiled datasets available online, is a typical task for which Karttur's GeoImagine Framework was built.

## Prerequisits

You must have installed Karttur's GeoImagine Framework.

## Process chain

The process chain is available at [Kartturs GitHub account](#) and also found under the <span class= 'button'>Hide/show</span> button below. The process chain is an ordinary text file linking to a series of <span class='file'>json</span> files. Empty rows and rows starting with a "#" are ignored. When called from karttur's GeoImagine Framework, the processes of each json file are sequentially run. In the process chain below you can click on each json file name to see the complete content. The following sections also explain in more detail the processes of each jscon command file.

<button id= "toggleprocesschain" onclick="hiddencode('processchain')">Hide/Show Process chain</button>

<div id="processchain" style="display:none">

{% capture text-capture %}
{% raw %}

\# Search online products for CopDEM 90 m version
[0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/)

\# Download online products for CopDEM 90 m version
[0110_DownloadCopernicus_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0110_DownloadCopernicus_CopDEM-90m.json/)

\# Explode (UnZip) donwloaded CopDem 90m products
[0120_UnZipRawData_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0120_UnZipRawData_CopDEM-90m.json/)


{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Search and download

Search and download differ for almost all datasets. Details for searching and different DEMs is in the post [Digital Elevation Models (DEMs)](../demproj-global-DEMs/).

#### Search

[0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/)

The search for the Copernicus DEM 90 m product is done using the special process _SearchCopernicusProducts_. The search requires that you installed <span class='terminalapp'>wget</span>, see the post on [Python virtual environment](https://karttur.github.io/geoimagine03-docs-main/prep/prep-conda-environ/), that also covers installing <span class='terminalapp'>wget</span>.

The process <span class='process'>SearchCopernicusProducts</span> results in an <span class='file'>html</span> file with url links to all the resources to download. For the process defined in [0100_SearchCopernicusProducts_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0100_SearchCopernicusProducts_CopDEM-90m.json/), the result is stored in the file <span class='file'>copernicusDEM90.html</span>

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

[0110_DownloadCopernicus_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0110_DownloadCopernicus_CopDEM-90m.json/)

Also downloading the Copernicus DEM 90 m version is defined with a customized process, _DownloadCopernicus_. This process reads the html file above (<span class='file'>copernicusDEM90.html</span>), extracts the url tag (\<a\>), looks for a local copy of the DEM tile, and if not found (or _overwrite_ set to _true_) downloads the tile. The downloading is predefined to a path on the destination disk and can not be changed. If you want to store that data somewhere else you have to move them manually, but the system will hten lose track of them. The original tiles are not registered in the database. It is thus recommended that you leave the original data in the predefined location (for the data used here the default path is _/DISKPATH/DOWNLOADS/CopernicusDem90_).

### Unzip

[0120_UnZipRawData_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0120_UnZipRawData_CopDEM-90m.json/)

Downloaded data that is retrieved as zip files can be unzipped with the command _UnZipRawData_. This is a generic process, and you need to give the full path to both the source (zip) and target (expanded) datasets. The disk (volume) path is as usual given as the parameters _srcpath_ and _dstpath_, but additioally you must also give the directories on the disks for both _srcdir_ and _dstdir_ under _parameters_. If _srcdir_ is a file, the script assumes that it is a list of the files to unzip; if _srcdir_ is a directory the script will explode all zip files in that directory.

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

### Bounding Boxes for raw tiles

The process <span class='process'>BBoxTiledRawData</span> creates bounding boxes for each raw tile. In the example used here, each tile to include is listed in a csv file, created in a similar way as the list of zip files in the provious section.

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

![xspectre-connect-admin](../../images/CopDEM90_gs-nw-_bboxes.png){: .pull-left}
A closer scrutiny of the north-western corner of the map reveals that the tiles do not cover the entire western hemisphere, but leave a gap west of -179 degrees longitude. The map to the left shows the part of Russia between -180 and -170 degrees longitude and how the most western parts are not covered by the tiles of the CopDEM 90 m version available from gisco-services.

### Mosaic raw tiles

[0125_MosaicAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-90m.json/)

Downloaded data that are delivered as arbitrary tiles need to be mosaicked together before importing to the framework. (SMAP, MODIS and Sentinel data do not need mosaicking of raw tiles as these datasets either are downloaded in predefined tiling systems or as complete global datasets). The mosaicking must be done as virtual (<span class='file'>.vrt</span>) dataset. To actually create a true global mosaic at e.g 90 m, as in this example, would take a long time and also take up unnecessary much disk space. The raw tile mosaic is only going to be used as an intermediate for creating the projection system predefined tiles that will later be used for all further analysis. The process <span class='process'>MosaicAncillary</span> does not import (organize in the terminology of the Framework) into the database, that is done in the next step.

<figure>
<img src="../../images/CopDEM90_rawtiles_mosaic.png">
<figcaption>Virtual mosaic of the Copernicus DEM 90 m raw tiles.</figcaption>
</figure>

### Organzie

[0125_MosaicAncillary_CopDEM-30m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-30m.json/)

With the CopDEM 90 m data downloaded, unzipped and mosaicked, the data can be organised for (or imported to) the Framework. For the Copernicus DEM data, the process _OrganizeAncillary_ as defined in the json object below only imports the virtual mosaic linked to the original (downloaded and unzipped) tiles.

Once you have run the json command, you can check in the Framework database, and you should find that the _ancillary_ schema contains a _layer_ with the _compid_ _dem_copdem_.

### Next step

The next step is tile the imported CopDEM to fit the projection system that you want to use for working with the data. The processing chain starting with the tiling, is, however, the same regardless of whihc version of CopDEM you choose to download, and is covered in the [next](#) instruction post.
