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
| Compatibility | Goobi Workflow 3.0.6 and newer |
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

```markup
<config>
    <docType>
        <parent>Chapter</parent>
        <children>Chapter</children>
    </docType>

    <images>
        <resolution>300</resolution>
        <format>tif</format>
    </images>

    <properties>
        <fulltext>
            <name>OCRDone</name>
            <value exists="true">YES</value>
            <value exists="false">NO</value>
        </fulltext>
    </properties>
</config>
```

`docType` controls which structure types the entries extracted from the PDF table of contents receive in the METS file. The parent element is the main element in which all other table of contents entries land. If it is omitted, all entries are entered directly into the main element of the METS file.

The `images`-tag controls the resolution \(in DPI\) and the output format for the extracted images.

With `properties`, process properties are written depending on the result of the extraction. The configuration given here as an example writes the process property `OCRDone` with value `YES` if full text was found in the PDF and with value `NO` if there was no full text in the PDF file. This is particularly helpful if the workflow is to be changed afterwards, for example to omit an OCR step if full text already exists.

## Settings in Goobi

After the plugin has been installed, it can be configured in the user interface in a workflow step.

![Configuration of the step in Goobi workflow](../.gitbook/assets/intranda_step_pdf_extraction.png)

## Usage

To use the plugin, a PDF file must be in the master folder of the process at the time of execution. This is then automatically divided into individual pages. In addition, the full text \(if available\) is extracted and the table of contents of the PDF file is read in order to be entered as structural elements in the METS file.

It is therefore recommended to pre-store another workflow step for the workflow step with this plugin, in which files are loaded into the master folder. This can be done by linking the process folder to the home folder of the user or, for example, in the File-Upload plug-in.
