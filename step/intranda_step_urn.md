---
description: >-
  Goobi Step Plugin for the creation of Uniform Resource Names (URN).
---

# Creation of Uniform Resource Names (URN)


## Introduction
This documentation describes the installation, configuration and use of the Step Plugin for the generation of Uniform Resource Names in Goobi workflow.

| Details |  |
| :--- | :--- |
| Identifier | intranda_step_urn |
| Source code | [https://github.com/intranda/goobi-plugin-step-urn](https://github.com/intranda/goobi-plugin-step-urn) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2022.03 |
| Documentation date | 25.03.2022 |


## How the plugin works
The plugin is usually executed fully automatically within the workflow. It first determines whether a Uniform Resource Name (URN) already exists. If no URN exists yet, a new URN is registered. If a URN already exists in the metadata, an attempt is made to update the metadata of the URN.


## Operation of the plugin
This plugin is integrated into the workflow in such a way that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a workflow step, it should be configured as shown in the screenshot below.

![Integration of the plugin into the workflow](../.gitbook/assets/intranda_step_urn_en.png)


## Installation
The plugin consists of the following file:

```text
plugin_intranda_step_urn.jar
```

This file must be installed in the correct directory so that it is available at the following path after installation:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_urn.jar
```

In addition, there is a configuration file that must be located in the following place:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_urn.xml
```


## Configuration
The configuration of the plugin is done via the configuration file `plugin_intranda_step_urn.xml` and can be adjusted during operation. The following is an example configuration file:

```markup
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

        <!-- URI of the URN API, must use httos -->
        <uri>https://api.nbn-resolving.org/v2/</uri>

        <!-- namespace in which new URNs shall be created -->
        <namespace>urn:nbn:de:gbv:XX</namespace>

        <!-- name of the API user -->
        <apiUser>UserName</apiUser>

        <!-- password of the API user -->
        <apiPassword>Password</apiPassword>

        <!--target url URN will forward to -->
        <publicationUrl>https://viewer.example.org/{meta.CatalogIDDigital}</publicationUrl>

        <!--metadataType in METS-File -->
        <metadataType>_urn</metadataType>
    </config>
</config_plugin>
```

| Parameter | Explanation |
| :--- | :--- |
| `project` | This parameter determines for which project the current block `<config>` should apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which workflow steps the block `<config>` should apply. The name of the workflow step is used here. This parameter can occur several times per `<config>` block. |
| `uri` | The URL of the API must be stored in this parameter. As a rule, the standard entry `https://api.nbn-resolving.org/v2/` can be used.  |
| `namespace` | The namespace in which the new URNs are created. |
| `apiUser` | The name of the API user. |
| `apiPassword` | The password of the API user. |
| `publicationUrl`   | The URL under which the digitised work will be available in the future. As a rule, the publication URL will follow a pattern, e.g. `https://viewer.example.org/{meta.CatalogIDDigital}`. In this case, it is assumed that the works will be published in the future under a URL containing the metadate 'identifier'. |
| `metadataType`  | Specifies the metadata type under which the URN is to be recorded. The default should not be changed here.  |
