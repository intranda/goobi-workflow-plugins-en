# Ausgewählter Analyseschritt

This area is only visible when an analysis step is selected and is then named after the respective step. It contains configuration parameters for each step, which determine the automatic analysis of this step for all images. The most important parameters are directly visible, all others only in the extended view. Help on the meaning of each parameter is displayed in the Goobi header bar when you click `Show help texts`.

![Selected analysis step with settings](../../../../.gitbook/assets/intranda_step_crop_08.png)

Below the parameters, this area also has three buttons that control the execution of the analysis:

| Icon | Description |
| :--- | :--- |
| ![](../../../../.gitbook/assets/intranda_step_crop_34.png) | This button analyses the current image for the selected analysis step, regardless of the status of the analysis step. If previous analysis steps have not yet been executed, these steps are also executed. |
| ![](../../../../.gitbook/assets/intranda_step_crop_35.png) | This button resets the analysis status of all images for the selected analysis step. The step and all subsequent ones are then considered as not executed for all images. Existing analysis results are discarded. This is important if you want to perform an automatic analysis again, since analysis steps that are considered to have already been performed are not performed again otherwise. |
| ![](../../../../.gitbook/assets/intranda_step_crop_36.png) | This button resets the analysis status for the current and all subsequent images. |

