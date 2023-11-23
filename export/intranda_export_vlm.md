---
description: >-
  This is a technical documentation for the VLM Export Plugin. It enables the export to a VLM instance.
---

# VLM Export

## Introduction

This documentation describes the installation, configuration and use of the VLM export plugin in Goobi.

Using this plugin for Goobi, Goobi operations can be exported to the configured location for VLM within one step.

| Details |  |
| :--- | :--- |
| Identifier | intranda_export_vlm |
| Source code | [https://github.com/intranda/goobi-plugin-export-vlm](https://github.com/intranda/goobi-plugin-export-vlm) |
| Licence | GPL 2.0 or newer |
| Documentation date | 04.Sep.2023 |

## Installation

This plugin is integrated into the workflow in such a way that it is executed automatically. For use within a workflow step, it should be configured as shown in the screenshot below.

![Integration of the plugin into the workflow](../.gitbook/assets/intranda_export_vlm_en.png)

The plugin must first be copied to the following directory:

```text
/opt/digiverso/goobi/plugins/export/plugin_intranda_export_vlm.jar
```

In addition, there is a configuration file that must be located in the following place:

```text
/opt/digiverso/goobi/config/plugin_intranda_export_vlm.xml
```
## Configuration

The plugin is configured via the configuration file `plugin_intranda_export_vlm.xml`. The configuration can be adjusted during operation. The following is an example configuration file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
	<!-- 
	Order of configuration is: 
	1.) project name matches
	2.) project is * 
	-->

	<!-- There could be multiple config blocks. -->
	<!-- Please make sure that the project names of different config blocks are also different. -->
	<!-- Given two config blocks with the same project name, the settings of the first one will be taken. -->
	<config>
		<!-- The name of the project -->
		<!-- MANDATORY -->
		<project>Archive_Project</project>
		
		<!-- The name of the system, e.g. AlmaIDDigital, AlephIDDigital, CatalogIDDigital.  -->
		<!-- This tag has the following two OPTIONAL attributes:
				- @anchorSplitter: if configured with a non-blank string, then it will be used to split the metadata value into two parts, where its head will be used 
						as the main folder's name, while its tail will be used as part of the volume's name. DEFAULT value is an empty string, i.e. no splitting expected.
				- @volumeFormat: only works when @anchorSplitter is configured with a non-blank string.
						It is used as the left padding if the volume's name is shorter than it. DEFAULT value is an empty string, i.e. no padding needed.
		 -->
		<!-- MANDATORY -->
		<identifier anchorSplitter="" volumeFormat="000">CatalogIDDigital</identifier>
	    
		<!-- The name to be used to distinguish between different volumes of one book series. -->
		<!-- Alternatively one may also choose "TitleDocMain", just assure its difference between volumes. -->
		<!-- Leave the default value unchanged if the book is a one-volume work. -->
		<!-- MANDATORY -->
		<volume>CurrentNoSorting</volume>
	    
		<!-- The place you would like to use for the export. -->
		<!-- Absolute path expected. No difference whether you append the directory separator '/' to the end or not. -->
		<!-- If left blank, then the default setting '/opt/digiverso/viewer/hotfolder' will be used. -->
		<path></path>
	    
		<!-- The prefix you would like to use for subfolders for different volumes. -->
		<!-- Leave it blank if no common prefix is needed. -->
		<subfolderPrefix>T_34_L_</subfolderPrefix>
		
		<!-- Whether or not use SFTP for the export. -->
		<!-- If true then use SFTP. If false then perform local export. -->
		<!-- If left blank, then the default setting 'false' will be used. -->
		<sftp>true</sftp>
		
		<!-- If true then use ssh key for connection. If false then use password. OPTIONAL. DEFAULT false. -->
		<useSshKey>false</useSshKey>
		
		<!-- Absolute path to the location of the file 'known_hosts'. -->
		<!-- If left blank, then the default setting '{user.home}/.ssh/known_hosts' will be used. OPTIONAL. -->
		<knownHosts></knownHosts>
		
		<!-- User name at the remote host. -->
		<!-- MANDATORY if sftp is set to be true. -->
		<username>CHANGE_ME</username>
		
		<!-- Name of the remote host. -->
		<!-- MANDATORY if sftp is set to be true. -->
		<hostname>CHANGE_ME</hostname>
		
		<!-- Port of the remote host. -->
		<!-- OPTIONAL. DEFAULT 22. -->
		<port>CHANGE_ME</port>
		
		<!-- Password to log into the remote host 'username'@'hostname'. -->
		<!-- MANDATORY if sftp is set to be true, while useSshKey is set to be false or not set. -->
		<password>CHANGE_ME</password>
		
		<!-- Path to the private key file, e.g. ~/.ssh/id_rsa -->
		<!-- The key is expected to be of PEM format, beginning with `BEGIN RSA PRIVATE KEY`. -->
		<!-- The OPENSSH format, beginning with `BEGIN OPENSSH PRIVATE KEY`, is not supported yet. -->
		<!-- MANDATORY if sftp and useSshKey are both set to be true. -->
		<keyPath>CHANGE_ME</keyPath>
	</config>
	
	<config>
		<project>Manuscript_Project</project>		
		<identifier>CatalogIDDigital</identifier>		
		<volume>CurrentNoSorting</volume>	
		<!-- Setting up path using a goobi variable. -->
		<!-- No difference whether you add a '/' between '}' and '..' or not. -->		
		<path>{goobiFolder}../viewer/hotfolder/</path>
		<!-- No common prefix needed. -->
		<subfolderPrefix></subfolderPrefix>
		
		<sftp>false</sftp>
		<!-- Use the default setting '{user.home}/.ssh/known_hosts'. -->
		<knownHosts></knownHosts>
		
		<username></username>
		<hostname></hostname>
		<password></password>
	</config>
	
	<config>
		<project>*</project>
		<identifier>CatalogIDDigital</identifier>
		<volume>CurrentNoSorting</volume>		
		<!-- Setting up path using an ABSOLUTE path. -->
		<path>/opt/digiverso/viewer/hotfolder</path>
		<!-- No common prefix needed. -->
		<subfolderPrefix></subfolderPrefix>
		
		<!-- Use the default setting 'false'. -->
		<sftp></sftp>
		<!-- Use the default setting '{user.home}/.ssh/known_hosts'. -->
		<knownHosts></knownHosts>
		
		<username></username>
		<hostname></hostname>
		<password></password>
	</config>

