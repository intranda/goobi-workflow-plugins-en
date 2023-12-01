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
        
         <!-- Process property whose value shall be separated into parts, and it accepts four attributes:
              - @name: name of the process property that shall be splitted
              - @separator: separator that shall be used to split the value of the process property into smaller parts. OPTIONAL. DEFAULT "\n".
              - @target: configure with this attribute where and how to save the splitted parts. OPTIONAL.
                              - IF NOT configured, then all splitted parts will be saved as process properties, and the default property names depend on the configuration of @enabled of the tag <stepToDuplicate>:
                                If @enabled is true, then the default property name will be the step's name that is to be duplicated.
                                If @enabled is false, then the default property name will be the property's @name.
                              - IF configured without using a colon, then all splitted parts will be saved as process properties, and the configured @target will be the new properties' names.
                              - IF configured with a colon, then the part before that colon will control where the changes land, while the part after that colon will define the names of the splitted new parts:
                                Before the colon there are three options: property | metadata | person. For "metadata" and "person", changes will be saved into the METS file. For "property" changes will be saved as properties.
              - @useIndex: determines whether to use an index as suffix to each new process property / metadata entry to distinguish them between each other. OPTIONAL. DEFAULT true.
         -->
         <!-- ATTENTION: there can only be one such tag configured for each step, to split several properties, one has to do that in several steps. -->
        <property name="AssetUri" separator="," target="property:AssetUriSplitted" useIndex="true" />
        
        <!-- Name of the step that shall be duplicated. OPTIONAL. If not configured, then the next step following the current one will be used as default. It accepts an attribute:
              - @enabled: true if some step's duplication is needed, false otherwise. OPTIONAL. DEFAULT true.
         -->
        <stepToDuplicate enabled="true">Metadata enrichment</stepToDuplicate>
    </config>

</config_plugin>
```

In this configuration file, the block `<config>` can occur repeatedly for different projects or work steps in order to be able to perform different actions within different workflows.

| Value | Description |
| :--- | :--- |
| `project` | This parameter determines the project for which the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which work steps the block `<config>` should apply. The name of the workflow step is used here. This parameter can occur several times per `<config>` block. |
| `property` | This value determines which process property shall be used to control the duplication. It accepts four attributes, where only `@name` is mandatory. See the configuration example above for more details. |
| `stepToDuplicate` | This OPTIONAL parameter can be used to configure the name of the task that shall be duplicated. If not configured, then the next step following the current one will be used by default. It accepts one OPTIONAL attribute `@enabled` whose default value is `true`. This attribute controls whether there is a step to duplicate or not. |

## Working logic of this plugin

### With duplication of a step
1. The plugin gets the value of the configured process property and splits it into parts using the possibly configured separator *(or `\n` if not)*.
2. For each part of the original property, the possibly configured step *(or the very next step of the current one if not configured)* will be duplicated once more, and names of these duplicated new steps will be the original step name plus the ordering number among these duplications.
3. For each duplicated new step there will be one new process property or metadata created named according to the configuration of `@target`, with its value set to be the part of the original property, based on which this step was duplicated.
4. When duplications are done for every part of the original property, the original step to duplicate will be deactivated.

### With no duplication of a step
1. The plugin gets the value of the configured process property and splits it into parts using the possibly configured separator *(or `\n` if not)*.
2. For each part of the original property, there will be one new process property or metadata created named according to the configuration of `@target`, with its value set to be exactly this part of the original property.
   
