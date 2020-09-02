---
description: >-
  This workflow plugin allows the processing of multiple LayoutWizzard
  operations in one step.
---

# LayoutWizzard workflow plugin

## Introduction

This plugin allows you to edit multiple LayoutWizzard processes in one place. It searches for all processes with the correct open step and offers a LayoutWizzard correction for all these processes.

## Overview

| Details |  |
| :--- | :--- |
| Version | 1.0.0 |
| Identifier | intranda\_workflow\_crop |
| Source code | Source code not yet publicly available |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 20.08 |
| Documentation date | 02.09.2020 |

## Installation

To install the plugin the following two files must be installed:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin_intranda_workflow_crop.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_workflow_crop-GUI.jar
```

To configure how the plugin should behave, different values can be adjusted in the configuration file. The configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_workflow_crop.xml
```

The content of this configuration file looks like this:

```markup
<config>
    <!-- this step must be open in a process for the process to appear in the plugin-->
    <allowed-step>
        LayoutWizzard manuelle Kontrolle
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

## Using the plugin

If the plugin has been installed and configured correctly, it can be found within the menu item Workflow. After entering the plugin, a LayoutWizzard preview view is opened, very similar to the one in the step-plugin.

![Preview view in the LayoutWizzard workflow plugin](../.gitbook/assets/lw_workflow_01.png)

The operation is also identical to that in the preview mode, the only difference is that the individual processes are visually separated from each other and are completed with a simple click. The view then jumps to the next process.

![Mark a process as done](../.gitbook/assets/lw_workflow_02.png)

