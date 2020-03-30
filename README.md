---
description: >-
  Documentation for the plugins of the Open-Source-Software Goobi workflow from
  intranda
---

# Overview

On the following pages you will find documentation for various plug-ins and extensions for Goobi workflow. Please select the desired plugin from the table of contents on the left to access the documentation.

Please note that within Goobi workflow there are different types of plugins for the respective application scenarios.

## Export Plugins

Export plugins are used to export data from Goobi workflow to another system. They are executed either automatically as part of the workflow or manually by clicking on the corresponding icon in the process list. They are usually installed within this path:

```text
/opt/digiverso/goobi/plugins/export/
```

Export plug-ins within Goobi are set up in such a way that they are selected from the list of step plug-ins within a workflow for a step and the `Export` checkbox is also activated. Usually, the checkbox `Automatic task` is also selected in order to have the exports executed automatically in the course of the workflow.

![Export Plugin for Fedora within a task](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZnn2ZtNTCCKiXobWA9%2F-LZnnEnsDf8TtAE84tHr%2Foverview_export_de.png?alt=media&token=594da179-6178-4ed1-90e6-f0ff25e56686)

Some export plugins have their own configuration file. This file is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Step Plugins

Step plugins are used to extend tasks within the Goobi workflow. Such plugins can be used, for example, to integrate individual functionality into the workflow that Goobi does not provide out-of-the-box. Examples of such plug-ins include special conversion plug-ins, entry masks, image manipulations, etc.

Such step plugins are installed in the folder:

```text
/opt/digiverso/goobi/plugins/step/
```

If a plugin also has a user interface in addition to the actual functionality, the part of the user interface must also be installed in this folder:

```text
/opt/digiverso/goobi/plugins/GUI/
```

Step plugins in Goobi are set up in such a way that they are selected as plugins within a task.

![Step Plugin for Image Control within a Task](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZnn2ZtNTCCKiXobWA9%2F-LZnnHXkWmQ690kcz76y%2Foverview_step_de.png?alt=media&token=de702227-b691-425c-9d76-d1c031e8f5fe)

Please note that there are currently three different types within Step Plugins:

| Type | Description |
| :--- | :--- |
| **No GUI** | The plugin does not have its own user interface and is executed in the background on the server side. Example: A plugin for the automatic conversion of images into another file format. |
| **Part GUI** | The plugin brings along a part for a user interface and is visually integrated within a processed task as if it were part of the Goobi core. Here the user can interact with the user interface. Example: A plugin for uploading images within a task. |
| **Full GUI** | The plugin comes with a complete user interface. This is not directly integrated into the task. Instead, the user is offered a button to enter the plugin so that he can interact with it. Example: Plugin for image control. |

![Step Plugin for image control within a task](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoBRcPBnhIxP9vIP8r%2Foverview_step2_de.png?alt=media&token=d0752552-3d61-40fe-999f-038eadbd03b4)

Some Step Plugins have their own configuration file. This file is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Opac Plugins

Opac plugins are used for communication with external data sources. Typical examples are plugins for the connection of library catalogues or databases. Depending on the data source, different implementations exist for this in order to correctly address the respective interface to be used.

Opac plugins are usually installed in this path:

```text
/opt/digiverso/goobi/plugins/opac/
```

After installing such a plugin, it is available in the `Search in Opac` field within the Create Processes in Goobi screen.

![Opac Plugin for querying data from an external data source \(catalogue\)](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZnn2ZtNTCCKiXobWA9%2F-LZnnKSOopU9zjkQkcnP%2Foverview_opac_de.png?alt=media&token=f0be182c-c04a-49da-8334-a5df11188a3f)

## Import Plugins

Import plugins are used for the execution of larger mass imports. Unlike Opac plugins, they do not query from a single data source, process by process. Instead, import plugins usually import hundreds or thousands of data at the same time, often in different formats. Common examples here include import plugins for importing SQL dumps, Excel tables or other proprietary data sources.

The import plugins are installed in the folder:

```text
/opt/digiverso/goobi/plugins/import/
```

These plugins are used in a separate mask for mass imports in which you select the different import mechanism and the desired plugin before selecting the data.

![Import Plugin for data transfer from provided directories with xml files as data source](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoBfsdsUHN9J9U2ecg%2Foverview_import_de.png?alt=media&token=407fa8ca-a567-4d4c-96a9-7038f98978e2)

Some import plugins have their own configuration file. This is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Administration Plugins

Administration plugins are available for some special use cases. The special feature is that these plugins are not functionally restricted. They are not explicitly integrated at a given point within the workflow nor are they executed at a defined moment. Instead, they usually have their own user interface and offer independent functionality as an extension of Goobi. Examples of this include administrative intervention in process data or the administration of controlled vocabularies.

The installation of the administration plugins takes place in the folder:

```text
/opt/digiverso/goobi/plugins/administration/
```

‌Since most administration plugins have a user interface in addition to the actual functionality, this must also be installed in the following folder:

```text
/opt/digiverso/goobi/plugins/GUI/
```

