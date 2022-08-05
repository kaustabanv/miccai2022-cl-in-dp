# Characterizing Continual Learning Scenarios for Tumor Classification in Histopathology Images
Supplemental information to accompany paper on Characterizing Continual Learning Scenarios for Tumor Classification in Histopathology Images (MICCAI MOVI 2022) <br/>

**Pseudocode for Creating Augmented CRC Dataset**
---
1. Split publically available H&E CRC dataset into 5 equal sections to simulate 5 data sources.
2. Keep section 1 unchanged. This is referred to as "Domain 1".
3. For each of the remaining 4 parts, do:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(i) Perform stain unmixing of H&E images. (L2-normalized eosin color vector in optical density space: [0.167,0.869,0.466]; L2-normalized hematoxylin color vector in optical density space: [0.660,0.667,0.346]) <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(ii) Apply section-specific augmentation settings as described in table below. <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(iii) Remix augmented stain-unmixed images from previous step.

*Augmentation Details*
| Domain  &nbsp;    | Augmentation Description        | Augmentation Setting                                                                                                                                   | Simulated Data Source                           |
|----------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| Domain 2  &nbsp;  | Increased stain intensity       | Eosin intensity was increased with a scaling factor sampled from [1.75, 2.75] <br/> Hematoxylin (HTX) intensity scaling factor sampled from [1.5, 2.0] | Change in stainer, staining protocol or reagent concentration |
| Domain 3  &nbsp;  | Decreased eosin stain intensity | Eosin intensity scaling factor sampled from [0.4, 2.75]                                                                                                | Variation in staining or use of an older slide with fading stain  |
| Domain 4   &nbsp; | Change of hue                   | Eosin hue scaling factor sampled from [-0.05, -0.03] <br/> HTX from [0.05,0.08]                                                                        | Change in reagent manufacturer, scanner, stainer           |
| Domain 5   &nbsp; | Change of hue and saturation    | Hue was changed for eosin ([0.03, 0.05]) <br/> Saturation was increased for both stains ( eosin by [1.2, 1.4], HTX by [1.1, 1.3])                      | Change in reagent manufacturer, scanner, stainer           |


<br/>

**Hyperparameter Grid Considered in Experiments Across CL Scenarios** 
---
An initial gridsearch was performed for the number of epochs, learning rate, and the decay and milestones for learning rate scheduler. The best values were used in the subsequent method-specific hyperparameter grid as described below. The model was evaluated on the test dataset; the validation dataset was not used in grid search.


| Method     | Hyperparameters Tuned                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------|
| EWC        | Regularization: [1, 3, 10, 30, 100]                                                                            |
| Online EWC | Regularization: [1, 3, 10, 30, 100] <br /> Decay factor: [0.75, 0.8, 0.9, 0.95, 0.99]                                 |
| LwF        | Softmax temperature: [1,2,5,10] <br /> Loss balance weight in distillation loss: [0.75, 1, 1.25]                      |
| iCaRL      | Memory size: [100, 250, 500, 1000]                                                                             |
| A-GEM      | Episodic memory size: [32, 64, 128, 256, 512]                                                                  |
| CoPE       | Softmax temperature: [0.1, 0.5, 2] <br />Online batch size: [32, 64, 128] <br />Distillation loss weight: [0.5, 0.9, 0.99] |


