---
description: >-
  This is the technical documentation for the Goobi LayoutWizzard plugin for
  automated cropping of book page scans..
---

# LayoutWizzard

## 1. Introduction

LayoutWizzard is a tool for analyzing digitized book pages and similar materials, which recognizes the position of the physical page in the digitized image and can align and cut the image accordingly.

| Details | ​ |
| :--- | :--- |
| Plugin version | 1.0.0 |
| Identifier | intranda\_step\_crop |
| Source code | - Source code not yet publicly available - |
| Compatibility | Goobi workflow 3.0 |
| Documentation date | 07.02.2016 |

## 2. Installation <a id="2-installation"></a>

The installation consists of a total of four program libraries that must be accessible in Apache Tomcat or Goobi:

| Datei | Speicherort |
| :--- | :--- |
| `opencv.jar` | In Tomcat lib folder |
| `layoutwizzard.jar` | In the lib-folder of Goobi webapp in Tomcat |
| `plugin_intranda_step_LayoutWizzard.jar` | In the plugins/step folder in the Goobi working directory |
| `plugin_intranda_step_LayoutWizzard-GUI.jar` | In the plugins/GUI folder in the Goobi working directory |

Additionally, two configuration files are required, one for the Goobi plugin, and one for the underlying LayoutWizzard library.

The configuration file of the plugin `plugin_LayoutWizzardPlugin.xml` must be located in the `config` configuration directory within the Goobi working directory. This is usually the same as the path to the file:

```text
/opt/digiverso/goobi/config/plugin_LayoutWizzardPlugin.xml
```

‌Within this file the path to the actual configuration file of the LayoutWizard is specified. The structure of this file is as follows:

{% code title="plugin\_LayoutWizzardPlugin.xml" %}
```markup
<config_plugin>
        <layout-wizzard-config-path>
                /opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
        </layout-wizzard-config-path>
</config_plugin>
```
{% endcode %}

The actual configuration file specifies various parameters for the layout recognition process. These parameters are listed as examples in the following configuration file. This is located under the following path, as defined in the plug-in configuration file:

```text
/opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
```

For example, this configuration file has the following contents:

{% code title="layoutwizzard\_config.xml" %}
```markup
<!-- intranda Layout Wizzard configuration file -->
<config>
    <contentServerUrl>http://demo03.intranda.com/goobi/cs/cs</contentServerUrl>
    <defaultOutputFolderSuffix>media</defaultOutputFolderSuffix>
    <analysisImagesBasePath>/home/florian/LayoutWizzard/samples/</analysisImagesBasePath>
    <previews>
        <previewsPerPage>100</previewsPerPage>
        <maxPreviewsCached>10000</maxPreviewsCached>
        <previewWidth>700</previewWidth>
        <imageHeightLarge>800</imageHeightLarge>
    </previews>
    <outliers>
        <errorMultiplier>3.0</errorMultiplier>
        <weightExponent>2.0</weightExponent>
    </outliers>
    <saving>
        <defaultCompression quality="85">NONE</defaultCompression>
        <overwriteExistingImages>true</overwriteExistingImages>
    </saving>
    <analysis id="firstPageLeft">
        <firstPageOrientation>LEFT</firstPageOrientation>
    </analysis>
    <analysis id="firstPageRight">
        <firstPageOrientation>RIGHT</firstPageOrientation>
    </analysis>
    <analysis>
        <analysisStep name="PAGESKEW" type="edges" use="true">
            <saveAnalysisImages visibility="INVISIBLE" path="DESKEW">false</saveAnalysisImages>
            <deskewerMode visibility="VISIBLE">ALL_EDGES</deskewerMode>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode>GROUP_BY_DISTANCE</lineGroupingMode>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <analysisImageSize>300</analysisImageSize>
            <lowerCannyThreshold>70</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>10</minHoughLineLength>
            <houghLineThreshold>50</houghLineThreshold>
            <maxHoughLineGapSize>2</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>7</maxLineDistance>
            <rimAreaToIgnoreLines>0.0</rimAreaToIgnoreLines>
        </analysisStep>
        <analysisStep name="CONTENTAREA" use="true">
            <analysisImageSize>0</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="edgeDetection">false</saveAnalysisImages>
            <bitonalThreshold>150</bitonalThreshold>
            <bitonalInvert>false</bitonalInvert>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <contentPadding visibility="VISIBLE">0</contentPadding>
        </analysisStep>
        <analysisStep name="BOOKSPINE" use="true">
            <analysisImageSize>400</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="spineDetection">false</saveAnalysisImages>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode visibility="INVISIBLE">GROUP_BY_DISTANCE</lineGroupingMode>
            <croppingAggressiveness visibility="VISIBLE">BALANCED</croppingAggressiveness>
            <lowerCannyThreshold>40</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>20</minHoughLineLength>
            <houghLineThreshold>10</houghLineThreshold>
            <maxHoughLineGapSize>4</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>5</maxLineDistance>
            <featureSizeThreshold>0.1</featureSizeThreshold>
            <rimAreaToIgnoreLines>0.0125</rimAreaToIgnoreLines>
            <maxGroupAngleDeviation visibility="INVISIBLE">10</maxGroupAngleDeviation>
            <spineOffset visibility="VISIBLE">5</spineOffset>
        </analysisStep>
    </analysis>
</config>
```
{% endcode %}