![The Vocabulary Manager as Administration Plugin](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoBpiUKp1Q_hn1Z36f%2Foverview_administration_de.png?alt=media&token=cabebc54-d91f-4212-897d-1c0b4eec5a80)

Some administration plugins have their own configuration file. This file is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Workflow Plugins <a id="workflow-plugins"></a>

The workflow plugins are technically very similar to the administration plugins. They can also offer an independent user interface for the provision of additional functionality. In contrast to the administration plug-ins, however, access to these plug-ins is also possible without administrative rights within Goobi, so that a larger group of users usually has access to these functions.

The workflow plug-ins are installed in the folder:

```text
/opt/digiverso/goobi/plugins/workflow/
```

Since most workflow plugins have a user interface in addition to the actual functionality, it must also be installed in the following folders:

```text
/opt/digiverso/goobi/plugins/GUI/
```

![The workflow plugin for mass upload of images for automatic assignment to existing processes](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoBuXhcAjAkMs4B5X9%2Foverview_workflow_de.png?alt=media&token=e2a7b3f4-2ee0-4db1-a001-9c73182dc073)

Some administration plugins have their own configuration file. This file is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Dashboard Plugins <a id="dashboard-plugins"></a>

With the Dashboard Plugins it is possible to provide a special Dashboard with additional functionality instead of the standard start page. This could, for example, already display some statistical information that shows integration with other systems and also give an insight into the current monitoring.

The Dashboard Plugins are installed in the folder:

```text
/opt/digiverso/goobi/plugins/dashboard/
```

The user interface of the dashboards must also be installed in the following folders:

```text
/opt/digiverso/goobi/plugins/GUI/
```

![A dashboard plugin with advanced information about the Goobi instance](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoByyyrZnYO17qnknb%2Foverview_dashboard_de.png?alt=media&token=78b159ac-761f-4bf2-a8e1-d7c2b8a119e1)

Some Dashboard plugins have their own configuration file. This is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

Please also note that individual dashboards must always be activated within the main configuration file goobi\_config.properties. This can be done as follows:

```text
dashboardPlugin=intranda_dashboard_extended
```

## Statistics Plugins

The statistics plugins are available for the provision of individual statistics. Depending on which of these plugins are installed, a wide variety of statistical evaluations can be carried out, either as diagrams, tables or downloads in various formats.

The installation of the statistic plugins takes place in the folder:

```text
/opt/digiverso/goobi/plugins/statistics/
```

The user interface of the statistic plugins must also be installed in the following folders:

```text
/opt/digiverso/goobi/plugins/GUI/
```

![Statistics plugin for displaying completed tasks per year](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoC3py47dPGjz2q_Je%2Foverview_statistics_de.png?alt=media&token=67a4f9cb-77b4-4fa5-8811-3132054e7832)

## Validation Plugins <a id="validation-plugins"></a>

In Goobi, the validation plug-ins are used to ensure that data is available as required before completing a step. If the validation is not successful, the user cannot complete the task and therefore cannot remove it from his task list.

The validation plugins are installed in the folder:

```text
/opt/digiverso/goobi/plugins/validation/
```

The validation plug-in must then be selected accordingly in the `Validation plug-in` field within the task for the required workflow step.

![Validation Plugin for the validation of file names after the import of images](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoC8iMb0KQZdw0pUmD%2Foverview_validation_de.png?alt=media&token=0effebf7-fc56-4a74-aa5b-a76d5d767291)

Some validation plugins have their own configuration file. This is generally named like the plugin itself and is usually located at the following path:

```text
/opt/digiverso/goobi/config/
```

## Web-API Plugins <a id="web-api-plugins"></a>

For the communication of external systems with Goobi, Goobi workflow offers a Web API for which individual plug-ins are available. For example, external systems can be used to send messages on the process log of a process, complete tasks, query status information and much more.

Web-API Plugins are installed in the following folder:

```text
/opt/digiverso/goobi/plugins/command/‌
```

Web-API plugins do not have their own user interface and are only used for communication via HTTP calls.

![Web-API Plugin for executing a command via HTTP](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LZoBGIGyj5S-p4ygs2S%2F-LZoCEIFUPd5nQ5wzi7g%2Foverview_webapi.png?alt=media&token=24be2697-62cb-4d50-9832-8eadc284870e)

Please note that an access can be defined individually for each command. This is done via the following configuration file:

```text
/opt/digiverso/goobi/config/goobi_webapi.xml
```

This can be used to specify from which IP range access is allowed for which command.

## REST Plugins <a id="rest-plugins"></a>

With the REST plugins, Goobi has another way for external systems to communicate with Goobi. In contrast to the Web API, however, communication here is via REST and takes place largely via JSON.

REST plugins are installed in the following folder:

```text
/opt/digiverso/goobi/plugins/rest/
```

Like the Web-API plugins, the REST plugins do not have their own user interface. Also the access permission is controlled by the same configuration file and controls the access from selected IP addresses and authentication. Also for the REST Plugins the configuration is done in the following file:

```text
/opt/digiverso/goobi/config/goobi_webapi.xml
```

[  
](https://app.gitbook.com/@intranda/s/goobi-workflow-plugins-de/administration-plugins/catalogue-poller)

