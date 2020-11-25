---
description: >-
  This step plugin allows you to automatically adjust file names within Goobi
  processes.
---

# Renaming files

## Introduction

This plugin is used to conditionally rename files within the different folders of an operation of Goobi workflow. The naming is dependent on a configuration file, which may be structured differently for different workflows.

## Overview

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_rename-files |
| Source code | [https://github.com/intranda/goobi-plugin-step-rename-files](https://github.com/intranda/goobi-plugin-step-rename-files) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2020.02 |
| Dokumentation date | 10.05.2020 |

## Installation

To install the plugin, the following file must be installed:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_step_rename-files.jar
```

To configure how the plugin should behave, different values can be adjusted in the configuration file. The configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_rename-files.xml
```

As an example, the content of this configuration file looks like this:

```markup
<config_plugin>

    <config>
        <project>Monographs 1900-1950</project>
        <project>Monographs 1950-2000</project>
        <step>Automatic renaming</step>
        <startValue>1</startValue>
        <namepart type="counter">00000</namepart>
        <namepart type="static">-</namepart>
        <namepart type="variable">{projectid}</namepart>
    </config>

    <config>
        <project>*</project>
        <step>*</step>
        <startValue>1</startValue>
        <namepart type="variable">{processtitle}</namepart>
        <namepart type="static">_</namepart>
        <namepart type="counter">00000</namepart>
    </config>

</config_plugin>
```

## General configuration of the plugin

The configuration of the plugin is done within the already mentioned configuration file. There you can configure various parameters. The block `<config>` can occur repeatedly for different projects or work steps in order to be able to perform different actions within different workflows. The elements `<namepart>` are decisive for the generation of the file names.

| Value | Description |
| :--- | :--- |
| `project` | This parameter determines the project for which the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which work steps the block `<config>` should apply. The name of the workflow step is used here. This parameter can occur several times per `<config>` block. |
| `startValue` | This value controls with which start value the incrementing `counter` should start. |
| `namepart` | This parameter, which can also be used several times, controls the generation of the file names. It can contain static elements \(`static`\), use variables from Goobi \(`variable`\) and generate a counter. The number of digits defined is crucial for generating the counter \(`counter`\). For example, the value `00000` would generate five-digit numbers with any leading zeros. The components of the file name defined in this way are concatenated together for naming purposes and then supplemented by the actual file extension to name the file. |

## Mode of operation

The plugin is usually executed fully automatically within the workflow. It first determines whether there is a block within the configuration file that has been configured for the current workflow with regard to project name and work step. If this is the case, the individual elements `<namepart>` are evaluated, assigned the appropriate values for the counter and variables from Goobi workflow and then linked together. The file names created in this way are now applied to all the relevant directories in the Goobi process and are supplemented with the correct file name extensions \(e.g. `.tif`\).

If a file is found within the file that contains `barcode` within the file name, it will also be named according to the naming scheme. However, the value `0` is set as counter here.

Details of the Goobi workflow variables that can be used in this plugin can be found [in this documentation](https://docs.intranda.com/goobi-workflow-en/manager/8).

The plugin considers the files within the following subdirectories for naming:

* master
* media
* jpeg
* alto
* pdf
* txt
* xml

## How to use the plugin

This plugin is integrated into the workflow in such a way that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a step of the workflow it should be configured as shown in the following screenshot.

![Integration of the plugin into the workflow](../.gitbook/assets/intranda_step_rename-files.png)

