---
description: >-
  This step plugin for Goobi workflow is used to replace placeholder images
  within the master folder.
---

# Replace images

## Introduction

This plugin is used to replace previously imported placeholder images within the master folder of a Goobi workflow process with the actual master images. The plugin is operated by simply dragging and dropping the required files into the plugin's user interface.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_replace-images |
| Source code | [https://github.com/intranda/goobi-plugin-step-replace-images](https://github.com/intranda/goobi-plugin-step-replace-images) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2020.06 |
| Documentation date | 13.06.2020 |

## Installation

This plugin is delivered as a `tar` archive. To install it, unpack the `plugin_intranda_step_replace-images.tar` archive into the Goobi folder:

```bash
tar -C /opt/digiverso/goobi/ -xf plugin_intranda_step_replace-images.tar --exclude="pom.xml"
```

This plugin has no configuration file and is therefore not configurable.

## Usage of the plugin

This plugin is integrated into the workflow so that it is available for a selected task. After accepting the task, the user can enter the plugin.

![Integration of the plugin into a task](../.gitbook/assets/intranda_step_replace-images-1_en.png)

This gives the user access to the plugin's user interface, where the current content of the master folder is listed. Here, single or many images can be copied by Drag & Drop to the place where the images to be inserted should replace the existing placeholder images. The plugin also ensures during the upload that the newly uploaded files are renamed correctly.

![User interface to replace the existing placeholder images](../.gitbook/assets/intranda_step_replace-images-2_en.png)
