[Medium Article](https://medium.com/@steinsfu/diffusion-model-clearly-explained-cd331bd41166)
[YT vid 1](https://www.youtube.com/watch?v=yTAMrHVG1ew&ab_channel=AssemblyAI)
[Stable Diffusion Paper 1](https://arxiv.org/pdf/2112.10752.pdf)

## Overview

The idea of a diffusion model is to turn noise into output. To do this, it attempts to learn parameters that aim to denoise. Basically to undo diffusion. 

This isn't possible to do deterministically, as noise is true random a distribution of noise could have come from anything.

So we estimate using learned parameters.

A typical model would look like the following:

1. **Forward diffusion process:** Iteratively apply low levels of noise to an input until the input is sufficiently noisy (I'm assuming the amount of times you apply the noise is enough to where the input is now indistinguishable from the true gaussian noise you will feed into the model later) [[Diffusion Model#1|Note 1]]
2. **Reverse diffusion process:** Train a UNet on the steps previously created backwards to denoise an image

And that's it! Pretty simple concept to be honest


## Notes:

### 1
Each step applying the noise forms a [[Markov Chain]], effectively each step is independent. This makes the problem tractable, otherwise if previous steps affected multiple later steps it would turn into a factorial problem. 

![[Diffusion Model 2023-07-03 14.22.05.excalidraw]]

According to ChatGPT, the reason steps are applied instead of just applying heaps of noise immediately and getting a final noisy output in one step is it allows the network to learn more intricate details. There's a few reasons, but you can imagine that it is easier to learn how to undo a tiny bit of noise than it is to undo heaps of it.

Additionally, it suffices to heuristically stop the noising process when you think the data is noisy enough. ChatGPT says that it could potentially be an issue with mismatched distributions (I think it was just confirming my thought process rather than actually thinking for itself), but I think it's fine, since theoretically a Gaussian noised image could produce every possible image.
