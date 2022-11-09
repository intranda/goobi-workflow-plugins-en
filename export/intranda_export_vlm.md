---
description: >-
  This is a technical documentation for the VLM Export Plugin. It enables the export to a VLM instance.
---

# VLM Export

## Introduction

This documentation describes the installation, configuration and use of the VLM export plugin in Goobi.

Using this plugin for Goobi, Goobi operations can be exported to the configured location for VLM within one step.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2022.10 or newer |
| Documentation date | 07.11.2022 |

## Installation

This plugin is integrated into the workflow in such a way that it is executed automatically. For use within a workflow step, it should be configured as shown in the screenshot below.

![Integration of the plugin into the workflow](../.gitbook/assets/intranda_plugin_export_vlm_de.png)

The plugin must first be copied to the following directory:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_vlm.jar
```

In addition, there is a configuration file that must be located in the following place:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_vlm.xml
```
## Configuration

The plugin is configured via the configuration file `plugin_intranda_export_vlm.xml`. The configuration can be adjusted during operation. The following is an example configuration file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
  <!-- The object identifier, typically CatalogIDDigital  -->
  <!-- MANDATORY -->
  <identifier>CatalogIDDigital</identifier>

  <!-- The name to be used to distinguish between different volumes of multi volume works. -->
  <!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
  <!-- MANDATORY -->
  <volume>CurrentNoSorting</volume>

  <!-- The place you would like to use for the export. -->
  <!-- Absolute path expected. -->
  <!-- MANDATORY -->
  <path>/opt/digiverso/viewer/hotfolder/</path>

  <!-- The prefix you would like to use for subfolders for different volumes. -->
  <!-- MANDATORY -->
  <subfolderPrefix>T_34_L_</subfolderPrefix>

</config_plugin>
```

| Parameter         | Explanation                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | This parameter determines which metadatum is to be used as the folder name. |
| `volume`          | This parameter controls with which metadata the subdirectories for volumes are to be named. |
| `path`            | This parameter sets the export path where the data is to be exported. An absolute path is expected. |
| `subfolderPrefix` | This parameter describes the prefix to be placed in front of each volume of a multi-volume work in the folder name. (Example `T_34_L_`: Here `T_34` stands for the recognition for the creation of a structure node of the type `volume` and the `L` indicates that a text comes after it.). |
