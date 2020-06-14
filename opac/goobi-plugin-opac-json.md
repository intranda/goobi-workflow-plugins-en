---
description: OPAC Plugin for the data transfer of JSON data records
---

# Generic JSON Import

## Introduction

This documentation describes the installation, configuration and use of the plugin. You can use this plugin to retrieve data from an external system and transfer it to Goobi. The catalog must have an API that allows records to be delivered as JSON.

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_opac\_json |
| Source code | [https://github.com/intranda/goobi-plugin-opac-json](https://github.com/intranda/goobi-plugin-opac-json) |
| Licence | GPL 2.0 or newer |
| Compatibility | 2020.05 |
| Documentation date | ​13.06.2020 |

## Installation

The plugin consists of two files:

```bash
plugin_intranda_opac_json.jar
plugin_intranda_opac_json.xml
```

The file `plugin_intranda_opac_json.jar` contains the program logic and must be installed readable for the user `tomcat` at the following path:

```bash
/opt/digiverso/goobi/plugins/opac/plugin_intranda_opac_json.jar
```

The file `plugin_intranda_opac_json.xml` must also be readable by the user `tomcat` and must be located under the following path:

```bash
/opt/digiverso/goobi/config/plugin_intranda_opac_json.xml
```

## Configuration

The configuration of the plugin is done in the following files located in the directory `/opt/digiverso/goobi/config/`.

```bash
goobi_opac.xml
plugin_intranda_opac_json.xml
```

In the file `goobi_opac.xml` the interface to the desired catalog system must be made known. This is done with an entry that looks like the following:

```markup
<catalogue title="JSON">
    <config description="JSON OPAC" address="https://example.com/opac?id="
    port="443" database="x" iktlist="x" ucnf="x" opacType="intranda_opac_json" />
    <searchFields>
        <searchField label="Identifier" value="12"/>
    </searchFields>
</catalogue>
```

The `title` attribute contains the name under which the catalog can be selected in the user interface, `address` the URL to the API endpoint and `opacType` the plugin to be used. In this case the entry must be `intranda_opac_json`.

Mapping the contents of the JSON record to Goobi metadata is done within the `plugin_intranda_opac_json.xml` file. The fields within the JSON record are defined using `JSONPath`, the XPath equivalent of JSON.

```markup
<config_plugin>
    <config>
        <recordType field="[?(@.recordType=='archival_object')]" docType="Monograph" />
        <recordType field="$.children[?(@.itemCount &gt; 1)]"  docType="Volume" anchorType="MultiVolumeWork" />
        <recordType field="$.children[?(@.itemCount == 1)]" docType="Monograph" />

        <defaultPublicationType>Monograph</defaultPublicationType>

        <metadata metadata="PublicationYear" field="$.date" />
        <metadata metadata="DocLanguage" field="$.language" />
        <metadata metadata="CatalogIDDigital" field="$.identifier" docType="volume" />
        <metadata metadata="CatalogIDDigital" field="$.children[?(@.itemCount > 1)].children[0].itemId" docType="volume"  />
        <metadata metadata="CatalogIDDigital" field="$.uri" regularExpression="s/\/some-prefix\/(.+)/$1/g" docType="anchor"  />
        <metadata metadata="shelfmarksource" field="$.identifierShelfMark" docType="volume" />
        <metadata metadata="TitleDocMain" field="$.title" docType="volume" />  
        <metadata metadata="OtherTitle"  field="$.alternativeTitle" docType="volume"  />
        <metadata metadata="CurrentNo" field="$..children[0].children[0].sequenceNumber" docType="volume"  />
        <metadata metadata="CurrentNoSorting" field="$..children[0].children[0].sequenceNumber" docType="volume"  />

        <person metadata="Author" field="creator" firstname="s/^(.+?)\, (.+?)$/$2/g" lastname="s/^(.+?)\, (.+?)$/$1/g" validationExpression="/^.+?\, .+?\, .+$/" regularExpression="s/^(.+?)\, (.+?)\, .+/$1\, $2/g"/>

    </config>
</config_plugin>
```

The configuration file contains four types of fields:

| Field type | Description |
| :--- | :--- |
| `recordType` | This type is used to recognize the document type of the JSON record |
| `defaultPublicationType` | This type is used if no document type was recognized before |
| `metadata` | This type is used to map JSON fields to metadata |
| `person` | This type is used to map JSON fields to persons |

### Field type: recordType

The element `recordType` contains the attributes `field`, `docType` and `anchorType`. In `field` a JSONPath expression is specified that is applied to the record. If the type is a multi-volume work or newspaper/magazine, the `anchor` type to be used must be specified in the `anchorType` field. If a field with such an expression exists, the document type defined in `docType` is created. If not, the next configured `recordType` will be checked.

There are a number of characters that are masked in this file. This includes characters such as `< > & "`, which have a special meaning in XML and must therefore be specified as `&lt; &gt; &amp; &quot;`. Also affected is the `comma`, which must also be masked as `\,` using a backslash.

### Field type: defaultPublicationType

If none of the definitions apply, a document can be created with the type from `defaultPublicationType`. If this field is missing or empty, no record is created instead.

### Field types: metadata & person

The two fields `metadata` and `person` are used to import individual content from the JSON record into the respective metadata. A number of attributes are available for this purpose:

| Attribute | Meaning |
| :--- | :--- |
| `metadata` | Contains the name of the metadata or person |
| `field` | Path to the content within the JSON object |
| `docType` | May have the value `anchor` or `volume`. The default value is `volume`. Fields marked with `anchor` are only checked and imported for multi-volume works. |
| `validationExpression` | Regular expression, which checks if the found value matches the defined expression. If this is not the case, the value is ignored. |
| `regularExpression` | A regular expression to manipulate the value. This is applied after the `validationExpression` check. |
| `firstname` | A regular expression that determines the first name of a person from the field contents. |
| `lastname` | A regular expression that determines the last name of a person from the field contents. |

## Use

When you search for an identifier in Goobi, a request is sent to the configured URL in the background.

![Goobi workflow interface for querying the catalogue](../.gitbook/assets/plugin_opac_json_en.png)

According to the configuration described above, this corresponds approximately to the following URL:

```text
https://example.com/opac?id=[IDENTIFIER]
```

If a valid record is found under this URL, it will be searched for the fields defined within `recordType` in which the document type should be located. If no fields are defined or they are not found, the type from the configured element `defaultPublicationType` is used instead. The required structure element is then created with the determined type.

The configured expressions of the `metadata` and `person` are then evaluated in sequence. If data is found with an expression, the corresponding specified metadata is generated.

## Useful links

The following URLs could be of further help for the installation or especially for the configuration of the plugin:

JSONPath Online Evaluator: [https://jsonpath.com/](https://jsonpath.com/)

JSONPath Description: [https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html](https://support.smartbear.com/alertsite/docs/monitors/api/endpoint/jsonpath.html)

