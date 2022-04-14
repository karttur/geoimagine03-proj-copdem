---
layout: post
project: "Framework project: Digital Elevation Models (DEMs)"
title: Setup processing for Copernicus DEM
categories: copdem
datasource: dem
biophysical: elevation
excerpt: "Setup Kartturs GeoImagine Framework for processing Copernicus DEM"
tags:
  - DEM
image: avg-trmm-3b43v7-precip_3B43_trmm_2001-2016_A
date: '2021-08-12 T18:17:25.000Z'
modified: '2022-01-10 T18:17:25.000Z'
comments: true
share: true
figure1: soil-moisture-avg_SPL3SMP_global_2015-2018@D001_005

---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post is the first in a series of manuals for how to process the global Digital Elevation Model (DEM) provided by the European Space Agency (ESA) Copernicus program. This first part deals with setting up a user defined project inlcuding a spatial processing region (called _tract_ in the Framework), and how to add basic data on the layout of DEM data to the Framework.

Copernicus DEM is available in different resoltuons as well as from different online repositories. The next three posts in this series deals with three didifferent alterantives for accessing and importing the Copernicus DEM data to the Framework. You need to select one of these. At time of writing this (January 2022) the 90 m version available from [gisco-service](https://gisco-services.ec.europa.eu) lacks data between -180 and -179 degrees latitude. Thus I recommend using one of the AWS alternatives; alternatives for downloading and organising Copernicus DEM (CopDEM):

- CopDEM 90 m from gisco-services
- CopDEM 90 m from AWS
- CopDEM 30 m from AWS

After you have downloaded and organised the CopDEM version you want to process, the processing is the same, covered by the following insturction-posts:

- Fisksoppa
- Gurkstuvning

## Introduction

Processing a global DEM all the way from downloading tiles to create coherent indices and metrics of landscapes, is a typical task for which Karttur's GeoImagine Framework was built. Once the DEM is imported and organized within the Framework, it is easy to test different algorithms and visualizations. If you have access to some ground data you can also apply the Framework for comparing the results of different algorithms and parameters against ground data, and thus select the most appropriate landscape model.

## Prerequisits

You must have installed Karttur's GeoImagine Framework. You also need a registered project that covers the geographic regions of the DEM you want to process. The latter can, however, be done as part of the project itself.

## Process chain

The process chain is available at [Kartturs GitHub account](#) and also found under the <span class= 'button'>Hide/show</span> button below. The process chain is an ordinary text file linking to a series of <span class='file'>json</span> files. Empty rows and rows starting with a "#" are ignored. When called from karttur's GeoImagine Framework, the processes of each json file are sequentially run. In the process chain below you can click on each json file name to see the complete content. The following sections also explain in more detail the processes of each jscon command file.

<button id= "toggleprocesschain" onclick="hiddencode('processchain')">Hide/Show Process chain</button>

<div id="processchain" style="display:none">

{% capture text-capture %}
{% raw %}

\# Create project
[0000_ManageDefRegProj_copdem-ease2n.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/)

\# Create scaling for fitting DEM data to byte range
[0001_CreatesScaling_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0001_CreatesScaling_DEM/)

\# Add raster paletter for byte range DEM data
[0002_AddRasterPalette_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/)

\# Create legend for byte range DEM data
[0003_createlegends_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0003_createlegends_DEM/)

\# Export legend for DEM data
[0005_exportlegend_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0005_exportlegend_DEM/)

{% endraw %}
{% endcapture %}
{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Create a project

[0000_ManageDefRegProj_copdem-ease2n.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/)

Data specific processing requires that your user has a predefined project that includes a predefined _tract_, where the _tract_ **must** be linked to a default region. All spatial processing that you process from that user and with that project will then operate on system tiles linked to parent default region of your project _tract_. The whole processing is set up in thin manner to minimise double processing.

Unless you have done so before you must setup the project and tract to use for processing the Copernicus DEM data. The example is for processing the DEM for the entire basins draining into Arctic waters. Thus the example assign the default region _northlandease2n_ as the parent region. Both the user specific project and its region are called _karttur-northlandease2n_.

### Layout

To produce symbolised and labeled maps using the Framework, you have to define some Layout features and parameters. All map layout is done using unsigned byte range maps with values ranging from 0 to 255, Values 251 to 255 are reserved for specific purposes and not allowed for representing map values. The Framework Layout processes include:

- CreateScaling,
- AddRasterPalette,
- CreateLegend, and
- ExportLegend.

#### Create Scaling

[0001_CreatesScaling_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0001_CreatesScaling_DEM/)

The _CreateScaling_ process converts any input raster into an unsigned byte (0-255) range file. The byte file is then used for assigning a color ramp (defined in the process _AddRasterPalette_, see next section).

Scaling is thus not defined on the fly, instead each map composition (see the post on ??? for an explanation on the Framework map composition concept) must be associated with a predefined scaling. Once defined, this scaling can not be changed.

#### Add Raster Palette

[0002_AddRasterPalette_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0002_AddRasterPalette_DEM/)

In contrast to scaling, a _palette_ can be freely set for any map composition (but always using the same scaling). The _palette_ to use for any map layout is defined when calling the process that generates the layout. Thus the _palette_ must exist beforehand, created by using the process _AddRasterPalette_.

#### Create Legend

[0003_createlegends_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0003_createlegends_DEM/)

With the scaling set and a palette defined, you can define a legend - that is how the combined scaling and palette will be presented in graphical layouts.

#### Export Legend

[0005_exportlegend_DEM_v090.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0005_exportlegend_DEM/)

Once created you can export a legend. All legends are experted as png, csv and pdf. Exporting a legend requires that you have installed the application [<span class='app'>inkscape</span>](https://inkscape.org) as outlined in the post [Python virtual environment](https://karttur.github.io/geoimagine03-docs-main/prep/prep-conda-environ/). The exported legends are stored under a director <span class='file'>legends</span> on the destiantion disk/volume. The image represented the raster palette is under a subfolder <span class='file'>images</span> (saved as a png image) adn the legends with text added are under a subfolder <span class='file'>layouts</span>, and saved as three different versions:

- svg
- png
- pdf