## 3. Basic operation <a id="3-grundsaetzliche-arbeitsweise"></a>

The Goobi plugin LayoutWizzard \(hereinafter referred to as plugin\) allows the complete analysis of images and the subsequent saving of edited images. The terms analysis and editing are used synonymously and both refer to the image analysis of the source images in several steps.

![Figure 1: The LayoutWizard interface is embedded in Goobi as a plugin with its own user interface.](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkp-640LX3Gv-sytgT%2Flayoutwizzard_01.png?alt=media&token=306839dc-5b02-4ea2-9f0d-8fa85fe2c0bb)

Usually, the following three steps are performed during analysis and processing.

**1. Align:**   
The rotation of the book page relative to the edges of the image is determined in order to rotate it in the output image so that it is as horizontal as possible.

**2. Cropping:**   
The dimensions of the page or other objects are determined and a rectangular frame around the page or object is calculated. The output image contains only the contents of this rectangle, all external image contents are truncated.

 **3. Determine book spine:**   
The position of the book spine is determined. The right or left edge of the image is cut off at it. Since the alignment of the right or left book page is decisive for determining the book spine, this alignment is calculated by the plugin and can be adjusted by the user. If the material to be examined is not a bound book, the book spine recognition step can be deactivated. The orientation of the pages then has no meaning.

The analysis of all images is started by clicking on Edit all images. The results of the analysis are saved together with the used analysis configuration in the file `imageData.xml`, always by clicking on `Save and Exit` in the bottom right corner of the plugin interface or by clicking on Save results in the preview view. This file will then be used when reloading the plugin to restore the previous analysis results. The file will be deleted if you click `Discard and Restart`.

After completing the analysis, you can save the images rotated and cropped according to the analysis results to the output folder via `Open Save View`.

## 4. Using the Taskmanager-Plugins <a id="4-verwendung-der-taskmanager-plugins"></a>

Since both the analysis and the saving of the images can take a long time with large tasks, these automatic tasks are usually outsourced to the intranda Task Manager, where two plugins are available for these two tasks.

The Goobi plugin then takes over the analysis results from the TaskManager analysis plugin and displays that all images have been processed when it is opened. To check the results, you can switch directly to the preview view and make all necessary adjustments to the results from there. If one is finished with this check, one leaves the plugin via `Save Results and Exit`, so that the TaskManager memory plugin can create the output images on the basis of the results. Of course, you can also open the Goobi plugin again to continue checking and correcting the results. Only when the control is completely finished does one complete the step to send the results to the TaskManager plugin for saving.

![Figure 2: The jobs for analyzing and saving the images are processed as automatic tasks in the TaskManager.](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkpJVbzHtM8ZZiiriK%2Flayoutwizzard_02.png?alt=media&token=fe081bbc-1921-4c48-865d-29416ad029ec)

