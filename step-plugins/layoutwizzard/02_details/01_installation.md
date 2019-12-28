# Installation

## Program Libraries

The installation consists of a total of four program libraries that must be accessible in Apache Tomcat or Goobi:

| File | Location |
| :--- | :--- |
| `layoutwizzard.jar` | In the `lib` folder of the Goobi webapp in the Tomcat |
| `plugin_intranda_step_LayoutWizzard.jar` | In the `plugins/step` folder in the Goobi working directory |
| `plugin_intranda_step_LayoutWizzard-GUI.jar` | In the `plugins/GUI` folder in the Goobi working directory |

## Configuration files

In addition to these program files, two configuration files are required, one for the Goobi plug-in and one for the underlying LayoutWizzard program library.

### Plugin configuration

The configuration file of the plugin `plugin_LayoutWizzardPlugin.xml` must be located in the `config` configuration directory within the Goobi working directory. This is usually the path to the file:

```text
/opt/digiverso/goobi/config/plugin_LayoutWizzardPlugin.xml
```

Within this configuration file the path to the actual central configuration of the LayoutWizard is specified. The structure of this file looks like this:

{% tabs %}
{% tab title="plugin\_LayoutWizzardPlugin.xml" %}
```markup
<!-- Goobi Plugin configuration file -->
<config_plugin>
        <layout-wizzard-config-path>
                /opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
        </layout-wizzard-config-path>
</config_plugin>
```
{% endtab %}
{% endtabs %}

### LayoutWizzard configuration

The actual configuration file specifies various parameters for the layout analysis process. These parameters are listed as examples in the following configuration file. As defined in the plugin configuration file, it is located under the following path:

```text
/opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
```

As an example, this configuration file has the following content:

```markup
<!-- intranda Layout Wizzard configuration file -->
<config>
    <contentServerUrl>http://demo03.intranda.com/goobi/cs/cs</contentServerUrl>
    <defaultOutputFolderSuffix>media</defaultOutputFolderSuffix>
    <analysisImagesBasePath>/home/florian/LayoutWizzard/samples/</analysisImagesBasePath>
    <previews>
        <previewsPerPage>100</previewsPerPage>
        <maxPreviewsCached>10000</maxPreviewsCached>
        <previewWidth>700</previewWidth>
        <imageHeightLarge>800</imageHeightLarge>
    </previews>
    <outliers>
        <errorMultiplier>3.0</errorMultiplier>
        <weightExponent>2.0</weightExponent>
    </outliers>
    <saving>
        <defaultCompression quality="85">NONE</defaultCompression>
        <overwriteExistingImages>true</overwriteExistingImages>
    </saving>
    <analysis id="firstPageLeft">
        <firstPageOrientation>LEFT</firstPageOrientation>
    </analysis>
    <analysis id="firstPageRight">
        <firstPageOrientation>RIGHT</firstPageOrientation>
    </analysis>
    <analysis>
        <analysisStep name="PAGESKEW" type="edges" use="true">
            <saveAnalysisImages visibility="INVISIBLE" path="DESKEW">false</saveAnalysisImages>
            <deskewerMode visibility="VISIBLE">ALL_EDGES</deskewerMode>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode>GROUP_BY_DISTANCE</lineGroupingMode>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <analysisImageSize>300</analysisImageSize>
            <lowerCannyThreshold>70</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>10</minHoughLineLength>
            <houghLineThreshold>50</houghLineThreshold>
            <maxHoughLineGapSize>2</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>7</maxLineDistance>
            <rimAreaToIgnoreLines>0.0</rimAreaToIgnoreLines>
        </analysisStep>
        <analysisStep name="CONTENTAREA" use="true">
            <analysisImageSize>0</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="edgeDetection">false</saveAnalysisImages>
            <bitonalThreshold>150</bitonalThreshold>
            <bitonalInvert>false</bitonalInvert>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <contentPadding visibility="VISIBLE">0</contentPadding>
        </analysisStep>
        <analysisStep name="BOOKSPINE" use="true">
            <analysisImageSize>400</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="spineDetection">false</saveAnalysisImages>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode visibility="INVISIBLE">GROUP_BY_DISTANCE</lineGroupingMode>
            <croppingAggressiveness visibility="VISIBLE">BALANCED</croppingAggressiveness>
            <lowerCannyThreshold>40</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>20</minHoughLineLength>
            <houghLineThreshold>10</houghLineThreshold>
            <maxHoughLineGapSize>4</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>5</maxLineDistance>
            <featureSizeThreshold>0.1</featureSizeThreshold>
            <rimAreaToIgnoreLines>0.0125</rimAreaToIgnoreLines>
            <maxGroupAngleDeviation visibility="INVISIBLE">10</maxGroupAngleDeviation>
            <spineOffset visibility="VISIBLE">5</spineOffset>
        </analysisStep>
    </analysis>
</config>
```

For details on customizing the configurations, see the Configuration section.

