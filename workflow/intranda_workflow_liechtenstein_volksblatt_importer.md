---
description: >-
  This workflow plugin enables the mass import of individual newspaper editions for the Liechtensteiner Volksblatt
---

# Import of newspaper editions for the Liechtensteiner Volksblatt

## Introduction
This workflow plugin was implemented to read metadata from the file names and a configuration file and to correctly create or update processes and metadata. This plugin was originally developed for the import of newspaper issues of the Liechtensteiner Volksblatt, but can also be used for other imports as long as their page names follow the same pattern as `001_vbhp_4c_2019-01-11`, where the first three digits indicate the serial number of this page within its issue and the final date is the date of the issue. The description text in between does not matter as long as it does not match the regular expression `\d{4}-\d{2}-\d{2}`, which is reserved for storing the issue date.


## Overview
| Details |  |
| :--- | :--- |
| Identifier | intranda\_workflow\_liechtenstein\_volksblatt\_importer |
| Source code | [https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer](https://github.com/intranda/goobi-plugin-workflow-liechtenstein-volksblatt-importer) |
| Licence | GPL 2.0 or newer |
| Documentation date | 16.11.2023 |

## Installation

To install the plugin, the following two files must be installed:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_liechtenstein_volksblatt_importer.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_liechtenstein_volksblatt_importer-GUI.jar
```

To configure how the plugin should behave, various values can be adjusted in the configuration file. The configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_liechtenstein_volksblatt_importer.xml
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

The individual parameters are used as follows:

| Value | Description |
| :--- | :--- |
| `importFolder` | Path to the folder containing the separated newspaper pages for the import. |
| `workflow` | Name of the production template to be used. |
| `deleteFromSource` | If the value is 'true', the files are deleted from the import folder as soon as they have been successfully imported, otherwise all files in the import folder remain unchanged. |
| `metadata` | An independent metadata is created from each element specified here. It accepts six attributes, where `@value` and `@type` are mandatory, while `@var`, `@anchor`, `@volume` and `@person` are optional. Further details can be found in the comments within the sample configuration. |

## Using the plugin
* It is not important for the plugin which file formats the newspaper pages to be imported have, as all the metadata that needs to be saved is read directly from the file names and from the configuration file. The page files are distributed to the master folders of the corresponding Goobi processes.
* The file formats in the file links created by this plugin in the METS file are changed to `tiff` and `jpg`, as only these can be rendered correctly by the metadata editor. If the pages cannot be viewed correctly after import, the files may need to be converted first. In the event that these are PDF files that are to be imported
to be imported, such a step could look like this:

  1. Install the package `pdftoppm`, if not already done
  2. create a script file under the name `/opt/digiverso/goobi/scripts/script_convertPdfToTiff.sh`
   
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
  3. Create a workflow step in the workflow with the path to the script `/opt/digiverso/goobi/scripts/script_convertPdfToTiff.sh "{origpath}"`
