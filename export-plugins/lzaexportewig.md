# EWIG long-term preservation

## Introduction

This documentation describes the installation, configuration and use of a plugin to create a METS file for long-term preservation EWIG. ​

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | LzaExportEWIG |
| Source code | - Source code not yet publicly available - |
| Compatibility | Goobi workflow 3.0 and newer |
| Documentation date | 28.10.2019 |

## ​Precondition

​The prerequisite for using the plugin is the use of Goobi workflow 3.0, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired work steps of the workflows. ​ ​

## Installation and Configuration

​ The plugin consists of two files:​

```text
plugin_intranda_export_LZA_EWIG.jar
plugin_LzaExportEWIG.xml
```

​The file `plugin_intranda_export_LZA_EWIG.jar` contains the program logic and must be installed in the following directory so that the tomcat user can read it:

```markup
/opt/digiverso/goobi/plugins/export/
```

The `plugin_LzaExportEWIG.xml` file must also be readable by the user `tomcat` and installed into the following directory:

```markup
/opt/digiverso/goobi/config/
```

​ The following file is used to configure the plugin and must be structured as follows:

{% code title="plugin\_LzaExportEWIG.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <exportFolder>/opt/digiverso/</exportFolder>
</config_plugin>
```
{% endcode %}

​In the  element `<exportFolder>`, the location in the file system where the exported METS files are stored is specified.

## Settings in Goobi workflow

​After the plugin has been installed and configured, it can be used within one step. To do this, the `LzaExportEWIG` plugin must be selected within the desired task. Furthermore, the checkboxes Automatic task and Export must be set.

![](../.gitbook/assets/lzaexportewig.png)

## Other

The step within Goobi workflow exports all necessary files for the EWIG Ingest. The upload itself is done via the intranda TaskManager. This makes sense in order to avoid several parallel upload processes having conflicts with each other and slowing down the system. For the upload see [chapter 4.17](https://docs.intranda.com/intranda-taskmanager-de/4/4.17-upload-von-dateien-in-das-ewig-langzeitarchiv) in the intranda TaskManager documentation.