</config_plugin>
```

| Parameter         | Explanation                                                                                                            |
|:----------------- |:---------------------------------------------------------------------------------------------------------------------- |
| `identifier`      | This parameter determines which metadatum is to be used as the folder name. It has two optional attributes `anchorSplitter` and `volumeFormat` which will be used for the case when the value of this `identifier` itself contains both main folder's name as well as volume's name, separated by this configured `@anchorSplitter`. `@volumeFormat` is used in this case as the left padding for the volume's name. |
| `volume`          | This parameter controls with which metadata the subdirectories for volumes are to be named. |
| `path`            | This parameter sets the export path where the data is to be exported. An absolute path is expected. |
| `subfolderPrefix` | This parameter describes the prefix to be placed in front of each volume of a multi-volume work in the folder name. (Example `T_34_L_`: Here `T_34` stands for the recognition for the creation of a structure node of the type `volume` and the `L` indicates that a text comes after it.). |
| `sftp`            | This parameter determines whether to use SFTP for the export process or not. |
| `useSshKey`        | This parameter determines whether to use a SSH key file for the connection to the remote host. |
| `knownHosts`      | This parameter determines where the file `known_hosts` is. If left empty, then the default setting `{user.home}/.ssh/known_hosts` will be used. Otherwise, it is an absolute path expected here. |
| `username`        | This parameter determines the user name to log into the remote host. |
| `hostname`        | This parameter determines the name of the remote host or its IP address. |
| `port`        | This parameter determines the port number of the remote host that is to be used for the connection. |
| `password`        | This parameter determines the password to be used to log into the remote host as `username`@`hostname`. |
| `keyPath`        | This parameter determines the path to the SSH key file to be used to log into the remote host as `username`@`hostname`. |
