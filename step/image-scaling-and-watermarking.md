---
description: >-
  This step plugin scales images to maximum size and renders watermarks into the
  scaled images.
---

# Image scaling and watermarking

## Introduction

This plugin allows to scale images to a maximum size and render watermarks into the previously scaled images. The maximum size and the watermark to be rendered can be configured flexibly.

## Overview

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_step\_sudan-export-preparation |
| Source code | source code not yet publicly available |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2020.06 |
| Documentation date | 31.08.2020 |

## Installation

To install the plugin the following file must be installed:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_sudan-export-preparation.jar
```

For the correct execution of the plugin a configuration file is also required:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_sudan-export-preparation.xml
```

## Configuration of the plugin

The configuration allows you to specify the maximum size to which the image is scaled and which watermark \(images and text watermarks are supported\) is rendered. Also the positioning of the watermark can be defined. Several configurations are possible, which are distinguished by the project, the step name, the digital collection and the media type \(metadata in METS\). When the plugin is executed, the first configuration that matches the currently processed operation is used.

An example configuration looks like this:

```markup
<config_plugin>
    <gmPath>/usr/bin/gm</gmPath>
    <convertPath>/usr/bin/convert</convertPath>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
-->
    <config>
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- the source directory from which the images will be taken -->
        <sourceDir>cropped</sourceDir>
        <!-- the destination where the scaled and watermarked images will reside -->
        <destDir>media</destDir>
        
        <!-- only use this block with processes in collection "mycollection"
             and with mediaType "book" -->
        <imageConfig collection="mycollection" mediaType="book">
            <!-- The maximum size of the longest side of an image -->
            <resizeTo>1500</resizeTo>
            <!-- The watermark configuration -->
            <watermark>
                <!-- the image to use for watermarking -->
                <image>/opt/digiverso/goobi/scripts/watermark1.png</image>
                <!-- the location of the watermark. Possible values: north, 
                     northeast, east, southeast, south, southwest, 
                     west, northwest -->
                <location>southeast</location>
                <!-- these are the distances to the edges on the x- resp. y-axis -->
                <xDistance>100</xDistance>
                <yDistance>100</yDistance>
            </watermark>
        </imageConfig>
       
        <!-- use this block with processes in collection "myothercollection"
             and any mediaType --> 
        <imageConfig collection="myothercollection" mediaType="*">
            <resizeTo>1500</resizeTo>
            <watermark>
                <!-- you can also use a text-only watermark. -->
                <text>My watermark text</text>
                <location>southeast</location>
                <xDistance>600</xDistance>
                <yDistance>100</yDistance>
            </watermark>
        </imageConfig>
        
    </config>
</config_plugin>
```

## Integration of the plugin into the workflow

To use the plugin, it must be activated for one or more desired tasks in the workflow. This is done by selecting the plugin intranda\_step\_sudan-export-preparation from the list of installed plugins.

