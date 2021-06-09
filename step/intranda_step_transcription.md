---
description: >-
  This step plugin allows you to create transcriptions of works. The
  transcriptions are recorded without word or line coordinates.
---

# Transcription of image content

## Introduction

The transcription plugin allows the user to edit the txt OCR results of a Goobi process. An image and a rich text editor are displayed side by side where the text can be captured.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_transcription |
| Source code | [https://github.com/intranda/goobi-plugin-step-transcription](https://github.com/intranda/goobi-plugin-step-transcription) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 20.09 |
| Documentation date | 11.11.2020 |

## Installation

To use the plugin, these two files must be copied to the following locations:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_transcription.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_transcription-GUI.jar
```

The configuration of the plugin takes place within its configuration file `plugin_intranda_step_transcription.xml`. This is expected under the following path:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_transcription.xml
```

## Configuration

The configuration of the plugin is as follows:

```markup
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
    -->

    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>

        <!-- which folder to use (master|main|jpeg|source|...) -->
        <imageFolder>master</imageFolder>

        <!-- display button to finish the task directly from within the entered plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

The parameters within this configuration file have the following meanings:

| Value | Description |
| :--- | :--- |
| `project` | This parameter specifies for which project the current block `<config>` should apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which steps the block `<config>` should apply. The name of the step is used here. This parameter can occur several times per `<config>` block. |
| `imageFolder` | Specify here from which directory the images are to be displayed. Possible values are e.g. `master`, `media` or individual folders like `photos` and `scans`. |
| `allowTaskFinishButtons` | This parameter can be used to enable buttons for completing the task to be displayed in the regular plugin interface so that the plugin does not have to be exited first. |

## Integration of the plugin into the workflow

To put the plugin into operation, it must be activated for one or more desired tasks in the workflow. This is done as shown in the following screenshot by selecting the plugin `intranda_step_transcription` from the list of installed plugins.

![Assigning the plugin to a specific task](../.gitbook/assets/intranda_step_transcription1_en.png)

As soon as a user subsequently accepts a task that contains this plugin, he can enter it from within the task.

![Plugin within the accepted task](../.gitbook/assets/intranda_step_transcription2_en.png)

## Use of the plugin

After entering the plugin, the user can browse through the image files. A rich text editor is displayed on the right-hand side of each page, showing any existing transcription or OCR results. In this editor, the previous result can now be corrected or a completely new text can be transcribed.

![Plugin within the accepted task](../.gitbook/assets/intranda_step_transcription3_en.png)

{% hint style="info" %}
Please note that this plugin only allows a simple transcription of page content. It is not possible to enter coordinates for paragraphs, lines or words.
{% endhint %}