## 5. Operation of the user interface <a id="5-bedienung-der-benutzeroberflaeche"></a>

The surface of the plugin consists of three areas: On the left side there are several boxes with LayoutWizard settings, on the right side there is a display of the currently processed image or the first image to be processed if no processing is taking place yet. In the lower area there is a list of the file names of all images to be edited. Furthermore, there is a preview view, which is realized as a separate page and allows a quick control of all images.

![Figure 3: General interface of the LayoutWizard](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkpULAexvLZy4GMQRh%2Flayoutwizzard_03.png?alt=media&token=8dac4c53-7406-4fa7-b89c-c6120a548069)

### 5.1. Settings area <a id="5-1-bereich-fuer-die-einstellungen"></a>

The division of the left area depends essentially on whether you are working in the simple view or the expert view. The views can be changed using the gear symbol at the top right of the display. The simple view is usually sufficient for normal operation. The expert view opens additional options for selecting the images and for the individual analysis steps.

#### 5.1.1. Setting range in normal mode <a id="5-1-1-einstellungsbereich-im-normalmodus"></a>

In the normal view, two boxes are always available for general navigation in the plugin. These are explained below.‌

**5.1.1.1.** Box for the steps

The `Steps` box displays the work steps of the LayoutWizard that it goes through when analyzing an image.

![Figure 4: Box for the steps](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkqbWl6lH8jX5xxLys%2Flayoutwizzard_04.png?alt=media&token=fe47835d-82b8-40f3-b28b-cce135840d5a)

These steps are performed in the order in which they are listed here. Each step can be switched on or off using the `Active`/`Inactive` button. Clicking on the button This step takes you directly to the image analysis of this step to apply a specific step to one or more images or to change the configuration of the step. All steps already performed for this image are highlighted in green. The currently selected step is also highlighted in blue.

**5.1.1.2.** Box View

The box `View` provides options for general editing of the entire image set.

![Figure 5: Box for the view in the left area](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkradidA0VTUKOHn9C%2Flayoutwizzard_05.png?alt=media&token=0de144af-9aca-479c-a126-41f14a2bbb47)

The following options are available in the View box:‌

**5.1.1.2.1. edit all images** This button executes all active steps of the LayoutWizard for each selected image. All images in the input folder that have not been sorted out by a possible filtering are considered as selected, these are exactly the images that are listed in the Image List section. In the Goobi workflow, this function is usually taken over by a Task Manager plug-in, as it is time-consuming and computational. In this case, you can generally ignore the button and go directly to the result control. The button is highlighted in green if the images have not yet been processed.

**5.1.1.2.2 To preview** This button opens the preview view. It is used to check the analysis results of the LayoutWizard. See also the corresponding section.

**5.1.1.2.3 Back to image selection** This button exits the currently selected step or memory view and opens the fields for image selection and filtering in expert mode.

**5.1.1.2.4 Save View** This button opens the memory mode, which displays the images as they would be saved in the current analysis status. In addition, the save mode offers options to save one or more processed images to the output folder. Again, saving many images is time consuming and this function is usually performed by a subsequent Task Manager job. The memory view can be ignored in this case.

**5.1.1.1.2.5 Discard and Restart** This button discards all previous analysis results and re-reads the LayoutWizzard configuration file with the basic configuration. Process-specific configurations are ignored. In addition, this function deletes the file in which the current configuration of the operation and the analysis results are persistently stored. The call of this function can therefore not be undone. Of course, the analysis can be repeated at any time. It is important to note that the analysis results file will be re-created as soon as you click the Save and Exit button. This will also save the last imported configuration for this process. If you want to repeat the automatic analysis with the process-specific configuration, you must exit the plugin with the Cancel button.

**5.1.2 Settings Area in Expert Mode** In expert mode, two additional boxes are visible for image selection and general settings for all images. However, these boxes do not appear when an operation or memory view is active. In this case, you must first click Back to Image Selection to make these fields accessible.

**5.1.2.1 Box for Folder and File Options** In the Box Folder and File Options, one can change the image folders and edit further settings for the file export.

