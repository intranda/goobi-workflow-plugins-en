---
description: >-
  This workflow plugin allows the mass import of data from JSON files.
---

# Generic import plugin for JSON files

## Introduction

This workflow plugin was implemented to read metadata from multiple JSON files and import it into a workflow.

## Overview

| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_import\_json |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-import-json](https://github.com/intranda/goobi-plugin-workflow-import-json) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 23.07 |
| Documentation date | 01.Sep.2023 |

## Installation

* For the installation one needs `plugin_intranda_workflow_import_json.jar` and `plugin_intranda_workflow_import_json-GUI.jar`
```bash
{PATH_TO_GOOBI}/plugins/workflow/plugin_intranda_workflow_import_json.jar
{PATH_TO_GOOBI}/plugins/GUI/plugin_intranda_workflow_import_json-GUI.jar
```
* The configuration file `plugin_intranda_workflow_import_json.xml` is to be copied to:
```bash
{PATH_TO_GOOBI}/config/plugin_intranda_workflow_import_json.xml
```
One can configure different values in the configuration file. One example looks like this:

```markup
<config_plugin>
	<!-- folder that contains the json files -->
	<jsonFolder>/opt/digiverso/g2g/workspace/demo_content/</jsonFolder>

	<!-- folder used to hold the downloaded images temporarily -->
	<importFolder>/opt/digiverso/g2g/workspace/workflow/tmp/</importFolder>

	<!-- which workflow to use -->
	<workflow>KHM_Giza_Import_Workflow</workflow>

	<!-- which publication type to use -->
	<publicationType>GizaDocument</publicationType>

	<!-- URL types that hold downloadable resources, could be none or multiple -->
	<downloadableUrl>GizaPhotosMain</downloadableUrl>
	<downloadableUrl>GizaPrimaryDisplayMain</downloadableUrl>

	<!-- whether or not to save the partner url, DEFAULT false -->
	<partnerUrl save="true">
		<!-- at most one urlBase should be configured, DEFAULT "" -->
		<urlBase>giza.fas.harvard.edu</urlBase>
		<!-- there could be multiple urlParts-->
		<urlPart>$._type</urlPart>
		<urlPart>$._id</urlPart>
		<!-- at most one urlTail should be configured, DEFAULT "" -->
		<urlTail>full</urlTail>
		<!-- MetadataType that is to hold the value of the partner url -->
		<urlMetadata>GizaPartnerUrl</urlMetadata>
	</partnerUrl>

	<!-- same source could be used multiple times for different MetadataTypes -->
	<metadata source="$._source.id" target="TitleDocMain" />
	<metadata source="$._source.id" target="CatalogIDDigital" />
	<!-- source that does not start with $ or @ will be regarded as value -->
	<metadata source="Giza" target="singleDigCollection" />
	<!-- source that ends with [:] is an array -->
	<metadata source="$._source.allnumbers[:]" target="GizaAllNumbers" />

	<!-- group is for MetadataGroups, there could be multiple -->
	<!-- no need to add the [:] to the end of @source -->
	<!-- @type is the name of the MetadataGroupType -->
	<group source="$._source.sitedates" type="GizaSiteDatesGroup">
		<!-- source that starts with @ will be regarded as relative json path -->
		<metadata source="@.type" target="GizaSiteDateType" />
		<metadata source="@.date" target="GizaSiteDate" />
	</group>

	<!-- @altType is the name of an alternative MetadataGroupType, which will be used to create another MetadataGroup for all items that are filtered out. OPTIONAL. -->
	<!-- @key is the filtering key that is used to retrieve the value from the JSONObject. OPTIONAL. -->
	<!-- @value is used to compare with the value retrieved from the JSONObject. OPTIONAL. DEFAULT "". -->
	<!-- @method defines the logic for comparing values. Options are is, not, startsWith, endsWith, contains. OPTIONAL. DEFAULT 'is'. -->
	<group source="$._source.relateditems.modernpeople" type="ModernPeopleGroup" altType="_ModernPeopleGroupHidden" key="role" value="Excavator" method="is">
		<metadata source="@.role" target="GizaModernPeopleRole" />
		<!-- same source could be used multiple times for different MetadataTypes -->
		<metadata source="@.id" target="GizaModernPeopleId" />
		<metadata source="@.id" target="GizaModernPeopleId2" />
	</group>

	<!-- if no @altType is configured, then all items that are filtered out will not be imported. -->
	<group source="$._source.relateditems.ancientpeople" type="AncientPeopleGroup" key="role" value="Tomb Owner">
		<metadata source="@.role" target="GizaAncientPeopleRole" />
		<metadata source="@.id" target="GizaAncientPeopleId" />
	</group>

	<!-- child is for child DocStructs, there could be multiple -->
	<!-- no need to add the [:] to the end of @source-->
	<!-- the attributes @key, @value, @method are the same as in group -->
	<!-- there is NO @altType attribute for child, all items that are filtered out will not be imported -->
	<child source="$._source.relateditems.photos" type="GizaPhoto" key="number" value="KHM_AEOS_" method="startsWith">
		<!-- same source could be used multiple times for different MetadataTypes -->
		<metadata source="@.drs_id" target="GizaPhotosDrsId" />
		<metadata source="@.drs_id" target="CatalogIDDigital" />
	</child>

</config_plugin>
```

## General configurations of the plugin

| Value | Description |
| :--- | :--- |
| `jsonFolder` | Path to the folder containing the JSON files. |
| `importFolder` | Path to the folder where downloaded images should be saved before they are imported. |
| `workflow` | Name of the template workflow. |
| `publicationType` | Publication type of the new process. |
| `downloadableUrl` | Name of the types of the URL metadata, from which images are to be downloaded. Here can be configured several types, one tag for each. |
| `metadata` | There will be one Metadata object created out of every tag. The attribute `@source` is a json-path, if it starts with a `$`. The value for the new metadata will be read from this json-path. In these cases the attribute `@source` is regarded to be related to a list if it also ends with `[:]`. If the attribute starts with neither `$` nor `@`, then itself will be used as value for the new metadata. |
| `group` | Here can one configure metadata groups. There are six attributes configurable, see below for details. Under a `group` tag there can be several `metadata` tags configured, whose `@source` attributes, however, should start with `@`. |
| `child` | Here can one configure children DocStructs. There are five attributes configurable, see below for details. Under a `child` tag there can be several `metadata` tags configured, whose `@source` attributes, however, should start with `@`. |

## Configurations of attributes of a metadata group

The following attributes can be configured in a `group` tag:

| Value | Description |
| :--- | :--- |
| `@source` | JSON-path to the parent tag of a list. Starts with a `$`. |
| `@type` | Name of the MetadataGroupType. |
| `@altType` | Name of an alternative MetadataGroupType. If the alternative type is correctly configured, then all items that are filtered out will be redirected to a new MetadataGroup of this alternative type. If this attribute is not configured, or if the configured one can not be found, then all filtered out items will not be imported. OPTIONAL. |
| `@key` | Key of the JSON-objects that is to be used for the filtering. OPTIONAL. |
| `@value` | Value to be compared with by the filtering. OPTIONAL. |
| `@method` | Logic that is to be used by the filtering. Options are `is`, `not`, `startsWith`, `endsWith` and `contains`. OPTIONAL. DEFAULT `is`. |

## Configurations of attributes of a child DocStruct

The following attributes can be configured in a `child` tag:

| Value | Description |
| :--- | :--- |
| `@source` | JSON-path to the parent tag of a list. Starts with a `$`. |
| `@type` | Name of the DocStructType. |
| `@key` | Key of the JSON-objects that is to be used for the filtering. OPTIONAL. |
| `@value` | Value to be compared with by the filtering. OPTIONAL. |
| `@method` | Logic that is to be used by the filtering. Options are `is`, `not`, `startsWith`, `endsWith` and `contains`. OPTIONAL. DEFAULT `is`. |

## Configurations of the metadata for the partner's URL

It is possible to create and save a metadata for the partner's URL. The URL will be formulated as `{urlBase}/{urlPart_1}/{urlPart_2}/.../{urlPart_k}/{urlTail}/`.

| Value | Description |
| :--- | :--- |
| `partnerUrl` | The attribute `save` configures whether a metadata for the partner's URL should be created or not. DEFAULT `false`. |
| `urlBase` | Here one can configure the URL to the partner's server. OPTIONAL. MAXIMAL one tag. |
| `urlPart` | Here one can configure the parts whose values read from the JSON-objects should be attached one by one to the end of the `urlBase`. OPTIONAL. Several tags possible. |
| `urlTail` | Here one can configure the last part of the URL. OPTIONAL. MAXIMAL one tag. |
| `urlMetadata` | Name of the MetadataType for this metadata. |
