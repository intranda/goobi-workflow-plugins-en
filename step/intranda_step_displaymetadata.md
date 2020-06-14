---
description: >-
  This is the technical documentation for the Goobi plug-in for displaying any
  metadata in a workflow task.
---

# Display of metadata in a task

## Introduction

‌This documentation describes the installation, configuration and use of a plug-in to display metadata in a workflow step. The plugin can display any metadata in one step. The configuration of prefixes and suffixes is also possible.

| Details | ​ |
| :--- | :--- |
| Version | 1.1.0 |
| Identifier | intranda\_step\_displayMetadata |
| Source code | [https://github.com/intranda/goobi-plugin-step-displaymetadata](https://github.com/intranda/goobi-plugin-step-displaymetadata) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi Workflow 3.0.0 and newer |
| Documentation date | 28.11.2019 |

## ‌Precondition

‌The precondition for using the plugin is the use of Goobi workflow in version 3.0.0 or newer, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired workflow steps.

## Installation and configuration <a id="installation-und-konfiguration"></a>

‌To use the plugin, the two artifacts must be copied to the following locations:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_displayMetadata.jar/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_displayMetadata-GUI.jar
```

‌The configuration of the plugin is expected at the following path:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_displayMetadata.xml
```

‌Several metadata can be configured for display, additionally a prefix and a suffix can be displayed. The `key` attribute is used for the translation of the labels of the metadata:

```markup
    <?xml version="1.0" encoding="UTF-8"?>
    <config_plugin>
       <config>
            <project>*</project>
            <step>*</step>
            <metadatalist>
                <metadata>Author</metadata>
                <metadata>TitleDocMain</metadata>
                <metadata>_urn</metadata>
                <metadata prefix="http://svdmzgoobiweb01.klassik-stiftung.de/viewer/image/" suffix="/1/" key="url">CatalogIDDigital</metadata>
            </metadatalist>
        </config>
    </config_plugin>
```

In Goobi, the step in the workflow must then be configured. To do this, you must select `intranda_step_displayMetadata` as the step plug-in in the step configuration.

![Configuration of the step](../.gitbook/assets/displaymetadata_config.png)

If the step is then opened after successful configuration, all metadata - if available in the process - are displayed:

![](../.gitbook/assets/displaymetadata_view.png)