![Figure 6: Box for folder and file options in the left pane](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZksN1MbTM4S4K5gzil%2Flayoutwizzard_06.png?alt=media&token=a85f4fbb-172d-4261-9c59-7b2566fa6bf7)

The following functions are available in the Folder and File Options User Box.

**5.1.2.1.1 Input folder** The input folder is usually a directory within the images folder of the Goobi process from which the images are read. This is usually the master or orig folder. However, other folders can also be selected via a drop-down menu.

**5.1.2.1.2 Output folder** The output folder is the directory within the images folder of the Goobi process in which the analysis results are stored. Here you can also specify a folder that does not yet exist, which will then be created during saving. This setting only affects the saving of images in this plugin. The output folder for the Taskmanager plugin must be passed to it in the corresponding Goobi workflow step.

**5.1.2.1.3 Compression of the output images** The function for compressing the output images can be selected between 'no compression' or 'JPEG compression'. The images are generally saved in Tiff file format.

Please note: JPEG compression is currently only partially supported: The compressed output images lose any image metadata of the original image, especially information about the image resolution \(DPI\). Therefore, you should avoid compression during normal operation.

**5.1.2.1.4 Mark outliers**  After the image analysis, all images are checked to see whether they have values for rotation, size of the cropping area or the position of the book fold that differ greatly from the neighbouring images. Such conspicuous images are marked in color as outliers in the preview view and the image list. With the option Mark outliers you can set whether all outliers should be marked in this way or only the outliers of a certain step.

**5.1.2.1.5 Alignment of the first page** With the option of alignment of the first page you can determine whether the first page of the work is a left or right page. This alignment is used to determine the fold position and, if necessary, the size of the cutting area - see the next point. Changing this setting simultaneously changes the alignments of all pages in the process so that a right page always follows a left page and vice versa. Exceptions to this rule can be created via the image display. In addition, changing the orientation means that the folding position of all images must be recalculated. The processing progress for all images is therefore reset before the analysis of the fold position if necessary. To recalculate the folding position, you can then use the Edit all images option.

**5.1.2.1.6 Adjust cropping areas** You may want to adjust the area to which the image is cropped so that it is the same for all images, or at least for two opposite pages at a time, so that they can be displayed in a double-sided view at the same size and height. For this purpose, there are the following settings in this point.

_No adjustment_ No adjustment of the cropping area between images takes place.

_Opposite pages, left to right_ Opposite pages are adjusted so that they are truncated to the same height and the trimming areas always have the same height. It is assumed that one left side is always followed by the opposite right side.

_Opposite pages, right to left_ Like the previous point, but here it is assumed that the right side is followed by the opposite left side - like fonts that are read from right to left \(e.g. Hebrew\).

_All pages_ Same as the previous points, but the alignment is done for all pages so that all cropped images have the same height.

**5.1.2.2 Filter options** In the Filter options box, the pages to be edited can be restricted to all right or left pages or to an explicit selection of individual images.

![Figure 7: Sample specification of a filter option](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZktFa5eEuyPJgvX65p%2Flayoutwizzard_07.png?alt=media&token=185c4487-ba6e-4afb-bcb7-7abbfa0530ce)

This selection of images within the box can be entered either as a comma-separated list or as a range. The range must be specified in the form `Image1...Image2`, where `Image1` is the first image to be selected and `Image2` is the last image. All images whose file names are sorted alphabetically between `Image1` and `Image2` are also selected. Filtering to `Right Side Only` or `Left Side Only` is mutually exclusive, but both can be combined with Filtering to Image Selection.

#### 5.1.3. Boxes within the individual work steps <a id="H5.1.3.BoxeninnerhalbdereinzelnenArbeitsschritte"></a>

If a work step is activated, a box with the respective step name is visible in expert mode instead of the `Folder and Filter Options` box. This box allows this step to be performed for only the current image, all images or for this image and all subsequent images in the image list using the corresponding buttons. Please note that the execution of a step may depend on the previous steps. Therefore, before performing this step, all previous steps will be performed automatically..

