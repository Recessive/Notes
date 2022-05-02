# SimCLR
[Paper](https://arxiv.org/abs/2002.05709)
[Github](https://github.com/google-research/simclr)

"**A Simple Framework for Contrastive Learning of Visual Representations (SimCLR)**"

A model by Google

![[Pasted image 20220501195237.png]]

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


#model
#incomplete