---
description: >-
  This is the technical documentation for the Goobi plug-in for changing the
  workflow based on process properties.
---

# Changing the workflow based on process properties

## Introduction

This documentation describes how to install, configure, and use a plug-in to automatically change workflows at runtime. The plugin can open, close or deactivate steps \(depending on the configuration\). The decision as to what should happen is made on the basis of process properties.

| Details |  |
| :--- | :--- |
| Plugin version | 1.0.0 |
| Identifier | intranda\_step\_changeWorkflow |
| Source code | - Source code not yet publicly available - |
| Compatibility | Goobi Workflow 3.0.0 and newer |
| Documentation date | 29.04.2019 |

## Precondition

The precondition for using the plugin is the use of Goobi workflow in version 3.0.0 or higher, the correct installation and configuration of the plugin as well as the correct integration of the plugin into the desired workflow steps.

## Installation and Configuration

To use the plugin, it must be copied to the following location:

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_changeWorkflow.jar
```

The configuration of the plugin is expected under the following path:

```text
/opt/digiverso/goobi/config/plugin_intranda_step_changeWorkflow.xml
```

The following is a sample configuration with comments:

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
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>Register</project>
        <step>Check</step>
        <!-- name of the property to check -->
        <propertyName>TemplateID</propertyName>
        <!-- expected value -->
        <propertyValue>183</propertyValue>
        <!-- list of steps to open, if property value matches -->
        <steps type="open">
            <title>Box preparation</title>
        </steps>
        <!-- list of steps to deactivate -->
        <steps type="deactivate">
            <title>Image QA</title>
        </steps>
        <!-- list of steps to close -->
        <steps type="close">
            <title>Automatic LayoutWizzard Cropping</title>
            <title>LayoutWizzard: Manual confirmation</title>
        </steps>
        <!-- list of steps to lock -->
        <steps type="lock">
            <title>Automatic export to Islandora</title>
        </steps>
    </config>
   
    <config>
        <!-- which projects to use for (can be more than one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <!-- name of the property to check -->
        <propertyName>upload to digitool</propertyName>
        <!-- expected value -->
        <propertyValue>No</propertyValue>
        <!-- list of steps to open, if property value matches -->
        <steps type="open">
            <title>Create derivates</title>
            <title>Jpeg 2000 generation and validation</title>
        </steps>
        <!-- list of steps to deactivate -->
        <steps type="deactivate">
            <title>Rename files</title>
        </steps>
        <!-- list of steps to close -->
        <steps type="close">
            <title>Upload raw tiffs to uploaddirectory Socrates</title>
            <title>Automatic pagination</title>
        </steps>
        <!-- list of steps to lock -->
        <steps type="lock">
            <title>Create METS file</title>
            <title>Ingest into DigiTool</title>
        </steps>
    </config>
    
</config_plugin>
```

Each `config` block is responsible for a certain project and a certain step, whereby wildcards `*` and multiple answers of processes or steps are also possible. If a step in the workflow is executed with this plugin, the system searches for a `config` block that matches the currently opened step. For example, if in the project "PDF Digitization" the step with the title "Change workflow after PDF extraction" is configured and executed with this plugin, the plugin looks for a `config` block that looks like this:

```markup
<config>
    <project>PDF Digitization</project>
    <step>Change workflow after PDF extraction</step>
    [...]
</config>
```

In each `config` element it is then configured which process property is checked \(`propertyName`\) and which value is expected \(`propertyValue`\). All following `step` elements then describe an action that will be executed if the previously configured property matches the expected value. Steps can be opened `type="open"`, deactivated `type="deactivate"`, closed `type="close"` and locked `type="lock"`.

## Settings in Goobi

After the plugin has been installed and configured, it can be configured in the user interface in a workflow step. Make sure that the name of the step is the same as in the configuration file. In addition, a check mark should be set for `Automatic task`.

![Configuration of the Workflow step](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LZ4vYcdbp6Dw7s7NKy0%2F-LddEhXfAu_y7axe1GeP%2F-LddX7uc7QkwOwTuGvva%2FchangeWorkflow_step.png?alt=media&token=26f24d10-1b13-4043-b847-daf3a1ba4780)

## Usage

Since the plugin should run fully automatically, there is nothing else to consider for the use of the plugin.