![Figure 8: Settings for a work step using the example of the fold recognition work step](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkn_PEtMrNJyg3Pwiw%2F-LZkteAjZG95HM4LDFhS%2Flayoutwizzard_08.png?alt=media&token=659b25a7-53f1-4079-9ba5-ba8fcabf66f0)

In addition, this box lists some additional configuration parameters for the current work step, which can be changed to adjust the analysis results. For example, an additional frame can be placed around the cropping area. Changing these parameters will affect all future processing steps for all images in this process. This also applies to an analysis stored in the Task Manager if it is done after changing the parameters. The parameters can only be reset by the option Discard and restart. 

This box also has its own gear icon in the menu bar to display additional parameters. The parameters displayed in this way usually have a direct effect on the analysis algorithm and should not be changed without reason.

### 5.2. Image display area <a id="5-2-bereich-fuer-die-bildanzeige"></a>

The Image display area displays the currently selected picture. Images can be selected from the image list at the bottom of the page or from the Previous/Next Image or Previous/Next Outlier buttons above the image.

![Figure 9: Explanation of the image display area](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZku01rLziXSRR9XWLG%2Flayoutwizzard_09.png?alt=media&token=4cc9303c-6a63-4e91-915c-58da16f9c2a3)

The alignment - right side or left side - of the current image can also be set above the image. This setting also affects all subsequent images, assuming that left and right images alternate.

The image display serves to make the results of the analysis immediately visible on the image and to adjust them if necessary. This adjustment can be done by changing the parameters of the corresponding step as well as by manually correcting the image to overwrite the analysis results. In the following, the special display elements for the individual steps are described in more detail.

#### 5.2.1. Step: Align page <a id="H5.2.1.Arbeitsschritt:Seiteausrichten"></a>

To align a page, image lines are searched that could serve as edges of the current page. If this step was edited in the Goobi plug-in, all lines in question are displayed in blue in the image. These lines are grouped together to form groups that unite all the lines of a related image feature, such as a side edge. The contour lines of the image features calculated from these groups are displayed as white lines, also only if the editing was done in the plugin. From these contour lines the probable edges of the book page are selected, which are represented as thick red lines and from whose average inclination the orientation of the page is calculated.

![Figure 10: Alignment step for a page with activated expert settings](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZku7p6VF-olZhkZOcf%2Flayoutwizzard_10.png?alt=media&token=66de9363-d3c6-4003-9ea8-e0d2fd6d5238)

To manually align the page if the automatic analysis does not produce satisfactory results, there is a thick blue horizontal guide line at half height of the image that can be clicked and moved with the mouse to rotate the image. In order to allow the image to be aligned as horizontally as possible, there is also a crosshair consisting of two red lines perpendicular to each other and a gray center, which can be moved as desired to align edges horizontally or vertically.

A rotation set in this way is evaluated directly as an analysis result and applied to the output image.

#### 5.2.2. Step: Crop page <a id="H5.2.2.Arbeitsschritt:Seitezuschneiden"></a>

In order to crop a page, a frame is searched in which all objects found in the image, or more precisely all determined contours, fit. This analysis is made in the rotated image when rotation is activated. The frame is directly the border of the output image.

![Figure 11: Step for recognizing a page on the image with expert settings activated](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkuEkbm6llJgjADV44%2Flayoutwizzard_11.png?alt=media&token=3e45cea6-7046-4106-b254-446fdbe47b70)

The found frame is displayed as a turquoise rectangle. If you click with the mouse pointer into this frame, it will be deformable at the corners and edges by using the round gripping points shown there. You can also move the entire frame by clicking and dragging on the frame itself. If no frame is visible because no objects were found in the analysis, you can create a new frame by clicking and dragging.

Here, too, a frame that has been modified or dragged in this way is directly adopted as the analysis result for this step.

#### 5.2.3. Step: Book spine <a id="H5.2.3.Arbeitsschritt:Falzabschneiden"></a>

To determine the book fold, lines at edges are detected and grouped as in the `Align page` step. However, only vertical lines are taken into account here. The lines are displayed in the same way as in the `Align page` step. Depending on the orientation of the page, the best line group suitable for bookfolding is selected in the right or left third of the image and displayed as a wide red line. You can move this line by clicking and holding in the image to drag it to the desired position for the book fold. In the output image, the border is cut exactly in the middle of this line.

