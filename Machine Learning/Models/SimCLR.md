# SimCLR
[Paper](https://arxiv.org/abs/2002.05709)
[Github](https://github.com/google-research/simclr)

"**A Simple Framework for [[Contrastive Learning]] of Visual Representations (SimCLR)**"

A model by Google

![[SimCLR 2022-05-03 14.16.01.excalidraw]]

## 1. Data Augmentation
**SimCLR** uses 
- Random cropping then resizing to original size
- Colour distortions
- Gaussian blur
As augmentations get more complicated, the model becomes more performant

## 2. Base Encoder
Through much experimentation, [[ResNet]] was chosen. The deeper [[ResNet]] was, the better performance

## 3. Projection Head
Apparently it's useful? Not sure why

## 4. Loss Function
They used [[Normalized Temperature-scaled Cross Entropy Loss]] as their [[Contrastive Loss]] function. Basically after the original image has been all messed up into 2 separate streams, they try to get the network to agree from both streams. So basically, get the two outputs to be the same, meaning it effectively doesn't care about augmentation.

## Training process
Uses [[Layer-wise Adaptive Rate Scaling|LARS]] Optimizer for training.
- Two separate data augmentation operators are sampled from the same family of augmentations and applied to one training sample to obtain two correlated views (*This is done before anything is run through the network. So all training samples in the batch have this done first*)
- A base encoder network is run on both augmentation separately for each training sample
- The projection head is run on both augmentation separately, and [[Contrastive Loss]] applied to maximize agreement between augmentations, and disagreement between all other samples in the batch
- Both the projection head and base encoder are trained. There is only one base encoder network and one projection head, they are just duplicated for each augmented sample.


#### Once training is completed *the projection head is thrown away* and just the output from the base encoder is used

![[SimCLR_Algorithm.png]]


## The main finding
The research team found that **scaling up** massively increased performance. This means:
- Larger batch size
- Larger backbone
- More [[ML Glossary#Epoch|epochs]]

"In our contrastive learning, as positive pairs are computed in the same device, the model can exploit the local information leakage to improve prediction accuracy without improving representations." - Don't quite know what this means. It's an explanation as to why they use Global BN.


#machine-learning
#model