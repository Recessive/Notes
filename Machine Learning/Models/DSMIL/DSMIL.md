# Overview
[Paper](https://arxiv.org/pdf/2011.08939.pdf)
[Github](https://github.com/binli123/dsmil-wsi)


## Structure
The network follows a relatively simple structure. The order of operations is below:

![[DSMIL_overview2.png]]

## Pre-processing
1. The [[Whole Slide Image|WSI]] undergoes patch extraction. Essentially split the [[Whole Slide Image|WSI]] up into smaller patches, typically at some kind of magnification. There tends to be more than 1 magnification chosen, so the same [[Whole Slide Image|WSI]] may be split into a bunch of patches at 5x magnification, then at 20x magnification

## Feature extractor training (Contrastive Learning)
The feature extractor is trained using [[Contrastive Learning#Unsupervised|Self-Supervised Contrastive Learning]]. Specifically, [[SimCLR]] is trained to group patches from images. The idea behind this is patches that contain cancerous cells will share similar properties and be grouped together, and patches that don't will be grouped separately. Ultimately this produces a trained [[ResNet]] that is used as the feature extractor on it's own for the remainder of the process.

**DSMIL Uses 2 magnifications, 20x and 5x, so 2 separate [[ResNet|ResNets]] are trained.**

## Embedding
Once the feature extractors have been trained, all patches from both magnifications are run through said extractors to obtain their embeddings. This is done as it's own step so the training for the MIL Aggregator can be done with pre-computed features and be tested faster.

The output of the embeddings for the 2 magnifications are concatenated features. There will be 16 times as many 20x patches as 5x patches, so the concatenation works by duplicating the 5x patches 16 times over for the different 20x patches contained within the 5x patch. (Check the bottom left of the image in [[DSMIL#Structure|Structure]] for a visual explanation)




## MIL Aggregation


#machine-learning
#model
#incomplete 