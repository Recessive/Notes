# Code review for [[DSMIL]]
Just my take on what each piece of code is doing

## [compute_feats.py](https://github.com/binli123/dsmil-wsi/blob/master/compute_feats.py):
This is used for the pretrained [[Contrastive Learning]] segment of the network. It is capable of processing either a single magnification, or two magnifications at once (referred to as "low" and "high").

In the case for one magnification, it expects pretrained weights in the `--weighs` argument. This can also be `ImageNet` to instead just use the [[ResNet]] default pretrained weights. Simply enough, once the weights for the `backbone` network are loaded, the features are computed for all `patches` in each `batch` taken from the `bags_list`. They're then saved to the `save_path`

For two magnifications, it expects two sets of pretrained weights in the `--weights_high` and `--weights-low` arguments. It will load both sets of weights into two separate [[ResNet|ResNet's]] 


#machine-learning 
#code