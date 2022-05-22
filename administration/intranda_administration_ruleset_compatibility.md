---
description: >-
  Goobi Administration Plugin for checking ruleset compatibility for multiple
  processes
---

# Ruleset Compatibility

## Introduction

This documentation describes the installation, configuration and use of the Administration Plugin for automated checking of a large number of processes within Goobi workflow with the assigned rule set. Any incompatibilities with the respective rule sets are identified and a corresponding message about the specific incompatibility is displayed.

| Details            |                                                                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Identifier         | intranda\_administration\_ruleset\_compatibility                                                                                                               |
| Source code        | [https://github.com/intranda/goobi-plugin-administration-ruleset-compatibility](https://github.com/intranda/goobi-plugin-administration-ruleset-compatibility) |
| Licence            | GPL 2.0 or newer                                                                                                                                               |
| Compatibility      | Goobi workflow 2021.08                                                                                                                                         |
| Documentation date | 11.09.2021                                                                                                                                                     |

## Installation

The plugin consists of the following files to be installed:

```
plugin_intranda_administration_ruleset_compatibility.jar
plugin_intranda_administration_ruleset_compatibility-GUI.jar
plugin_intranda_administration_ruleset_compatibility.xml
```

These files must be installed in the correct directories so that they are available in the following paths after installation:

```bash
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_ruleset_compatibility.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_ruleset_compatibility-GUI.jar
/opt/digiverso/goobi/config/plugin_intranda_administration_ruleset_compatibility.xml
```

## Configuration

The plugin is configured via the configuration file `plugin_intranda_administration_ruleset_compatibility.xml` and can be adapted during operation. The following is an example configuration file:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>

    <!-- default filter to use -->
    <filter>stepdone:export</filter>

</config_plugin>
```

| Parameter | Explanation                                                                                                                                                                                                     |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `filter`  | With this parameter, a filter can be set as the default. This is automatically pre-filled when entering the plugin, but can then be adjusted as desired each time the plugin is used within the user interface. |

To use this plugin, the user must have the correct role authorisation. Therefore, please assign the role `Plugin_administration_ruleset_compatibility` to the user group.

![Correctly assigned role for users](../.gitbook/assets/intranda\_administration\_ruleset\_compatibility2\_en.png)

## Bedienung des Plugins

Wenn das Plugin korrekt installiert und konfiguriert wurde, ist es innerhalb des Menüpunkts `Administration` zu finden. Nach dem Betreten können in der Oberfläche die oben beschriebenen Parameter noch einmal individuell angepasst werden.

![User interface of the plugin](../.gitbook/assets/intranda\_administration\_ruleset\_compatibility1\_en.png)

After clicking on the button `Execute plugin` the check of the METS files starts. A progress bar informs about the progress. The processes already processed are listed within the table. Any incompatibilities are immediately displayed. In addition, it is possible to directly enter the metadata editor of individual processes.