![Figure 12: Work step for recognizing the book fold with activated expert settings](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkuR9evIpjidCqK10K%2Flayoutwizzard_12.png?alt=media&token=7e255a22-1932-4cf4-92d4-b76d955400f9)

The auxiliary lines that display the analysis results can be selectively hidden and displayed by pressing the gray + at the top right of the image display and activating or deactivating the individual line types.

### 5.3. Image list area <a id="5-3-bereich-bildliste"></a>

At the bottom of the page, all images selected by the folder and filter options are listed with their file names. The currently visible image is surrounded by a black frame. Clicking on another image loads that image.

![Figure 13: In the lower area you can find the list of all images that highlight the currently displayed image with a frame.](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkudY7Q3Y5bbTAAloI%2Flayoutwizzard_13.png?alt=media&token=5ae7c145-8af2-4ab4-98ac-f83fd0f3cc14)

All image names in this list are highlighted with a green/red progress bar, which indicates the processing progress of the respective image. An unprocessed image is completely highlighted in red. With each processing step, a piece of the bar turns green until it is completely filled in at the end of all processing steps. This display is not continuously updated when editing multiple images to save editing time. To update the display at any time, click on the Reload icon above the image list.

### 5.4. Preview <a id="5-4-vorschauansicht"></a>

The Preview View displays all selected images one below the other as a list, as they would be saved with the current analysis results, except for the trimming position of the book fold, which is displayed as a red line in the image.

In addition, right and left pages are displayed separately, always first a page with left images, followed by a page with its right neighbors. Thus, in this view, one always compares images with the fold on the same page. By scrolling over all images, one can quickly find and correct errors in the analysis results.

The correction of the analysis results is normally done by opening the image in the view of the work step to be corrected. For this purpose, there are buttons to the right of each image in order to open the image in any work step and to edit it there. Since a shift of the book fold is usually the most frequent correction, the book fold is displayed as a red vertical line in the image and can be shifted directly as in the image display of the book fold step. In this way, the book fold of individual pages can be changed directly in this preview view without having to switch to the book fold part.

![Figure 14: Representation of the images as thumbnails in the preview view separated by left and right pages](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkuudE32B1ktwFzUe1%2Flayoutwizzard_14.png?alt=media&token=3b50a98c-2600-402a-93f2-cb1fd21617c0)

In the preview view, a maximum of a certain configurable number of images are always displayed at the same time, usually one hundred. This means that the first page of this view displays the left or right pages of the first two hundred pages. Scrolling `forwards` or `backwards` at the top or bottom of the list takes you to the opposite right or left pages and then to pages `201` to `400`, and so on.

![Figure 15: Explanation of the preview view](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkv0Qn_J_Cx_Civob9%2Flayoutwizzard_15.png?alt=media&token=7b76994c-e8c3-4f18-a5aa-05b3806033e2)

Above the display of the pages, a table of all currently visible outliers is listed, i.e. those pages with analysis values that differ greatly from their neighbors. This list also contains an indication of the analysis steps in which these pages were conspicuous. By clicking on an entry in this table, you can jump directly to the page in question. All outliers are also marked with coloured arrows in the image display.

To exit the preview view, the Open Overview button is available on the left, which takes you back to the initial LayoutWizard interface. Below this is also the button Save Results, which saves all changes made to the analysis results so far.

![Figure 16: Buttons for editing a page and for exiting the LayoutWizard](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZkto_uG-FWR7nD15BB%2F-LZkv7MwUZ7A2pBP4f4v%2Flayoutwizzard_16.png?alt=media&token=8a2774f4-3862-4e6f-998b-0a68240db29b)

With the buttons `Cancel` and `Save and exit` results you can leave the plugin at any time. `Cancel` leads to the loss of all changes since the last click on `Save Results`.[  
](https://app.gitbook.com/@intranda/s/goobi-workflow-plugins-de/~/drafts/-LgXmDFUB2JKs4qolX5U/primary/step-plugins/intranda_step_pdfupload)

