---
description: >-
  This is a Goobi step plugin to allow the registration of digital objects at
  the DataCite DOI service.
---

# Plugin for registering DOI via the DataCite API

## Introduction

This documentation describes the installation, configuration and use of the plugin.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_datacite\_doi |
| Source code | [https://github.com/intranda/goobi-plugin-step-datacite-doi](https://github.com/intranda/goobi-plugin-step-datacite-doi) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2021.03 |
| Documentation date | 15.03.2021 |

## Installation

The plugin consists of these files:

```bash
plugin_intranda_step_datacite_doi.jar
plugin_intranda_step_datacite_doi.xml
plugin_intranda_step_datacite_mapping.xml
```

The file `plugin_intranda_step_datacite_doi.jar` contains the program logic. it needs to be installed at this path:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_datacite_doi.jar
```

The file `plugin_intranda_step_datacite_mapping.xml` is the mapping file, defining how local metadata should be translated to the form required for the DOI registration. It needs to be installed at this path:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_datacite_mapping.xml
```

The file `plugin_intranda_step_datacite_doi.xml` is the main configuration file for the plugin. It needs to be installed at this path:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_datacite_doi.xml
```

## Configuration of the plug-in

### Main configuration

The configuration is done via the configuration file `plugin_intranda_step_datacite_doi.xml` and can be adapted during operation. It is structured as follows:

```markup
<config_plugin>
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>

        <!-- authentication and main information -->
        <!-- For testing: for deployment, remove "test" -->
        <SERVICE_ADDRESS>https://mds.test.datacite.org/</SERVICE_ADDRESS>

        <base>10.80831</base>
        <url>https://viewer.goobi.io/idresolver?doi=</url>
        <USERNAME>YZVP.GOOBI</USERNAME>
        <PASSWORD>password</PASSWORD>

        <!-- configuration for DOIs -->
        <prefix>go</prefix>
        <name>goobi</name>
        <separator>-</separator>
        <handleMetadata>DOI</handleMetadata>

        <!-- configuration for DOIs -->
        <doiMapping>/opt/digiverso/goobi/config/plugin_intranda_step_datacite_mapping.xml</doiMapping>

        <!-- Type of DocStruct which should be given DOIs -->
        <typeForDOI>Article</typeForDOI>

        <!-- display button to finish the task directly from within the entered plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

The block `<config>` can occur repeatedly for different projects or workflow steps in order to be able to perform different actions within different workflows. The other parameters within this configuration file have the following meanings:

| Value | Description |
| :--- | :--- |
| `project` | This parameter determines for which project the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which workflow steps the block `<config>` should apply. The name of the workflow step is used here. This parameter can occur several times per `<config>` block. |
| `SERVICE_ADDRESS` | This parameter defines the URL for the Datacite service. In the example above, it is the test server. |
| `base` | This parameter defines the DOI base for the institution, which has been registered with Datacite. |
| `url` | The url parameter defines the prefix accorded to each DOI link. A DOI "10.80831/goobi-1", for example, will here be given the hyperlink "[https://viewer.goobi.io/idresolver?doi=10.80831/goobi-1](https://viewer.goobi.io/idresolver?doi=10.80831/goobi-1)" |
| `USERNAME` | This is the username that is used for the DataCite registration. |
| `PASSWORD` | This is the password that is used for the DataCite registration. |
| `prefix` | This is the prefix that may be given to the DOI before the name and ID of the document. |
| `name` | This is the name that may be given to the DOI before the ID number of the document. |
| `separator` | Define here a separator that shall be used between the different parts of the DOI. |
| `handleMetadata` | This parameter specifies under which metadata name the DOI is to be saved in the METS-MODS file. Default is `_urn`. |
| `doiMapping` | In this parameter the path to the mapping file for the DOI registration is defined. |
| `typeForDOI` | With this parameter the DocStruct type can be defined which will be given DOIs. If this is empty or missing, the top DocStruct elemtnet only will be given a DOI. If the parameter contains the name of a sub-DocStruct, then these will be given DOIs. |

## Configuration inside of the Mapping file

The mapping configuration file looks something like this:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Mapping>
  <!-- Mandatory fields: -->
  <map>
    <field>title</field>
    <metadata>TitleDocMain</metadata>
    <altMetadata>TitleDocMainShort</altMetadata>
    <altMetadata>Title</altMetadata>
    <default>Fragment</default>
  </map>

  <map>
    <field>author</field>
    <metadata>Author</metadata>
    <altMetadata>Composer</altMetadata>
    <altMetadata>IllustratorArtist</altMetadata>
    <altMetadata>WriterCorporate</altMetadata>
    <default>unkn</default>
  </map>

  <map>
    <field>publisher</field>
    <metadata>Publisher</metadata>
    <altMetadata>PublisherName</altMetadata>
    <altMetadata>PublisherPerson</altMetadata>
    <default>unkn</default>
  </map>

  <map>
    <field>publicationYear</field>
    <metadata>_dateDigitization</metadata>
    <default>#CurrentYear</default>
  </map>

  <map>
    <field>inst</field>
    <default>intranda</default>
  </map>


  <map>
    <field>resourceType</field>
    <default>document</default>
  </map>

  <!-- Optional fields: -->
  <map>
    <field>editor</field>
    <metadata>Editor</metadata>
  </map>

</Mapping>
```

For each `<map>`, the `<field>` specifies the name of the DOI element, and the `<metadata>` and `<altMetadata>` entries indicate from which metadata of the DocStruct the value should be taken, in order. If there is no such entry in the DocStruct, then the `<default>` value is taken. The value `"unkn"` for "unknown" is recommended by Datacite for data which is missing.

For the mandatory fields, a `<default>` must be specified; for optional fields this is not necessary, but may be done if wished.

The default entry `#CurrentYear` is a special case: it is replaced with the current year in the DOI.

## Integration of the plugin into the workflow

To put the plugin into operation, it must be activated for one or more desired tasks in the workflow. This is done as shown in the following screenshot by selecting the plugin `intranda_step_datacite_doi` from the list of installed plugins.

![Assigning the plugin to a specific task](../.gitbook/assets/intranda_step_datacite_doi_en.png)

Since this plugin is usually to be executed automatically, the workflow step should be configured as automatic in the workflow. As the plugin writes the DOI to the metadata file of the process, the checkbox for `Update metadata index when finishing` should be ticked.

## Operation of the plugin

The program examines the metadata fields of a METS-MODS file from a Goobi process. If a `<typeForDOI>` is specified, then it will go through each DocStruct of this type in the file. If not, then it will take the top DocStruct. From these it creates the data for a DOI, using the mapping file to translate. It then registers the DOI via the MDS API of DataCite, with DOI given by the `<base>` together with any `<prefix>` and `<name>`, and the ID of the document \(its `CatalogIDDigital`\) plus an increment, if there are more than one DOIs generated for the given document. The record is given a registered URL defined by the `<url>` followed by the DOI. The generated DOI is written into the METS/MODS file under the metadata specified in `<handleMetadata>`. If the `<typeForDOI>` is `Article` for example, then each Article in the METS/MODS file will be given a DOI, which is saved in the metadata under `<handleMetadata>` for each Article.

