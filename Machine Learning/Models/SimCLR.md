# SimCLR
[Paper](https://arxiv.org/abs/2002.05709)
[Github](https://github.com/google-research/simclr)

"**A Simple Framework for [[Contrastive Learning]] of Visual Representations (SimCLR)**"

A model by Google

![[SimCLR_overview.png]]

## 1. Data Augmentation
**SimCLR** uses 
- Random cropping
- Resizing
- Colour distortions
- Gaussian blur
As augmentations get more complicated, the model becomes more performant

## 2. Base Encoder
Through much experimentation, [[ResNet]] was chosen. The deeper [[ResNet]] was, the better performance

## 3. Projection Head
Apparently it's useful? Not sure why

## 4. Loss Function
They used [[Normalized Temperature-scaled Cross Entropy Loss]] as their [[Contrastive Loss]] function. Basically after the original image has been all fucked up into 2 separate streams, they try to get the network to agree from both streams. So basically, get the two outputs to be the same, meaning it effectively doesn't care about augmentation.

**Why 2 separate streams. Why stop there?** I assume the encoder isn't trained, I think only the Projection Head is trained. Also I assume that the two streams is actually just 2 inputs through the same network. I don't think there are 2 different networks here.


## The main finding
The research team found that **scaling up** massively increased performance. This means:
- Larger batch size
- Larger backbone
- More [[ML Glossary#Epoch|epochs]]
#model
#incomplete