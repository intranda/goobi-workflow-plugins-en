---
description: >-
  This is a technical documentation for the configurable Goobi Export plugin. It allows you to customise the export using the project configuration. By using export projects, it is also possible to export to different locations in one go.
---

# Configurable export

## Introduction

This documentation describes how to install, configure and use an export plugin in Goobi.

Using this export plugin for Goobi, Goobi operations can be exported to multiple locations simultaneously within one operation.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_configurable |
| Source code | [https://github.com/intranda/goobi-plugin-export-configurable](https://github.com/intranda/goobi-plugin-export-configurable) |
| Licence | GPL 2.0 oder neuer |
| Compatibility | Goobi workflow 2022.03 und neuer |
| Documentation date | 12.05.2022 |

## Installation

This plugin is integrated into the workflow in such a way that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a work step of the workflow, it should be configured as shown in the screenshot below.

![Integration of the plug-in into the workflow](../.gitbook/assets/plugin_intranda_export_configurable-step.png)

The plugin must first be copied into the following directory:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_configurable.jar
```

In addition, there is a configuration file that must be located in the following place:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_configurable.xml
```
## Konfiguration

The plugin is configured via the configuration file `plugin_intranda_export_configurable.jar` and via the project settings. The configuration can be adjusted during operation. The following is an example configuration file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
	<!--
        order of configuration is:
          1.) project name matches
          2.) project is *
	-->
	<config>
		<project>testocr</project>
    <includeMarcXml>false</includeMarcXml>
		<folder>
			<includeMedia>true</includeMedia>
			<includeMaster>true</includeMaster>
			<includeSource>false</includeSource>
			<includeImport>false</includeImport>
			<includeExort>false</includeExort>
			<includeITM>false</includeITM>
			<includeOcr>true</includeOcr>
      <includeValidation>false</includeValidation>
			<ocr>
				<suffix>alto</suffix>
			</ocr>
		</folder>
	</config>


	<config>
		<project>*</project>
		<target key="{meta.ViewerInstance}" value="evifaanddigihub" projectName="evifExportProject"/>
		<target key="{meta.ViewerInstance}" value="evifaanddigihub" projectName="digihubExportProject"/>
		<target key="{meta.ViewerInstance}" value="" projectName=""/>
    <includeMarcXml>false</includeMarcXml>
		<folder>
      <!-- as configured in goobi_config.properties -->
      <!--genericFolder>thumbs</genericFolder-->
			<includeMedia>true</includeMedia>
			<includeMaster>true</includeMaster>
			<includeOcr>false</includeOcr>
			<includeSource>false</includeSource>
			<includeImport>false</includeImport>
			<includeExort>false</includeExort>
			<includeITM>false</includeITM>
			<includeValidation>false</includeValidation>
		</folder>
	</config>
</config_plugin>

```

| Parameter | Explanation |
| :--- | :--- |
| `project` | This parameter determines for which project the current block `<config>` should apply. The name of the project is used here. The `<config>` block with the `project` `*` is always used if no other block matches the project name.  
| `target` | This parameter has 3 mandatory attributes: In the `key` parameter, a Goobi variable of the form `{meta.metadata name}` should be used. The attribute `value` can then be used to specify the desired value. If `value=""` is set, the condition will be met if the metadata is empty or not set. The attribute `projectName` should contain the name of the export project with whose settings the export is to take place. If an empty string is assigned to the attribute `projectName=""`, the settings of the project of the operation will be used for export. If no target condition is set, a normal export will be performed. An export is triggered for each target condition that applies.  |
|`includeMarcXml`| This parameter determines whether any existing MARC-XML data should be embedded in the exported metafile. The default value is `false`.|

The block `<config>` is repeatable and can thus define different metadata in different projects. The block with `<project>*</project>` is applied if no block with the project name of the project exists.

### The folder block

The `folder` block is located inside each `config` element. It controls which directories are to be taken into account for the export.

| Parameter | Explanation |
| :--- | :--- |
| `includeMedia` | Here you can define whether the media folder should be exported.
| `includeMaster` | Here you can define whether the master folder should be exported. |
| `includeOcr` | Here you can define whether the ocr folder should be exported. |
| `includeSource` | Here you can define whether the source folder should be exported. |
| `includeImport` | Here you can define whether the import folder should be exported. |
| `includeExort` | Here you can define whether the export folder should be exported. |
| Here you can define whether the TaskManager folder is to be exported. |
| `includeValidation` | Here you can define whether the validation folder should be exported. |
| `ocr` | The `ocr` element is needed when using OCR folders with different suffixes. The specific suffix can then be specified in the `suffix` sub-element. |

The default value for each of these parameters except `ocr` is `false`. If the respective parameter is not mentioned in the configuration, no export of the corresponding folder will take place.

The configuration of the destination folder can be done within the project settings in the Goobi workflow user interface. If the checkbox for `Create task folder` is set there, the task will be stored in a subfolder with its title as name in the target folder.

![Project settings within Goobi workflow](../.gitbook/assets/plugin_intranda_export_configurable-project.png)
