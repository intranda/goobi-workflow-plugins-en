---
description: >-
  This is a technical documentation for the plugin for the automatic enriching the process with images based on metadata with the file names in the process.
---

# Copying files from metadata fields

## Introduction
This documentation describes the installation, configuration and use of the plugin. With the help of this plugin, images can be copied or moved to the desired folder in the process using the file name stored in the process. 

| Details |  |
| :--- | :--- |
| Identifier | intranda-step-fetch-images-from-metadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata](https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata) |
| Licence | GPL 2.0 or newer |
| Documentation date | 25.11.2022 |


## How the plugin works
The plugin is usually executed fully automatically within the workflow. It first determines whether the metadata specified in the configuration exists and then analyses it. The file specified in the metadata is then copied or moved to the media folder of the process based on its name and file extension.


## Operation of the plugin
This plugin is integrated into the workflow so that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a work step, the workflow must be configured accordingly and the plugin selected there. Manual use of this plugin by a user is not necessary.


## Installation and configuration
The plugin consists of two files:

```bash
plugin_intranda_step_fetch_images_from_metadata.jar
plugin_intranda_step_fetch_images_from_metadata.xml
```

The file `plugin_intranda_step_fetch_images_from_metadata.jar` contains the programme logic and must be installed in the following directory so that it can be read by the `tomcat` user:

```bash
/opt/digiverso/goobi/plugins/step/
```

The configuration file `plugin_intranda_step_fetch_images_from_metadata.xml` must also be readable for the `tomcat` user and must be installed in the following directory:

```bash
/opt/digiverso/goobi/config/
```

This configuration file is structured as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
	-->
    
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- metadata containing the file name -->
        <filenameMetadata>SeparatedMaterial</filenameMetadata>
         <!-- fileHandling:
              mode: Defines if files are copied or moved. Possible values are "copy" and "move". Defaults to "copy".
              ignoreFileExtension: If the filenameMetadata contains the value "image1.tif" for example you can configure here if the plugin
              shall search for the exact filename or if the file extension shall be ignored which
              allowes to find "image1.jpg", too. Possible values are "true" and "false". Default is "false".
              folder: Absolut path to the directory where to search for the files to be imported.
        -->
        <fileHandling mode="copy|move" ignoreFileExtension="true|false" folder="/opt/digiverso/import/images/" />
    </config>

</config_plugin>
```

The individual parameters have the following function:

| Parameter | Explanation |
| :--- | :--- |
| `project` | This parameter defines which project the current block `<config>` should apply to. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls which work steps the `<config>` block should apply to. The name of the work step is used here. This parameter can occur several times per `<config>` block. |
| `filenameMetadata` | The name of the metadata field (usually from the METS file) containing the file name of the file to be imported is specified here. |

The attributes of the `fileHandling` element are configured as follows:

| Attribute | Explanation |
| :--- | :--- |
| `mode` | This attribute defines whether the files to be transferred should be copied to the folder of the process or whether they should be moved there. |
| `ignoreFileExtension` | This attribute controls whether the file extension should be ignored for the copy process or must be exactly correct. |
| `folder` | This attribute specifies the folder in which the files to be imported are located. |
