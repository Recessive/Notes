# Attention

Initially invented in the 90s under a different name (hypernetworks) it simply takes the [[Dot product]] of a key and a query to produce an attention score, which is then used in a [[Dot product]] with the value.

## Basic overview
Attention consists of a **key, value** and a **query**. They **key** and **query** may be anything, but the **value** is always the input:

![[Attention drawing 1|900]]
The above can be summarised as more or less being a big matrix multiplication. If you take a look at the [[Dot product]], you can see that it can also be represented as a matrix multiplication. Using this form, the flow is ($K$=**key**, $Q$=**query**, $V$=**value**): 
$w'=QK^T$ -> $w=$softmax($w'$) -> $y^T=wV^T$
in the above case $V$ would be the entire input sequence.

Something this diagram is missing is the [[Parameter|parameters]] for attention. Attention typically has [[Parameter|parameters]] for the **key, query** and **value**. So in this case, between the **value** $x_1$ and the multiply gate there would be some weight and bias matrix $W_V$ and $B_V$ that would be multiplied and added to the **value** $x_1$. Note that typically attention has just 1 set of [[Parameter|parameters]] for each of the **key, query** and **value**. This makes attention able to handle indeterminate sequence lengths.

### An important aside
Keep in mind that the attention score for each **value is a single number**. This is the nature of how the [[Dot product]] works.

## Why is it good?
Attention allows for extremely easy gradient-flow, in fact, it's a linear operation and as such avoids any kind of vanishing gradients when flowing backward. This can be seen below:

![[Attention 2022-04-13 17.38.38.excalidraw|500x|center]]
As can be seen, a flow of the gradient entirely avoids the [[Softmax]] function and can freely travel through the network uninterrupted. And as a bonus, the [[Softmax]] function ensures that the gradient that passes back through the $w$ calculation is non-linear, allowing for non-linear fitting as well.

## Bells and whistles
On it's own, attention isn't enough. It's [[Permutation equivariance|permutation equivariant]] (*importantly, this means it doesn't care about order, so for a sequence, it has no concept of first or last*), so needs some addons.

- **Scale by $\sqrt{K}$**: With the dimension of the input being $K$, the average size of the [[dot product]] scales by $\sqrt{K}$, so we must divide the [[dot product]] by $\sqrt{K}$ to keep the values in a reasonable range and prevent explosions.
- **[[Multi-Head Attention]]**: The [[Permutation equivariance]] of attention means for each input, attention does not know it's neighbours. To remedy this, you use [[Multi-Head Attention]]


## Sub types of attention
Attention includes some subtypes, the major one being [[Self attention]]. 

#ofcitwasthe90s
#concept
#attention