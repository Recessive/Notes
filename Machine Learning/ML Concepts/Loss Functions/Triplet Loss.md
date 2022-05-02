# Triplet loss

An extension to [[Contrastive Loss]] that uses triplets instead of pairs. It takes an **anchor** sample, a **positive** sample and a **negative** sample. Used in [[Contrastive Learning]].

During training, the loss enforces that the distance between the **anchor** and **positive** samples is *less* than the distance between the **anchor** and **negative** samples. Basically, it ensures:
$$||\text{anchor}-\text{positive}||^2 < ||\text{anchor}-\text{negative}||^2$$
The loss function is defined as:
$$\huge L(a, p, n) = \text{max}(0, ||a-p||^2-||a-n||^2+m)$$
Where $a$ is the **anchor** sample, $p$ is the **positive** sample, $n$ is the **negative** sample and $m$ is a [[Parameter#Hyper-parameter|hyper-parameter]] defining the upper-bound distance between dissimilar samples [[Contrastive Loss#Why need an upper-bound|the reasoning for which is here]]

**Triplet Loss** takes less time to converge than [[Contrastive Loss]] as each training step simultaneously trains using both similar and dissimilar samples.

#loss-function 