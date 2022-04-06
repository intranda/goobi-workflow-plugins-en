---
description: >-
  Goobi Administration Plugin for restoring image folders from external storage
---

# Restoring archived image folders

## Introduction
This plugin for Goobi workflow restores image folders that were previously archived with the plugin `goobi-plugin-step-archiveimagefolder`.


| Details |  |
| :--- | :--- |
| Identifier | intranda_administration_restorearchivedimagefolders |
| Source code | [https://github.com/intranda/goobi-plugin-administration-restorearchivedimagefolders](https://github.com/intranda/goobi-plugin-administration-restorearchivedimagefolders) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2022.03 |
| Documentation date | 29.03.2023 |


## Installation
The plugin consists of the following files to be installed:

```text
goobi_plugin_administration_restorearchivedimagefolders.jar
goobi_plugin_administration_restorearchivedimagefolders-GUI.jar
plugin_intranda_administration_restorearchivedimagefolders.xml
```

These files must be installed in the correct directories so that they are available in the following paths after installation:

```bash
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_restorearchivedimagefolders.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_restorearchivedimagefolders-GUI.jar
/opt/digiverso/goobi/config/plugin_intranda_administration_restorearchivedimagefolders.xml
```

## Configuration
The configuration file is empty at the moment, but must still be present.

```xml
<config_plugin>
  <!-- currently no config needed -->
</config_plugin>
```

The information from where the data is to be fetched is stored in the respective process folder in an XML file by the archiving plug-in.

For authentication on ssh servers, public keys are searched for in the usual places (`$USER_HOME/.ssh`). Other authentication methods such as username/password are not provided.  


## Operation of the plugin
The plugin offers a graphical user interface that can be opened via the menu `Administration`. There, a search filter can be used, as it is used in other parts of Goobi workflow (e.g. for the task list). Clicking on 'Run Plugin' will then restore the images for the tasks found via the filter entered. The user interface updates automatically.
