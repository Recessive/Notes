# Overview
[Paper](https://arxiv.org/pdf/2011.08939.pdf)
[Github](https://github.com/binli123/dsmil-wsi)


The network follows a relatively simple structure. The order of operations is below:

![[DSMIL_overview.png]]

## Pre-processing
1. The [[Whole Slide Image|WSI]] undergoes patch extraction. Essentially split the [[Whole Slide Image|WSI]] up into smaller patches, typically at some kind of magnification. There tends to be more than 1 magnification chosen, so the same [[Whole Slide Image|WSI]] may be split into a bunch of patches at 5x magnification, then at 20x magnification

## Feature extractor training 
The feature extractor is trained using [[Contrastive Learning#Unsupervised|Self-Supervised Contrastive Learning]]. Specifically, [[SimCLR]] is trained to associate patches from the same image into clumps in a batch of sub-images.


## MIL Aggregation


#machine-learning
#model
#incomplete 