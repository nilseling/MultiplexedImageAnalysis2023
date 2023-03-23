# Multiplexed Image Analysis

This repository hosts the scripts to reproduce the analysis presented in the practical session of [Tumors and the immune system 2023 - High dimensional spatial profiling of tumor microenvironment](https://www.cb.uzh.ch/en/Education/Compulsory-courses/ModuleB.html)

## Obtain the repository

First, you will need to download the current repository. 
For this you can either use `git` (Linux and Mac OS via the Terminal/Command line):

```
git clone https://github.com/nilseling/MultiplexedImageAnalysis2023.git
```

or you can navigate to [https://github.com/nilseling/MultiplexedImageAnalysis2023](https://github.com/nilseling/MultiplexedImageAnalysis2023) and download the ZIP file under "Code" (green button on the top right).

## Install software

To follow along, please install the following software:

* [FIJI](https://imagej.net/software/fiji/downloads)
* [Ilastik](https://www.ilastik.org/download.html)
* [CellProfiler](https://cellprofiler.org/releases)
* [QuPATH](https://qupath.readthedocs.io/en/0.4/docs/intro/installation.html)

In addition, please also install the [FIJI plugin for Ilastik](https://www.ilastik.org/documentation/fiji_export/plugin#installation)

## Download the data

We use an example image for this tutorial, please download the `LuCa-7color_[13860,52919]_1x1component_data` image from [here](https://downloads.openmicroscopy.org/images/Vectra-QPTIFF/perkinelmer/PKI_fields/) and place it in the `data` folder.

## Follow along

The practical session is split into 4 sessions:

1. [File type conversion](#file-type-conversion)
2. [Image visualization with FIJI](#image-visualization-with-fiji)
3. [Pixel classification-based image segmentation](#pixel-classification-based-image-segmentation)
4. [Interactive image analysis with QuPATH](#interactive-image-analysis-with-qupath)

## File type conversion

To work with the image analysis software, we will first need to convert the image into different file types.

1. Generate a FIJI-readable image stack

As you can see when opening the image in FIJI, only the first channel is read in. 
Using the `BioFormats` FIJI plugin circumvents this issue but we can also use QuPATH to save the image in form of a FIJI-readable image stack.

- open QuPATH and drag the image into the software
- click on `File > Export images... > Original pixels` and save the image under a new name (no spaces, no dashes) in the `data` folder

2. Generate a HDF5 file for ilastik training

You can now load the new image in FIJI by dragging it in the user interface.
For `Ilastik` pixel classification, you will need the same image in HDF5 format:

- load the new image in FIJI
- click on `Plugins > ilastik > Export HDF5` and save the image as `<image name>.h5` (replace `<image name>` by the name of the image)

## Image visualization with FIJI

In the second practical session, we will learn simple operations in FIJI.

1. Split the multi-channel image into individual images

- load the image (TIFF format as exported by QuPATH) into FIJI
- click the `Image > Stacks > Stack to Images` button

2. Generate a 3 color RGB image

- click on `Image > Color > Merge channels...` and select CK in red, CD8 in green and FoxP3 in blue

3. Watershed segment the nucleii

- close all images except the DAPI channel
- click on `Image > Lookup Tables > Grays`
- apply a Gaussian blur by clicking `Process > Filters > Gaussian Blurr` with a sigma value of 2
- next, click `Image > Adjust > Threshold` and select Default, B&W and click Apply
- now you can click on `Process > Binary > Watershed` which will segment the image
- finally, `Analyze > Analyze particles` will analyse individual nucleii

## Pixel classification-based image segmentation 

We saw that the previous way of segmenting nucleii did not result in great segmentation efficiency.
In the next session, we will use Ilastik pixel classification together with watershed-based segmentation to segment whole cells.

### Train an Ilastik pixel-classifier

1. Open Ilastik and create a new Pixel classification project
2. Under `Input Data` select the previously generated HDF5 file
3. Under `Feature Selection` select all features
4. Under `Training` generate three classes: `nucleus`, `cytoplasm`, `background`
5. Under `Raw Input` you can change the channel that is being displayed
6. Label pixels as nuclear, cytoplasmic and background
7. Under `Export Settings` select `Convert to Data Type` --> `integer 16-bit` and press `Renormalize`. Select `Format` --> `tiff`
8. Under `Batch Processing` click on `Select Raw Data Files...` and select the HDF5 file
9. Press `Process all files`

### Segment the pixel probabilities using CellProfiler

1. Open `CellProfiler`
2. Click on `File > Import > Pipeline from File...` and select the `segment_ilastik.cppipe` file.
3. Follow the instructions in the pipeline file

## Interactive image analysis with QuPATH

Create project

Annotation

Cell detection 

Tumor classification

Positivity detection

 - Add smoothed features
 - Draw tumor and stroma region and classify cells
 - 

