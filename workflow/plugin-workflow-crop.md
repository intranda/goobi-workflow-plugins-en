---
description: >-
  This workflow plugin allows you to process images from multiple Goobi processes within the LayoutWizzard interface.
---

# LayoutWizzard workflow plugin

## Introduction

This workflow plugin allows you to edit several images from different Goobi processes in a common LayoutWizard interface. To do this, it determines all the processes whose workflow is currently in the open workflow step to be configured and offers a LayoutWizzard correction for them.

## Overview

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_workflow\_crop |
| Source code | Source code nicht öffentlich verfügbar |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 20.08 |
| Documentation date | 02.09.2020 |

## Installation

To install the plugin the following two files must be installed:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_crop.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_crop-GUI.jar
```

To configure how the plugin should behave, various values can be adjusted in the configuration file. The configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_crop.xml
```

The content of this configuration file is as follows:

```markup
<config>
    <!-- this step must be open in a process for the process to appear in the plugin-->
    <allowed-step>
        LayoutWizzard
    </allowed-step>
    <singleImage>
        <cropFrame>
            <linewidth>2</linewidth>
            <linecolor>#00fa9a</linecolor>
            <fillcolor>#ffffff</fillcolor>
            <clickradius>20</clickradius>
            <fillcolor>#ffffff</fillcolor>
        </cropFrame>
        <spineMarker>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <fillcolor>#ffffff</fillcolor>
            <clickradius>20</clickradius>
        </spineMarker>
    </singleImage>
    <!-- Config for appearance of images in preview mode -->
    <preview>
        <cropFrame>
            <linewidth>2</linewidth>
            <linecolor>#368EE0</linecolor>
            <fillcolor>#f1f2f3</fillcolor>
            <clickradius>20</clickradius>
            <fillcolor>#f1f2f3</fillcolor>
        </cropFrame>
        <spineMarker>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <fillcolor>#f1f2f3</fillcolor>
            <clickradius>10</clickradius>
        </spineMarker>
    </preview>

    <previewCroppingOptions>
        <show>true</show>
    </previewCroppingOptions>
</config>
```

## Operation of the plugin

If the plugin has been correctly installed and configured, it can be found within the menu item `Workflow`. After entering the plugin, a LayoutWizzard preview view is opened, which is very similar to the [Step plugin for Layoutwizzard](https://docs.goobi.io/goobi-workflow-plugins-en/step/layoutwizzard/01_use/01_preview).

![Preview view in the LayoutWizzard workflow plugin](../.gitbook/assets/lw_workflow_01.png)

The operation of this plugin is largely identical to that of the regular LayoutWizard within the preview mode. The only difference worth mentioning here is the presentation of the individual processes, which are visually separated from each other and can be completed by a simple click on the corresponding button. The display of the images is then updated and shows the next process.

![Completion of all images of a process](../.gitbook/assets/lw_workflow_02.png)
