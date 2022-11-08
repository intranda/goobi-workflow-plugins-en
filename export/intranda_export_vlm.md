---
description: >-
  This is a technical documentation for the configurable Goobi Export plugin. It allows you to customise the export using the project configuration. By using export projects, it is also possible to export to different locations in one go.
---

# Configurable VLM export

## Introduction

This documentation describes how to install, configure and use the vlm export plugin in Goobi.

Using this export plugin for Goobi, Goobi operations can be exported to configured locations simultaneously within one operation.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Licence | GPL 2.0 oder neuer |
| Compatibility | Goobi workflow 2022.10 und neuer |
| Documentation date | 07.11.2022 |

## Installation

This plugin is integrated into the workflow in such a way that it is executed manually. For use within a work step of the workflow, it should be configured as shown in the screenshot below.

![Integration of the plug-in into the workflow](../.gitbook/assets/intranda_plugin_export_vlm_en.png)

The plugin must first be copied into the following directory:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_vlm.jar
```

In addition, there is a configuration file that must be located in the following place:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_vlm.xml
```
## Konfiguration

The plugin is configured via the configuration file `plugin_intranda_export_vlm.xml` and via the project settings. The configuration can be adjusted during operation. The following is an example configuration file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
  <!-- The name of the system, e.g. AlmaIDDigital, AlephIDDigital, CatalogIDDigital.  -->
  <!-- MANDATORY -->
  <identifier>CatalogIDDigital</identifier>

  <!-- The name to be used to distinguish between different volumes of one book series. -->
  <!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
  <!-- Leave the default value unchanged if the book is a one-volume work. -->
  <!-- MANDATORY -->
  <volume>CurrentNoSorting</volume>

  <!-- The place you would like to use for the export. -->
  <!-- Absolute path expected. -->
  <!-- MANDATORY -->
  <path>/opt/digiverso/viewer/hotfolder/</path>

  <!-- The prefix you would like to use for subfolders for different volumes. -->
  <!-- Leave the default value unchanged if the book is a one-volume work. -->
  <!-- MANDATORY -->
  <subfolderPrefix>T_34_L_</subfolderPrefix>

</config_plugin>

```

| Parameter         | Explanation                                                                                                           |
|:----------------- |:--------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | This parameter determines which system will be used. The name of the system will be used together with `IDDigital`.   |
| `volume`          | This parameter determines how to distinguish between different volumes of a multi-volume work.                        |
| `path`            | This parameter determines the place where the folders for books should be created. An absolute path is expected here. |
| `subfolderPrefix` | This parameter determines how VLM would work on the exported data further. For example, `T_34` would trigger VLM to recognize and create structure nodes for the type `volume`, and `L` signifies the following of a text. |

