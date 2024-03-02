---
description: >-
  This is the technical documentation for the Goobi plugin for automatically
  reading information from PDF files.
---

# Split PDFs, Extract Full Text and Read Table of Contents

## Introduction

This documentation describes how to install, configure and use a plugin to extract images, full texts and the table of contents from PDF files. The plugin always extracts only what is present in the PDF and does not write an error message if no full text or table of contents can be found.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_pdf-extraction |
| Source code | [https://github.com/intranda/goobi-plugin-step-pdf-extraction](https://github.com/intranda/goobi-plugin-step-pdf-extraction) |
| Licence | GPL 2.0 or newer |
| Documentation date | 09.04.2019 |

## Precondition

The precondition for using the plugin is the use of Goobi workflow in version 3.0.6 or higher, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired workflow steps.

The command line program Ghostscript is also required. It can be installed from the system's package sources.

## Installation and Configuration

To use the plugin, it must be copied to the following location:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_pdf-extraction.jar
```

The configuration of the plugin is expected under the following path:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_pdf-extraction.xml
```

An example configuration could look like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configs>

	<config>
		<!--
        order of configuration is:
        1.) project name and step name matches
        2.) step name matches and project is *
        3.) project name matches and step name is *
        4.) project name and step name are *
        -->
		<project>*</project>
		<step>*</step>

		<validation>
			<failOnMissingPDF>true</failOnMissingPDF>
		</validation>

		<overwriteExistingData>true</overwriteExistingData>

		<docType>
			<parent>Chapter</parent>
			<children>Chapter</children>
		</docType>

		<mets>
			<write>true</write>
			<failOnError>false</failOnError>
		</mets>

		<images>
			<write>true</write>
			<failOnError>true</failOnError>
			<resolution>300</resolution>
			<format>tif</format>
			<generator>pdftoppm</generator>
			<generatorParameter>-cropbox</generatorParameter>
		</images>

		<plaintext>
			<write>true</write>
			<failOnError>false</failOnError>
		</plaintext>

		<alto>
			<write>true</write>
			<failOnError>false</failOnError>
		</alto>

		<pagePdfs>
			<write>true</write>
			<failOnError>false</failOnError>
		</pagePdfs>

		<properties>
			<fulltext>
				<name>OCRDone</name>
				<value exists="true">YES</value>
				<value exists="false">NO</value>
			</fulltext>
		</properties>

	</config>

	<!-- more configurations can be added for projects/steps
	<config>
	</config>
	-->

</configs>
```

## Settings in the configuration file

In the configuration file any number of configurations can be defined for projects or processes with specific names. For this purpose, different `<config>` blocks can be used among each other, in each of which the `<project>` and `<step>` properties must be specified. The `<config>` blocks are applied to a particular step in the following order:

1) `<project>` and `<step>` correspond to the current project and workflow step.
2) `<step>` corresponds to the current workflow step and `<project>` is set to `*`.
3) `<project>` corresponds to the current project and `<step>` is set to `*`.
4) `<project>` and `<step>` are set to `*`.

The `<failOnMissingPDF>` tag within the `<validation>` tag can be set to `true` to issue a warning if no PDF files could be found. Warnings will then be written to the journal and the server-internal log files. If this option is disabled with `false`, the case that no PDF files may exist will be ignored.

The `<overwriteExistingData>` tag can be used to set globally for this plugin whether existing PDF files may be overwritten or not.

The `<docType>` tag controls which structure types the entries extracted from the PDF contents directory get in the METS file. The `<parent>` element is the main element where all other table of contents entries end up. If it is omitted, all entries will be placed directly into the main element of the METS file. The `<children>` element is used to specify the structure type of the subelements of the entry extracted from the PDF table of contents.

The `<pagePdfs>`, `<alto>`, `<plaintext>`, `<images>` and `<mets>` elements each have a `<write>` and `<failOnError>` property. This can be used to set, according to the XML element for PDF files, ALTO files, TXT files, general image files, and the METS file, whether files of these types should be written or overwritten respectively, and whether an error message should be issued and further execution aborted if they could not be written.

In the `<images>` tag some further settings for image files are possible. The values in `<resolution>` and `<format>` can be used to specify the image resolution \(in DPI\) and the output file format for the extracted images.

The `<generator>` sub-element within `<images>` specifies which executable program on the server should be used to extract the images. Valid values are usually `pdftoppm` and `ghostscript`. The `<generatorParameter>` element can be used multiple times, each containing a command line parameter for the program specified in `<generator>`.

With `<properties>` process properties are written depending on the result of the extraction. The configuration given here as an example writes the operation property `OCRDone` with value `YES` if full text was found in the PDF, and with value `NO` if there was no full text in the PDF. This is especially useful if the workflow is to be changed afterwards, for example to omit an OCR step if full text already exists.

## Settings in Goobi

After the plugin has been installed, it can be configured in the user interface in a workflow step.

![Configuration of the step in Goobi workflow](../.gitbook/assets/intranda_step_pdf_extraction.png)

## Usage

To use the plugin, a PDF file must be in the master folder of the process at the time of execution. This is then automatically divided into individual pages. In addition, the full text \(if available\) is extracted and the table of contents of the PDF file is read in order to be entered as structural elements in the METS file.

It is therefore recommended to pre-store another workflow step for the workflow step with this plugin, in which files are loaded into the master folder. This can be done by linking the process folder to the home folder of the user or, for example, in the File-Upload plug-in.

