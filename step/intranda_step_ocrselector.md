---
description: >-
  This is the technical documentation for the Goobi plugin for selecting single
  pages for OCR execution or non-execution.
---

# OCR page selection

## Introduction

This documentation describes the installation, configuration and use of a page selection plugin for downstream OCR processing. This plugin can be used to determine on a single-page basis which images from a process are sent to OCR with which font.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_ocrselector |
| Source code | [https://github.com/intranda/goobi-plugin-step-ocrselector](https://github.com/intranda/goobi-plugin-step-ocrselector) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 3.0.4 and newer |
| Documentation date | 04.03.2019 |

## Precondition

The precondition for using the plugin is the use of Goobi workflow in version 3.0.4 or higher, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired work steps of the workflow. In addition, a plugin is required for the actual OCR process and for merging the results.

## Installation and Configuration

The following files must be installed to use the plugin:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_ocrselector.jar
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/css/style.css
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/app.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/riot.min.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/tags.js
/opt/digiverso/goobi/static_assets/plugins/intranda_step_ocrselector/js/ugh.js
```

The first file is the Java part of the plugin, all following files are needed for the graphical display.

The plugin does not have its own configuration, but reads the default value for all pages from the metadata of the process.

## Settings in Goobi

After the plugin has been installed, it can be configured in the user interface in a workflow step.

![Task-Details](../.gitbook/assets/intranda_step_ocrselector_config.png)

## Usage

If the corresponding task was opened by the respective user within which the plugin was configured, the plugin is displayed to the user in the task. After the plugin has been entered, a new view opens in which all images belonging to the process are displayed.

![Plugin interface](../.gitbook/assets/intranda_step_ocrselector_entry.png)

The current selection \(antiqua, fracture, no OCR\) is displayed below each image. The preselection is read from the `Font Type` process property.

Individual images can now be selected here by left-clicking. Multiple selection is possible by `Ctrl + click` for individual pages and `Shift + click` for a range of pages.

![Multiple selection](../.gitbook/assets/intranda_step_ocrselector_selection.png)

If one or more pages are selected, a context menu can be opened by right-clicking on one of the selected pages.

![Context menu](../.gitbook/assets/intranda_step_ocrselector_context.png)

Here you can choose between the three options `antiqua`, `fracture` and `no OCR`. Click on one of the three options to apply it to all selected pages.

![Updated - no OCR for cover and blank pages](../.gitbook/assets/intranda_step_ocrselector_updated.png)

The plugin can be exited by clicking on `Exit plugin` after successful selection. The plugin will automatically be saved again. The `Save` button can be used for temporary saving.

