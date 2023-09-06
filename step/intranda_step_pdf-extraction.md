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
| Documentation date | 06.Sep.2023 |

## Precondition

The precondition for using the plugin is the use of Goobi workflow in version 3.0.6 or higher, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired workflow steps.

The command line program `Ghostscript` and/or the tool `pdftoppm` from the package `poppler-utils` will also be required, depending on the configuration of the tag `<generator>`. They can be installed from the system's package sources.

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

		<project>*</project>
		<step>OCR-Extraktion</step>

		<validation>
			<!-- set to false to skip this step if no PDF files exist in the source folder -->
			<!-- DEFAULT true -->
			<failOnMissingPDF>true</failOnMissingPDF>
		</validation>
		
		<!-- if true then all old data from tifFolder, pdfFolder, textFolder and altoFolder will be deleted, and all file references will be removed if current book is not empty -->
		<!-- DEFAULT true -->
		<overwriteExistingData>true</overwriteExistingData>

		<mets>
			<!-- DEFAULT true -->
			<write>false</write>
			<!-- DEFAULT true -->
			<failOnError>false</failOnError>
			<!-- Settings for writing Mets-Structure -->
			<docType>
				<!-- If this element exists and is not empty then for each imported pdf 
				a new StructElement of the given type is created within the top StructElement. -->
				<parent>Chapter</parent>
				<!-- If this element exists and is not empty then the table-of-content structure of the pdf is
				written into the Mets file. Each structure element of the PDF is written as a StructElement of the given type. -->
				<children>Chapter</children>
			</docType>
		</mets>

		<images>
			<!-- DEFAULT true -->
			<write>false</write>
			<!-- DEFAULT true -->
			<failOnError>true</failOnError>
			<!-- The resolution with which to scan the PDF file. This has a large impact on both image file size and quality. DEFAULT 300. -->
			<resolution>300</resolution>
			<!-- The image format for the image files written. DEFAULT tif. -->
			<!-- Allowed formats for the generator pdftoppm are png, jpg, jpeg, jpegcmyk, tif, tiff.  -->
			<format>tif</format>
			<!-- Select the command line tool which should be used to create the images. Either 'ghostscript' or 'pdftoppm'. -->
			<generator>pdftoppm</generator>						
			<!-- A parameter to add to the generator call. Repeatable -->
			<generatorParameter>-cropbox</generatorParameter>
			<!-- Hardcoded parameters for ghostscript are: -dUseCropBox, -SDEVICE, -r<res>, -sOutputFile, -dNOPAUSE, -dBATCH.
			     Useful parameters for configuration are:
			     ===================================================
			     -q                         `quiet`, fewer messages
			     ...................................................
			     -g<width>x<height>          page size in pixels 
			     ===================================================
			-->
			<!-- Hardcoded parameters for pdftoppm are: -{format}, -r.
			     Useful parameters for configuration are:
			     ======================================================================================================
			     -f <int>                           first page to print
			     ......................................................................................................
			     -l <int>                           last page to print
			     ......................................................................................................
			     -o                                 print only odd pages
			     ......................................................................................................
			     -e                                 print only even pages
			     ......................................................................................................
			     -singlefile                        write only the first page and do not add digits
			     ......................................................................................................
			     -scale-dimension-before-rotation   for rotated pdf, resize dimensions before the rotation
			     ......................................................................................................
			     -rx <fp>                           X resolution, in DPI
			     ......................................................................................................
			     -ry <fp>                           Y resolution, in DPI
			     ......................................................................................................
			     -scale-to <int>                    scales each page to fit within scale-to*scale-to pixel box
			     ......................................................................................................
			     -scale-to-x <int>                  scales each page horizontally to fit in scale-to-x pixels
			     ......................................................................................................
			     -scale-to-y <int>                  scales each page vertically to fit in scale-to-y pixels
			     ......................................................................................................
			     -x <int>                           x-coordinate of the crop area top left corner
			     ......................................................................................................
			     -y <int>                           y-coordinate of the crop area top left corner
			     ......................................................................................................
			     -W <int>                           width of crop area in pixels (DEFAULT 0)
			     ......................................................................................................
			     -H <int>                           height of crop area in pixels (DEFAULT 0)
			     ......................................................................................................
			     -sz <int>                          size of crop square in pixels (sets W and H)
			     ......................................................................................................
			     -cropbox                           use the crop box rather than media box
			     ......................................................................................................
			     -hide-annotations                  do not show annotations
			     ......................................................................................................
			     -mono                              generate a monochrome PBM file
			     ......................................................................................................
			     -gray                              generate a grayscale PGM file
			     ......................................................................................................
			     -sep <string>                      single character separator between name and page number (DEFAULT -)
			     ......................................................................................................
			     -forcenum                          force page number even if there is only one page
			     ......................................................................................................
			     -overprint                         enable overprint
			     ......................................................................................................
			     -freetype <string>                 enable FreeType font rasterizer: yes, no
			     ......................................................................................................
			     -thinlinemode <string>             set thin line mode: none, solid, shape. DEFAULT none.
			     ......................................................................................................
			     -aa <string>                       enable font anti-aliasing: yes, no
			     ......................................................................................................
			     -aaVector <string>                 enable vector anti-aliasing: yes, no
			     ......................................................................................................
			     -opw <string>                      owner password (for encrypted files)
			     ......................................................................................................
			     -upw <string>                      user password (for encrypted files)
			     ......................................................................................................
			     -q                                 don't print any messages or errors
			     ......................................................................................................
			     -progress                          print progress info
			     ......................................................................................................
			     -tiffcompression <string>          set TIFF compression: none, packbits, jpeg, lzw, deflate
			     ======================================================================================================
			-->
		</images>

		<plaintext>
			<!-- DEFAULT true -->
			<write>true</write>
			<!-- DEFAULT true -->
			<failOnError>false</failOnError>
		</plaintext>

		<alto>
			<!-- DEFAULT true -->
			<write>true</write>
			<!-- DEFAULT true -->
			<failOnError>false</failOnError>
		</alto>

		<pagePdfs>
			<!-- DEFAULT true -->
			<write>true</write>
			<!-- DEFAULT true -->
			<failOnError>true</failOnError>
		</pagePdfs>

		<properties>
			<!-- Write this process property after extraction is done. The value depends on whether any ocr files with content have been written. -->
			<!-- If there exist some properties named so, then the first one will be picked up to accept the value.
			Otherwise a new process property will be created for this purpose. ONLY 1 fulltext tag is allowed.  -->
			<fulltext>
				<!-- process property name. If blank, no property will be written -->
				<name>OCRDone</name>
				<!-- property value when there are alto contents or text contents created. DEFAULT TRUE. -->
				<value exists="true">YES</value>
				<!-- property value when there are neither contents nor text contents created. DEFAULT FALSE. -->
				<value exists="false">NO</value>
			</fulltext>
		</properties>

	</config>
