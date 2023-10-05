---
description: >-
  This plugin is originally implemented to communicate with ALMA system and process returned responses. But thanks to its general design, it can be used to communicate with other systems via REST as well.
---

# ALMA API plugin

## Introduction

​This plugin is used to send requests to a service system, e.g. ALMA, and process the returned responses. Multiple commands can be configured to compose a complicated task and the plugin will run these commands one by one in the same order.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_alma\_api |
| Source code | [https://github.com/intranda/goobi-plugin-step-alma-api](https://github.com/intranda/goobi-plugin-step-alma-api) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2023.09 |
| Dokumentation date | 05.10.2023 |

## Installation

​To use the plugin, the jar file is expected to be located to: ​

```text
{GOOBI_INSTALLATION_PATH}/plugins/step/plugin_intranda_step_alma_api.jar
```

​The configuration file `plugin_intranda_step_alma_api.xml` is expected to be located to: ​

```text
{GOOBI_INSTALLATION_PATH}/config/plugin_intranda_step_alma_api.xml
```

## Configuration of the plugin

​The configuration of the plugin is structured as follows: ​

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

        <!-- Base URL -->
        <url>https://api-eu.hosted.exlibrisgroup.com</url>

        <!-- API key -->
        <api-key>CHANGE_ME</api-key>

        <!-- Variables that can be used for following commands.
              @name: name of the variable, e.g. VARIABLE. To use this variable's value, one can simply use {$VARIABLE}.
              @value: value to initialize this variable. It can be plain string value, or a Goobi variable, e.g. {meta.NAME} for a Metadata named NAME.
         -->
        <variable name="MMS_ID" value="{meta.RezenssionsZssDBID}" />
        <!-- There can be multiple variables defined before all commands. -->
        <variable name="SIGNATURE" value="{meta.shelfmarksource}" />

        <!-- A command is a step to perform. There can be several commands configured, and if so, they will be run one by one in the same order as they are defined.
              @method: get | put | post | patch
              @endpoint: raw endpoint string without replacing the placeholders enclosed by {}. For every placeholder say {PLACEHOLDER}, one has to configure it in a
                         sub-tag, and in our example it would be <PLACEHOLDER>. Options for values of these sub-tags are:
                         - plain text value
                         - any variable defined by a <variable> tag before all <command> blocks
                         - any variable defined by a <target> sub-tag of any previous <command> block
         -->     
        <command method="get" endpoint="/almaws/v1/bibs/{mms_id}/holdings/ALL/items">
        	<!-- define the value of the placeholder {mms_id} using the variable named MMS_ID -->
        	<mms_id>{$MMS_ID}</mms_id>

        	<!-- Filter condition that is to be applied on the REST call response. OPTIONAL.
        	      @key: JSON path from which the values are to be fetched for the filtering process
        	      @fallback: JSON path that shall be used when the JSON path configured by @key contains no value. OPTIONAL.
        	      @value: value to be compared with, which can be a plain text value, or a variable defined previously in the format {$VARIABLE}
        	      @alt: alternative option if there is no match found. Options are: all | first | last | random | none. OPTIONAL.
                      - all: filter nothing out, search the whole JSONObject for the following targets
                      - first: get the first JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - last: get the last JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - random: get a random JSONObject related to the common heading path shared by filter and targets, and search for targets within it
                      - none: filter everything out and stop searching. DEFAULT.
        	 -->
        	<filter key="item.item_data.alternative_call_number" fallback="item.holding_data.permanent_call_number" value="{$SIGNATURE}" alt="all" />

        	<!-- Target values that is to be retrieved from the REST call response and saved as a variable. OPTIONAL.
        	      @var: name of the variable that is to be used to save the target values retrieved.
                      If the variable was already defined before, then its value will be updated. Otherwise a new variable under this name will be created.
        	      @path: JSON path from where values are to be retrieved.
        	 -->
        	<target var="ITEM_PID" path="item.item_data.pid" />

        	<!-- There can be multiple targets configured. -->
        	<!-- In this example, the new variable will be named HOLDING_ID and it can be reused in the following steps using {$HOLDING_ID}. -->
        	<target var="HOLDING_ID" path="item.holding_data.holding_id" />
        </command>

        <command method ="post" endpoint="/almaws/v1/bibs/{mms_id}/holdings/{holding_id}/items/{item_pid}">
        	<!-- define the value of the placeholder {mms_id} using the variable named MMS_ID -->
        	<mms_id>{$MMS_ID}</mms_id>
        	<!-- define the value of the placeholder {holding_id} using the variable named HOLDING_ID -->
        	<holding_id>{$HOLDING_ID}</holding_id>
        	<!-- define the value of the placeholder {item_pid} using the variable named ITEM_PID -->
        	<item_pid>{$ITEM_PID}</item_pid>

          <!-- A parameter tag can be used to define parameters that shall be sent by the REST request. There can be multiple parameters configured.
                  @name: parameter name
                  @value: parameter value, which can only be plain text values here
            -->
        	<parameter name="op" value="scan" />
        	<parameter name="library" value="" />
        	<parameter name="department" value="InDigiZ_Dep" />
        	<parameter name="work_order_type" value="InDigiZ" />
        	<parameter name="done" value="false" />
        </command>

        <!-- A property tag is used to define a process property that is to be saved after running all previous commands. OPTIONAL.
              @name: name of the new process property
              @value: value of the new process property, possible values are
                      - a plain text value
                      - a variable defined before all <command> blocks via a <variable> tag
                      - a variable defined within some <command> block via a <target> tag
              @choice: indicates how many items should be saved into this new property, OPTIONS are first | last | all | random.
                       - first: save only the first one among all retrieved values
                       - last: save only the last one among all retrieved values
                       - random: save a random one from all retrieved values
                       - all: join all retrieved values to formulate a single string separated by commas and save it. DEFAULT.
              @overwrite: true if the old property named so should be reused, false if a new property should be created, DEFAULT false.
        -->
        <property name="holding_id" value="{$HOLDING_ID}" choice="all" overwrite="true" />
        <!-- There can be multiple property tags configured. -->
        <property name="item_pid" value="{$ITEM_PID}" choice="all" />

    </config>

</config_plugin>
```

### General configurations

| Value | Description |
| :--- | :--- |
| `project` | This parameter determines for which project the current block `<config>` is to apply. The name of the project is used here. This parameter can occur several times per `<config>` block. |
| `step` | This parameter controls for which work steps the block &lt;config&gt; should apply. The name of the work step is used here. This parameter can occur several times per `<config>` block. |
| `url` | Specify here the base URL of the system. |
| `api-key` | Put the api-key for connecting to the system here. |
| `variable` | This tag allows you to define a variable that can be used by all commands below. This tag has two attributes where `@name` defines its name and `@value` defines its value which expects a plain text value or a Goobi variable here. |
| `command` | A command block defines a command that shall be run in the order. It has two attributes itself where `@method` specifies the method to be used, and `@endpoint` specifies the raw endpoint path with all placeholders unreplaced. See the table below and the example configuration above for more details. |
| `property` | An optional property tag defines a process property that shall be saved after running all commands. It has two mandatory attributes, where `@name` defines the name of the process property, and `@value` determines the property's value, which can be a plain text value or a variable defined before. It also has two optional attributes, where `@choice` specifies which value shall be saved when there are multiple found, and `@overwrite` determines whether to reuse a previously created process property of the same name or not. |

### Configurations within command blocks

| Value | Description |
| :--- | :--- |
| `filter` | Here specifies one which parts of the response JSON shall be used to search for the `target` values. It has four attributes, where `@key` and `@value` are mandatory, while `@fallback` and `@alt` are optional. See the comments in the example configuration for more details. |
| `target` | Here specifies one which values shall be saved as variables for later use. It has two attributes, where `@var` specifies the variable name, and `@path` specifies the JSON path to retrieve the values. |
| `parameter` | Here specifies one parameters that shall be sent together via request to the system. It has two attributes, namely `@name` for parameter name, and `@value` for parameter value which can ONLY be plain text values. |
