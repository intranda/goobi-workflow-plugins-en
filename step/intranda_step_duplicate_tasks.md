---
description: >-
  This step plugin allows you to duplicate a task automatically according to the value of a process property
---

# Duplicate tasks

## Introduction

This plugin is used to automatically duplicate a task and save new properties referencing this duplication according to the value of a process property.

## Overview

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_duplicate\_tasks |
| Source code | [https://github.com/intranda/goobi-plugin-step-duplicate-tasks](https://github.com/intranda/goobi-plugin-step-duplicate-tasks) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 23.10 |
| Dokumentation date | 16.Nov.2023 |

## Installation

To install the plugin, the following file must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_duplicate_tasks.jar
```

The configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_duplicate_tasks.xml
```

## Configuration

The content of the configuration file looks like this:

```xml
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

        <!-- Process property whose value shall be separated into parts, and it accepts two attributes:
              - @name: name of the process property
              - @separator: separator that shall be used to divide the value of the process property into smaller parts. OPTIONAL. DEFAULT "\n".
         -->
        <property name="short property" separator="," />

        <!-- Name of the step that shall be duplicated. OPTIONAL. If not configured, then the next step following the current one will be used as default. -->
        <stepToDuplicate>Metadata enrichment</stepToDuplicate>
    </config>

</config_plugin>
```

In this configuration file, the block `<config>` can occur repeatedly for different projects or work steps in order to be able to perform different actions within different workflows.

| Value | Description |
| :--- | :--- |
| `project` | This parameter determines the project for which the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which work steps the block `<config>` should apply. The name of the workflow step is used here. This parameter can occur several times per `<config>` block. |
| `property` | This value determines which process property shall be used to control the duplication. It accepts two attributes, where `@name` is mandatory and `@separator` is optional with default value `\n`. |
| `stepToDuplicate` | This OPTIONAL parameter can be used to configure the name of the task that shall be duplicated. If not configured, then the next step following the current one will be used by default. |

## Working logic of this plugin

1. The plugin gets the value of the configured process property and splits it into parts using the possibly configured separator *(or `\n` if not)*.
2. For each part of the original property, the possibly configured step *(or the very next step of the current one if not configured)* will be duplicated once more, and names of these duplicated new steps will be the original step name plus the ordering number among these duplications.
3. For each duplicated new step there will be one new process property created named exactly after this new step's name, with its value set to be the part of the original property, based on which this step was duplicated.
4. When duplications are done for every part of the original property, the original step to duplicate will be deactivated.
