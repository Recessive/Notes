A kind of [[Contrastive Loss]]. For preamble, let the cosine similarity between $u$ and $v$ be:

$$\huge \text{sim}(u, v) = u^Tv/||u||||v||$$
The loss function is then defined as:

$$\huge L(a, b) = -\text{log} \frac{\text{exp(sim}(a, b)/\tau)}{\sum_{k=1}^{2N} {}_{[k \neq a]} \text{exp(sim}(a, k)/\tau)}$$

Essentially, the [[Fractions#Denominator|denominator]] is the cosine distance between $a$ (the positive sample) and all the negative samples. In the [[Unsupervised Learning|unsupervised]] case, that would be all samples in the training data that isn't some augmented version of the positive input.