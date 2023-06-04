---
description: >-
  Goobi Step Plugin for fetching images automatically from the mets file.
---

# Fetch Images From Metadata


## Introduction
This step plugin for Goobi workflow automatically fetches images from a configured folder or from some URLs according to the registered names or registered URLs in the mets file respectively. The images can be further exported if configured so.

| Details |  |
| :--- | :--- |
| Identifier | fetch-images-from-metadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata](https://github.com/intranda/goobi-plugin-step-fetch-images-from-metadata) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2023.05 |
| Documentation date | 04.Jun.2023 |


## How the plugin works
The plugin checks the existing images in the `media` folder of the process to see if the wanted image was already imported, and if not:
* if `useUrl` is set `true`, the plugin will download the image from the given URL; 
* if `useUrl` is set `false` or not set at all, the plugin will search the configured folder to see if the image is there ready to be imported.

In either case the order of the imported images will be updated and saved into the mets file:
* if `useUrl` is set `true`, then the files' order will be the same as their registered order in the mets file;
* if `useUrl` is set `false` or not set at all, then any file named after the pattern `{InventoryNumber} Nr_{SerialNumber}.{suffix}` will be promoted to the first place, while the other images will just be sorted by their names.


## Installation
The plugin consists of the following files:

```text
plugin_intranda_step_fetch_images_from_metadata.jar
plugin_intranda_step_fetch_images_from_metadata.xml
```

The file `plugin_intranda_step_fetch_images_from_metadata.jar` must be installed in the correct directory so that it is available at the following path after installation:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_fetch_images_from_metadata.jar
```

In addition, there is a configuration file that must be located in the following place:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_fetch_images_from_metadata.xml
```

## Configuration

The configuration of the plugin is done via the configuration file `plugin_intranda_step_fetch_images_from_metadata.xml` and can be adjusted during operation. The following is an example configuration file:

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
      
        <!-- true if the images should be fetched from a url, false if the images should be fetched from the following configured folder. DEFAULT false -->
        <useUrl>false</useUrl>
        
        <!-- metadata containing the file name -->
        <filenameMetadata>SeparatedMaterial</filenameMetadata>
        <!-- mode="copy|move"   ignoreFileExtension="true|false"-->
        <fileHandling mode="copy" ignoreFileExtension="true" folder="/opt/digiverso/import/images/" />
        <!-- enabled= true|false exportImages=true|false -->
        <export enabled="true" exportImages="true" />

    </config>
</config_plugin>
```

| Parameter | Explanation |
| :--- | :--- |
| `useUrl` | This parameter determines the source location of the to be fetched images. If set `true` then the images will be fetched from registered URLs in the mets file, if set `false` or not set, then the images will be fetched from the following configured folder. |
| `project` | This parameter determines the project for which the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which workflow steps the block `<config>` is to apply. The name of the step is used here. This parameter can occur several times per `<config>` block. |
| `filenameMetadata` | This parameter defines the metadata which holds the names of the wanted images.  |
| `fileHandling` | The `@mode` attribute defines whether to import the images via copy or move. The `@ignoreFileExtension` attribute defines whether or not to ignore the file extensions while searching through the images. The `@folder` attribute holds the absolute path to the folder containing the to-be-imported images.  |
| `export` | The `@enabled` attribute defines whether or not to export the process, while the `@exportImages` attribute defines whether or not the export the images.  |


## Integration of the plugin into the workflow
This plugin is integrated into the workflow in such a way that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a workflow step, it should be configured as shown in the screenshot below.

![Integration of the plugin into the workflow](../.gitbook/assets/intranda_step_fetch_images_from_metadata_en.png)
