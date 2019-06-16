---
description: >-
  Goobi plugin for exporting Goobi processes to a Fedora repository for the
  Victoria Public Record Office
---

# Fedora Export PROV

## Introduction

This documentation describes the installation, configuration and use of the Fedora Export Plugin in Goobi workflow.

| Details |  |
| :--- | :--- |
| Plugin version | 1.0.0 |
| Identifier | plugin\_intranda\_export\_fedora\_prov |
| Source code | [https://gitea.intranda.com/goobi-workflow/goobi-plugin-export-fedora-prov](https://gitea.intranda.com/goobi-workflow/goobi-plugin-export-fedora-prov) |
| Compatibility | Goobi workflow 3.0 and newer |
| Documentation date | 19.02.2019 |

## Configuration

The configuration is done via the configuration file `intranda_export_fedora.xml` and can be adapted during operation.

```markup
<config_plugin>
    <config>
        <!-- which workflow to use for (can be more then one, otherwise use *) -->
        <workflow>*</workflow>

        <!-- general Fedora configuration data -->
        <fedoraUrl>http://localhost:8080/fedora/rest</fedoraUrl>
        <useVersioning>false</useVersioning>

        <!-- which content to ingest -->
        <ingestMaster>true</ingestMaster>
        <ingestMedia>false</ingestMedia>
        <ingestJp2>false</ingestJp2>
        <ingestPdf>false</ingestPdf>
        
        <!-- command for specific property including the parameter for Barcode and for the unit-or-item-type -->
        <externalLinkContent>
            PREFIX crm: &lt;http://www.cidoc-crm.org/cidoc-crm/&gt; 
            INSERT { &lt;&gt; crm:P70_documents &lt;http://access.prov.vic.gov.au/public/component/daPublicBaseContainer?component=daView[UNIT_ITEM_CODE]&amp;entityId=[BARCODE]#&gt; } 
            WHERE { }
        </externalLinkContent>

        <!-- command for specific property including the parameter for full_partial -->
        <fullPartialContent>
            PREFIX crm: &lt;http://www.cidoc-crm.org/cidoc-crm/&gt; 
            INSERT { &lt;&gt; crm:P3_has_note "[FULL_PARTIAL]" } 
            WHERE { }
        </fullPartialContent>
        
        <!-- Properties query for the /images container -->
        <imagesContainerMetadataQuery>
            PREFIX ldp: &lt;http://www.w3.org/ns/ldp#&gt; 
            PREFIX pcdm: &lt;http://pcdm.org/models#&gt;
            INSERT {
                &lt;&gt; a ldp:DirectContainer\,pcdm:Object ;
                ldp:membershipResource &lt;[URL]&gt; ;
                ldp:hasMemberRelation pcdm:hasMember .
            } 
            WHERE { }
        </imagesContainerMetadataQuery>
        
        <!-- Properties query for the /files container -->
        <filesContainerMetadataQuery>
            PREFIX ldp: &lt;http://www.w3.org/ns/ldp#&gt; 
            PREFIX pcdm: &lt;http://pcdm.org/models#&gt;
            INSERT {
                &lt;&gt; a ldp:DirectContainer\,pcdm:Object ;
                ldp:membershipResource &lt;[URL]&gt; ;
                ldp:hasMemberRelation pcdm:hasFile .
            } 
            WHERE { }
        </filesContainerMetadataQuery>
        
        <!-- Properties query for the /fcr:metadata part of a file -->
        <imageFileMetadataQuery>
            PREFIX exif: &lt;https://www.w3.org/2003/12/exif/ns#&gt; 
            INSERT {
                &lt;&gt;  exif:imageLength [HEIGHT] ;
                exif:imageWidth [WIDTH] .
            } 
            WHERE { }
        </imageFileMetadataQuery>

    </config>
    
    <config>
        <!-- which workflow to use for (can be more then one, otherwise use *) -->
        <workflow>My_special_workflow</workflow>
        ...
    </config>
</config_plugin>
```

| Parameter | Erläuterung |
| :--- | :--- |
| `fedoraUrl` | REST Endpoint of the Fedora application |
| `useVersioning` | If `true`, the versioning of Fedora is used. In this case, each time the export step is executed, a new version of the process is created in the repository. The default value is `true`. |
| `ingestMaster` | If `true` is set, the master images of the operation are exported. The default value is `true`. |
| `ingestMedia` | If `true` is set, the derivatives of the transaction are exported. The default value is `true`. |
| `ingestJp2` | If `true`, the JPEG2000 images of the operation are exported to the `/media` subcontainer. The default value is `true`. |
| `ingestPdf` | If `true`, the PDFs of the operation are exported to the `/media` subcontainer. The default value is `true`. |
| `ingestMetsFile` | If `true` is set, a METS/MODS file is created and exported to the container. Default value is `true`. |
| `exportMetsFile` | If `true` is set, a METS/MODS file is created and written to the usual export folder \(e.g. `/hotfolder`\). Default value is `true`. |
| `externalLinkContent` |  |
| `fullPartialContent` |  |
| `imagesContainerMetadataQuery` | Optional SPARQL query to add additional attributes and links to the `/images` container. |
| `filesContainerMetadataQuery` | Optional SPARQL query to add additional attributes and links to the `/files` container. |
| `imageFileMetadataQuery` | Optional SPARQL query to write additional attributes for all image files in the repository \(under e.g. `../00000001.tif/fcr:metadata`\). |

The `block` config is repeatable and can define different metadata in different projects. The `workflow` sub-element is used to check whether the current block is to be used for the current step. The system checks whether there is an entry that contains both the workflow name and the current step. If this is not the case, the block is used with `<workflow>*</workflow>`.

## Usage <a id="nutzung-in-goobi"></a>

An export step must be configured:

* Export DMS
* Automatic task
* Plugin for step: FedoraExport

When the step is executed, the Goobi process is exported \(in the same way as it is exported to the file system\) to the configured Fedora Repository, taking into account the configuration \(see above\). 

The following process properties are used to create container URLs or additional container attributes \(and are mandatory\):

* barcode
* unit\_Item\_code
* full\_partial

The process data can then be retrieved from the repository using the following URL pattern:

```text
http(s)://<Fedora REST endpoint>/records/<barcode.substring(0,4)>/<barcode.sunstring(4,8)>/<barcode.substring(8,10)>/
```

## Examples of URLs after successful ingest to Fedora

Example \(`barcode=“barcode123”`\):

### Main container for the pictures

[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/)

### Container for the master images

[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/1/files/master\_00000001.tif](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/1/files/master_00000001.tif)  
[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/2/files/master\_00000002.tif](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/2/files/master_00000002.tif)  
[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/3/files/master\_00000003.tif](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/3/files/master_00000003.tif)

### Containers for JP2 derivatives

[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/1/files/00000001.jp2](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/1/files/00000001.jp2)  
[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/2/files/00000002.jp2  
](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/2/files/00000002.jp2)[http://localhost:8888/fedora/rest/records/barc/ode1/234/images/3/files/00000003.jp2](http://localhost:8888/fedora/rest/records/barc/ode1/234/images/3/files/00000003.jp2)