```
The `<mets>` tag controls the generation of mets files, under which many configurations are possible, for example the `<docType>` tag controls which structure types the entries extracted from the PDF table of contents receive in the METS file. The parent element is the main element in which all other table of contents entries land. If it is omitted, all entries are entered directly into the main element of the METS file.

The `<images>`-tag controls the resolution \(in DPI\) and the output format for the extracted images.

The `<plaintext>` `<alto>` and `<pagePdfs>` tags control the generation of text files, alto files and pdf files for separated pages.

With `<properties>`, process properties are written depending on the result of the extraction. The configuration given here as an example writes the process property `OCRDone` with value `YES` if full text was found in the PDF and with value `NO` if there was no full text in the PDF file. This is particularly helpful if the workflow is to be changed afterwards, for example to omit an OCR step if full text already exists.

## Settings in Goobi

After the plugin has been installed, it can be configured in the user interface in a workflow step.

![Configuration of the step in Goobi workflow](../.gitbook/assets/intranda_step_pdf_extraction.png)

## Usage

To use the plugin, a PDF file must be in the master folder of the process at the time of execution. This is then automatically divided into individual pages. In addition, the full text \(if available\) is extracted and the table of contents of the PDF file is read in order to be entered as structural elements in the METS file.

It is therefore recommended to pre-store another workflow step for the workflow step with this plugin, in which files are loaded into the master folder. This can be done by linking the process folder to the home folder of the user or, for example, in the File-Upload plug-in.

