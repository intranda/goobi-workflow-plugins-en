---
description: >-
  This workflow plugin allows the mass import of single pages of Volksblatt from Liechtenstein.
---

# Workflow-plugin for the import of Newspaper pages of Liechtenstein Volksblatt

## Introduction

This workflow plugin was implemented to read metadata from the files' names as well as a configuration file and create or update processes and metadata properly. This Plugin is designed originally for the import of Newspaper pages from Liechtenstein Volksblatt, but it can also be used for other Newspaper import tasks as long as their page names follow the same pattern as `001_vbhp_4c_2019-01-11` where the first three digits indicate the ordering number of this page among its issue, and the tailing date is the issue's date. The description substring in between does not matter, as long as it does NOT match the regular expression `\d{4}-\d{2}-\d{2}` which is reserved for saving the issue's date.

## Overview

| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_liechtenstein\_volksblatt\_importer |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer](https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 23.10 |
| Documentation date | 16.Nov.2023 |

## Installation

* For the installation one needs `plugin_intranda_workflow_liechtenstein_volksblatt_importer.jar` and `plugin_intranda_workflow_liechtenstein_volksblatt_importer-GUI.jar`
```bash
{PATH_TO_GOOBI}/plugins/workflow/plugin_intranda_workflow_liechtenstein_volksblatt_importer.jar
{PATH_TO_GOOBI}/plugins/GUI/plugin_intranda_workflow_liechtenstein_volksblatt_importer-GUI.jar
```
* The configuration file `plugin_intranda_workflow_liechtenstein_volksblatt_importer.xml` is to be copied to:
```bash
{PATH_TO_GOOBI}/config/plugin_intranda_workflow_liechtenstein_volksblatt_importer.xml
```

## Configuration

Configurations are supposed to be done in the configuration file, which may look like the following example:

```xml
<config_plugin>
	<!-- which folder to use as import source -->
	<importFolder>/opt/digiverso/import/sample/</importFolder>

	<!-- which workflow to use -->
	<workflow>Newspaper_workflow</workflow>

	<!-- Whether or not to delete the images from the import folder once they are imported. OPTIONAL. DEFAULT false. -->
	<deleteFromSource>true</deleteFromSource>

	<!-- Configure here the metadata that shall be added to the anchor file or the volume part of the mets file. -->
	<!-- This tag accepts the following attributes:
		- @value: metadata value template, which may contain a variable defined by @var wrapped with _ from both sides
		- @type: metadata type
		- @var: variable that is ready to be used in @value. To use it, wrap it with _ from both sides and put it into the @value string. OPTIONAL.
					- Options are YEAR | MONTH | DAY | DATE | DATEFINE, where cases only matters for the references in @value string.
					- The only difference between DATE and DATEFINE are their representations of the date: DATE keeps the original format "yyyy-mm-dd" while DATEFINE takes a new one "dd. MMM. yyyy".
					- The value of an unknown variable will be its name.
					- For example,
						- IF one defines a @var by "year", then it should be referenced in @value using "_year_"
						- IF one defines a @var by "YEAR", then it should be referenced in @value using "_YEAR_", although YEAR and year are actually the same option
						- IF one defines a @var by "unknown", then any occurrences of "_unknown_" will be replaced by "unknown"
						- IF one wants to have a @value template containing "_Year_" as hard-coded, then "Year" should be avoided to be @var. If in such cases such a
						  variable is still needed, then one can define @var to be something like "yEaR".
		- @anchor: true if this metadata is an anchor metadata, false if not. OPTIONAL. DEFAULT false.
		- @volume: true if this metadata is a volume metadata, false if not. OPTIONAL. DEFAULT false.
		- @person: true if this metadata is a person metadata, false if not. OPTIONAL. DEFAULT false.
	 -->
	<metadata value="CHANGE_ME" type="TitleDocMain" anchor="true" volume="false" person="false" />
	<!-- The @volume attribute is by default false. -->
	<metadata value="CHANGE_ME" type="CatalogIDSource" anchor="true" person="false" />
	<!-- The @person attribute is by default false. -->
	<metadata value="CHANGE_ME" type="CatalogIDDigital" anchor="true" />

	<metadata value="CHANGE_ME" type="CurrentNoSorting" anchor="false" volume="true" />
	<!-- The @anchor attribute is by default false. -->
	<metadata value="zeitungen#livb" type="SubjectTopic" volume="true" />
	<metadata value="pt_zeitung" type="Publikationstyp" volume="true" />
	<metadata value="zeitungsherausgeber#livb" type="Classification" volume="true" />
	<metadata value="eli" type="ViewerInstance" volume="true" />

	<!-- Use @var to generate the final metadata values in the run. -->
	<metadata var="YEAR" value="Newspaper Volume _YEAR_" type="CatalogIDDigital" volume="true" />
	<metadata var="YEAR" value="Liechtensteiner Volksblatt (_YEAR_)" type="TitleDocMain" volume="true" />
	<metadata var="YEAR" value="_YEAR_" type="CurrentNo" volume="true" />
	<metadata var="YEAR" value="_YEAR_" type="PublicationYear" volume="true" />

</config_plugin>
```

| Value | Description |
| :--- | :--- |
| `importFolder` | Path to the folder that contains the separated newspaper pages ready for import. |
| `workflow` | Name of the template workflow. |
| `deleteFromSource` | If `true` then the files from the import folder will be deleted once they are successfully imported, otherwise all files in the import folder will remain untouched. |
| `metadata` | There will be one Metadata object created out of every tag. It accepts six attributes, where `@value` and `@type` are mandatory, while `@var`, `@anchor`, `@volume` and `@person` are all optional. See the comments in the example configuration for more details. |

## Instruction for use

* This plugin does not care about the actual file formats of the newspaper pages that are to be imported, since all metadata that need to be recorded will be read directly from the files' names as well as from the configuration file. The page files will be distributed into the master folders of proper Goobi processes.
* The file formats in the file links that will be recorded in the METS file by this plugin will be changed to `tiff` and `jpg` because only they can be properly rendered by the Metadata Editor. If one wants to see the pages correctly after the import, one has to add a conversion step after the import. ***ATTENTION***: even if this conversion step is supposed to run automatically, one has to trigger it manuelly, alternatively one can add a blank step between the import step and this conversion step and manuelly change this blank step's status when the import is done.
* Following is an example to illustrate the creation and use of such a conversion step, in the original use case for Liechtenstein Volksblatt, where the original newspaper pages are of format PDF:

  1. Install `pdftoppm` if not done yet
  2. Put the following script file to `{PATH_TO_GOOBI}/scripts/script_convertPdfToTiff.sh`
    ```bash
    #!/bin/bash

    # first variable: folder
    folder="$1"

    # change directory
    cd $folder

    # convert all pdf files to tiff
    for pdfFile in *.pdf; do
       pdftoppm $pdfFile -tiff -r 300 -cropbox ${pdfFile%.pdf};
    done

    # remove the stupid -1 added by pdftoppm
    for tifFile in *.tif; do
       mv -- "$tifFile" "${tifFile%-1.tif}.tif";
    done

    # delete all pdf files
    rm *.pdf
    ```
  3. Create a script step with script path `{PATH_TO_GOOBI}/scripts/script_convertPdfToTiff.sh "{origpath}"`
