---
description: >-
  With the plugin for the quality control of images, images can be checked in
  detail for their quality
---

# Quality control of images

## Introduction

​ This plugin is used to visually check the quality of images. It allows different views of images as thumbnails, in large display or even in full screen mode. In addition, the full text of the image can be displayed and various functions for download or image manipulation can be activated. ​

| Details |  |
| :--- | :--- |
| Identifier | intranda\_step\_image\_selection |
| Source code | [https://github.com/intranda/goobi-plugin-step-image-selection](https://github.com/intranda/goobi-plugin-step-image-selection) |
| Licence | GPL 2.0 or newer |
| Compatibility | Goobi workflow 2023.03 |
| Dokumentation date | 21.03.2023 |

## Installation

​ To use the plugin, these two files must be copied to the following locations: ​

```text
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_image_selection.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_step_image_selection-GUI.jar
```

​ The configuration of the plugin takes place within its configuration file `plugin_intranda_step_imageQA.xml`. It is expected to be located under the following path: ​

```text
/opt/digiverso/goobi/config/plugin_intranda_step_image_selection.xml
```

## Configuration of the plugin

​ The configuration of the plugin is structured as follows: ​

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
        
        <!-- define how many images shall be loaded in the beginning, DEFAULT 20 -->
        <defaultNumberToLoad>20</defaultNumberToLoad>
        
        <!-- define how many more images shall be loaded when the window is scrolled to the bottom, DEFAULT 10 -->
        <defaultNumberToAdd>10</defaultNumberToAdd>
        
        <!-- define which folder shall be used to display the thumbnails, possible values are master | main | jpeg | source | ... -->
        <folder>main</folder>
        
        <!-- define how many images shall be selected as maximum, DEFAULT 5 -->
        <!-- if smaller than min, then no selection is permitted -->
        <max>5</max>
        
        <!-- define how many images shall be selected as minimum, DEFAULT 1 -->
        <!-- if greater than max, then no selection is permitted -->
        <min>0</min>
        
        <!-- display button to finish the task directly from within the entered plugin -->
        <allowTaskFinishButtons>true</allowTaskFinishButtons>
    </config>

</config_plugin>
```

​ The parameters within this configuration file have the following meanings: ​

| Value | Description |
| :--- | :--- |
| `defaultNumberToLoad` | With this parameter you can define how many images shall be loaded in the beginning. Default 20. |
| `defaultNumberToAdd` | With this parameter you can define how many more images shall be loaded when scrolled to the bottom. Default 10. |
| `folder` | Specify here the configured name of the folder from which the images are to be displayed. Possible values are `master`, `main`, `jpeg`, `source`... as long as they are well configured. |
| `max` | Here you can define the maximum number of thumbnails that could be selected. |
| `min` | Here you can define the minimum number of thumbnails that should be selected in order to save as a process property. |
| `allowTaskFinishButtons` | With this parameter you can define whether or not to enable this button. |


### Integration of the plugin into the workflow

​To put the plugin into operation, it must be activated for one or more desired tasks in the workflow. This is done as shown in the following screenshot by selecting the `intranda_step_image_selection` plugin from the list of installed plugins. ​​

![Assigning the plugin to a specific task](../.gitbook/assets/intranda_step_image_selection1.png)

![Integration of the plugin into a task within the workflow](../.gitbook/assets/intranda_step_image_selection2.png)

### Function and operation of the plugin

1. The plugin will show some images from the configured folder in the left box, and when the window is scrolled to the bottom, more images will be shown as long as there are still some available.
2. When the mouse cursor is over an image, a zoomed-in view of the image will be shown, which can be used to check the details of the image.
3. The selected images will be shown in the right box, and the top one will be bigger than the others.
4. New images can be selected via Drag & Drop. If the relative position to drop is captured correctly, then the new selected image will be inserted there, otherwise it will be appended to the end.
5. If the configured `max` is already reached, or if the to-be-selected image was already selected once, then the selection won't work.
6. Selected images can be deselected via Drag & Drop. Just drag the image from the right box and drop it to the left one.
7. Selected images can be reordered via Drag & Drop with one limitation regarding the last image: the last position is not supported yet for drop, therefore if one wants to let some other image to take the last position, one has to do some swaps.
8. Remember to click the `Save Property` button to save the information of the selected into the process property. Otherwise the changes will be discarded and will not be restored the next time the plugin is started.
9. One can also exit the plugin without saving the changes by clicking the button next to `Save Property`.
